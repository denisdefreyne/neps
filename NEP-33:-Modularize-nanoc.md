This page describes the current problems with nanoc as a monolithic piece of software and the versioning and distribution of plugins.

Glossary
--------

* **module**: A bit of functionality that is currently part of the main nanoc distribution, but could be extracted into a plugin and live in on its own

* **plugin**: A bit of functionality that used to be part of the main nanoc distribution, but was split into its own separate project

Problem
-------

* **nanoc is monolithic**: nanoc is currently a monolithic distribution: the distribution includes the core of nanoc itself, all filters, all helpers, all data sources, all rake tasks, etc.

* **New modules are only introduced in the next minor release**: nanoc uses the Semantic Versioning scheme, which means that new backwards-compatible functionality is only added in minor releases (x.Y.z). With nanoc, the time between individual minor releases is usually rather large (6 months or more is not uncommon), and having to wait for the next minor release is therefore a pain.

* **There’s no standard way for third parties to release plugins**: It is possible for other developers to release gems and let users of nanoc require this gem in order to use it, but there is no standardized approach here. There is no list of available plugins, and no uniform way of installing plugins.

Goals
-----

* **nanoc should be lighter**: The main distribution should ideally only contain the most essential plugins (e.g. only the erb filter, no helpers, no tasks, …).

* **Plugin ownership should be passed on easily**: It should be easy to give pass on ownership of plugins to other developers, which then become the plugin’s maintainer.

* **It should not be necessary to wait to get new modules**: When a new module is written and does not depend on features that are only available in the next minor release, it should be able to be installed right away.

* **There should be a central plugin repository**: To make plugins easy to install, there needs to be a central place where such plugins can be stored. Such a central repository should allow installing plugins, searching for plugins, adding new plugins, etc.

* **Third parties should be able to release plugins**: Third-party developers should be able to write plugins and make immediately them available for all nanoc users.

* **It should be easy to distinguish official from non-official plugins**: In order to guarantee the quality of plugins, there should be a clear distinction between official and non-official plug-ins. Official plugins are either maintained by myself or a trusted developer. Unofficial plugins can (and should) become official plugins after a review.

Possible solutions
------------------

### Using Rubygems

Ruby already comes with a package manager called Rubygems. Since this is a tried-and-true tool for distributing Ruby software, it is a natural choice for distributing nanoc plugins as well.

The major advantage is that when using Rubygems, no plugin distribution system needs to be written anymore, because it already exists. Rubygems allows most of the goals mentioned above to be fulfilled.

It does not, however, allow a distinction to be made between official and non-official plugins. Rubygems (or, perhaps more precisely: Gemcutter) does not allow namespaces to be reserved; there is no way to claim ownership of all current and future gems named “nanoc3-*”.

### Using Rubygems with a custom index

In order to solve the issue of not being able to distinguish between official and non-official gems, the nanoc site could host its own gem index for official plugins.

This still would not ensure a distinction between official and non-official gems, because after installation, it would be impossible to determine the source of a plugin.

### Using a custom plugin manager

Another possibility is to write a custom plugin/package manager with support for distinguishing between official and non-official plugins, and with more strictly enforceable rules ownership rules. All of the goals mentioned above can be achieved this way.

The main disadvantage of this approach is that it takes time and effort to implement and test.
