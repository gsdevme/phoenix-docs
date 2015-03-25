.. _phoenix-bundle-access-voters:

Voters
======

We will write a simple voter that votes on the basis of the current day of the week.

Voters must implement VoterInterface. Here is our first pass of the voter - voting true if the current day of the week is Monday.

.. code-block:: php

	<?php

	use Phoenix\Component\Access\Voter\VoterInterface;

	class MondayVoter implements VoterInterface
	{
	    public function __construct(
	    
	    ) {
	    	// The name of the voter is used to refer to it later on in configuration
	        $this->name = 'day_of_week';
	        $this->priority = 1;
	    }

	    /**
	     * {@inherit}
	     */
	    public function getName()
	    {
	        return $this->name;
	    }

	    /**
	     * {@inherit}
	     */
	    public function setParams(array $params)
	    {
	    }

	    /**
	     * {@inherit}
	     */
	    public function getParams()
	    {
	    }

	    /**
	     * {@inherit}
	     */
	    public function vote(array $params)
	    {
	    	$now = new \DateTime('now');
	        if (strtolower($now->format('l')) !== 'monday') {
	            return VoterInterface::ACCESS_DENIED;
	        }

	        return VoterInterface::ACCESS_GRANTED;
	    }

	    /**
	     * {@inherit}
	     */
	    public function supportsParams(array $params)
	    {
	        return true;
	    }

	    /**
	     * @return integer
	     */
	    public function getPriority()
	    {
	        return $this->priority;
	    }

	    /**
	     * @param integer
	     */
	    public function setPriority($priority)
	    {
	        $this->priority = $priority;
	    }
	}

This voter does a job - but it's not very flexible, it would be better if we could allow it to be configured directly to adjust which day of the week we are checking for. We can do this by using the getParams and setParams methods. 
.. warning:: Parameter index is important because this simplifies the configuration of voters in the yaml file later on.

.. code-block:: php

	<?php

	use Phoenix\Component\Access\Voter\VoterInterface;

	class DayOfWeekVoter implements VoterInterface
	{
		/**
		 * @param  {String} $dayOfWeek A valid value for the 'l' value of date(), e.g 'monday', 'tuesday', 'sunday' et.c
		 */
	    public function __construct(
	    	$dayOfWeek = 'monday'
	    ) {
	    	// The name of the voter is used to refer to it later on in configuration
	        $this->name = 'day_of_week';
	        $this->priority = 1;
	        //we should add some validation here to check the date being provided is valid!
	        $this->dayToCheck = $dayOfWeek;
	    }

	    /**
	     * {@inherit}
	     */
	    public function getName()
	    {
	        return $this->name;
	    }

	    /**
	     * {@inherit}
	     */
	    public function setParams(array $params)
	    {
	    	// when this voter comes to be configured later on by the yaml configuration, it will provide the confguration as an indexed array. Hence the parameter for _this_ function is an indexed array.
	    	// we should add some validation here to check the date being provided is valid!
	    	$this->dayOfWeek = $params[0];
	    }

	    /**
	     * {@inherit}
	     */
	    public function getParams()
	    {
	    	return [$dayOfWeek];
	    }

	    /**
	     * {@inherit}
	     */
	    public function vote(array $params)
	    {
	    	$now = new \DateTime('now');
	        if (strtolower($now->format('l')) !== $this->dayOfWeek) {
	            return VoterInterface::ACCESS_DENIED;
	        }

	        return VoterInterface::ACCESS_GRANTED;
	    }

	    /**
	     * {@inherit}
	     */
	    public function supportsParams(array $params)
	    {
	        return true;
	    }

	    /**
	     * @return integer
	     */
	    public function getPriority()
	    {
	        return $this->priority;
	    }

	    /**
	     * @param integer
	     */
	    public function setPriority($priority)
	    {
	        $this->priority = $priority;
	    }
	}

Finally - what if this voter didn't need to vote on the basis of the *current* day of the week, but on the basis of when something else happened. In that instance we would need to provide the value of that 'day' to this voter in order for it to make a decision. If your voter requires a parameter like this, it reduces the situations in which it can be used significantly, because the parameter needs to be provided to the subject at the time the access decision is being made. Unfortuantely this introduces a strong coupling between the voter and subjects on which it can vote on. However this pattern is often used without issue, you just need to be mindful of the consequenses when you interoduce it.

From the above code - we now look at the 'supportsParams' and 'vote' methods. The 'supportsParams' method will be run against the parameters provided at the time the access decision is made, eg if the access decision manager is called like this:

.. code-block:: php

	$container->get('phoenix_access.decision_manager')->can('delete_the_customer', [$customer])

The the runtime parameters for this subject will be [$customer]. Most subjects have a natural subject, which is usally the 'object' the verb is being taken on. E.G 'delete_the_category' would have a category as a parameter. *This* voter is being altered now in a way which means it can only be used by subjects where a 'DateTime' is provided as a parameter. Here we move the code for extracting a \DateTime object from an array of params to a common method to allow it to be reused.

.. code-block:: php

	<?php

	use Phoenix\Component\Access\Voter\VoterInterface;

	class DayMatchesVoter implements VoterInterface
	{
		/**
		 * @param  {String} $dayOfWeek A valid value for the 'l' value of date(), e.g 'monday', 'tuesday', 'sunday' et.c
		 */
	    public function __construct(
	    	$dayOfWeek = 'monday'
	    ) {
	        $this->name = 'day_matches';
	        $this->priority = 1;
	        $this->dayToCheck = $dayOfWeek;
	    }

	    /**
	     * {@inherit}
	     */
	    public function getName()
	    {
	        return $this->name;
	    }

	    /**
	     * {@inherit}
	     */
	    public function setParams(array $params)
	    {
	    	$this->dayOfWeek = $params[0];
	    }

	    /**
	     * {@inherit}
	     */
	    public function getParams()
	    {
	    	return [$dayOfWeek];
	    }

	    /**
	     * {@inherit}
	     */
	    public function vote(array $params)
	    {
	    	foreach($params as $param) {
	        	if ($param instanceof \DateTime && $param->format('l') === $this->dayOfWeek) {
	        		return  VoterInterface::ACCESS_GRANTED;
	        	}
	        }

	        return VoterInterface::ACCESS_DENIED;
	    }

	    /**
	     * {@inherit}
	     */
	    public function supportsParams(array $params)
	    {
	        foreach($params as $param) {
	        	if ($param instanceof \DateTime) {
	        		return true;
	        	}
	        }
	        // The behavior of the phoenix_access.decision_manager service is to throw an UnsupportedParamsException
	        // when a voter is being used when the incorrect params are being provided by the subject. You may wish
	        // to return 'true' from this method and support this situation in your 'vote' method instead.
	        return false;
	    }

	    /**
	     * @return integer
	     */
	    public function getPriority()
	    {
	        return $this->priority;
	    }

	    /**
	     * @param integer
	     */
	    public function setPriority($priority)
	    {
	        $this->priority = $priority;
	    }
	}

We can futher add to the voter to make it more useful when a dateTime isn't provided. We can reasonably guess that this voter might be used for parameters with a 'getCreated' method, and if we account for that we can add the ability to support those parameters.

.. code-block:: php

	<?php

	use Phoenix\Component\Access\Voter\VoterInterface;

	class DayMatchesVoter implements VoterInterface
	{
		/**
		 * @param  {String} $dayOfWeek A valid value for the 'l' value of date(), e.g 'monday', 'tuesday', 'sunday' et.c
		 */
	    public function __construct(
	    	$dayOfWeek = 'monday'
	    ) {
	        $this->name = 'day_matches';
	        $this->priority = 1;
	        $this->dayToCheck = $dayOfWeek;
	    }

	    /**
	     * {@inherit}
	     */
	    public function getName()
	    {
	        return $this->name;
	    }

	    /**
	     * {@inherit}
	     */
	    public function setParams(array $params)
	    {
	    	$this->dayOfWeek = $params[0];
	    }

	    /**
	     * {@inherit}
	     */
	    public function getParams()
	    {
	    	return [$dayOfWeek];
	    }

	    /**
		 * {@inherit}
		 */
		public function vote(array $params)
		{
			$dateTimeParam = $this->extractDateTime($params);
			if (!$dateTimeParam) {
				return VoterInterface::ACCESS_DENIED;
			}
			if ($dateTimeParam->format('l') !== $this->dayOfWeek) {
				return VoterInterface::ACCESS_DENIED;
			}

		    return VoterInterface::ACCESS_GRANTED;
		}

		/**
		 * {@inherit}
		 */
		public function supportsParams(array $params)
		{
		   	return $this->extractDateTime($params);
		}

		private function extractDateTime(array $params)
		{
			foreach($params as $param) {
		    	if ($param instanceof \DateTime) {
		    		return $param;
		    	}
		    	if (method_exists($param, 'getCreated')) {
		    		$getCreated = new ReflectionMethod($param, 'getCreated');
					if ($getCreated->getNumberOfParameters() == 0 && ($param->getCreated() instanceof \DateTime())) {
		    			return $param->getCreated();
					}
		    	}
		    }
		    return null;
		}

	    /**
	     * @return integer
	     */
	    public function getPriority()
	    {
	        return $this->priority;
	    }

	    /**
	     * @param integer
	     */
	    public function setPriority($priority)
	    {
	        $this->priority = $priority;
	    }
	}

Now - it is likely that this voter will fill the role it was designed for. Voters shouldn't be designed to take account of any possible permutation of parameters, they are cheap to write and configure and you can reimplement variations for specific subjects if required, however the idea is that there are enough out of the box voters to get you started with configuring access, and the voting mechanism allows you to add new voters that fill in the gaps. Read the documentation on configuring subjects via voters to see how to use a combination of voters to provide an access decision about a subject.

Service Container
=================

To use this voter with our application we need to add it to the service container and tag it.

.. code-block:: yaml

	cool_bundle.voter.day_matches:
        class: Client\Bundle\CoolBundle\Voter\DayMatchesVoter
        tags:
            - { name: phoenix_access.voter }

You can check to make sure your voter is now registered by using the console command for this purpose.

'app/console phoenix:access:voters:view'

