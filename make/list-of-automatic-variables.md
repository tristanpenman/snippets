# List of automatic variables

This is a summary of the automatic variables that I find useful when writing Makefiles:

`$@`

The file name of the target of the rule. If the target is an archive member, then `$@` is the name of the archive file. In a pattern rule that has multiple targets (see Introduction to Pattern Rules), `$@` is the name of whichever target caused the rule`s recipe to be run.

`$%`

The target member name, when the target is an archive member. See Archives. For example, if the target is foo.a(bar.o) then `$%` is bar.o and `$@` is foo.a. `$%` is empty when the target is not an archive member.

`$<`

The name of the first prerequisite. If the target got its recipe from an implicit rule, this will be the first prerequisite added by the implicit rule.

`$?`

The names of all the prerequisites that are newer than the target, with spaces between them. If the target does not exist, all prerequisites will be included. For prerequisites which are archive members, only the named member is used (see Archives).

`$^`

The names of all the prerequisites, with spaces between them. For prerequisites which are archive members, only the named member is used (see Archives). A target has only one prerequisite on each other file it depends on, no matter how many times each file is listed as a prerequisite. So if you list a prerequisite more than once for a target, the value of `$^` contains just one copy of the name. This list does not contain any of the order-only prerequisites; for those see the `$|` variable, below.

`$+`

This is like `$^`, but prerequisites listed more than once are duplicated in the order they were listed in the makefile. This is primarily useful for use in linking commands where it is meaningful to repeat library file names in a particular order.

## Reference

Full documentation can be found here:

 * https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html