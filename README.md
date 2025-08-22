[![add-on registry](https://img.shields.io/badge/DDEV-Add--on_Registry-blue)](https://addons.ddev.com)
[![tests](https://github.com/iljapolanskis/ddev-buggregator/actions/workflows/tests.yml/badge.svg?branch=main)](https://github.com/iljapolanskis/ddev-buggregator/actions/workflows/tests.yml?query=branch%3Amain)
[![last commit](https://img.shields.io/github/last-commit/iljapolanskis/ddev-buggregator)](https://github.com/iljapolanskis/ddev-buggregator/commits)
[![release](https://img.shields.io/github/v/release/iljapolanskis/ddev-buggregator)](https://github.com/iljapolanskis/ddev-buggregator/releases/latest)

# DDEV Buggregator

## Overview

This add-on integrates [Buggregator](https://github.com/buggregator/server) into your [DDEV](https://ddev.com/) project. Buggregator is a powerful debugging server that helps with application debugging and monitoring by collecting logs, profiling data, and other debugging information.

## Installation

```bash
ddev add-on get iljapolanskis/ddev-buggregator
ddev restart
```

After installation, make sure to commit the `.ddev` directory to version control.

## Usage

| Command | Description |
| ------- | ----------- |
| `ddev describe` | View service status and used ports for Buggregator |
| `ddev logs -s buggregator` | Check Buggregator logs |

### Accessing Buggregator UI

Open `https://mysite.ddev.site:8000` in your browser to access the Buggregator web interface.

Additional information on using Buggregator can be found in the [Buggregator documentation](https://github.com/buggregator/server#configuration).

## Configuration

You can customize the Buggregator service by setting environment variables in your `.ddev/.env` file:

```bash
# Buggregator image version
BUGGREGATOR_IMAGE=ghcr.io/buggregator/server:latest

# Port configurations
BUGGREGATOR_HTTP_PORT=8000
BUGGREGATOR_SMTP_PORT=1025
BUGGREGATOR_VAR_DUMPER_PORT=9912
BUGGREGATOR_MONOLOG_PORT=9913
BUGGREGATOR_SENTRY_PORT=9511
BUGGREGATOR_RAY_PORT=23517
```

After making changes, restart DDEV:

```bash
ddev restart
```

## Configuration Options

| Variable | Default | Description |
| -------- | ------- | ----------- |
| `BUGGREGATOR_IMAGE` | `ghcr.io/buggregator/server:latest` | Docker image version to use |
| `BUGGREGATOR_HTTP_PORT` | `8000` | HTTP port for web interface |
| `BUGGREGATOR_SMTP_PORT` | `1025` | SMTP port for email debugging |
| `BUGGREGATOR_VAR_DUMPER_PORT` | `9912` | Port for Symfony VarDumper |
| `BUGGREGATOR_MONOLOG_PORT` | `9913` | Port for Monolog handler |
| `BUGGREGATOR_SENTRY_PORT` | `9511` | Port for Sentry integration |
| `BUGGREGATOR_RAY_PORT` | `23517` | Port for Ray debugging tool |

## PHP Usage Example

Here's an example of how to send logs to Buggregator from within your PHP application:

First, install the required PHP library:

```bash
composer require monolog/monolog
```

Then use it in your code (playground.php file):

```php
<?php

require_once 'vendor/autoload.php';

use Monolog\Logger;
use Monolog\Handler\SocketHandler;
use Monolog\Formatter\JsonFormatter;

// Create a log channel
$logger = new Logger('myapp');

// Create socket handler for Buggregator
$handler = new SocketHandler('buggregator:9913');
$handler->setFormatter(new JsonFormatter());
$logger->pushHandler($handler);

// Send different log levels to Buggregator
$logger->info('Application started');
$logger->warning('This is a warning message');
$logger->error('An error occurred', ['user_id' => 123, 'action' => 'login']);
$logger->debug('Debug information', ['query' => 'SELECT * FROM users']);
```

Note: This example uses the default Buggregator service hostname (`buggregator`) which is accessible from within the DDEV environment.

```shell
ddev exec php playground.php
```

## Credits

**Contributed and maintained by [@iljapolanskis](https://github.com/iljapolanskis)**

This add-on uses the [Buggregator Server](https://github.com/buggregator/server) Docker image as the base for the debugging service.

