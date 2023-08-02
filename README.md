[![tests](https://github.com/iljapolanskis/ddev-buggregator/actions/workflows/tests.yml/badge.svg)](https://github.com/ddev/ddev-buggregator/actions/workflows/tests.yml) ![project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-buggregator <!-- omit in toc -->

* [What is ddev-buggregator?](#what-is-ddev-buggregator)
* [How to Use](#how-to-use)

## What is ddev-buggregator?

This repository implements [Buggregator](https://github.com/buggregator/server) as a DDEV add-on service.

Can be installed from the command line using the 
```sh
ddev get iljapolanskis/ddev-buggregator
```

## How to use

Open the `mysite.ddev.site:8000` to see the Buggregator UI. Additional info on how to use Buggregator can be found in the [Buggregator documentation](https://github.com/buggregator/server#configuration)

At the moment I tested the `monolog` only (used in Magento 2 by default). Snippet on how to use it in a single file in Magento 2:

```php
<?php

require __DIR__ . '/../app/bootstrap.php';

use Monolog\Logger;
use Monolog\Handler\SocketHandler;
use Monolog\Formatter\JsonFormatter;

// Create a log channel
$log = new Logger('buggregator'); // Any name you want
$handler = new SocketHandler('buggregator:9913'); // The name of the service and the port, by default 9913 for monolog
$handler->setFormatter(new JsonFormatter());
$log->pushHandler($handler);

// Send records to the Buggregator
$log->warning(print_r($handler, true));
```

