.. _phoenix-bundle-access-configuration:

Configuration
=============

Once you have set up your :ref:`Subjects <phoenix-bundle-access-subjects>` and :ref:`Voters <phoenix-bundle-access-voters>` you can configure them within your application.

A common pattern for answering questions is the 'role+business logic' combination. We will build on our earlier examples to describe how to configure the 'missile_be_launched' to be dependent on the current day of the week.

Configuration is provided as follows:

.. code-block:: yaml

    # config.yml
	phoenix_access:
	    security:
	        - subject:
	            voter1: configvalue1
	            voter2: [configvalue1, configvalue2]


So for our specific example (and generally you want to have all of this configuration in the same place so we advise you create an 'access.yml' file and place all of your configuration here.)

.. code-block:: yaml

	# config.yml
	phoenix_access:
	    security:
	        - missile_be_launched:
	            day_matches: monday
	            ensure_admin_role: 'ROLE_MISSLE_LAUNCHER'

The first voter 'day_matches' expects the subject to provide a 'day of the week' in order to make a decision, so anywhere that the 'missile_be_launched' subject is being checked it will need to be provided, in the below examples we assume we have access to a parameter called 'day' which is a dateTime object representing the current day.


.. code-block:: twig

    // your_template.html.twig
    {# day is a parameter passed into the tempalte from the controller #}
    {% if can('missile_be_launched', day) %}
          <button id="launch">
            Launch
          </button>
    {% endif %}

.. code-block:: php
    
    // LaunchController.php
    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    <?php

    class LaunchController extends Controller
    {
        public function LaunchAction()
        {
        	$day = new \DateTime('now');
            if (!$this->get('phoenix_access.decision_manager')->can('missile_be_launched', $day)) {
                throw new Phoenix\Component\Access\Exception\SecurityException('You cant launch!');
            }
            $this->get('launcher')->launch();
        }
    }

The second voter 'ensure_admin_role' is a built in voter provided by the AdminUserBundle. It votes on the basis of the built in Symfony Security ACL, so you need to make sure you have a relevant role in your security.yml

.. code-block:: yaml

    # security.yml
    role_hierarchy:
        ROLE_SUPER_ADMIN:     [ROLE_MISSILE_CONTROL_ROOM]
        ROLE_MISSILE_CONTROL_ROOM:        [ROLE_MISSILE_LAUNCHER, ROLE_MISSILE_PILOT]

It's likely that the functionality you have written to launch missiles will be used by different clients, or that the clients roles change. You have now decoupled the code that launches missiles, and determines if they can be launched (which is generic and reuseable), from the particular logic in this installation of your code (which is specific to this one application).

You can use the build it console command to check that your congiguration is correct:
'app/console phoenix:access:subjects:view'
