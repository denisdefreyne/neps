--- 
status: new
--- 

The current `watch` command only looks for files in `content/`, `lib/`, `layout/`, etc. If you are using other data sources that get their data elsewhere, the `watch` command won’t notice changes to data in these other locations.

The data sources should each provide pluggable watchers to avoid this.
