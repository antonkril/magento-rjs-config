# magento-rjs-config

r.js build file for Magento 2.2.0-rc30 CE Luma theme.

*NOTE*: manual modification of build file for your project IS NOT REQUIED. Major benefit will be achieved by any blank or luma-based installation with this build configuration without modifications. Tweaking it is optional, if you want to achieve 100% improvements.

Magento 2 built-in bundler relies only on php environment, so it is not very efficient.

The build file from this repo can be used to execute budnling and minification on deployed static content with R.js tool. It will optimize main pages of Magento 2.

Should be used only during deployment to production.

## Usage
* Install [r.js](http://requirejs.org/docs/optimization.html)
* Download `build.js` from this repo
* Edit `build.js` to remove/add files from your custom theme to bundles
* Run `magento setup:static-content:deploy` to deploy Magento 2 static content to `{magentoDir}/pub/static/` folder
* For every theme locale:
  * Move `{magentoDir}/pub/static/{vendor}/{theme}/{locale}` folder to `{magentoDIr}pub/static/{vendor}/{theme}/{locale}_source`
  * Run `r.js -o build.js baseUrl={magentoDir}/pub/static/{vendor}/{theme}/{locale}_source dir={magentoDir}/pub/static/{vendor}/{theme}/{locale}`

## Output
* Bundles all files common for all Magento 2 Luma storefront pages into `requirejs/require.js` file
* Generates 5 page-specific bundle files:
  * `bundles/common.js` - should be added to `default` layout handle
  * `bundles/cart.js` - should be added to `checkout_cart_index` layout handle
  * `bundles/checkout.js` - should be added to `checkout_index_index` layout handle
  * `bundles/catalog.js` - should be added to `catalog_category_view` and `catalog_product_view` layout handles
  * `bundles/product.js` - should be added to `catalog_product_view` layout handle

### Adding bundles to handle
To add a bundle file to corresponding layout handle, add following instruction to the handle layout file in your module:
```xml
  <head>
    <script src="bundles/{bundleFile}.js" />
  </head>
```

Example for cart page
```xml
// My/Module/view/frontend/layout/checkout_cart_index.xml
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
