--- 
status: new
--- 

Adding a `postprocess` block affords site-level postprocessing, such as search indexing, uploading assets to a CDN, and more.

The `postprocess` block should have access to files which have been remove (such as would be deleted by `prune`). It should also have knowledge of which items or reps have been recompiled, which were identical, and which were skipped because none of their dependencies changed.
