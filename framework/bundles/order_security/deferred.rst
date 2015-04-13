.. _phoenix-bundle-order-security-review-deferred:

##################
Deferred Execution
##################

You may have a requirement to defer a decision. Typically this is because a third party provider needs some time before it can provide a security decision. In this situation you can do the following.

First, you need to implement a security rule to read a local status from the third party. Typically this would be a database table holding information about the response from the third party. This table should be empty of information until the third party has rendered an appropriate decision.

You configure your rule to 'FAIL' when this information is not present, meaning the rule will fail until the information about the decision is present.

A service can be written to communicate with the third party, first to send the information about the order and customer (to allow a decision to be rendered), and afterwards to request the decision. This request is made repeatedly on a schedule until a decision is returned. Once the decision is returned, you trigger a re-evaluation of the order security review based on the decision.

The flow of information for this order would be as follows:

1) Order Created
2) Information sent to third party
3) Security Service evaluation takes place. Order 'fails' and is placed into security review as no decision exists for the third party
4) Recurring job checks for decision - order remains in security review until decision rendered.
5) Once decision is available, either release the order from security review, or keep it in security review (or automatically cancel the order depending on business logic)