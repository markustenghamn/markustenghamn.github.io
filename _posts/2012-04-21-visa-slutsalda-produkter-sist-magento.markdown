---
layout: post
title:  "Visa slutsålda produkter sist i Magento"
redirect_from:
   - /visa-slutsalda-produkter-sist-magento
date:   2012-04-21 22:00:34 +0100
categories: best domain registrar
description: Ett problem med Magento är att när produkter är slutsålda så visas de fortfarante i Magento sök och i kategorier. Här är ett snabbt och enkelt sätt att ändra så att de visas sist i listan....
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

Ett problem med Magento är att när produkter är slutsålda så visas de fortfarante i Magento sök och i kategorier. Här är ett snabbt och enkelt sätt att ändra så att de visas sist i listan.  
  
 I följande del av Magento hittar vi List.php  
  
 /app/code/core/Mage/Catalog/Block/Product/  
  
 Om du inte vill ändra Magentos core så kan du kopiera filen och lägga den i /app/code/local/Mage/Catalog/Block/Product/  
  
 Hitta följande, för mig var det på linje 86.

`<pre lang="php">$this->_productCollection = $layer->getProductCollection();`  
 Och ändra det till  
`<pre lang="php">$this->_productCollection = $layer->getProductCollection()->joinField('inventory_in_stock', 'cataloginventory_stock_item', 'is_in_stock', 'product_id=entity_id','is_in_stock>=0', 'left')->setOrder('inventory_in_stock','desc');`  
 Så där, nu är det klart. Ladda om cache i Magenot och testa sedan hemsidan. [BilligaApan.se](BilligaApan.se) använder samma modifiering om ni vill se hur det fungerar.