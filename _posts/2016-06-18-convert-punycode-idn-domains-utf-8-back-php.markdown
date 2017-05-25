---
layout: post
title:  "Convert Punycode IDN domains to UTF-8 and back again with PHP"
redirect_from:
   - /convert-punycode-idn-domains-utf-8-back-php
date:   2016-06-18 01:35:07 +0100
categories: best domain registrar
description: So I am sitting here late at night and I registered a few swedish .xyz domains that contain characters åäö. For example you may have the domain for horse in swedish, häst.xyz, this domain is actua
---

So I am sitting here late at night and I registered a few swedish .xyz domains that contain characters åäö. For example you may have the domain for horse in swedish, häst.xyz, this domain is actually written as xn--hst-qla.xyz. Now I wanted to create a  temporary catch-all page for these swedish domains until I can build a site for them. What I did was to get the domain name as a variable which I echo in the title and body of the page using the following server variable:

`$_SERVER['HTTP_HOST']` this works ok but for domains with äåö it will output the weird looking punycode which is not really seo or user friendly. To solve this we can use the [`idn_to_utf8`()](http://php.net/manual/en/function.idn-to-utf8.php) function in PHP which takes the punicode and converts it to UTF-8 characters which will show the åäö characters. For my site I also wanted to make the first character upper case so I used the [ucfirst()](http://php.net/manual/en/function.ucfirst.php) function. My page ended up looking like this. ```
<html>
<head>
    <title><?php echo ucfirst(idn_to_utf8($_SERVER['HTTP_HOST'])); ?> - Coming soon!</title>
</head>
<body>
<center>
    <br/>
    <br/>
    <br/>
    <h1><?php echo ucfirst(idn_to_utf8($_SERVER['HTTP_HOST'])); ?></h1>
</center>
</body>
</html>
``` That takes care of that. Now the opposite function of [`idn_to_utf8`()](http://php.net/manual/en/function.idn-to-utf8.php) is the [`idn_to_ascii`()](http://php.net/manual/en/function.idn-to-ascii.php) function. This will take the åäö characters and convert them to the punycode. I hope some of you find this useful.</body></html>