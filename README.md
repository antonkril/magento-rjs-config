# magento-rjs-config

**NOTE** Magento is working on an external bundling solution with asynchronous bundle loading. This configuration will be deprecated once this solution arrives.

This `r.js` build file is built for Magento 2.2.0-rc30 CE Luma theme.

**NOTE**: Manual modification of the `build.js` file for your project IS NOT REQUIRED. A major benefit will be achieved by any `Magento/blank` or `Magento/luma`-based installation with this build configuration without the need for any modifications. Tweaking the `build.js` file is optional if you want to achieve 100% improvements.

The Magento 2 built-in bundler relies only on PHP, which is not very efficient and could lead to potential issues. The build file `build.js` from this repository can be used to execute bundling and minification on deployed static content with the `r.js` tool shipped with the NPM version of RequireJS. It will optimize the main pages of Magento 2.

The process described here should be applied only during deployment to production.

## Usage
* Install [r.js](http://requirejs.org/docs/optimization.html)
* Download `build.js` from this repo
* Edit `build.js` to remove/add files from your custom theme to bundles (optional)
* Run `magento setup:static-content:deploy` to deploy Magento 2 static content to `{magentoDir}/pub/static/` folder
* For every theme locale that you use:
  * Move `{magentoDir}/pub/static/{area}/{vendor}/{theme}/{locale}` folder to `{magentoDIr}pub/static/{area}/{vendor}/{theme}/{locale}_source`
  * Run `r.js -o build.js baseUrl={magentoDir}/pub/static/{area}/{vendor}/{theme}/{locale}_source dir={magentoDir}/pub/static/{area}/{vendor}/{theme}/{locale}`

## Output
* Bundles all files common for all Magento 2 Luma storefront pages into `requirejs/require.js` file
* Generates 5 page-specific bundle files:
  * `bundles/default.js` - should be added to `default` layout handle
  * `bundles/cart.js` - should be added to `checkout_cart_index` layout handle
  * `bundles/checkout.js` - should be added to `checkout_index_index` layout handle
  * `bundles/catalog.js` - should be added to `catalog_category_view` and `catalog_product_view` layout handles
  * `bundles/product.js` - should be added to `catalog_product_view` layout handle

### Adding bundles to pages
To add a bundle file to the corresponding page, add following instruction to the page layout update file in your module:
```xml
  <head>
    <script src="bundles/{bundleFile}.js" />
  </head>
```

Example for cart pages, create a file `My/Module/view/frontend/layout/checkout_cart_index.xml` with the following contents:
```xml
<?xml version="1.0"?>
<page xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <head>
        <script src="bundles/cart.js" />
    </head>
</page>
```
### Fixing missconfiguration of jquery.cookie
JQuery.cookie module should be added to 'deps'. To add, create a 'requiejs-config.js' file in your module:
```javascript
//My/Module/view/frontend/requirejs-config.js
var config = {
    'deps': ['jquery/jquery.cookie']
};
```
