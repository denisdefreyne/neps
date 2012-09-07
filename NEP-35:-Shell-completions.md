Status: accepted

It would be great to have completions for zsh and bash. If such completions are written, they should query nanoc for the available commands, options and arguments. This should be fairly easy. Here’s an example of how commands can be queried:

```ruby
require 'nanoc'
require 'nanoc/cli'
puts Nanoc::CLI.root_command.subcommands.map { |c| c.name }.sort.first(3).join(', ')
# output:
# autocompile, compile, create_item
deploy_cmd = Nanoc::CLI.root_command.subcommands.find { |c| c.name == 'deploy' }
deploy_cmd.option_definitions.each { |d| puts " -%s --%s %s" % [ d[:short], d[:long], d[:desc] ] }
# output:
# -t --target specify the location to deploy to
# -L --list list available locations to deploy to
# -n --dry-run show what would be deployed
```
