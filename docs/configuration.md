---
title: Configuration
date: 2018-09-15 07:42:34
slug: configuration
---

## Requirements

The requirements are:

- PHP 7.1
- Composer

## Installation

To start using this package, first it's necessary clone the repository.

```bash
git clone https://github.com/freeswitch-xml-php
```

After this you need to cd in you project and run composer:

```bash
cd freeswitch-xml-php
composer install
```

## Configuration

To get the library working, first we need to set-up some variables.

In the file `bootstrap.php`, it's necessarily set the database variables, you need to define:

- host
- database
- username
- password

Then it's possible to set-up a logger and the error handler.

## Usage

To use the library, it's necessary setup a webserver that redirects the traffic to the file `index.php`. The file `index.php` it's going to receive all the requests and it's going to process the requests connecting to the database and returning an xml file.

To see what sections are available in the package go to [#Available Sections](#available-sections).

### Freeswitch Configuration

To get this working, you need to enable in Freeswitch the module `mod_xml_curl` and then edit the file `autoload_configs/xml_curl.conf.xml` and set the following lines:

```xml
<configuration name="xml_curl.conf" description="cURL XML Gateway">
  <bindings>
    <binding name="localhostweb"> 
      <param name="gateway-url" value="http://host/index.php" bindings="directory|dialplan"/>
      </binding>
  </bindings>
</configuration>
```

You need to change the variable host for your IP Address or domain.

Also it's necessary edit the folders dialplan and directory, disabling the `.xml` files to avoid that Freeswitch loads the configuration from the files instead that the database.

## Available sections

A section it's a functionality that it's available to use, to see the full list go to the [Freeswitch Webpage](https://freeswitch.org/confluence/display/FREESWITCH/mod_xml_curl)

The available sections in the library are:

- dialplan
- directory


## Functions

There are some functionality that it's available to use:

### Logger

In the `bootstrap.php` file, you can set-up the Logger. By default the logger it's setted and it's store the log information in the `logs` folder.

### Store and Debug XML

When the library runs it's generate in the end a xml file. There is a function that it's called `storeXml`:

```php
$director = new \FreeswitchXmlPhp\XmlBuilder\RunDirector();
$director->run($_REQUEST);
$director->builder()->storeXml('fs-curl-request-examples/');
```

The function `storeXml()` by default store the output in the `/tmp/` folder. In the example above, it's going to store the xml in the `fs-curl-request-examples/` folder.

This function it's very useful for debugging purposes. If you need to know what it's the XML that the library builds, set-up the function and you can see the XML that the library serves to Freeswitch.


## Exceptions
The package throws exceptions accordingly the state of the application.

All the exceptions are in the namespace:

```php
namespace FreeswitchXmlPhp\Exceptions
```

### SectionException

You are gone to receive this exception in this cases:

```bash
Section cannot be empty
```

This message happen when in the request from Freeswitch to the package, the section it's empty.

```bash
Invalid Section "section name" doesn't exist
```

This message happen when in the request from Freeswitch to the package, the section doesn't exist.

For more information see [#Available Sections](#available-sections)