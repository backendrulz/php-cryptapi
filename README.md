![CryptAPI](https://i.imgur.com/IfMAa7E.png)

# CryptAPI's PHP Library
Official PHP library of CryptAPI

## Requirements:

```
PHP >= 5.6
ext-curl
```



## Install


```
composer require cryptapi/php-cryptapi
```

[on GitHub](https://github.com/cryptapi/php-cryptapi) &emsp;
[on Composer](https://packagist.org/packages/cryptapi/php-cryptapi)

## Usage

### Generating a new address

```php
<?php
require 'vendor/autoload.php'; // Where your vendor directory is

$ca = new CryptAPI\CryptAPI($coin, $my_address, $callback_url, $parameters, $cryptapi_params);
$payment_address = $ca->get_address();
```

Where:

``$coin`` is the coin you wish to use, from CryptAPI's supported currencies (e.g `'btc', 'eth', 'erc20_usdt', ...`)

``$my_address`` is your own crypto address, where your funds will be sent to  

``$callback_url`` is the URL that will be called upon payment

``$parameters`` is any parameter you wish to send to identify the payment, such as `['order_id' => 1234]`

``$cryptapi_params`` parameters that will be passed to CryptAPI _(check which extra parameters are available here: https://cryptapi.io/docs/#/Bitcoin/btccreate)_

``$payment_address`` is the newly generated address, that you will show your users


### Getting notified when the user pays

The URL you provided earlier will be called when a user pays, for easier processing of the request we've added the ``process_callback`` helper

```php
<?php

require 'vendor/autoload.php'; // Where your vendor directory is

$payment_data = CryptAPI\CryptAPI::process_callback($_GET);
```

The `$payment_data` will be an array with the following keys:

`address_in` - the address generated by our service, where the funds were received

`address_out` - your address, where funds were sent

`txid_in` - the received TXID

`txid_out` - the sent TXID or null, in the case of a pending TX

`confirmations` - number of confirmations, or 0 in case of pending TX

`value` - the value that your customer paid

`value_coin` - the value that your customer paid, in the main coin denomination (e.g `BTC`)

`value_forwarded` - the value we forwarded to you, after our fee

`value_forwarded_coin` - the value we forwarded to you, after our fee, in the main coin denomination (e.g `BTC`)

`coin` - the coin the payment was made in (e.g: `'btc', 'eth', 'erc20_usdt', ...`)

`pending` - whether the transaction is pending, if `false` means it's confirmed

plus, any values set on `$params` when requesting the address, like the order ID.

&nbsp;

From here you just need to check if the value matches your order's value.


### Checking the logs of a request

```php
<?php

require 'vendor/autoload.php'; // Where your vendor directory is

$ca = new CryptAPI\CryptAPI($coin, $my_address, $callback_url, $parameters);
$data = $ca->check_logs();
```

Same parameters as before, the `$data` returned can be checked here: https://cryptapi.io/docs/#/Bitcoin/btclogs


## Help

Need help?  
Contact us @ https://cryptapi.io/contact/