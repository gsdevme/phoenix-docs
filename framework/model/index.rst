.. _phoenix-model-index:

Model
=====

The Model folder contains the domain model of the application and holds base classes which are given persistence by entities configured in the 'core' system bundles.
This folder also contains repository interfaces that describe how the model is accessed. Additionally all instructions to modify the model (making system state changes) take place using commands (instructions) sent to handlers(that perform the instructions) defined in this folder.

The Phoenix components are shared resources of the model layer.


The Command/Handler Pattern
---------------------------

Actions in Phoenix should be taken using the command bus which is an implementation of the Message Pattern. This mechanism is driven by the `Simplebus Libraries <https://github.com/SimpleBus/>`_.

.. seealso::
	The concepts used here, and the advantages and disadvantages are `discussed by the Author of Simplebus on his blog <http://php-and-symfony.matthiasnoback.nl/2015/01/some-questions-about-the-command-bus/>`_.


Commands
--------
Commands represent an instruction to alter the model. All actions that require a change to system state, or to 'do' something must be modeled via commands. Commands extend the MessageInterface and are sent to the 'CommandBus' to be handled. You can see the anatomy of currently modeled commands by checking the contents of 'Phoenix/Model/*/Command'. Read the :ref:`Command Documentation <phoenix-model-commands>` to learn more on the structure of commands.

Handlers
--------
Handlers take command classes and perform system actions based on the contents of the command. Handlers are executed via the 'Command Bus' either synchronously (immediately) or asynchronously (via a system such as rabbitMQ, Beanstalkd or ironMQ). Read the :ref:`Handler Documentation <phoenix-model-handlers>` to learn more on how to declare a handler.

Command Bus
-----------
The `Command Bus` is responsible for handling all commands registered in the system. This service should be accessed wherever you create a command that needs to be executed:

.. code-block:: php
    
    // ButtonController.php
    <?php
    
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;

    class LaunchController extends Controller
    {
        public function LaunchAction(Missle $missile)
        {
        	$launchCommand = new $launchCommand
            $this->get('command_bus')->launch();
        }
    }


Refactoring 'Phoenix Job Queue' Commands to use the Command Bus
---------------------------------------------------------------
@todo Explain how to refactor earlier iterations of this pattern via the 'Phoenix Job Queue'

What to watch out for when using the Command Bus
------------------------------------------------
- @todo Discuss implications of CommandBus not being able to give a return value.