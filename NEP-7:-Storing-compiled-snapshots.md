**Status: Implemented.**

It is currently not possible to write items at different stages of their compilation process. It may be useful to allow storing snapshots as well, so that different versions of an item rep can be saved. The DSL for this could look like this:

	compile "/stylesheet/" do
	  filter :sass
	  filter :tidy_css # not a real filter, but you get the idea
	  snapshot :debuggable
	  filter :rainpress
	end

	route "/stylesheet/", :snapshot => :debuggable
	  "/stylesheet-verbose.css"
	end

	route "/stylesheet/"
	  "/stylesheet.css"
	end