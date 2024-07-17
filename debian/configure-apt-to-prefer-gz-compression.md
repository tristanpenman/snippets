# Configure apt to prefer gz compression

On newer versions of Ubuntu (and presumably other Debian derivatives), using `apt` to fetch updated packages will often produce warnings about missing package index files. This happens because `apt` defaults to preferring compression types other than `.gz`. This is often the case when fetching from third-party apt repos that may only provide `Packages.gz` files.

This is very easy to change. First, edit (or add) the following file:
```
sudo vim /etc/apt/apt.conf.d/99compress-gzip
```

Add the following line:
```
Acquire::CompressionTypes::Order::"gz";
```

The added line simply instructs `apt` to prefer downloading `.gz` compressed packages over other compression types.

Save the file, and re-run `sudo apt update`.
