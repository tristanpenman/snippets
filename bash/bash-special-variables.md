# Bash Special Variables

A quick refresher on special variables that can be used in Bash scripts:

* `$1`, `$2`, `$3` for positional parameters
    * their corresponding array representation, count and IFS expansion are `$@`, `$#`, and `$*` respectively
* `$-` is the current options set for the shell
* `$$` is the PID of the current shell (not subshell)
* `$_` most recent parameter (or the abs path of the command to start the current shell immediately after startup)
* `$IFS` is the (input) field separator
* `$?` is the most recent foreground pipeline exit status
* `$!` is the PID of the most recent background command
* `$0` name of the shell or shell script

Gentoo bash reference:
http://devmanual.gentoo.org/tools-reference/bash/index.html
