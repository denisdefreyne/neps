--- 
Status: accepted
is_quick_win: true
--- 

Adding a new colorizer should not require patching the colorizer filter. Perhaps an abstract `Colorizer` class that implements a `#run(content, params={})` method would be useful.
