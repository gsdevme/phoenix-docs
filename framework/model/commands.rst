.. _phoenix-model-commands:

Commands
========

This is an example of a command to 'Launch a Missile'. The command must implement 'Commmand/LaunchMissileInterface';

.. code-block:: php
	// Commmand/LaunchMissileInterface
	<?php

	namespace Phoenix\Model\Catalog\Command;

	use SimpleBus\Message\Name\NamedMessage;

	interface LaunchMissileInterface
	{	
		
	    public function getMissileReference();
	}

.. code-block:: php
	// Commmand/LaunchMissile
	<?php

	namespace Phoenix\Model\Catalog\Command;

	use SimpleBus\Message\Name\NamedMessage;

	class LaunchMissile implements LaunchMissileInterface, NamedMessage
	{
	    /**
		 * String
		 */
		private $missileReference;

		/**
		 * @param  {string} $missileReference A valid reference for a missile
		 */
	   	public function __construct($missileReference)
	   	{
			$this->missileReference = $missileReference;
		}

		/**
		 * @return {string}
		 */
		public static function messageName()
		{
			return 'LaunchMissile';
		}
	}

Best Practices for Commands
---------------------------
	- Arguments for commands should be scalars or objects from the 'Component' or 'Model' folders. DO NOT use entites or objects containing current application 'state' as command arguments.
	- Favor scalars over objects unless your command is particularly complex.
	- If your command is taking action on an object that already exists in the database, refer to it using a 'reference' rather than by auto-increment ID and pass the reference into the handler. This allows the command to be more easily constructed and sent to the handler by systems that do not have access to the same persistance layer, or than may only have a reference to the missile.