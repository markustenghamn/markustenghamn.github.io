---
layout: post
title:  "Fixing auto increment on MySQL tables when foreign key constrainst fail"
redirect_from:
   - /fixing-auto-increment-on-mysql-tables-when-foreign-key-constrainst-fail
date:   2016-02-09 07:54:40 +0100
categories: best domain registrar
description: Please note that you should NOT do this if you don't know what is wrong. Foreign keys are there for a reason and bypassing them could cause you to lose data and break applications.
---

Please note that you should **NOT** do this if you don't know what is wrong. Foreign keys are there for a reason and bypassing them could cause you to lose data and break applications. Lately I had been dealing with a large mysql import where the export had been handled wrongly and thus caused all of my database tables to lose auto increment on the id field. When trying to add this auto increment back I would get foreign key constraint failed errors. To get around this error you can turn off foreign key checks while running your query. This will allow you to bypass the restrictions while fixing the database structure. I used the following code to fix all my tables. ```
SET foreign_key_checks = 0;
ALTER TABLE tablename MODIFY COLUMN id INT NOT NULL AUTO_INCREMENT;
SET foreign_key_checks = 1;
``` On the first line we turn off foreign key checks allowing us to make changes to the table freely. On the second line we change our table to use auto increment, replace "tablename" with the name of your table and change "id" to the name of your column. On the third line we turn foreign key checks back on again.