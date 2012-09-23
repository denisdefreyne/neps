--- 
Status: accepted
--- 

Tracking more specific dependencies (e.g. between an item and specific attributes of another item) would improve performance. The following dependencies need to be tracked:

* item depends on item content
* item depends on item attribute
* item depends on layout content
* item depends on layout attribute
* item depends on site configuration

Attribute dependencies should be stored at a fairly fine-grained level. For example, if an item accesses the `item[:foo][:bar][:baz]` attribute, then changes to the attribute that do not affect `item[:foo][:bar][:baz]` should not cause the item to be considered outdated. The same applies to the site configuration, which is a hash, just like attributes.

Let’s consider the following hash in the next few examples:

	#!ruby
	hash = {
	  :foo => {
	    :xyz => 1,
	    :bar => [
	      'hooray',
	      'whee',
	      'yeehaw'
	    ]
	  }
	}

For fine-grained attribute dependency tracking to work, “access paths” for the attributes hash should be stored. Such an access path is a list of keys/indexes that were used to access certain values in this hash/array. For example, accessing `hash[:foo][:bar][0]` would result in the access path [ :foo, :bar, 0]. Such access paths can only be created if the hash/array can keep track of which values were requested; a subclass of `Hash`/`Array` or a class that proxies to `Hash`/`Array` will therefore be necessary.

The raw, uncompiled attributes for each item will need to be stored for this to work. Per-attribute checksums are possible, but may be a waste of space.
