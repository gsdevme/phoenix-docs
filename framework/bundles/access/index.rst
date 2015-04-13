.. _phoenix-bundle-access-index:
############
AccessBundle
############

The AccessBundle configures the Access Component for use in Phoenix.

Introduction
~~~~~~~~~~~~
The Phoenix Access Component is used to control access to functionality within phoenix based on user authorization, business logic and system state (or any other rules you require). It builds on the work of the `Symfony Security Component <http://http://symfony.com/doc/current/components/security/introduction.html>`_ to allow an easy way to configure access to sytem functionality within Phoenix.

Why not the security component alone?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The Symfony Security component introduces a powerful set of tools to be able to control Authentication and Authorization within Symfony Applications. The role of *this* component is to work in conjunction with this component in a way that makes it easy to write functionality and be able to control access to that functionality through configuration.

It may help to read `The Symfony cookbook on using voters <http://symfony.com/doc/current/cookbook/security/voters_data_permission.html>`_ to get some understanding of the general approach. What this bundle provides is the ability to define configurable *subjects*, where the configuration allows you to define what voters should be used to answer that subject.

This component ultimately provides one piece of functionality, allowing you to ask the application 'Can I do something', where that 'something' is a verb. E.G 'Can I delete this user', 'Can I dispatch this shipment' etc. The logic backing this question is abstracted so that the same question is answered in the same way - no matter where it is asked (e.g in a Template and in a controller), and can be easily changed on a per application basis.

There are three main actors in this component. Subjects, Voters and Configuration.

Subjects
~~~~~~~~

Subjects define a question that needs to be answered. When writing your component you should wrap any functionality or UI that requires the question to be answered first in order to prevent it from displaying or executing. For example:

.. code-block:: twig

    // your_template.html.twig

    {% if can('missile_be_launched') %}
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
            if (!$this->get('phoenix_access.decision_manager')->can('missile_be_launched')) {
                throw new Phoenix\Component\Access\Exception\SecurityException('You cannot launch!');
            }
            $this->get('launcher')->launch();
        }
    }

In the :ref:`Subject<phoenix-bundle-access-subjects>` documentation we create a subject which we can reference in our templates, controllers and handlers to check if something should be done, before allowing it to happen.

Voters
~~~~~~

How does the Decision Manager decide the answer to a question? Using Voters! Voters in this component do the same thing as in the Symfony Component, they vote to 'grant', 'reject' or 'abstain'. They are simple logical checks to determine if something is true or false (or in rare cases if it cannot be determined from the available information, however generally in these situations the voter would vote to deny). An example of a voter would be the 'AdminAwareVoter' which votes on the basis of the currently logged in Admin, and a configuration checking the role of the admin.

In the :ref:`Voter<phoenix-bundle-access-voters>` documentation we create a voter that is able to vote on the basis of the current day of the week.

Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~

The final piece of the puzzle is in the configuration layer. Via yaml, this bundle provides an easy way to assign voters to subjects in order to provide the logic to answer these access questions. The :ref:`Configuration <phoenix-bundle-access-configuration>` documentation describes how to do this.