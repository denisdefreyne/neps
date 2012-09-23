--- 
status: released
--- 

An “explain” command that explains what items exist, how they are filtered, laid out and where they are written to would be useful to debug problems. Sample output (from @justinclift):

```
$ nanoc explain
Item /foo/ (from content/foo.html):
  Representation default:
    1. filter :erb
    2. filter :colorize_syntax, arguments { :moo => 123 }
    3. layout /default/ (using filter :haml)
    4. filter :rubypants
    5. write to output/foo/index.html
Item /bar/ (from content/bar.html):
  Representation default:
    1. filter :erb
    2. filter :colorize_syntax, arguments { :moo => 123 }
    3. layout /default/ (using filter :haml)
    4. filter :rubypants
    5. write to output/bar/index.html
Item /baz/ (from content/baz.html):
  Representation default:
    1. filter :erb
    2. filter :colorize_syntax, arguments { :moo => 123 }
    3. layout /default/ (using filter :haml)
    4. filter :rubypants
    5. write to output/baz/index.html

$ nanoc explain foo.html
Item /foo/ (from content/foo.html):
  Representation default:
    1. filter :erb
    2. filter :colorize_syntax, arguments { :moo => 123 }
    3. layout /default/ (using filter :haml)
    4. filter :rubypants
    5. write to output/foo/index.html
```

The explain command could take zero arguments (to explain everything) or one (to explain a single item). This single argument could be a file relative to `content/`, relative to the current directory, or an identifier. So, the following should all work:

```
$ nanoc explain
$ nanoc explain content/foo.html
$ nanoc explain foo.html
$ nanoc explain output/foo/index.html
$ nanoc explain /foo/
```

~ @ddfreyne, @justinclift
