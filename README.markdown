# FetLife Maltego

FetLife Maltego is a package of PHP-based [Local Transforms](https://www.paterva.com/web6/documentation/developer-local.php) for the [Maltego OSINT and forensics data mining tool](https://en.wikipedia.org/wiki/Maltego) that act on FetLife.com.

## Prerequisites

This tool requires PHP version 5.3 or greater, with [PHP's CURL extension](https://php.net/manual/en/book.curl.php) installed. Additionally, this tool requires two libraries:

* [libFetLife](https://github.com/meitar/libFetLife)

We'll get these in the next step.

## Installing and configuring

This section documents how to install and use FetLife Maltego.

### Installing and importing local transforms (recommended)

    git clone git://github.com/meitar/fetlife-maltego.git # Clone this code.
    cd fetlife-maltego
    git submodule init                                    # Install the libraries,
    git submodule update                                  # and then fetch them.
    cp fl-mt-config.ini.php-sample fl-mt-config.ini.php   # Create the config file.
    vi fl-mt-config.ini.php                               # Edit the config file.

Then, in Maltego:

* Maltego icon -> Import -> Import Configuration
    * Select `fetlife-maltego.mtz`, provided with this package.

### Configuring

Before you can use the FetLife Maltego local transforms, you have to tell it which FetLife account you want to use. By default, FetLife Maltego configures `libFetLife` library is already configured to auto-select a proxy server from which to contact FetLife.com.

FetLife Maltego looks for a configuration file named `fl-mt-config.ini.php` in the same directory as its main workhorse script, `FetLifeTransform.php`. The program ships with a sample configuration file called `fl-mt-config.ini.php-sample`. Rename or copy the sample file to the expected name, then enter your preferred settings.

The configuration file uses [PHP's `ini` file](http://php.net/parse_ini_file) syntax. Edit the config file in your favorite text editor and set values for the FetLife username and password you'll use to query FetLife.com.

## Command line use

All transforms can be run from the `FetLifeTransform.php` script. To choose a transform, use the `-t` short option or the equivalent `--transform` long option. For instance, to run the `friends` transform against the FetLife user `JohnBaku`, use:

    /usr/bin/php /path/to/FetLifeTransform.php --transform friends JohnBaku

Alternatively, invoke the `fetlifetransform-friends.php` script as follows for the same effect:

    /usr/bin/php /path/to/fetlifetransform-friends.php JohnBaku

This may take a little while if a user has a lot of "friends." :P

## Adding local transforms yourself (not recommended)

To run FetLife Maltego's transformations in your Maltego client, you first need to add them to your list of available transforms. Follow Paterva's instructions for [Adding a new transform](https://www.paterva.com/web6/documentation/developer-local.php#6). When adding a new transform in the Local Transform Wizard, be mindful of the following settings:

* In the `Input entity type` field, choose the entity you'd like to subject to a transform. FetLife Maltego currently has transforms for the following input entity types:
    * Alias
* In the **Command line** step, enter the following details, adjusted for your environment:
    * **Command**: `/usr/bin/php`
    * **Parameters**: `fetlifetransform-TRANSFORM_NAME.php`, where *TRANSFORM_NAME* is the transform you want to invoke.
    * **Working directory**: `/path/to/fetlife-maltego`

## Troubleshooting

* Ensure your `lib/FetLife` directory and its `fl_sessions` is read and writable by your user.
