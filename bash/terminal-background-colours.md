# Terminal Background Colours

I've used the snippets below as a convenient way to know which system I'm logged into on a given shell.

## Set background

Add this to `.bashrc` on the remote host to set the background colour:

```bash
case $- in *i*) ;; *) return ;; esac
printf '\033]11;#1a001a\a'
trap "printf '\033]111\007'" EXIT
```

The hex code in this case is `#1a001a`. Replace that with whatever you like. The `trap` attempts to reset the background colour when an error causes the session to fail.

## Unset background

It's also a good idea to add this to your local profile:

```bash
# Always unset background when SSH exits
ssh() {
  command ssh "$@"
  rc=$?
  # runs after ssh exits (success or failure)
  printf '\033]111\007'
  return $rc
}
```

This will ensure the background colour is reset when SSH dies.
