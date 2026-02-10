# Insert a new root commit

Sometimes you want to inject a new (empty) root commit into an existing Git repo. A common example is when you're preparing a new repo to be pushed to a remote for the first time.

Git has a well-known, or at least sort-of-well-known, empty tree whose SHA1 is:

```
4b825dc642cb6eb9a060e54bf8d69288fbee4904
```

We can inject that into a repo like so:

```
git rebase --root --onto $(git commit-tree -m 'Initial commit (empty)' 4b825dc642cb6eb9a060e54bf8d69288fbee4904)
```

## Edit a commit date

Once we've done that, we may want to tweak the dates a little bit, to accurately reflect the beginning of the project:

```
git commit --amend --date="Fri Aug 1 19:07:18 2025 +1000" --no-edit
git rebase --continue
```

## Ensure that commit and author dates are consistent

Finally, if you want to ensure that commit and author dates are consistent, then you can do this while performing any other final adjustments:

```
git rebase --committer-date-is-author-date -i --root
```
