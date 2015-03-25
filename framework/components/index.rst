.. _phoenix-component-index:

Components
==========

The following components are provided. These components are heavily used within Phoenix, and their behavior (where appropriate) can altered via configuration within :ref:`bundles<phoenix-bundles-index>`.

For code to be moved to a component it must:
- Have a well defined scope
- Model something useful outside the scope of the Phoenix application
- Have an acceptable level of - :ref:`code quality <phoenix-contributing-coding-standards>`.

Current Phoenix Components
~~~~~~~~~~~~~~~~~~~~~~~~~~
- Access
- File
- Market (migration in progress)
- Money
- Tracking (requires refactoring evaluation)

Current Third Party Components
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- Markup/Addressing

Candidates Components
---------------------
These are candidate components and will be documented after being abstracted from bundle code:

- Pricing (from ProductBundle)
- Tax (from ProductBundle)
- Cache (from CacheBundle)
- Stock (from ProductBundle)
- CreditCard (from CreditCardBundle)
- Subscription (from SubscriptionBundle)
- Article (from ClothingProductBundle)
- Order (from OrderBundle)


Removal Candidates
------------------
These are currently components and are candidates for removal (should be moved to bundles where they are used?)

- Distribution
- DomainAwareKernel