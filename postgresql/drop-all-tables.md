# Drop all tables

These instructions were found [here](https://stackoverflow.com/questions/3327312/how-can-i-drop-all-the-tables-in-a-postgresql-database).

If all of your tables are in a single schema, this approach could work:

    DROP SCHEMA public CASCADE;
    CREATE SCHEMA public;

This assumes the schema name is `public`. Use an alternative schema name if required.

If you are using PostgreSQL 9.3 or later (almost certainly the case), you may also need to restore the default grants:

    GRANT ALL ON SCHEMA public TO postgres;
    GRANT ALL ON SCHEMA public TO public;
