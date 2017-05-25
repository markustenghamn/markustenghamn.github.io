---
layout: post
title:  "Skapa ett Tag Cloud med PHP"
redirect_from:
   - /skapa-ett-tag-cloud-med-php
date:   2012-03-15 00:12:45 +0100
categories: best domain registrar
description: Jag har tidigare lagt upp kod för att skapa ett Tag Cloud med PHP på Sourcecodedb.com...
---

Jag har tidigare lagt upp kod för att skapa ett Tag Cloud med PHP på Sourcecodedb.com [http://sourcecodedb.com/Create-a-tag-cloud-with-php.html](http://sourcecodedb.com/Create-a-tag-cloud-with-php.html "Create a tag cloud with php") Jag använder mig utav kod från [Steven York](http://stevenyork.com/tutorial/creating_accessible_tag_cloud_in_php_css_mysql), han har skrivit det mästa av koden som jag använder i detta exempel. Jag fick ändra delar av koden för SourceCodeDB.com eftersom vi sparar tags i databasen som strings som ser ut ungefär så här "C#, loop, code" istället för att spara varje tag en och en. Här är min kod som jag har kommenterat för att göra det lättare att förstå.

`<pre lang="php">` För att visa tag cloudet i en footer eller direkt på en sida så är det bara att använda PHPs include funktion för att inkludera denna koden på den delen av sidan. Kommentera gärna om ni har frågor!