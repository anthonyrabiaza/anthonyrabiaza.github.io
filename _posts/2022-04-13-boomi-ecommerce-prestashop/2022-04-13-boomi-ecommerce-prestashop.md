---
layout: post
title:  "Boomi Integration with Prestashop eCommerce Platform"
permalink: /boomi-ecommerce-prestashop/
---
Integration with Prestashop eCommerce with Grafana Cloud
===========================================================

## Overview

Boomi can be used to interconnect different types of systems. In this article, we will see how to automate the retrieval and update of information from one of the most used eCommerce Platform: Prestashop.

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop.png)

## Enabling Prestashop Web Services

The first step consist of configuring the Web Services component in Prestashop:

- Enable PrestaShop's webservice
- Create a Web Service Key

![Prestashop](/assets/boomi-ecommerce-prestashop/prestashop-enable-ws.png)

This step will expose a /api endpoint on the server and the Web Service Key can used to interact with Prestashop.

## Calling Prestashop Web Services

### Initial the connectivity and creation Boomi Profile

This section will describe the steps to follow to retrive the orders:

![Prestashop](/assets/boomi-ecommerce-prestashop/prestashop-orders.png)

First, define a variable to store the Web Service Key (the key is masked in the following example) and the prestashop endpoint:

```
export ws_key=CUS***********************************
export ws_endpoint=https://prestashop.demo.boomi-apj.com/api
```

Then, you can query the order:

```
curl $ws_endpoint/orders"?ws_key=${ws_key}&display=full"
```

The output will looks like:
```
<?xml version="1.0" encoding="UTF-8"?>
<prestashop xmlns:xlink="http://www.w3.org/1999/xlink">
  <orders>
    <order>
      <id><![CDATA[1]]></id>
      <id_address_delivery xlink:href="https://prestashop.demo.boomi-apj.com/api/addresses/5"><![CDATA[5]]></id_address_delivery>
      <id_address_invoice xlink:href="https://prestashop.demo.boomi-apj.com/api/addresses/5"><![CDATA[5]]></id_address_invoice>
      <id_cart xlink:href="https://prestashop.demo.boomi-apj.com/api/carts/1"><![CDATA[1]]></id_cart>
      <id_currency xlink:href="https://prestashop.demo.boomi-apj.com/api/currencies/1"><![CDATA[1]]></id_currency>
      <id_lang xlink:href="https://prestashop.demo.boomi-apj.com/api/languages/1"><![CDATA[1]]></id_lang>
      <id_customer xlink:href="https://prestashop.demo.boomi-apj.com/api/customers/2"><![CDATA[2]]></id_customer>
      <id_carrier xlink:href="https://prestashop.demo.boomi-apj.com/api/carriers/2"><![CDATA[2]]></id_carrier>
      <current_state xlink:href="https://prestashop.demo.boomi-apj.com/api/order_states/6"><![CDATA[6]]></current_state>
      <module><![CDATA[ps_checkpayment]]></module>
      <invoice_number><![CDATA[0]]></invoice_number>
      <invoice_date><![CDATA[0000-00-00 00:00:00]]></invoice_date>
      <delivery_number><![CDATA[0]]></delivery_number>
      <delivery_date><![CDATA[0000-00-00 00:00:00]]></delivery_date>
      <valid><![CDATA[0]]></valid>
      <date_add><![CDATA[2022-04-12 01:01:47]]></date_add>
      <date_upd><![CDATA[2022-04-12 01:01:47]]></date_upd>
      <shipping_number notFilterable="true"><![CDATA[]]></shipping_number>
      <note><![CDATA[Test]]></note>
      <id_shop_group><![CDATA[1]]></id_shop_group>
      <id_shop><![CDATA[1]]></id_shop>
      <secure_key><![CDATA[b44a6d9efd7a0076a0fbce6b15eaf3b1]]></secure_key>
      <payment><![CDATA[Payment by check]]></payment>
      <recyclable><![CDATA[0]]></recyclable>
      <gift><![CDATA[0]]></gift>
      <gift_message><![CDATA[]]></gift_message>
      <mobile_theme><![CDATA[0]]></mobile_theme>
      <total_discounts><![CDATA[0.000000]]></total_discounts>
      <total_discounts_tax_incl><![CDATA[0.000000]]></total_discounts_tax_incl>
      <total_discounts_tax_excl><![CDATA[0.000000]]></total_discounts_tax_excl>
      <total_paid><![CDATA[61.800000]]></total_paid>
      <total_paid_tax_incl><![CDATA[68.200000]]></total_paid_tax_incl>
      <total_paid_tax_excl><![CDATA[66.800000]]></total_paid_tax_excl>
      <total_paid_real><![CDATA[0.000000]]></total_paid_real>
      <total_products><![CDATA[59.800000]]></total_products>
      <total_products_wt><![CDATA[59.800000]]></total_products_wt>
      <total_shipping><![CDATA[7.000000]]></total_shipping>
      <total_shipping_tax_incl><![CDATA[8.400000]]></total_shipping_tax_incl>
      <total_shipping_tax_excl><![CDATA[7.000000]]></total_shipping_tax_excl>
      <carrier_tax_rate><![CDATA[0.000]]></carrier_tax_rate>
      <total_wrapping><![CDATA[0.000000]]></total_wrapping>
      <total_wrapping_tax_incl><![CDATA[0.000000]]></total_wrapping_tax_incl>
      <total_wrapping_tax_excl><![CDATA[0.000000]]></total_wrapping_tax_excl>
      <round_mode><![CDATA[0]]></round_mode>
      <round_type><![CDATA[0]]></round_type>
      <conversion_rate><![CDATA[1.000000]]></conversion_rate>
      <reference><![CDATA[XKBKNABJK]]></reference>
      <associations>
        <order_rows nodeType="order_row" virtualEntity="true">
          <order_row>
            <id><![CDATA[1]]></id>
            <product_id xlink:href="https://prestashop.demo.boomi-apj.com/api/products/1"><![CDATA[1]]></product_id>
            <product_attribute_id><![CDATA[1]]></product_attribute_id>
            <product_quantity><![CDATA[1]]></product_quantity>
            <product_name><![CDATA[Hummingbird printed t-shirt - Color : White, Size : S]]></product_name>
            <product_reference><![CDATA[demo_1]]></product_reference>
```

More information on Prestashop Web Service can be found [here](https://devdocs.prestashop.com/1.7/webservice/){:target="_blank"} 

Now you can import the XML document as Boomi XML Profile and fix the profile accordingly:  
![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-orders-profile.png)

### Call from Boomi

Define the HTTP Connection as follow:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection.png)

Define the HTTP Operation as follow (similarly to the previous example):

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection-op.png)

The ws_key can set as 'replacement variable' and defined in the Connection shape:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection-op-param.png)

The process will look like this:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-process.png)

Validate that the process execute successfully and returned the same input as the curl command:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-process-execution.png)

### Update of Order status

The update of order status in Prestashop can be done using the order_histories API.

The order #5 before the update:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/prestashop-orders-line.png)

Overview of order statuses:

![Prestashop](/assets/boomi-ecommerce-prestashop/prestashop-order-statuses.png)

More information on order statuses [here](https://doc.prestashop.com/display/PS17/Statuses){:target="_blank"} 

Update with curl:

```
curl -X POST $ws_endpoint/order_histories"?ws_key=${ws_key}"  -d \
"<prestashop>
    <order_history>
        <id_order_state>3</id_order_state>
        <id_order>5</id_order>
    </order_history>
</prestashop>"
```

The updated order:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/prestashop-orders-line-updated.png)


Updating the orders in Boomi will use a new XML Profile and a new operation:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-order-histories-add-profile.png)

A filter can also be added in the Query operation:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection-op-param-filter.png)

And the Shape defining the parameters:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection-op-filter-param.png)

Then, the process can be updated to add the update status operation:

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-connection-op-update.png)

Execution of the full process, please note the Set property shape used to set 'ws_key' as we are using a 'Send' action (we are not using parameters):

![Boomi AtomSphere](/assets/boomi-ecommerce-prestashop/boomi-prestashop-process-execution-update.png)