.. _phoenix-bundle-access-subjects:

Subjects
========

Generally subjects are simple questions that need an answer. 'can I do X (on a Y)', where X is a verb and Y is a noun.
To define a subject, you can specify it via a tagged service definition. The majority of the time a subject can follow the standard pattern, indeed all of the build in subjects use this pattern by extending the 'Generic' security subject, where the first (and only required) argument is the name of the subject we want to define.

.. code-block:: yaml

	# CoolBundle/Resources/config/services.yml
	cool_bundle.subject.missile_be_launched:
	        parent: phoenix_access.subject.generic
	        arguments:
	            - missile_be_launched
	        tags:
	            - { name: phoenix_access.subject }

You can check to make sure your subject is registered with the application by using the built in console command:

'app/console phoenix:access:subjects:view'

This console command not only displays a list of current subjects, but also displays which voters are currently registered against those subjects. It can be used to make sure your subject, voter and configuration have all been set up as you expect.