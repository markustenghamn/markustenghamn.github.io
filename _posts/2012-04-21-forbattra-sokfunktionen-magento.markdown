---
layout: post
title:  "Förbättra sökfunktionen i Magento"
redirect_from:
   - /forbattra-sokfunktionen-magento
date:   2012-04-21 20:42:53 +0100
categories: best domain registrar
description: Tidigare skrev jag om hur man kan ändra Magento så att produkter som är slutsålda alltid visas sist. Här är en till sak som Magento kan göra bättre. Sökfunktionen visar som default de mest re
---

Tidigare skrev jag om hur man kan ändra Magento så att produkter som är slutsålda alltid visas sist. Här är en till sak som Magento kan göra bättre. Sökfunktionen visar som default de mest relevanta produkter sist i sök resultaten. Om man har flera sök termer så försöker den hitta en match en åt gången. Detta kan vi ändra på relativt lätt med lite kod. Ändra filen /catalogsearch/form.mini.phtml i ditt tema Där finns det en html form där vi kan lägga till följande kod var som helst mellan form taggarna Detta ändrar så att våra mest relevanta sök resultat kommer först Sedan, gör en kopia av app/code/core/Mage/CatalogSearch/Model/resource/Fulltext.php Och lägg den här app/code/local/Mage/CatalogSearch/Model/resource/Fulltext.php På linje 356 finns följande kod

`<pre lang="php">$likeCond = '(' . join(' OR ', $like) . ')';` Och ändra det till `<pre lang="php">$likeCond = '(' . join(' AND ', $like) . ')';` Sen på linje 378 hitta `<pre lang="php">$where .= ($where ? ' OR ' : '') . $likeCond;` Och ändra det till `<pre lang="php">$where .= ($where ? ' AND ' : '') . $likeCond;` Så där det är allt, nu bör sök fungera bättre. [BilligaApan.se](BilligaApan.se) använder samma ändringar som jag har gjort här om ni vill se ett exempel.