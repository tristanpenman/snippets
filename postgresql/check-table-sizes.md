# Check table sizes

This is actually very easy to do:

```
SELECT
  table_name,
  pg_total_relation_size(quote_ident(table_name)) AS size
FROM
  information_schema.tables
WHERE
  table_schema = 'public'
ORDER BY size DESC;
```

Example output:

```
          table_name          |  size
------------------------------+---------
 vehicles                     | 3661824
 bookings                     | 2236416
 customers                    | 1638400
 confirmation_letters         | 1097728
 transport_details            |  974848
 photos                       |  368640
```

## Prettify

Making the size human readable is just a minor modification:

```
SELECT
  table_name,
  pg_size_pretty(pg_total_relation_size(quote_ident(table_name))),
  pg_total_relation_size(quote_ident(table_name)) AS size
FROM
  information_schema.tables
WHERE
  table_schema = 'public'
ORDER BY size DESC;
```

Note that this is sorted by the original size column, since the prettified size won't sort correctly:

```
          table_name          | pg_size_pretty |  size
------------------------------+----------------+---------
 vehicles                     | 3576 kB        | 3661824
 bookings                     | 2184 kB        | 2236416
 customers                    | 1600 kB        | 1638400
 confirmation_letters         | 1072 kB        | 1097728
 transport_details            | 952 kB         |  974848
 photos                       | 360 kB         |  368640
```

## Indexes

We can go a step further and find the space taken by indexes.

For example, to find the space taken by indexes on table called `events`:

```
SELECT
  i.relname AS index_name,
  pg_size_pretty(pg_total_relation_size(i.oid)) AS index_size
FROM
  pg_class t
JOIN
  pg_index idx ON t.oid = idx.indrelid
JOIN
  pg_class i ON i.oid = idx.indexrelid
WHERE
  t.relname = 'events';
```

```
                      index_name                       | index_size
-------------------------------------------------------+------------
 events_pkey                                           | 6 GB
 index_events_on_created_at                            | 3 GB
 index_events_on_name                                  | 8 GB
(3 rows)
```