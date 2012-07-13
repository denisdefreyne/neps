On a large site, the output from `nanoc compile` is very noisy, and hard to grok at a glance. A few things would make this better:

* Outputting a summary line at the end of compiling: `3 created, 52 updated, 15 identical, 5000 skipped`.
* Making the default output less verbose. A good model for this could be unit test style: `..UCC...UI...`.
* At the end of each row, outputting a `15/1500 (1%)` summary.
* Adding multiple levels of verbosity; possibly `--verbose` doing the line-by-line as it compiles version that `nanoc compile` currently uses and add a `--debug` for even more verbose debugging info.
* Having at least a flag in config to have `nanoc compile` log :identical with level :low instead of :high, as with sites with a lot of files, there usually is no point in seeing what hasn't changed.