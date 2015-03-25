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

Best Practices for Handlers
---------------------------
	- DO NOT use services in within that rely on current application state, or use services which reach these services. This includes services such as 'security_context', 'phoenix_access.decision_manager'. It is OK to rely on system state (i.e the database) that has already been persisted.