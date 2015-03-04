'Tracking' Snippets & Third Party Javascript
=======

Introduction
------------

The `PhoenixTrackingBundle` provides a mechanism for inserting third party javascript snippets into templates. Snippets allow:

- Code reuse across projects
- Snippets to be enabled/disabled on a per environment basis
- Configuration to be provided for snippets (which can differ per environment)

Where possible we reccomend the use of Google Tag Manager or other third party javascript tag management for this purpose. However this is not always possible, and in any event, you may still need to implement javascript in order to send information to a data layer in a third party system.

Implementations
---------------

The following snippets already have implementations and can be used out of the box, providing you implement the correct contexts in your templates. Each implmentation has its own configuration reference:

- :ref:`phoenix-snippet-adform`.
- Affiliate Window
- Chango
- CrazyEgg
- Criteo
- Dotmailer
- Google Tag Manager
- MediaMind
- MicrosoftAdCenter
- Optimizely
- Peerius
- PollDaddy
- Quantcast
- Rakuten
- RavenJs
- RTBHouse
- Salecycle
- VeInteractive
- Vergic

Contexts/Twig Extension
-----------------------

Tracking snippets are rendered by use of a common twig extension. This snippet declares 'blocks' or areas into which snippets will be rendered. It is up to you which areas to declare in your templates, but if using the built in snippets variations, the following contexts will be assumed to exist:

- Global
- Product
- Category
- Checkout
- Confirmation

Cookbook / Implementing your own Snippet
--------

