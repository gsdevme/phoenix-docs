.. _phoenix-contributing-coding-standards:

################
Coding Standards
################
Phoenix code should adhere (with a few exceptions) to the `Symfony Coding Standards <http://symfony.com/doc/current/contributing/code/standards.html>`_. Make yourself familiar with them.

.. warning:: Much of the code in Phoenix does not hold to these standards, but new code being contributed will be. If you see code that needs attention please look to refactor the bundle first before making a PR to fix coding standard issues. See :ref:`The Phoenix Bundle Refactoring Guide<phoenix-bundles-refactoring-guide>` for more

Exceptions
==========
Service Naming Conventions
^^^^^^^^^^^^^^^^^^^^^^^^^^
	- Class names should be used in service declerations directly, without a parameter
	- Service names should follow the format
		{bundle}.{underscore_seperated_folders}.{class}

		e.g The file /ZooBundle/Keeper/LionKeeper would be named:
		'zoo.keeper.lion'

License
^^^^^^^
	- Phoenix files require no 'License' block


Best Practices
==============
	- Use .yml for denfining bundle configuration
	- Use .yml for defining doctrine entities
	- Don't use annotations
