Status: released

Goal: allow non-ASCII characters and ANSI color codes to be written when the destination supports it. For example, an UTF-8 terminal will both have colors and special characters, while a file as output will have ANSI color codes stripped.

Situation before nanoc 3.4
--------------------------

Non-ASCII characters were removed by calling `#make_compatible_with_env` on the string, which checked the environment variables and decided to remove non-ASCII characters.

**Problems:** This approach is not scalable, the cleaning happens all over the place (not centralized) and you tend to forget to use the method.

Situation in nanoc 3.4
----------------------

`$stdout` and `$stderr` are replaced with streams that filter ASCII and UTF-8 characters.

**Problems:** Disabling/enabling the streams is difficult. There is no way to un-wrap streams and get the original `$stdout`/`$stderr` back. The command runner may wrap `$stdout`/`$stderr` multiple times. There is no way to set up the streams when handling an option (and therefore not executing a command).

Situation in nanoc 3.5
----------------------

There will be one cleaning output stream, which is created by wrapping `$stdout` at startup. This stream can be told to enable/disable features.
