.. _phoenix-contributing-roadmap:

Roadmap
=======

A working document describing current development and targets.

1.0 Release
-----------

The current focus in Phoenix is refactoring code to prepare for a 1.0 release. In order to achieve this all code is being subjected to analysis and being either:

- Moved to third party bundles (if functionality is not core) - even if the bundle has dependecy on core Phoenix components
- Refactored (to Model and Component Layers)
- Deleted

Components and bundles considered to have met the goals of the :ref:`phoenix-contributing-coding-standards` document will be tagged as being suitable for 1.0.
:ref:`A guide to current bundle refactoring can be seen here<phoenix-bundles-index>`

Pre 1.0:
--------
- Refactor all code to meet coding standards
- Outstanding functionality (Merchandising/Promotions/Email Preview)
- Remove susy/sass/assetic and outdated plugins and pipeline, replacing with grunt/libsass


Post 1.0:
---------
- Api