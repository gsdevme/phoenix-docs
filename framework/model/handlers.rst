.. _phoenix-model-handlers:

Handlers
========

Writing a command handler
-------------------------

The below handler handles an instruction to launch a missle. A handler must implements 'SimpleBus\Message\Handler\MessageHandler'.

.. code-block:: php
    
    // MissileLaunchHandler.php
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    <?php

	use SimpleBus\Message\Handler\MessageHandler;
	use SimpleBus\Message\Message;

	class LaunchMissleHandler implements MessageHandler
	{
	    /**
	     * @var MissleLauncher
	     */
	    private $launcher;

	    /**
	     * @var MissleRepositoryInterface
	     */
	    private $missleSilo;

	    public function __construct(
	        MissleLauncher $launcher,
	        MissleRepositoryInterface $missleSilo
	    ) {
	        $this->launcher = $launcher;
	        $this->missleSilo = $missleSilo;
	    }

	    public function handle(Message $message)
	    {
	        $reference = $message->getMissileReference();
	        $missile = $missleSilo->find($reference);
	        if (!$missle) {
	            throw new InvalidMissileReferenceException($reference . ' is not a valid reference');
	        }
	        $this->launcher->launch($missle);
	    }
	}

Configuring a handler to handle a command
-----------------------------------------
To configure a handler to be used with a particular command, we need to add configuration for the handler in the Service Container. By convention in Phoenix this is done by including a 'command_handlers.yml' file in your bundle and including this in the bundle depency injection extension.

.. code-block:: php
    
	<?php

	namespace Client\Bundle\CoolBundle\DependencyInjection;

	use Symfony\Component\DependencyInjection\ContainerBuilder;
	use Symfony\Component\Config\FileLocator;
	use Symfony\Component\HttpKernel\DependencyInjection\Extension;
	use Symfony\Component\DependencyInjection\Loader;

	class CoolBundleExtension extends Extension
	{
	    /**
	     * {@inheritDoc}
	     */
	    public function load(array $configs, ContainerBuilder $container)
	    {
	        $configuration = new Configuration();
	        $config = $this->processConfiguration($configuration, $configs);

	        $loader = new Loader\YamlFileLoader($container, new FileLocator(__DIR__.'/../Resources/config'));
	        $loader->load('command_handlers.yml');
	    }
	}

You then tag your handler service telling it which named command it should handle.

.. code-block:: yaml

	# CoolBundle/Resources/config/command_handlers.yml
		services:
		    cool.handler.launch_missle:
		        class: Cool\Bundle\CoolBundle\Handler\MissileLaunchHandler
		        arguments:
		            - @cool.launcher.missile
		            - @cool.repository.missle
		        tags:
		            - { name: command_handler, handles: LaunchMissile }


Best Practices for Handlers
---------------------------
	- DO NOT use services in within that rely on current application state, or use services which reach these services. This includes services such as 'security_context', 'phoenix_access.decision_manager'. It is OK to rely on system state (i.e the database) that has already been persisted.