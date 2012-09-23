--- 
Status: released
--- 

I’m not entirely happy with the overall architecture of nanoc. Class responsibilities are not clearly separated, and there is a lack of distinction between classes that represent source data (items, configuration, layouts, snippets) and classes that are used for compiling the site.

An example of this is the way that item representation outdatedness is checked. Item representations need to access the site object to determine whether the configuration, rules and code snippets are outdated. The method chain itself is already a good indication that something is wrong here: `@item.site.code_snippets.any? { |cs| cs.outdated? `}.

The site should be only used to store raw data and query it. It should not need to know anything about outdatedness or compilation. A site should simply be a fairly stupid data store. It should not have a reference to the compiler. It should not have a preprocessor, checksums, a DSL, and it should not be responsible for building the item representations either.

The compiler should be responsible for compiling the site, but also for determining outdatedness (perhaps delegated to a separate object), caching already compiled content, tracking dependencies, loading the rules, creating the DSL, building the item representations, etc. In other words, everything that is related to compiling the site.

There should be a cleaner distinction between three kinds of data:

1. Original source data (items, layouts, configuration, code snippets)
2. Compilation-related data (compiled item representations, rules)
3. Infrastructural data (checksums, modification times, cached content/attributes, …)

This implies the following architectural changes:

1. **Checksum store**: There should be a class (`ChecksumStore`) that keeps track of old and new checksums of items, layouts, etc. **Done**

2. **Checksum-based outdatedness checker**: There should be a class (`OutdatednessChecker`) that returns `true` or `false` given an item, indicating whether this item needs to be rebuilt (does not track dependencies). Items and item representation should not know about their outdatedness. **Done**

3. **Dependency-based outdatedness checker**: There should be a class (`DependencyTracker`) that returns `true` or `false` given an item, indicating whether this item is outdated due to dependencies or not (not necessary for already outdated items). This class depends on the `OutdatednessChecker` class. **Done**

4. **Compiled content cache**: There should be a class (`CompiledContentCache`) that returns the compiled content for a given item, if this item is not outdated and not outdated because of dependencies either. **Done**

Some points worth noting:

* Without the infrastructural functionality and data that speed up the compilation process, the site should still compile just fine, but will recompile everything every time. **Canceled:** This kind of modularity would not be very useful.

* `Nanoc3::Item` should no longer hold a reference to the old and new checksums; the checksums should be stored elsewhere, i.e. in a `Nanoc3::ChecksumStore`. Whether an item is outdated due to dependencies should also not be stored in the item itself. **Done**

* `Nanoc3::Item` should, for backwards compatibility, still reference the item representations. Item representations are not part of the source data, but there is no better place than to leave them where they are. Functions such as `#rep_named`, `#path` and `#compiled_content` should remain, too. **Done**

* `Nanoc3::Site` should know nothing about site compilation. It should not reference checksums, and it should not even hold a reference to the compiler (although the `#compiler` method, and even a new `#compile` method, may be there for convenience). The site should also not be responsible for generating the item representations, loading the rules or creating the DSL.

* `Nanoc3::Item` should probably not know about the site. **Done**

* `Nanoc3::ItemRep` should not know how to gather all assigns (site, config, item, item rep, items, layouts). **Done**

* `Nanoc3::ItemRep` should not know how to generate diffs. Perhaps a Differ class (with different implementations for *nix and Windows) would be useful here. **Done**

The `lib/nanoc3/base` directory has gained three additional subdirectories named `source_data`, `result_data` and `compilation`. The directory structure of these three directories looks like this:

	.
	|-- compilation
	|   |-- checksum_store.rb
	|   |-- checksummer.rb
	|   |-- compiled_content_cache.rb
	|   |-- compiler.rb
	|   |-- compiler_dsl.rb
	|   |-- dependency_tracker.rb
	|   |-- filter.rb
	|   |-- outdatedness_checker.rb
	|   |-- outdatedness_reasons.rb
	|   |-- rule.rb
	|   `-- rule_context.rb
	|-- result_data
	|   `-- item_rep.rb
	`-- source_data
	    |-- code_snippet.rb
	    |-- data_source.rb
	    |-- item.rb
	    |-- layout.rb
	    `-- site.rb
