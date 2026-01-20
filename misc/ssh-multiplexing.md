# SSH Multiplexing

Here's a handy SSH config snippet for when you regularly open multiple SSH sessions to the same host and want them to share the same underlying TCP connection and port forwards:

```
Host myhost
  HostName myhost.example.com
  User you
  ControlMaster auto
  ControlPath ~/.ssh/cm-%r@%h:%p
  ControlPersist 10m
  LocalForward 5432 127.0.0.1:5432
  ```

## Use Case

This is a great way to keep a stable tunnel open for a database connection (e.g. Postgres on 5432) or other "only reachable from inside" services, while avoiding the "Address already in use” error that arises from trying to recreate the same `LocalForward` in multiple SSH sessions.

It is also a nice optimisation when using multiple interactive shells, `scp` transfers, and other ad-hoc commands, as it avoids paying the connection authentication cost each time.

## How it works

`ControlMaster` turns the first SSH connection you open into a master session that listens on a local UNIX socket (located at `ControlPath`). Subsequent `ssh` invocations that match the same host/user/port discover the UNIX socket and attach as
lightweight "multiplexed" channels over the existing connection, rather than creating a new one.

`ControlPersist` keeps the master alive for a while after the last client disconnects, so your forwards can keep running (and reconnects are fast) until the persist timeout expires or you explicitly close the master.
