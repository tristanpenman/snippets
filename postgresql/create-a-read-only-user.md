# Create a read only user

Based on the instructions found here:
https://stackoverflow.com/questions/760210/how-do-you-create-a-read-only-user-in-postgresql

## Steps

Create a new role:
```
CREATE ROLE ro_user WITH LOGIN PASSWORD 'REDACTED';
```

Allow connection using new role:
```
GRANT CONNECT ON DATABASE vivi_db_production TO ro_user;
```

Allow read only access to schema:
```
GRANT USAGE ON SCHEMA public TO ro_user;
```

Allow read only access to all tables:
```
GRANT SELECT ON ALL TABLES IN SCHEMA public TO ro_user;
```

Apply by default to new tables:
```
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO ro_user;
```
