While the standard Ruby implementation (MRI) does not provide true parallelism, there are a few Ruby implementations that can make use of multiple threads. This includes JRuby and soon Rubinius, and hopefully more later on.

nanoc should take advantage of multiple cores and/or multiple CPUs, since this would provide a significant speedup when compiling sites. Compilation of a nanoc site is almost “embarrassingly parallel”: there are dependencies between items, but nanoc can figure out these dependencies at run time (and should eventually be able to make a very good up-front estimate of the dependency graph).

nanoc’s current architecture is not quite ready for parallelization. There are several disguised global variables which need to be removed (perhaps replaced by thread-local storage). For example, there exists a single compilation stack which is used for determining dependencies.

All in all, parallelizing nanoc is a low-priority goal because of the state of parallelism in the current major Ruby implementations.