# Partition a table by month

This is useful in a scenario where you'd like to partition data over some regular time period (e.g. months, weeks, days) and you don't want to repeatedly define new partitions.

Instead, you can partition based on a computed property of an attribute (e.g. extract month from a timestamp):

    CREATE TABLE events (
        id integer NOT NULL,
        data jsonb DEFAULT '{}'::jsonb NOT NULL,
        created_at timestamp without time zone)
    PARTITION BY HASH (EXTRACT(MONTH FROM created_at));

Now you can quickly create a partition for each month:

    DO $$
    DECLARE
        i INT;
    BEGIN
        FOR i IN 1..12 LOOP
            EXECUTE format(
                'CREATE TABLE events_%s PARTITION OF events FOR VALUES WITH (MODULUS 12, REMAINDER %s)',
                i, i-1
            );
        END LOOP;
    END $$;
