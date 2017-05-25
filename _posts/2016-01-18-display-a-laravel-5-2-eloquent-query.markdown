---
layout: post
title:  "Display a Laravel 5.2 eloquent query"
redirect_from:
   - /display-a-laravel-5-2-eloquent-query
date:   2016-01-18 02:42:09 +0100
categories: best domain registrar
description: Sometimes when working with Laravel it can be nice to see the actual query that you are generating, especially if you are dealing with really large queries. It surprised me how easy this is to do in L
---

Sometimes when working with Laravel it can be nice to see the actual query that you are generating, especially if you are dealing with really large queries. It surprised me how easy this is to do in Laravel as explained in this post <https://scotch.io/tutorials/debugging-queries-in-laravel>. Basically all we need to do is replace '->get()' at the end of our statement with '->toSql()' which will print our statement.```


```