[![tests](https://github.com/iljapolanskis/ddev-buggregator/actions/workflows/tests.yml/badge.svg)](https://github.com/iljapolanskis/ddev-buggregator/actions/workflows/tests.yml)
![Project is maintained](https://img.shields.io/maintenance/yes/2024.svg)

# ddev-buggregator <!-- omit in toc -->

DDEV add-on for [Buggregator](https://github.com/buggregator/server) – a debugging utility that aggregates logs from various PHP tools like Monolog, Symfony VarDumper, Laravel Telescope, and more.

---

## Table of Contents

* [What is ddev-buggregator?](#what-is-ddev-buggregator)
* [Installation](#installation)
* [Usage](#usage)
* [Example: Using with Monolog in Magento 2](#example-using-with-monolog-in-magento-2)
* [Troubleshooting](#troubleshooting)

---

## What is ddev-buggregator?

This project integrates [Buggregator](https://github.com/buggregator/server) as a DDEV add-on service.
It enables capturing debugging data from your PHP applications directly in your local development environment.

---

## Installation

### For DDEV v1.23.5 and newer

```bash
ddev add-on get iljapolanskis/ddev-buggregator
```

### For earlier versions of DDEV

```bash
ddev get iljapolanskis/ddev-buggregator
```

After installation, restart DDEV to ensure the Buggregator service is up and running:

```bash
ddev restart
```

---

## Usage

Once installed and running, Buggregator is available at:

```
http://<your-project>.ddev.site:8000
```

Replace `<your-project>` with your actual DDEV project name.

Refer to the [Buggregator documentation](https://github.com/buggregator/server#configuration) for more advanced configuration options.

---

## Example: Using with Monolog in Magento 2

Buggregator supports Monolog via a socket connection on port `9913`. Below is a sample script demonstrating how to send logs from Magento 2.

### PHP Script Example

```php
<?php

require __DIR__ . '/../app/bootstrap.php';

use Monolog\Logger;
use Monolog\Handler\SocketHandler;
use Monolog\Formatter\JsonFormatter;

// Create a logger instance
$logger = new Logger('buggregator');
$handler = new SocketHandler('buggregator:9913'); // 'buggregator' is the service name in DDEV
$handler->setFormatter(new JsonFormatter());

$logger->pushHandler($handler);

// Send a test warning
$logger->warning('Hello from Monolog to Buggregator!');
```

### Magento 2 Integration Example

Place the following code inside any Magento 2 class or controller where you want to log data:

```php
$logger = new \Monolog\Logger('buggregator');
$handler = new \Monolog\Handler\SocketHandler('buggregator:9913');
$handler->setFormatter(new \Monolog\Formatter\JsonFormatter());

$logger->pushHandler($handler);
$logger->warning('This is a test warning from Magento 2.');
```

💡 **Tip**: Make sure the Buggregator container is running and that port `9913` is open internally (which it is by default in this DDEV add-on).

---

## Troubleshooting

* **Cannot connect to Buggregator:** Ensure the DDEV project is running and the Buggregator service is listed with `ddev describe`.
* **Port conflicts:** Port 8000 may conflict with other services. You can modify the port in `.ddev/docker-compose.buggregator.yaml` if necessary.
  
---

## Contributing

Feel free to open issues or PRs
