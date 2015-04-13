.. _phoenix-bundle-order-security-review-rules:

#####
Rules
#####

It is simple to create your own security rule.

Rules must implement \Phoenix\Bundle\OrderSecurityReviewBundle\Model\RuleInterface. The name you pick for the security rule (via the 'getName' method) will be used in configuration when enabling the rule.

Additionally you can also implement one or more of the following interfaces:

- 'CustomerDependentRuleInterface'
- 'OrderDependentRuleInterface'
- 'PaymentInstructionDependentRuleInterface'

You can use these interfaces to access pertinent information about the order in order to perform rule evaluation. For example if you are implementing a rule that needs to send a request to a third party system you might need access to the customer, and the order. If you implement 'CustomerDependentRuleInterface' and 'OrderDependentRuleInterface', then you will have access to the Customer and Order objects within your rule in order to form your request.

For some rules, you may want to only use them in certain narrow criteria. E.G your rule may only be applicable to certain card types. In this instance you can implement 'RuleRequiresApplicabilityEvaluationInterface' and add your logic determining if the rule should be taken into account within the method 'isApplicable'. This allows you to restrict the execution of your rule based on the current context.