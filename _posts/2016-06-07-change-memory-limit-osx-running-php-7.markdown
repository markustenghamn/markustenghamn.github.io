---
layout: post
title:  "Change Memory Limit on OSX running PHP 7"
redirect_from:
   - /change-memory-limit-osx-running-php-7
date:   2016-06-07 20:22:17 +0100
categories: best domain registrar
description: Every now and then when testing local applications you may run into an error that says something like "Allowed memory size of xxxxx bytes exhausted (t...
---

Every now and then when testing local applications you may run into an error that says something like "Allowed memory size of xxxxx bytes exhausted (tried to allocate xxxx bytes)". Now, sometimes this may be caused by a coding issue such as a loop that gets stuck which is way you may not want to set this limit too high. In my case I felt that I needed to raise this limit as I am working with parsing large excel files which require a lot more than the standard 128M of memory. First thing you can do is check the memory limit php is currently set to by typing "php -i | grep memory" in the terminal. It will say something like 128M which is 128 megabytes. Then type "php -i | grep php.ini" in the terminal, this will show you the path to the php.ini file. In my case the path is /usr/local/etc/php/7.0/php.ini. You can then use your favorite editor to change the file, you will probably need to use sudo. I prefer using nano to edit files. Simply type "sudo nano /usr/local/etc/php/7.0/php.ini" to begin editing the file. You can use ctrl+w to search for memory\_limit or 128M in order to get to the right line. I changed the value to 4G which is very high, you could try doubling the value to 256M to see if that is enough to get you going, Once you have changed the value press ctrl+x to close, press y and enter to save the changes. All we have to do now is restart apache and we should be good to go, type "apachectl restart" in the terminal. Then you can check the memory limit again using "php -i | grep memory".