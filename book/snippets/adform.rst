.. _phoenix-snippet-adform:

Adform
------

This bundle allows configuration of Adform_ Tracking Snippets. In terms of structure and configurability, this bundle is a perfect example of how NOT to implement tracking snippets. Do not use this bundle as a reference for adding tracking snippets.

## Register the Bundle

.. code-block:: php

    // app/AppKernel.php
    
    public function registerBundles()
	{
	    $bundles = array(
	        // ...
	        new Phoenix\Bundle\Tracking\AdformBundle\PhoenixTrackingAdformBundle,
	    );
	}

## Add configuration

This bundle does not use semantic configuration, but instead uses parameters to set up the snippets. The following parameters are required after registering this bundle:

.. code-block:: yaml

    // app/parameters.yml

    # the id of the overall account
	adform_account_number: xxxx
	# the id of the conversion event on the homepage
	adform_homepage_conversion_id: xxxx
	# the id of the conversion event on the checkout
	adform_submit_checkout_conversion_id: 1901641
	# the id of the conversion event on the confirmation page
	adform_order_confirmation_conversion_id: 1901642
	# should the snippet be rendered
	adform_enabled: false


## Rendering Snippet

To render snippets for this bundle, the following twig extension:


Homepage
--------
.. code-block:: twig

    // app/Resources/views/home.html.twig

    {{ phoenix_adform_snippet('homepage') }}

Checkout Submit
--------
.. code-block:: twig

    // app/Resources/views/checkout_payment.html.twig

    <script>
    	$('form.main')
            .on('submit', function (e) {
               {{ phoenix_adform_snippet('submit_checkout')|raw }}
            });
    </script>

.. _Adform: http://site.adform.com/