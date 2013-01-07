--- 
status: implemented
--- 

## Original situation

The nanoc enhancement proposals (NEPs) are tracked on the wiki. There is a single list of all NEPs, a small list of quick wins, and for every NEP there is a single wiki page. This has several drawbacks:

1. There is no central place in which metadata (status, is quick win) is stored. The main list of NEPs contains the status for each NEP, but the status is also mentioned on the individual NEP pages. This has a high chance of becoming inconsistent at some point in the future.

2. There is no easy way to quickly find a new, unused NEP number. It has happened in the past that multiple NEPs had the same NEP number.

## Proposed solution

1. There will be a central repository containing the NEPs, formatted as Markdown.

2. There will be a web application for browsing the NEPs. This web application will be a simple web site (since it does not require server-side processing) built with nanoc. It will contain a list of NEPs, a list of NEPs grouped by status, and a list of quick wins, as well as individual pages for every NEP.

3. There will be a well-defined way for third parties to deliver feedback and contribute to NEPs. GitHub issues and pull requests are probably a good idea for this, since with minimal effort a working system can be built.
