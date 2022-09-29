# Fetch many repos at once

I often want to run `git fetch` on the many repos that I have cloned onto my dev machine. I usually do this using `git fetch --all --prune` so that old remote branches are cleaned up.

This is a little script that I wrote for doing this:

```bash [git-fetch-all.sh]
#!/bin/bash

dir="$(pwd)"
cleanup() {
  cd $dir
  echo Bye!
  exit 1
}

trap "cleanup" INT HUP TERM

find . -name .git -type d -prune | while read d; do
  cd `dirname $d`
  echo `pwd`
  git fetch --all --prune || true
  cd $dir
done

cleanup
```

I run this from the directory where all my Git repos live:

```bash
$ cd ~/Workspace
$ git-fetch-all.sh
```

Note that this will ignore errors when running `git fetch`.
