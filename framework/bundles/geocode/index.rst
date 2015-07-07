.. _phoenix-bundle-geocode-index:
############
GeocodeBundle
############

The GeocodeBundle allows for an IP address lookup using a third party library GeoCoder in Phoenix although provides an interface to allow any library to be used.

Introduction
~~~~~~~~~~~~

The bundle has one controller which is used to look up the client IP address against a third party IP-to-country resolver, once the country is known it uses the MarketProvider to find a suitable market and calls ``getSuggestedMarketResponse(MarketInterface $market)``, if none are found the response method called will be ``getUnknownMarketResponse()`` anything else will be ``getDefaultResponse()`` 

Configuration
~~~~~~~~

There are five configuration options within the bundle of which none are required, the important configuration is the ``enabled`` which is a boolean to enable or disable it (the default is disabled). The default resolver used is the ``Geocoder`` which wraps a third party library and has two configuration options found within the ``parameters.yml.dist``

.. code-block:: yml

    // example config.yml
    phoenix_geocode:
        enabled: true

.. code-block:: yml

    // example parameters.yml
    maxmind_user_id: 123456
    maxmind_api_key: wiggle

The ``GeoCoderService`` can filter based on user agent with a configuration ``useragent_blacklist``, this is useful to prevent bots wasting API calls

.. code-block:: yml

    // example config.yml
    phoenix_geocode:
        enabled: true
        useragent_blacklist:
        - Googlebot
        - Bingbot
        - Baiduspider
        - 'Yahoo! Slurp'
        - 'CrazyEgg'

``cookie_expiry`` will be used in the default responses and returned to the frontend for use with creating a cookie

.. code-block:: yml

    // example config.yml
    phoenix_geocode:
        enabled: true
        cookie_expiry: 2592000

Development Configuration
~~~~~~~~

There is two configuration values which are very useful within a development environment;

``test_country`` which will cause the ``GeocodeServiceTestMode`` to be used which skips the API call to the resolver and uses the country specificed in the configuration. This should almost always be used as we don't want to waste allocation on any API. 

.. code-block:: yml

    // your config.yml
    phoenix_geocode:
        enabled: true
        test_country: AU

``test_ip`` will override the ``Request`` object and use it within the ``GeocodeService`` rather than the ``Request->getClientIp()`` (note on the VM your client IP won't be a WAN IP)

.. code-block:: yml

    // your config.yml
    phoenix_geocode:
        enabled: true
        test_ip: 8.8.8.8

Use within an application
~~~~~~~~

The ``WidgetController`` has 3 protected methods which should be extended to customise the response 

.. code-block:: php
    protected function getDefaultResponse()
    {
        //...
    }

    protected function getUnknownMarketResponse()
    {
        //...
    }

    protected function getSuggestedMarketResponse(MarketInterface $market)
    {
        //...
    }


Cookie aware
~~~~~~~~

The ``WidgetController`` is used to provide an easy ajax call point from the frontend of the application, its aware of a cookie which has the default name of ``phoenix_skip_geocode`` if this cookie is present it skips all logic and returns early. Therefore if you want to control the frequency of the API calls the frontend can create a cookie.
