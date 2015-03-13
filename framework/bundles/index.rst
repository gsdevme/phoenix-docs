Bundles
=======

A working document outlining the current structure of Phoenix bundles, with notes on which bundles should be refactored and how.

What do the core bundles have to do with the framework?
In terms of 'how to use the framework' that information should be reserved to 'the book', so maybe this
section should just have links to the bundles explaining what areas of concern the cover. The bundles themselves should hold a configuration reference document.

.. error:: Error blocks denote bundles which are currently are target for deletion after refactoring are marked with this note

.. important:: Core bundles provide configuration to the Model layer of the application are markets with this note

Existing Bundles
----------------

AddressingBundle
~~~~~~~~~~~~~~~~
 - Builds on the MarkupAdressingComponent and MarkupAdressingBundle. The majority of this bundle can be moved into components and the third party (markup) bundle. All that should remain is code to adapt addresses for use in co-operating bundles (e.g Order), component configuration etc.

AddressBundle
~~~~~~~~~~~~~
 - Renders a snippet. Needs refactored to use tracking snippet bundle.

AdminBundle
~~~~~~~~~~~
 - Dashboards/Menus
 - Switching of locale's within the backend
 - Writing of current phoenix version to file for reading via menu (needs refactored - sketchy)
 - Change money formatting in backend
 - Menus in admin area - very sketchy. Need to use KnpMenuBundle instead

AdminUserBundle
~~~~~~~~~~~~~~~
 - Extension of FOSUserBundle
 - Admin users log into the system - their actions can be logged
 - Can impersonate a customer
 - CRUD admin users in backend

AssemblaBundle
~~~~~~~~~~~~~~
.. error:: DELETE THIS

CacheBundle
~~~~~~~~~~~
 - Ability to define caches via configuration
 - Caches can have various backends (e.g redis)
 - Can differ by market/local
 - Good for storing html snippets that are expensive to generate
 - Twig integration to allow portions of twig templates to be cached
 - Some aspects of this bundle are candidates for components

CartBundle
~~~~~~~~~~
 - Cart stores products against a session (user adds items that they want to buy)
 - Cart items can be decorated with tax information
 - Provides default templates for rendering a cart and minicart
 - Some aspects of this bundle are candidates for components

CheckoutBundle
~~~~~~~~~~~~~~
 - Provides functionality for constructing multi-step checkout forms
 - Bridges with (has dependencies on) many other bundles
 - Bridges with payment bundle to make requests on - and process payments
 - Bridges with Order bundle to create orders after the checkout process has finished
 - Bridges with Courier bundles to offer delivery options, delivery estimates and costs
 - Bridges with Product(Stock) to adjust stock after checkout is complete
 
 
ClothingProductBundle
~~~~~~~~~~~~~~~~~~~~~
 - 'Clothing Products' are product with attributes.
 - Attributes are a mixture of 'entities' (hard coded), 'metadata' (configurable fields) and 'tags' (user controlled EAV)
 - Metadata can be inserted via spreadsheet upload/download
 - Tags can be managed via backend UI
 - Entites are used for more complicated concepts (e.g Size, Color), which require distinct management screens
 - Provides a product hierarchy (One style has many Articles, has many skus (Skus ⇔ ProductReference))
 - 'Articles' are the commonly referenced level of aggregation here.
 - Articles have state which affects which products can be viewed/bought etc.
 - Articles can be exported to search backends (e.g solr)
 - Delegates adding of products to carts
 - 'Feed' Generation of products
 - 'Stock' Notification
 - Facebook/Opengraph integration
 - Controllers for products
 - Routers and Url Generation for products
 - Redirect strategy for products based on state
 - BUNDLE IS PRIME CANDIDATE FOR EXTENSIVE REFACTOR/REDUCTION IN SCOPE

Content
~~~~~~~
 - Holds 'Contentful' Bundle - a bridge between phoenix and contentful bundles (a markup bundle for contentful)
 - Adds functionality to contentful bundle based on phoenix concepts (Product/Category Links/CDN-Media configuration etc)
 - Could this be done another way?

Courier
~~~~~~~
 - A bundle that hodls a variety of third party integrations to courier services (Dhl/Dpd/Memnon/Posten/Ups)
 - Core bundle (Courier Bundle) is the adapter between Phoenix and these other systems ('Plugins')
 - Provides shipping options to checkout
 - Models tracking information (parcel in transit at destination etc) for use primarliy with Click and Collect
 - Ability to form tracking numbers for parcels and form links (<a href="">) to third party systems to track parcels
 - Estimates arrival times for parcels for communication to customers in checkout and emails
 - Models Parcels and Shipments (Event containing one or more parcels). Parcels contain 'lineitems' which correspond to 'dispatchableLines' from the order bundle
 - Calculates which shipping options are relevant given the contents of a CART
 - Sends out emails on shipping arrival (move to email specific bundle)


CreditCardBundle
~~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT
  - This Models a credit card
  - Has a form definition (used in checkout? If so could be moved to payment bundle or checkout bundle)
  - Some aspects of this bundle could be moved to a component

CustomerBundle
~~~~~~~~~~~~~~
  - Extends FOSUSer to provide model for webshop customer
  - Customer CRUD. Customers can be tagged, Admin area for all of this.
  - Address Book for customer
  - Customer Sign in form
  - Email functions (account registration/password renewal etc.) - should be moved to dedicated email bundle
  - As per other bundles - a mishmash of translation files. WHERE SHOULD TRANSLATIONS BE MANAGED - either in the app/or the bundle - not both?!

DashboardBundle
~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT
  - Components and configuration for dashboard widgets
  - Could be merged with AdminBundle or more correctly integrated
  - Scope for code to be converted to components

DotMailerBundle
~~~~~~~~~~~~~~~
.. error:: SHOULD BE MADE A THIRD PARTY BUNDLE OR AN ADAPTER IN THE SUBSCRIBERBUNDLE
  - Provides integration of 'subscribers', 'customers' to dotmailer

EmailBundle
~~~~~~~~~~~
  - Models events relating to emails, and some utility classes for decorating swiftmailer for use with third party systems like sendgrid.
  - Classes to aggregate email events together
  - Heavy refactoring to occur here, moving functionality from other bundles and allowing fuller decoupling of email from other domains

EventBundle
~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/MOVING OF FUNCTIONALITY TO ANOTHER BUNDLE
  - This bundle has no well defined scope and should be merged with another bundle

FacebookOpenGraphBundle
~~~~~~~~~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR MOVEMENT TO THIRD PARTY/CONVERSION TO COMPONENT
  - This would make a good candidate for a component
  - The D/I configuration should be moved from core to a third party bundle

FeatureBundle
~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/MOVING OF FUNCTIONALITY TO ANOTHER BUNDLE
  - This bundle provides an extension allowing the system to check if a feature is enabled.
  - Idea is that other bundles register themselves as a feature which can then tested against before being used
  - This may not be necessary and could possibly be achieved by compiler passes

FeefoBundle
~~~~~~~~~~~
.. error:: CANDIDATE FOR MOVEMENT TO THIRD PARTY BUNDLE
  - Provides integration with Feefo (a third party product review provider)
  - Sends information to feefo when a package is shipped (which triggers an email to the customer to review the product)

FormFlowExtensionBundle
~~~~~~~~~~~~~~~~~~~~~~~
.. error:: CANDIATE FOR MERGE INTO CHECKOUT BUNDLE
  - Extends the form flow bundle
  - Used in checkout

FrameworkBundle
~~~~~~~~~~~~~~~
  - Extends Symfony framework, adding various functions and utility classes
  - Cache warming
  - Translation management
  - A Mixed bag - needs class by class analysis and a more well defined scope (although the bundle will still be required in some form)

GeocodeBundle
~~~~~~~~~~~~~
  - Provides phoenix specific functions relating to geocode
  - Relies on component: http://geocoder-php.org/
  - The above should be market as core component dependency


GiftCardBundle
~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/MOVING OF FUNCTIONALITY TO ANOTHER BUNDLE (Payment/Checkout)
  - Implementation of ProductReference
  - Majority of functionality now provided by ClothingProductBundle
  - Hooks in to Checkout/Payment to provide ability to pay by credit card

H5BPBundle
~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION
  - Provides configuration of Html5Boilerplate
  - Similar functionality to https://github.com/Oryzone/OryzoneBoilerplateBundle
  - Candidate to open source or convert sites to using above existing community bundle
  - CANDIDATE FOR DELETION

InvoiceBundle
~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION
  - Provides ability to configure invoices (templates) for use in admin area
  - Generation of PDF documents related to shipping and order invoices
  - Linked to Courier/Order Bundle
  - Could have majority/all code moved to those bundles

MailChimpBundle
~~~~~~~~~~~~~~~
.. error:: SHOULD BE MADE A THIRD PARTY BUNDLE OR AN ADAPTER IN THE SUBSCRIBERBUNDLE
  - Provides integration of 'subscribers', 'customers' to mailchimp

MarkdownEditingBundle
~~~~~~~~~~~~~~~~~~~~~
  - Provides ability to manage markdown via the database
  - Markdown can be included in templates and edited via the backend
  - Requires additional work (caching management, previewing and addition of javascript Markdown editor)
  - Should have API added to make moving markdown content between environments easier

MarketBundle
~~~~~~~~~~~~
  - Majority of code to be moved to component
  - Remaining code will configure this component and provide services for use in other bundles
  - Provides controllers for switching current 'languageLocale' which sets cookies used to select language
  - A core concept and important core bundle

MoneyBundle
~~~~~~~~~~~
  - Majority of code to be moved to component
  - Remaining code will configure this component and provide services for use in other bundles
  - A core concept and important core bundle

MonitoringBundle
~~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT
  - Sends email (notifications) on system events
  - Could be converted to component if it offers some functionality not already provided elsewhere
  - Used by only one function currently (Order bundle notifies that there have been no orders in the last period of time - suggest this function could be moved to order bundle using anything in this bundle via a component)

MutexBundle
~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT
  - Provides a disk based Mutex system which is used by some other bundles
  - Should be converted to component and eventually phased out (disk based mutex not that useful for our infrastructure)

OrderBundle
~~~~~~~~~~~
  - Sprawling bundle
  - Candidate for conversion of some code to component
  - Remaining code should bridge in 
  - Creation and management of orders
  - Searching of orders based on denormalized 'status' table
  - Show order history to customers
  - Bridges to Payment/Checkout/Courier/Customer/Invoicing
  - Picking batches for use in fulfillment (this should be abstracted to WMStype bundle)
  - RMA Management
  - Sends emails (lots of them) around fulfillment. Should be moved to email bundle
  - Provides 'OrderSecurity' Layer, which needs to be abstracted. This service is used extensively to control user access, and control which functionality should be available based on system modeled on Symfony Security (e.g voters and strategies)

OrderSecurityReviewBundle
~~~~~~~~~~~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT & MOVING FUNCTIONALITY TO OTHER BUNDLES

  - Provides structure for adding security rules around orders
  - Orders are put into security review depending on various factors
  - provides structure for other bundles to add security rules (via third parties or information added by other bundles)
  - Candidate for component
  - Needs additional tests
  - Could/Should be moved to Order Bundle (reluctant to add more code to that bundle until it itself has been refactored)

Payment
~~~~~~~
  - Provides structure to take payments via third party payment services
  - Uses JMSPaymentCoreBundle as do all existing plugins
  - Core Bundle here is 'PaymentPaymentBundle'
  - Interacts with Order and Checkout, bridges to third party JMSPaymentCore via Bridging entity 'PaymentInstructionBridge'

ProductBundle
~~~~~~~~~~~~~
  - Provides core entity and interfaces for product
  - Common point of reference between bundles referencing products (avoiding reliance on ClothingProductBundle)
  - Handles stock and pricing
  - Provides interfaces for 'ProductViews'
  - Large scope for converting aspects to components, splitting out stock and pricing into separate bundles if configuration of those components is required
  - Handling of customer subscription on stock events (should be removed from here)
  - Logs changes to stock to allow tracking of stock via events
  - Interfaces for accessing product images and building of collections of product images
  - Price Formatters
  - Core loaders (to be removed and refactored as model layer commands)
  - Bleeding of concerns into ClothingProductBundle should be removed
  - Bleeding of concerns into Shipping MUST be removed
  - Wide scope for components to be created (Product, Stock, Price, Pricing and Tax)
  - Resolvers relating to configuration of system. E.G 'What price group should the customer be shopping in', 'What stock should be being sold from', based on current site (Market/Domain) and other factors

ProductCatalogBundle
~~~~~~~~~~~~~~~~~~~~
  - Handles categorization of products
  - Creation and management of filters and facets based on attributes of ClothingProductBundle
  - Strong dependency on ClothingProductBundle (Not necessarily a problem)
  - Strong dependency on search backend (current Solr) via Needle and NeedleBundle
  - Caching of which products are in which categories via Redis (reverse category lookup)
  - Formation of breadcrumbs

ProductImportBundle
~~~~~~~~~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/CONVERSION TO COMPONENT & MOVING FUNCTIONALITY TO OTHER BUNDLES
  - Base classes for ETL type Importing operations
  - Overly complicated with no documentation
  - This will soon not be required by any client - suggest we remove this at that time.

ProductPromotionBundle
~~~~~~~~~~~~~~~~~~~~~~
  - Allows setup of promotions, which change the price charged against cart line items
  - Can only set up promotions via CLI, GUI is required
  - Poor test coverage
  - Complicated/Buggy interactions with checkout and cart - related to the retention of state
  - Bundle is mis-named, should just be 'PromotionBundle'
  - Promotion classes should be moved to component
  - Requires documentation to allow continued development

RedisBundle
~~~~~~~~~~~
.. error:: CANDIDATE FOR DELETION/MOVE FUNCTIONALITY TO OTHER BUNDLES
  - Sets up and configures cache spaces in Redis (Move to Cache Bundle)
  - Setup of Redis for caching doctrine results (Can we use the now native Doctrine functionality to do this instead)
  - Commands for flushing redis caches (Move to Cache Bundle)

ReportingBundle
~~~~~~~~~~~~~~~
  - Framework for building up data to be used in reports
  - Heavily relies on MySQL Backend (Doctrine specific extensions extend DQL)
  - Allows Reporting 'Facts' to be generated by 'Builders' registered in other bundles
  - Reports can be specified via YAML and these reports can be accessed through well defined interface
  - ReportViews can be adapted for use in tables, exported to spreadsheets or form graphs (or json used by Javascript Charting plugins)
  - Currently Broken due to introduction of Market/PriceIdentity Facets that don't work

ShopBundle
~~~~~~~~~~
.. error::  CANDIDATE FOR DELETION/MOVE FUNCTIONALITY TO ANOTHER BUNDLE (this is a weak recommendation and needs discussion)
  - Allows configuration of templates being used on frontend of the site
  - Market based configuration of customer services information (contact details for customer services)
  - This isn't necessary?
  - Sends Emails... Should be moved.
  - Provides a twig environment for use in the frontend namespace (could this be moved to another bundle)

SitemapBundle
~~~~~~~~~~~~~
  - Can generate sitemap in XML, TXT or HTML
  - Providers of sitemap data are registered in and then sitemap can be formed using these providers
  - Handles saving and serving of these sitemaps (via gaufrette).
  - The majority of this bundle should be moved to Component, and the 'providers' moved to more relevant bundles (Catalog/ClothingProduct)

StatsBundle
~~~~~~~~~~~
.. error::  CANDIDATE FOR DELETION
  - Sends stats to third party StatsD service
  - Not Used
  - CANDIDATE FOR DELETION

StoreDirectoryBundle
~~~~~~~~~~~~~~~~~~~~
  - Models a store directory
  - Stores are used in checkout (Click and Collect) and information pages (store opening hours)
  
SubscriptionsBundle
~~~~~~~~~~~~~~~~~~~
  - Models newsletter subscriptions (independent of customers)
  - Links to customer bundle 
  - Customers can log in and modify preferences for information they are interested in (EAV)
  - These subscriptions can be synced to third party providers (dotmailer and mailchimp)
  - Third parties should be modeled as adapters for use by this bundle

SymfonyConfigurationBundle
~~~~~~~~~~~~~~~~~~~~~~~~~~
.. error::  CANDIDATE FOR DELETION/MOVE FUNCTIONALITY TO ANOTHER BUNDLE
  - Has one function currently (flushing doctrine cache before console clear), to avoid errors during deployment

TaxBundle
~~~~~~~~~
  - Models calculation of tax information
  - Tax information can be modeled simply in Phoenix, more complicated modeling (e.g US tax) can be provided by third party systems
  - Tax information from Product Bundle should be moved to a component and used by this bundle

Tracking
~~~~~~~~
  - Ability to configure 'snippets' of javascript from third party systems
  - Snippets can be configured and rendered into templates based on environment configuration
  - All sub-bundles here need to be moved to separate repositories?
  - sub-bundles have varying degrees of quality, some have been constructed quite incorrectly and require refactoring

TwigProfilerBundle
~~~~~~~~~~~~~~~~~~
.. error::  CANDIDATE FOR DELETION
  - Forward compatibility of symfony 2.7 twig profiling in later versions
  - Can be removed when system dependency bumped to symfony 2.7

UtilBundle
~~~~~~~~~~
  - A variety of utility classes with no better place to put them
  - Many of these are candidates for components

