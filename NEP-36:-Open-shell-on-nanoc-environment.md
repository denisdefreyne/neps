It would be useful to be able to open an interactive Ruby shell with the site’s nanoc environment preloaded. This means that:

**Definitely:**

1. `@items`, `@layouts`, … are filled in

**Maybe:**

2. Allow executing filters in an easy way (e.g. `i = @items.find { |i| i.identifier == '/abc/' } ; filter i, :erb`}