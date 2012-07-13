On a large site, the output from `nanoc compile` is very noisy, and hard to grok at a glance.

## Brainstorming

Some ideas:

* Outputting a summary line at the end of compiling: `3 created, 52 updated, 15 identical, 5000 skipped`.
* Making the default output less verbose. A good model for this could be unit test style: `..UCC...UI...`.
* At the end of each row, outputting a `15/1500 (1%)` summary.
* Adding multiple levels of verbosity; possibly `--verbose` doing the line-by-line as it compiles version that `nanoc compile` currently uses and add a `--debug` for even more verbose debugging info.
* <del>Having at least a flag in config to have `nanoc compile` log :identical with level :low instead of :high, as with sites with a lot of files, there usually is no point in seeing what hasn't changed.</del> (implemented by ddfreyne in https://github.com/ddfreyne/nanoc/commit/38420793583f242a5242661501852062fe2990f0 (branch release-3.4.x))

## Points of action

1. Do not include compiled items that have identical output (“identical” lines in the `nanoc compile` output). Including them serves no purpose. (**implemented**)

2. Continually keep the user informed about the compilation progress. This should happen in two ways:

    1. Give feedback when filters take longer than a certain amount to finish. This is already implemented, but has threading issues, which occasionally leads to ugly output.
    2. Give feedback when an item is compiled, even when it is “identical”. This should happen by adding a status line that shows the current progress, both in absolute numbers (X out of Y items compiled) as well as relative (Z% done).