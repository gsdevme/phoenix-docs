.. _phoenix-contributing-coding-standards:

################
Coding Standards
################
Phoenix code should adhere (with a few exceptions) to the `Symfony Coding Standards <http://symfony.com/doc/current/contributing/code/standards.html>`_. Make yourself familiar with them.

.. warning:: Much of the code in Phoenix does not hold to these standards, but new code being contributed will. If you see code that needs attention please look to refactor the bundle first before making a PR to fix coding standard issues. See :ref:`The Phoenix Bundle Refactoring Guide<phoenix-bundles-refactoring-guide>` for more


These issues can be automatically addresses by the php-cs-fixer. You should configure your Editor/IDE to automatically fix these issues as you work using this tool.

`Sublime Text <https://github.com/benmatselby/sublime-phpcs>`_
` PHPStorm <http://arnolog.net/post/92715936483/use-fabpots-php-cs-fixer-tool-in-phpstorm-in-2>`_

Exceptions
==========
Service Naming Conventions
^^^^^^^^^^^^^^^^^^^^^^^^^^
	- Class names should be used in service declarations directly, without a parameter
	- Service names should follow the format
		{bundle}.{underscore_seperated_folders}.{class}

		e.g The file /ZooBundle/Keeper/LionKeeper would be named:
		'zoo.keeper.lion'

License
^^^^^^^
	- Phoenix files require no 'License' block

House Rules
===========

File names
^^^^^^^^^^
- DO name .twig files using underscore_case_ending_with.html.twig
- DO name sass files with underscore_case_ending_with.scss

CSS/SASS
^^^^^^^^
- DO use .scss syntax (not .sass)
- DO use .dashes-as-class-and #id-names in .scss files
- DO NOT use vendor prefixes in your code, these should be added by the compiler
- DO NOT overly nest your selectors - try to keep to 3 levels deep maximum
- DO Use default bootstrap styles - this allows us to keep things consistent
- DO Add a component if you repeating yourself as regards html and css patterns
- DO NOT Add a component that you use in exactly one place
- Place your mixins/variables and non 'code generating' sass in a file under '/partial', prefixed by an underscore
- Place code which is going to be included in another file, but which *does* generate code under '/includes'
- Be consistent with spaces/tabs. Use a `Sass Beautifier<https://packagecontrol.io/packages/SassBeautify>`_.

JS
^^
.. warning:: This section is under development. The JS code needs to be modularised more explicilty and we need more clarity around best practice JS. Until then use these guidelines

- Be consistent with spaces/tabs. Use a `JS Beautifier<https://packagecontrol.io/packages/Javascript%20Beautify>`_.
- Try not to write non procedural/jQuery laden javascript, split your functionality into small packets of functionality using `an appropriate pattern <http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html>`_. Use object literal notation at the very least.
- Don't import 3rd party/framework libraries without discussion first, we want to encourage common development patterns through well understood (by our team) libraries.

Best Practices
==============
- Use .yml for defining bundle configuration
- Use .yml for defining doctrine entities
- Don't use annotations in PHP files
