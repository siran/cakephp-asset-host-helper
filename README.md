# CakePHP 2.x CDN/Clould Front helper (domain sharding)

Inspired by:
[Teknoid's cakephp asset host helper](https://github.com/teknoid/cakephp-asset-host-helper)

## Background

This helper split web page resources across multiple domains (to make pages load faster).

## Installation

Clone/Copy the files in this directory into `app/Plugin/AssetHost`.

Then make sure you load the plugin:

```php
// in app/Config/bootstrap.php
CakePlugin::load('AssetHost');
```

## Configuration

Load the helper in your Controller:

```php
public $helpers = array(
    'AssetHostHelper.Cf' => array(
        'assetHost' => 's%d.example.com',
        'numHostsMin' => 0,
        'numHostsMax' => 2,
        'sslHost' => 'www.example.com'
    )
);
```

### Options

    `
    - assetHost :

        Where are the assets hosted?
        Possible options: 'assets.example.com', if you only have one host
        Or: 'assets%d.example.com', if you have multiple hosts. %d gets replaced with host number

    - numHostsMin & numHostsMax

        If above is 'assets%d.example.com' will generate host names from 0 - 3
        i.e. assets0.example.com

    - sslHost

        Serving assets via SSL is slow, let's use a unique host (for better caching)

    - imgDir

        Where are the images relative to web root (local should mirror remote)
        Try to stick to cake conventions.

    - jsDir
        Where are the JS files relative to web root (local should mirror remote)
        Try to stick to cake conventions.

    - cssDir
        Where are the CSS files relative to web root (local should mirror remote)
        Try to stick to cake conventions.

    - assetDir

        Will set asset directory depending on the asset type (css, js, img)

    - forceTimestamp

        We should really force the timestamp to improve caching.
        Trun on the option in core.php

    - remoteCompressedFiles

        If Cloudfront origin is S3 then we can supply the user
        with compressed files if we add them to S3 first and then
        change the file that is requested.
        To make this work we need a gzip version of each css and
        js file in the format cake.gz.css.
        If this is turned on the app will check that the agent
        accepts gzip encoded files and will server the gz version
        instead.
        This feature is not required if the origin of cloudfront
        is your website.

        Thx to redthor ! (https://github.com/redthor)
    `

## Usage

Use it in your views like Html Helper, ex :

```php
echo $this->Cf->image('logo.png', array(
        'width'=> '150',
        'height'=> '50',
        'alt'=> 'alternative text',
        'url' => array(
            'controller' => 'examples',
            'action' => 'index',
        )
    )
);
```

All options are same as default CakePHP helpers.
