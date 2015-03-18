Components
==========

The following components are provided. These components are heavily used within Phoenix, and their behavior (where appropriate) can altered via configuration within bundles(?).

For code to be moved to a component it must:
- Have a well defined scope
- Model something useful outside the scope of the Phoenix application

Current
-------

Phoenix Components
~~~~~~~~~~~~~~~~~~
- File
- Money

Third Party Components
~~~~~~~~~~~~~~~~~~~~~
- Markup/Addressing

Candidates
----------
These are candidate components and will be documented after being abstracted from system code:

- Pricing (from ProductBundle)
- Tax (from ProductBundle)
- Market (from MarketBundle)
- Cache (from CacheBundle)
- Stock (from ProductBundle)
- CreditCart (from CreditCartBundle)
- Subscription (from SubscriptionBundle)
- Article (from ClothingProductBundle)
- Order (from OrderBundle)


Removal Candidates
----------
These are currently components and are candidates for removal (should be moved to bundles where they are used)

- Distribution
- DomainAwareKernel
- Tracking