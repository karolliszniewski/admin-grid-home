# admin-grid-home

# 1. Register module

file `app/code/Aware/Checkout/registration.php`

```php
<?php

use Magento\Framework\Component\ComponentRegistrar;

ComponentRegistrar::register(
    ComponentRegistrar::MODULE,

    // The name of the module we're registering
    'Aware_Checkout',
    __DIR__
);

```

file `app/code/Aware/Checkout/etc/module.xml`
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Module/etc/module.xsd">
    <module name="Aware_Checkout" setup_version="1.0.0"/>
</config>
```

First, in the PHP file, we inform Magento that our module exists. Then, in the XML file, we specify the module's version and can also define any dependencies.





