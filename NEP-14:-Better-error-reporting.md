Status: released

nanoc’s error reporting is not ideal for a few reasons.

* The terminal output is too big, so users lose overview of what has happened and what went wrong.
* The error log does not include enough data to identify what the problem may be.

I propose that the terminal output should be reduced by only writing the first 10 line of the stack trace, and by removing extra information such as the ruby and nanoc versions. There should be crashlog file that contains all data, and more (including for example a list of all gems and their versions) so that people can attach this crashlog in e-mails to get a better idea of whether something outside of nanoc is wrong, and to be able to reproduce the issue by recreating the environment in which nanoc crashes.

~ @ddfreyne, @justinclift

Crash log file
--------------

Building further on what has been said above, I believe a crash log file should contain the following:

1. Information about the crash itself
    1. error message
    2. possible resolution, if any (e.g. “install gem X to fix this load error”)
    3. compilation stack
    4. stack trace
2. Environment information
    1. nanoc version
    2. Ruby version
    3. Rubygems version
    4. List of installed and loaded gems
    5. Environment variables
    6. Load paths

~ @ddfreyne
