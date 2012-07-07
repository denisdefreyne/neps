**Status: Implemented.**

The compiler currently uses a queue that contains all uncompiled items. When an item cannot be compiled due to an unmet dependency, items get pushed to the end of the queue to ensure that they will only be recompiled after all other pages are compiled.

This strategy works, but can be inefficient in some cases. For example, if item A depends on item B, which depends on item C, and when these items are added to the queue in the given order, the first pass will cause the compilation of items A and B to fail while compilation of item C works; the second pass will then again fail to compile A because of exactly the same reason, but compilation of item B will work; only in the final pass will all items be finally compiled.

A better solution would be to build a content dependency graph at compile time (not to be confused with the graph maintained by the dependency tracker). An edge from item A to item B indicates that item B cannot be compiled until item A is compiled. Such an edge is added when an item fails to be compiled due to an unmet dependency. The roots of this graph will then be items that have no known dependencies.
