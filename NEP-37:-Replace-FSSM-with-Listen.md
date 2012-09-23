--- 
status: implemented
is_quick_win: true
--- 

It seems that the [fssm](http://rubygems.org/gems/fssm) gem is deprecated and is replaced by the [listen](http://rubygems.org/gems/listen) gem ([source](https://github.com/nex3/sass/issues/351)). nanoc currently still uses `fssm` for the watcher, and it would be a good idea to move to `listen`.
