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

Lets create table in database

file `app/code/Aware/Checkout/etc/db_schema.xml`

```xml
<?xml version="1.0"?>
<schema xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:noNamespaceSchemaLocation="urn:magento:framework:Setup/Declaration/Schema/etc/schema.xsd">
    <table name="your_table_name" resource="default" engine="innodb" comment="Your Table Comment">
        <column xsi:type="int" name="id" padding="10" unsigned="true" nullable="false" identity="true" comment="ID"/>
        <column xsi:type="varchar" name="name" nullable="false" length="255" comment="Name"/>
        <constraint xsi:type="primary" referenceId="PRIMARY">
            <column name="id"/>
        </constraint>
    </table>
</schema>
```

And route for our admin page



file: `app/code/Aware/Checkout/etc/adminhtml/routes.xml`
```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:App/etc/routes.xsd">
    <router id="admin">
        <route id="aware_checkout" frontName="aware_checkout_admin">
            <module name="Aware_Checkout"/>
        </route>
    </router>
</config>
```

lets put our page in admin menu

file `app/code/Aware/Checkout/etc/adminhtml/menu.xml`

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Menu/etc/menu.xsd">
    <menu>
        <add id="Aware_Checkout::parent"
             title="Aware Checkout"
             module="Aware_Checkout"
             sortOrder="100"
             resource="Aware_Checkout::main"/>

            <add id="Aware_Checkout::main"
             title="Main"
             module="Aware_Checkout"
             sortOrder="100"
             resource="Aware_Checkout::main"
             parent="Aware_Checkout::parent"
             action="aware_checkout_admin/index/index"/>

             


    </menu>
</config>
```

let also non admin users see our module seting up acl.xml

file: `app/code/Aware/Checkout/etc/acl.xml`

```xml
<?xml version="1.0"?>
<config xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Acl/etc/acl.xsd">
    <acl>
        <resources>
            <resource id="Magento_Backend::admin">
                <resource id="Aware_Checkout::main" title="Aware Checkout Access" sortOrder="10" />
            </resource>
        </resources>
    </acl>
</config>
```

and now lets create controller

file `app/code/Aware/Checkout/Controller/Adminhtml/Index/Index.php`

```php
<?php

namespace Aware\Checkout\Controller\Adminhtml\Index;

use Magento\Backend\App\Action;
use Magento\Backend\App\Action\Context;
use Magento\Framework\View\Result\PageFactory;

class Index extends Action
{
    protected $resultPageFactory;


    public function __construct(Context $context, PageFactory $resultPageFactory)
    {
        parent::__construct($context);
        $this->resultPageFactory = $resultPageFactory;
    }

    public function execute()
    {
        $resultPage = $this->resultPageFactory->create();
        $resultPage->getConfig()->getTitle()->prepend((__('Posts')));

        return $resultPage;
    }
}
```








