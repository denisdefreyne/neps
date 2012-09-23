--- 
Status: accepted
--- 

On a large site, the output from `nanoc compile` is very noisy, and hard to grok at a glance.

## Brainstorming

Some ideas:

* Outputting a summary line at the end of compiling: `3 created, 52 updated, 15 identical, 5000 skipped`.
* Making the default output less verbose. A good model for this could be unit test style: `..UCC...UI...`.
* At the end of each row, outputting a `15/1500 (1%)` summary.
* Adding multiple levels of verbosity; possibly `--verbose` doing the line-by-line as it compiles version that `nanoc compile` currently uses and add a `--debug` for even more verbose debugging info.
* <del>Having at least a flag in config to have `nanoc compile` log :identical with level :low instead of :high, as with sites with a lot of files, there usually is no point in seeing what hasn't changed.</del> (implemented by ddfreyne in https://github.com/ddfreyne/nanoc/commit/38420793583f242a5242661501852062fe2990f0 (branch release-3.4.x))
* When a page takes 10s or longer to compile, the alignment of the output is incorrect. Should nanoc cater for this? 10s or more is a very long time to compile a page, so perhaps the current situation should be left as-is.

## Levels of verbosity

I propose three levels:

**Quiet**: no output at al.

**Default**:

    % nanoc
    Loading site data...
    ....U.....UUUU..C....DDDDDDDSSSSSSSSSSSSSSSSSSSSSS
    Site compiled in 12.34s (1 created, 5 updated)
    % 

The characters mean the following:

* `.` = identical
* `U` = updated
* `C` = created
* `S` = skipped
* `D` = deleted (using the auto-pruner)

**Verbose** - One line per processed item representation.

Some questions:

* What do to with the statistics? Should they be present in the verbose level?

## Points of action

1. Do not include compiled items that have identical output (“identical” lines in the `nanoc compile` output). Including them serves no purpose. (**implemented**)

2. Continually keep the user informed about the compilation progress. This should happen in two ways:

    1. Give feedback when filters take longer than a certain amount to finish. This is already implemented, but has threading issues, which occasionally leads to ugly output.
    2. Give feedback when an item is compiled, even when it is “identical”. This should happen by adding a status line that shows the current progress, both in absolute numbers (X out of Y items compiled) as well as relative (Z% done).
