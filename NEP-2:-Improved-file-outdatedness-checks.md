Status: implemented

Currently (in nanoc 3.1), nanoc uses the file modification time (mtime) to check whethe
an item’s source content was updated since it was last compiled. This simple solution certainly is not the best, because it will cause pages to be needlessly recompiled if the source file was touched but not changed.

There needs to be a better way to find out whether a file was changed. This improved method should certainly avoid false negatives (file was changed but nanoc doesn’t realise) because false negatives may cause parts of a compiled site to become outdated. False positives (file was not changed but nanoc thinks it has) are acceptable, but should be avoided due to the performance hit it causes.

A better way of finding out whether a source file has changed is to calculate the hash of the source file and compare it with a hash (of the same file) that was calculated and stored during the latest site compilation. MD5 and SHA1 can be used for this, but they may be slow; perhaps CRC could be useful here as well.
