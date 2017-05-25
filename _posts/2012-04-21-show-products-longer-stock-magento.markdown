---
layout: post
title:  "Show products no longer in stock last in Magento"
redirect_from:
   - /show-products-longer-stock-magento
date:   2012-04-21 22:32:58 +0100
description: One big problem with Magento is that when products become out of stock they still show up very high in the product listings and this can get very annoying for some users if there are several products...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

One big problem with Magento is that when products become out of stock they still show up very high in the product listings and this can get very annoying for some users if there are several products out of stock. This very easy code fix will automatically make products that are out of stock show up last in all your categories and search results.  
  
 Go to the following folder in Magento where you will find List.php  
  
 /app/code/core/Mage/Catalog/Block/Product/  
  
 If you don't want to change the core files, which is always a bad habbit then copy the file to /app/code/local/Mage/Catalog/Block/Product/  
  
 Find the following around line 86.

`<pre lang="php">$this->_productCollection = $layer->getProductCollection();`  
  
 And change it to  
  
`<pre lang="php">$this->_productCollection = $layer->getProductCollection()->joinField('inventory_in_stock', 'cataloginventory_stock_item', 'is_in_stock', 'product_id=entity_id','is_in_stock>=0', 'left')->setOrder('inventory_in_stock','desc');`  
  
 There you go, now it's done, that's all you have to do to make it work. Feel free to ask me any questions you may have.