nanoc needs a fast way of finding items by identifier.

**Current situation:**

Items are retrieved using a construct such as `@items.find { |i| i.identifier == '/foo/' }`. This is long and cumbersome, so it occasionally is poured into a helper, which reduces the construct to `item_named('/foo/')`.

**Situation to be:**

nanoc should have a built-in way to find items by identifier. Perhaps the `@items` array should allow fetching items by identifier, like `@items['/foo/']`.