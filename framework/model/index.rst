Model
=====

The model outlines the domain model of the application and contains base classes which are given persistence by entities configured in the 'core' system bundles. 

Commands
--------
Commands represent an instruction to alter the model. All actions that require a change to system state - or to 'do' something must be modelled via commands. Commands extend the MessageInterface and are sent to the 'CommandBus' to be handled.

Handlers
--------
Handlers take Command classes and perform system actions based on the contents of the command