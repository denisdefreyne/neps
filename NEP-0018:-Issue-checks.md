--- 
status: implemented
--- 

It should be possible to define issue checks (comparable to unit tests) to make sure that a site is okay in order to be deployed. This will be achieved in two ways. First, there will be a `nanoc check` command that executes checks. Secondly, the `nanoc deploy` command will be enhanced so that it optionally performs checks before deploying the site.

Possibly useful checks
----------------------

* `html` - valid HTML
* `css` - valid CSS
* `js` - valid JavaScript (probably just try to parse it, error if it fails)
* `internal_links` - valid internal links
* `external_links` - valid external links
* `encoding` - valid encodings (parse output as UTF-8, or the encoding specified in the `<meta charset>`)
* `stray` - no stray files in `output/` (all output files must have a matching item in `content/`)

Other checks that should be possible:

* ensure that all JavaScript, CSS and HTML files are minified (not sure how to check that though)
* ensure that versioned items have their version number updated (again, not sure how to check this)

The `Checks` file
-----------------

The `Checks` file will have two goals. It will firstly allow you to define what checks to use for deployment, and it will secondly allow you to define custom quick checks. Here is an example:

```ruby
# Define a quick check
check :atom_feeds_not_empty => "Make sure that all atom feeds have at least one article" do
  feed = @items.find { |i| i.identifier == '/blog/feed/' }
  # HTML parsing simplified for demonstration purposes
  dom = html_parse(feed)
  assert !dom.xpath('//article').empty?
end

# Define which checks should be run before deploying
deploy_check :stray
deploy_check :internal_links
deploy_check :atom_feeds_not_empty
# Alternative way:
#   deploy_checks :stray, :internal_links, :atom_feed_not_empty
```

Usage
-----

* `nanoc check html`: Performs the given check (html)
* `nanoc check html css javascript`: Performs the given checks (html, css and javascript)
* `nanoc check --all`: Performs all defined checks
* `nanoc check --list`: Gives a list of all checks
* `nanoc check --deploy`: Performs all checks marked for deployment

Subject to check
----------------

* compiled files in output/
* ... what else?

Progress reporting
------------------

For long-running checks, it would be useful to get an overview of how many subjects have been processed and how many are still remaining. Perhaps even an ETA (although that is likely unnecessary).

Example
-------

```
% nanoc check --deploy
Running check: html... ok
Running check: css... ok
Running check: javascript... ok
Running check: internal_links... error
  /blah.html:
    [ ERROR ] links to non-existant file /foo.html
    [ ERROR ] links to non-existant file /bar.html
  /meh.html:
    [ ERROR ] links to non-existant file /whatever.html
Running check: external_links... warning
  /blah.html:
    [WARNING] link to http://other.example.com/ times out
Running check: encodings... ok
Check completed with 4 issues found.
%
```

Elsewhere
---------

Neat way of specifying checks (in JS): https://github.com/fat/haunt/blob/master/examples/basic.js
