# Bump a submodule without checking it out

Sometimes it can be useful to bump a Git submodule without checking it out.

We assume that you know SHA1 hash that you want to update the submodule to. Call this `<sha1>`.

Next, you'll need to find the mode for the submodule:

    git ls-files --stage <submodule_path>

For example:

    #  git ls-files --stage receiver
    160000 daa23cce5526aff20c097ce2926314451bc8cac3 0	receiver

In this case, the mode is `160000`.

Now you can put all three together to update the submodule:

    git update-index --add --cacheinfo  <mode>,<sha1>,<submodule_path>
