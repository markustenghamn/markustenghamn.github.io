---
layout: post
title:  "Improve the search function in Magento"
redirect_from:
   - /improve-search-function-magento
date:   2012-04-21 22:49:10 +0100
description: Earlier I wrote about how you can change Magento so that all the products that are sold out show up last. Here is another thing that can be improved in Magento. The search function shows by default th...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

Earlier I wrote about how you can change Magento so that all the products that are sold out show up last. Here is another thing that can be improved in Magento. The search function shows by default the most relevant results last. And if you have multiple search terms it tries to find a match one at a time. We can change this fairly easily with a small bit of code.   
  
 Change the file /catalogsearch/form.mini.phtml in your theme  
  
 In that file is a html form. Add the following code anywhere between the form tags

`<pre lang="HTML"><input name="order" type="hidden" value="relevance"></input><br></br><input name="dir" type="hidden" value="desc"></input>`  
  
 This changes the search so that our most relevant results show first.  
  
 Then make a copy of app/code/core/Mage/CatalogSearch/Model/resource/Fulltext.php  
  
 And place it here app/code/local/Mage/CatalogSearch/Model/resource/Fulltext.php  
  
 On line 356 is the following code  
  
`<pre lang="php">$likeCond = '(' . join(' OR ', $like) . ')';`  
  
 Change it to the following  
  
`<pre lang="php">$likeCond = '(' . join(' AND ', $like) . ')';`  
  
 Then on line 378 find  
  
`<pre lang="php">$where .= ($where ? ' OR ' : '') . $likeCond;`  
  
 And change it to  
  
`<pre lang="php">$where .= ($where ? ' AND ' : '') . $likeCond;`  
  
 That's it, now your search results should be much more relevant! Let me know if you have any questions.