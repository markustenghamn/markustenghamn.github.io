---
layout: post
title:  "Bash script for new Laravel install on Ubuntu 14"
redirect_from:
   - /bash-script-new-laravel-install-ubuntu-14
date:   2016-01-23 16:35:27 +0100
categories: laravel
description: The following is a simple script that I have used to set permissions on development builds for Laravel, it usually fixes permission issues and gets me up and running quickly. You can put it into a .sh...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

The following is a simple script that I have used to set permissions on development builds for Laravel, it usually fixes permission issues and gets me up and running quickly. You can put it into a .sh file to run all the commands or simply copy and paste. I also have this running with git as a post-receive hook that will copy the master branch to a testing server.

```
<pre class="code highlight">```
<br></br><span class="line" id="LC4">mkdir bootstrap/cache/;</span><br></br><span class="line" id="LC5">mkdir storage/framework/cache;</span><br></br><span class="line" id="LC6">mkdir storage/framework/views;</span><br></br><span class="line" id="LC7">mkdir storage/framework/sessions;</span><br></br><span class="line" id="LC8">sudo chown -Rf $USER:www-data .;</span><br></br><span class="line" id="LC9">sudo chmod -R o+w storage;</span><br></br><span class="line" id="LC10">sudo chmod -R o+w bootstrap/cache;</span><br></br><span class="line" id="LC11">sudo chown -Rf www-data:www-data bootstrap/cache;</span><br></br><span class="line" id="LC12">sudo chown -Rf www-data:www-data storage;</span><br></br><span class="line" id="LC13">php artisan clear-compiled;</span><br></br><span class="line" id="LC14">php artisan config:clear;</span><br></br><span class="line" id="LC15">php artisan optimize;</span><br></br><span class="line" id="LC16">php artisan migrate;</span><br></br><span class="line">php artisan db:seed;<br></br></span>sudo apt-get install npm;<br></br>
sudo apt-get install node;<span class="line" id="LC17"><br></br></span><span class="line" id="LC18">npm install gulp;</span> <br></br><span class="line" id="LC19">npm install laravel-elixir;</span> <br></br><span class="line" id="LC20">sudo gulp;</span>
```
```