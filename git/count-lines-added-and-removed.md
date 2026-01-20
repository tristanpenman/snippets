# Count lines added and removed

What do you do if you need to gather some stats on the number of lines added/removed on a Git repo, between two dates? Thankfully `git` has built-in support for this:

```bash
git log --since=2025-01-01 --until=2026-01-01 --pretty=tformat: --numstat
```

The output is format as three columns (lines added, lines removed, and filename):

```
1       1       cpp/marian_rknn.cc
2       0       docker-compose.yml
12      7       cpp/marian_rknn.cc
11      4       cpp/marian_rknn.cc
4       6       cpp/rknn_utils.cc
75      0       cpp/type_half.h
8       8       cpp/marian_rknn.cc
7       3       cpp/rknn_utils.cc
1       1       cpp/rknn_utils.h
2       2       cpp/marian_rknn.cc
19      15      cpp/rknn_utils.cc
2       3       cpp/rknn_utils.h
0       137     cpp/rknn_utils.cc
0       3       cpp/rknn_utils.h
1       1       Marian-ONNX-Converter
10      4       README.md
17      13      cpp/marian_rknn.cc
6       3       cpp/marian_rknn.h
```

## Counting

This can be counted using `awk`.

To show aggregate lines added:

```bash
git log --since=2025-01-01 --until=2026-01-01 --pretty=tformat: --numstat \
  | awk { added += $1 } END { print added }'
```

Or to include both added and removed line counts:

```bash
git log --since=2026-01-09 --until=2026-01-10 --pretty=tformat: --numstat \
  | awk '{ added += $1; removed += $2 } END { print "added: " added " removed: " removed }'
```
