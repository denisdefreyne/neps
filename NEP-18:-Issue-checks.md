A `nanoc check` command could be used to performs issue checks on your site. An issue check is comparable to a unit test for your site.

Checks to include
-----------------

* `html` - valid HTML
* `css` - valid CSS
* `js` - valid JavaScript
  * probably just try to parse it, error if it fails
* `ilinks` - valid internal links
* `elinks` - valid external links
* `enc` - valid encodings
  * parse output as UTF-8, or the encoding specified in the `<meta charset>`
* `stray` - no stray files in `output/`
  * all output files must have a matching item in `content/`

Other checks that should be possible:

* ensure that all JavaScript, CSS and HTML files are minified (not sure how to check that though)
* ensure that versioned items have their version number updated (again, not sure how to check this)

Quick checks
------------

It should be possible to define ad-hoc checks in a “checks” file of sort. It would look like this:

```ruby
check "feed should not be empty" do
  feed = @items.find { |i| i.identifier == '/blog/feed/' }
  # HTML parsing simplified for demonstration purposes
  dom = html_parse(feed)
  assert !dom.xpath('//article').empty?
end
```

Usage
-----

* `nanoc check`: Performs all checks on the site
* `nanoc check html`: Performs the given check (html)
* `nanoc check html css javascript`: Performs the given checks (html, css and javascript)

Subject to check
----------------

* compiled files in output/
* ... what else?

Result statuses
---------------

* **OK** - the check was performed and the result is certainly okay
* **Error** - the check was performed and the result is certainly not okay
* **Warning** - the check was performed but the result is inconclusive
  * e.g. redirect limit exceeded
  * e.g. timeout
* **Skipped** - the check could not be performed
  * e.g. links checker does not know how to follow certain URLs, such as `mailto:` or `irc://`

Progress reporting
------------------

For long-running checks, it would be useful to get an overview of how many subjects have been processed and how many are still remaining. Perhaps even an ETA (although that is likely unnecessary).

Pre-deployment checking
-----------------------

`nanoc deploy` could run the checkers before deploying. If any errors or warnings occur, deployment would be cancelled, and can only be forced using `--force`.

Whether checking is required or not, would depend on the target: one could always deploy to staging, but not to production.

Example
-------

```
% nanoc check
Running check: html... ok
Running check: css... ok
Running check: javascript... ok
Running check: links_internal... error
  /blah.html:
    [ ERROR ] links to non-existant file /foo.html
    [ ERROR ] links to non-existant file /bar.html
  /meh.html:
    [ ERROR ] links to non-existant file /whatever.html
Running check: links_external... warning
  /blah.html:
    [WARNING] link to http://other.example.com/ times out
Running check: encodings... ok
Check completed with 4 issues found.
%
```