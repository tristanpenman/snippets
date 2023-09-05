# Dump a Database from RDS

## Problem

You have a PostgreSQL database in RDS, and you would like to work with the data locally (e.g. to experiment with query optimisations).

## Solution

1. Change your database RDS instance security group to allow your machine to access it.
    * Add your public IP address to the security group, to allow access to the RDS instance.

2. Make a copy of the database using `pg_dump`:
    * `pg_dump -h <public-dns> -U <username> -f <dump.sql> <db-name>`
    * you will be asked for the database password
    * a dump file `<dump.sql>` will be created

3. Restore that dump file to your local database:
    * You might need to drop the database and create it first
    * `psql -U <postgresql username> -d <database name> -f <dump file that you want to restore>`

4. Alternatively, use `pg_restore`:
    * `pg_restore -h <host> -U <username> -c -d <db-name>  <dump.sql>`

## Reference

These notes are based on [this GitHub Gist](https://gist.github.com/syafiqfaiz/5273cd41df6f08fdedeb96e12af70e3b).
