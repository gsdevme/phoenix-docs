.. role:: bash(code)
:language: bash

Importing data from Magento
===========================

Import of data from magento must be run in stages.
 - download of customer data from magento to mongod db storage :bash:`gant:magento:import:customers`
 - dowload of customer details and addresses :bash:`rabbitmq:consumer asynchronous_commands`
 - load customer data to phoenix database :bash:`gant:magento:load:customers --domain fr`
 - load  customer addresses to phoenix database :bash:`gant:magento:load:customer:addresses --country FR`
 - download of orders :bash:`gant:magento:import:orders`
 - download of order addresses and order lines :bash:`rabbitmq:consumer asynchronous_commands`
 - load of order data :bash:`gant:magento:load:orders --domain fr --country FR`

Download of orders can run during dealing with customer (probably it makes sense to wait until download of customers is finished).
Loading of order data should be run when all customers are loaded to database.

Prerequisites
-------------
Make sure that mongoDB is installed, by creating indexes :bash:`./app/console doctrine:mongodb:schema:update`
and that all queues are set up :bash:`./app/console rabbitmq:setup-fabric`

Try to get access to rabbit mq console, make sure the same user/pass as in paramteres.yml is working
if tunnel is needed something like that might work
:bash:`ssh -i ./private_key -p 2222 vagrant@127.0.0.1 -L 15672:localhost:15672`
you can also use application called rabbitmqctl: :bash:`sudo rabbitmqctl`

Customers
---------

Download Customer Data
~~~~~~~~~~~~~~~~~~~~~~
First obtain all customer data from Magento instance.
:bash:`./app/console gant:magento:import:customers "2013-01-01" "2014-01-01"`
Don’t worry about importing the same customer twice - it will be omitted.

In the same time about 3 worker threads should run to fetch details and addresses
:bash:`./app/console  rabbitmq:consumer asynchronous_commands -w --no-debug`
Put customer data from mongo to phoenix  :bash:`./app/console gant:magento:load:customers --domain fr --limit=10`

Load customer addresses
~~~~~~~~~~~~~~~~~~~~~~~
Run :bash:`./app/console gant:magento:load:customer:addresses --country FR`
It can be run few minutes after customer load has started. In case that customer does not exist yet for particular address it will be postponed to be imported in another run (notice in log will be issued too).`

Orders
------

Download Order Data
~~~~~~~~~~~~~~~~~~~

It's good to login to magento admin of instance the data is pulled from to check the first order date and the overall amount. 
:bash:`./app/console gant:magento:import:order "2012-03-01" "2012-05-01"`

It’s probably better to download orders in smaller intervals, like 2 months

Make sure that workers are running
:bash:`./app/console  rabbitmq:consumer asynchronous_commands -w --no-debug`

Load order data
~~~~~~~~~~~~~~~

First, create placeholder product, which will be used when sku is not found :bash:`./app/console gant:magento:import:create_placeholder_product`
It creates product with sku configured in order_import.placeholderProductEan13 parameter.

Then run
:bash:`./app/console gant:magento:load:orders --domain fr --country FR --no-debug`
to put orders into the phoenix database
Update order statuses
When all customers and orders are loaded into phoenix db, run command:
:bash:`app/console phoenix:order:status:update_all --domain fr`
It is run for every order automatically now.
Troubleshooting
If anything goes wrong with download for single cutomer or order you can
issue a fetch command

```
./app/console gant:magento:import:issue_fetch_command FetchCustomerAddresses 22050
./app/console gant:magento:import:issue_fetch_command FetchCustomerInfo 22050
```

Roll back procedure
-------------------

Removing data from mongo ::

  db.MagentoCustomer.remove({});
  db.MagentoCustomerAddress.remove({});
  db.MagentoOrder.remove({});
  db.MagentoOrderLine.remove({});
  db.MagentoOrderAddress.remove({});

How many customers are already loaded? ::

  db.MagentoCustomer.find({"imported":true}).count();

If roll back is required, because for any reason the loaded data is corrupt we can remove orders.

:bash:`./app/console gant:magento:r:orders --domain fr -l 1000`

This commands iterates through orders in mongo marked as imported and does removal action in phoenix database (searches order by reference)

It is also possible to remove single order by reference
:bash:`./app/console gant:magento:r:orders --domain fr -r mage_fr_100009007`

