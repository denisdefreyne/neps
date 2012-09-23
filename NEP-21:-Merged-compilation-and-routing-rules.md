--- 
Status: accepted
--- 

+1.

I've only just started using nanoc, but a change like this seems much more
intuitive to me: Almost all of my `compile` and `route` rules are paired,
and this change would unify them. Similarly, it would remove an initial
cognitive hurdle in understanding whether or not paired rules interact.

For instance, I wanted to route files from `/content/scss/foo.scss` to
`/output/css/foo.css`, but also pass them through the `sass` filter.  I
wrote the routing rule first, but then paused writing the compilation
rule: Would it be matching against an identifier generated from the source
path of `/content/scss/foo.scss`, or would it be matching against the
newly routed destination of `/output/css/foo.css`? The correct answer
wasn't readily obvious to me at that point, but it would be under this
proposal.

Lastly, another sample of rules that this change would simplify:

```ruby
# Pass through other static resources unchanged
compile '/static/*/' do
end

route '/static/*/' do
  item.identifier.chop + '.' + item[:extension]
end
```

Would be reduced to:

```ruby
# Pass through other static resources unchanged
compile '/static/*/' do
  write item.identifier.chop + '.' + item[:extension]
end
```

Though perhaps relabeling the block to `process` might better convey the
unified approach? The only change would be semantic, not functional, but I
feel like it might better express what's happening: you're filtering,
laying out, and writing documents from a given source.

~ _callahad_
