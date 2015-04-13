.. _phoenix-bundle-order-security-review-configuration:
#############
Configuration
#############

Security rules when registered as disabled by default. By adding a security rule to the overall application configuration it will automatically be enabled. Additionally whatever configuration has been defined will be passed to the rule (via the 'setConfiguration' method).

For example the following configuration will enable the rules 'your_cool_rule' and 'order_max_value', passing a simple configuration to the latter.

.. code-block:: yaml

    # config.yml
    phoenix_order_security_review:
        strategy: unanimous
        rules:
            - name: your_cool_rule
            - name: order_max_value
              config: {'value' : 250}


You can also configure your rule directly via the configuration for your bundle if required. As long as your security rule has been tagged correctly it will be picked up and registered. Hiowever it will not be enabled unless the 'enableRule' method has been called in the Security Review Service. This happens automatically if you configure your rule via the 'phoenix_order_secuerity_review' configuration tree. See \\Phoenix\\Bundle\\OrderSecurityReviewBundle\\DependencyInjection\\PhoenixOrderSecurityReviewExtension for more.