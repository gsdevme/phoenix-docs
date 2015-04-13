.. _phoenix-bundle-order-security-review-index:

#########################
OrderSecurityReviewBundle
#########################

The OrderSecurityBundle provides a framework for defining and configuring rules which trigger the 'security review' status for an order.

Introduction
============

Once an order is placed, it is sometimes desirable to flag orders which may be high risk (in terms of fraudulent transactions). This can be done using various criteria, either simple rules based on order value, contents of the order shipping location or a decision made by a more sophisticated third party service like Sift Science.

The Order Security bundle provides some simple implementations out of the box, but additional rules can be added by other bundles (e.g a third party provider plugin, or a payment service with an integrated security system).

The Security Review Service
===========================

The security review service is the core service used to register and execute security review rules. 'Rules' are defined as services which implement \Phoenix\Bundle\OrderSecurityReviewBundle\Model\RuleInterface and are tagged with the name 'security_review.rule'. Any services in the dependency injection container which have been tagged with this rule will be available for use in in the Security Review Service. Rules return either 'PASS', 'FAIL' or 'DEFER' from the 'evaluate' method. See the documentation on :ref:`Order Security Rules<phoenix-bundle-order-security-review-rules>` for more information.

The out of the box rules are able to be configured and are marked as 'disabled' by default. In order to use these rules in your application you need to add a configuration to enable them, and to provide values for the rules (e.g the OrderValueRule can be configured with a minimum order value at which the security rule is triggered). See the :ref:`Configuration Reference<phoenix-bundle-order-security-review-configuration>` for more information on how to configure these rules.

The Security Review Service is able to iterate over configured rules and process them, before returning a 'ReviewResult' object. This object will return a boolean representing the decision about the order. The way that this decision is reached is configurable. There are two strategies: 'STRATEGY_CONSENSUS' and 'STRATEGY_UNANIMOUS'. The 'unanimous' strategy (default) will return a 'false' decision if any of the individual rules returns 'FAIL'. The 'consensus' strategy will tally the pass and fails from individual rules and return a decision based on the higher quantity (note, at present there is no weighting against rules so every enabled rule carries the same weight when making a consensus decision).

Execution
=========
The security review service is utilized directly after an order has been placed. After order creation rules are evaluated, a decision returned, and then a manual 'security review' flag is set against the order (should the decision be 'fail'). If you need to defer a decision to a later time, you will need to implement this yourself (manually). See the section on implementing :ref:`Deferred security review decisions<phoenix-bundle-order-security-review-deferred>` for ideas on how to accomplish this.

For testing purposes you can execute an order security rule against an order via the command line, this will provide a security result and breakdown of each rule executed:

`phoenix:order:security_rule:result order_reference`

Viewing available rules
=======================
You can see all currently registered rules in the application by running the console command 'phoenix:order:security_rule:view_rules'. This allows you to see which rules are available.