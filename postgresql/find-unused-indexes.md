## Find unused indexes

It's possible to determine which indexes are unused or under-utilised for a given PostgreSQL database.

This snippet provides a 'view' that can be used for this:

```
CREATE OR REPLACE VIEW public.zz_helper_find_unused_indexes
AS
WITH table_scans AS (
        SELECT tables.relid,
          tables.idx_scan + tables.seq_scan AS all_scans,
          tables.n_tup_ins + tables.n_tup_upd + tables.n_tup_del AS writes,
          pg_relation_size(tables.relid::regclass) AS table_size
          FROM pg_stat_user_tables tables
      ), all_writes AS (
        SELECT
              CASE
                  WHEN sum(table_scans.writes) = 0::numeric THEN 1::numeric
                  ELSE sum(table_scans.writes)
              END AS total_writes
          FROM table_scans
      ), indexes AS (
        SELECT idx_stat.relid,
          idx_stat.indexrelid,
          idx_stat.schemaname,
          idx_stat.relname AS tablename,
          idx_stat.indexrelname AS indexname,
          idx_stat.idx_scan,
          pg_relation_size(idx_stat.indexrelid::regclass) AS index_bytes,
          indexes.indexdef ~* 'USING btree'::text AS idx_is_btree
          FROM pg_stat_user_indexes idx_stat
            JOIN pg_index USING (indexrelid)
            JOIN pg_indexes indexes ON idx_stat.schemaname = indexes.schemaname AND idx_stat.relname = indexes.tablename AND idx_stat.indexrelname = indexes.indexname
        WHERE pg_index.indisunique = false
      ), index_ratios AS (
        SELECT indexes.schemaname,
          indexes.tablename,
          indexes.indexname,
          indexes.idx_scan,
          table_scans.all_scans,
          round(
              CASE
                  WHEN table_scans.all_scans = 0 THEN 0.0
                  ELSE indexes.idx_scan::numeric / table_scans.all_scans::numeric * 100::numeric
              END, 2) AS index_scan_pct,
          table_scans.writes,
          round(
              CASE
                  WHEN table_scans.writes = 0 THEN indexes.idx_scan::numeric
                  ELSE indexes.idx_scan::numeric / table_scans.writes::numeric
              END, 2) AS scans_per_write,
          pg_size_pretty(indexes.index_bytes) AS index_size,
          pg_size_pretty(table_scans.table_size) AS table_size,
          indexes.idx_is_btree,
          indexes.index_bytes
          FROM indexes
            JOIN table_scans USING (relid)
      ), index_groups AS (
        SELECT 'Never Used Indexes'::text AS reason,
          index_ratios.schemaname,
          index_ratios.tablename,
          index_ratios.indexname,
          index_ratios.idx_scan,
          index_ratios.all_scans,
          index_ratios.index_scan_pct,
          index_ratios.writes,
          index_ratios.scans_per_write,
          index_ratios.index_size,
          index_ratios.table_size,
          index_ratios.idx_is_btree,
          index_ratios.index_bytes,
          1 AS grp
          FROM index_ratios
        WHERE index_ratios.idx_scan = 0 AND index_ratios.idx_is_btree
      UNION ALL
        SELECT 'Low Scans, High Writes'::text AS reason,
          index_ratios.schemaname,
          index_ratios.tablename,
          index_ratios.indexname,
          index_ratios.idx_scan,
          index_ratios.all_scans,
          index_ratios.index_scan_pct,
          index_ratios.writes,
          index_ratios.scans_per_write,
          index_ratios.index_size,
          index_ratios.table_size,
          index_ratios.idx_is_btree,
          index_ratios.index_bytes,
          2 AS grp
          FROM index_ratios
        WHERE index_ratios.scans_per_write <= 1::numeric AND index_ratios.index_scan_pct < 10::numeric AND index_ratios.idx_scan > 0 AND index_ratios.writes > 100 AND index_ratios.idx_is_btree
      UNION ALL
        SELECT 'Seldom Used Large Indexes'::text AS reason,
          index_ratios.schemaname,
          index_ratios.tablename,
          index_ratios.indexname,
          index_ratios.idx_scan,
          index_ratios.all_scans,
          index_ratios.index_scan_pct,
          index_ratios.writes,
          index_ratios.scans_per_write,
          index_ratios.index_size,
          index_ratios.table_size,
          index_ratios.idx_is_btree,
          index_ratios.index_bytes,
          3 AS grp
          FROM index_ratios
        WHERE index_ratios.index_scan_pct < 5::numeric AND index_ratios.scans_per_write > 1::numeric AND index_ratios.idx_scan > 0 AND index_ratios.idx_is_btree AND index_ratios.index_bytes > 100000000
      UNION ALL
        SELECT 'High-Write Large Non-Btree'::text AS reason,
          index_ratios.schemaname,
          index_ratios.tablename,
          index_ratios.indexname,
          index_ratios.idx_scan,
          index_ratios.all_scans,
          index_ratios.index_scan_pct,
          index_ratios.writes,
          index_ratios.scans_per_write,
          index_ratios.index_size,
          index_ratios.table_size,
          index_ratios.idx_is_btree,
          index_ratios.index_bytes,
          4 AS grp
          FROM index_ratios,
          all_writes
        WHERE (index_ratios.writes::numeric / (all_writes.total_writes + 1::numeric)) > 0.02 AND NOT index_ratios.idx_is_btree AND index_ratios.index_bytes > 100000000
ORDER BY 14, 13 DESC
      )
SELECT index_groups.reason,
  index_groups.schemaname,
  index_groups.tablename,
  index_groups.indexname,
  index_groups.index_scan_pct,
  index_groups.scans_per_write,
  index_groups.index_size,
  index_groups.table_size
  FROM index_groups;
```

I'm not even sure how this works. However, the view can be queried easily:
```
SELECT * FROM zz_helper_find_unused_indexes;
```

The output will look like this:
```
         reason         | schemaname |    tablename     |                 indexname                    | index_scan_pct | scans_per_write | index_size | table_size
------------------------+------------+------------------+----------------------------------------------+----------------+-----------------+------------+------------
 Never Used Indexes     | public     | special_requests | index_special_requests_on_special_request_id |           0.00 |            0.00 | 16 kB      | 8192 bytes
 Low Scans, High Writes | public     | facilities       | index_facilities_on_facility_id              |           6.54 |            0.18 | 56 kB      | 104 kB
(2 rows)
```
