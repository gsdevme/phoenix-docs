.. _phoenix-bundles-index:
#######
Bundles
#######

Phoenix Bundles configure the 'Phoenix/Component' and 'Phoenix/Model' layers for use in Phoenix.

.. note::  A large number of bundles are subject to refactoring, :ref:`a guide to the current state of these bundles is provided <phoenix-bundles-refactoring-guide>`, please read it if you are looking for the responsibilities of a Bundle:


AccessBundle
============
The Phoenix Access Bundle provides the mechanism which is used to control access to functions within Phoenix. :ref:`Read the AccessBundle Documentation <phoenix-bundle-access-index>`.

TrackingBundle
==============
The Phoenix Tracking Bundle provides an easy way to add third party javascript and tracking snippets to instances of Phoenix (also Google Tag Manager). These snippets are configurable and can be enabled on a per instance basis. :ref:`Read the TrackingBundle Documentation <phoenix-bundle-tracking-index>`.

OrderSecurityReviewBundle
==============
The Phoenix Order Security Review Bundle provides a framework for adding rules which are triggered after an order is placed. These rules can place an order into a 'security review' status in order for the details fo the order to be manually checked by an operator before dispatching the order. :ref:`Read the OrderSecurity Documentation <phoenix-bundle-order-security-review-index>`.
