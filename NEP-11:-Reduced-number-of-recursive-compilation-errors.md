--- 
status: released
--- 

At this point, nanoc throws recursive compilation errors even in cases where it is not strictly necessary. Requesting the #compiled_content of the item that is currently being processed is impossible even though there are valid cases where this would be possible and useful.

The reason why nanoc does not allow this in 3.2 and before is because there is no trivial way to find out whether an item rep should be compiled before its content is requested. The easy way out was (is) to require compilation of the item whose content is requested.

One solution would be to not require compilation when the compiled content at the requested snapshot already exists. This is not optimal, though, because it will lead to recursive compilation errors when the compiled content at the given snapshot does not exist because it is never created.

This problem can be solved using the new Rules analysis phase that was introduced in nanoc 3.2. This phase could be extended to remember all snapshot names for all item reps, so that if the compiled content at a given snapshot is requested, it is known whether that snapshot will exist or not. If during actual compilation (as opposed to rule analysis) a non-existant snapshot is requested, an appropriate new error could be thrown.

One problem remains to be tackled: the problem of moving snapshots. This concerns both the implicit :pre, :post and :last snapshots as well as explicit snapshots which are moving due to multiple calls to #snapshot with the same name for the same item rep. The latter should simply be prohibited: a second call to #snapshot with the same name for the same item rep should raise an error. The implicit moving snapshots will continue to behave the way they behave now, but it will not be possible to request the compiled content at an implicit moving snapshot of the item that is currently being processed.
