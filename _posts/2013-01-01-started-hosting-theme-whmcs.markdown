---
layout: post
title:  "Getting Started With The Just Hosting Theme For WHMCS"
redirect_from:
   - /started-hosting-theme-whmcs
date:   2013-01-01 01:05:44 +0100
categories: best domain registrar
description: I recently purchased the Just Hosting theme for whmcs which is a great looking theme that I want to use for m new site Zunem.com. Here I will go throu...
---

I recently purchased the Just Hosting theme for whmcs which is a great looking theme that I want to use for m new site Zunem.com. Here I will go through the initial steps needed to set everything up so that you can start selling web hosting! [Read my continued post here](http://markustenghamn.com/just-hosting-theme-whmcs "Just Hosting Theme for WHMCS")

### What you will need

- [WHMCS](http://www.whmcs.com/members/aff.php?aff=3089 "WHMCS")
- [Just Hosting WHMCS Theme - http://themeforest.net/item/just-hosting/408834](http://themeforest.net/item/just-hosting/408834?ref=bigideaguy "Just Hosting WHMCS Theme")
- [Hosting and a domain](http://zunem.com "Zunem Hosting")
 
 Sponsor: ### Setting up WHMCS

 Before we begin configuring the Just Hosting Theme we need to install WHMCS. I won't go through the details of how to set this up, guides on how to do this can be found [here](http://docs.whmcs.com/Installing_WHMCS "Setting up WHMCS"), if needed help you can always hire me to set it up. I installed my WHMCS in a folder called "whmcs" which is the default when extracting the whmcs files. Other common names for this folder are shop, store, or clients. Some also run whmcs on a subdomain or in some cases on a seperate domain. For this tutorial I will assume that you are using a folder called whmcs. ### Installing the Theme

 After downloading the theme you will find a documentation folder which explains most of what needs to be done, however with the recent release of a new version of WHMCS your theme will no longer work if uploaded with the underscore that it currently has. (image removed) So to begin, open the folder called "just hosting twitter and whmcs" and then proceed to open the whmcs folder. Here you will find the theme folder called just\_hosting, rename this to something without an underscore like "justhosting". Then proceed to upload this folder to your whmcs theme path which is /whmcs/templates/. After doing this you should now be able to select the theme in your WHMCS configuration. However the theme will try to look for css and image files in your public or www directory where your whmcs folder is located and it may look a bit weird, but don't worry. As the documentation says, you will need to upload the files from a folder of your choice to your / directory. So open either the "just hosting", "just hosting twitter and whmcs" or "just hosting contact and twitter" folder depending on what you would like to have as your home page. Then proceed to upload the index.html file, the images and js folder, and any css and php files. Now we should have a working front page and a working WHMCS theme! ### Configuring the theme

 Login to your WHMCS admin area and then go to Setup > Products/Services and setup a hosting package. For more information on how to do this follow the WHMCS documentation [here](http://docs.whmcs.com/Products_and_Services "Setup a product or service WHMCS"). (image removed) When editing a product you can go to the links tab to find the url that links to this product in the client area. For me this link is http://zunem.com/whmcs/cart.php?a=add&pid=1 You can type the link in your address bar to see that it works. Now we wat to add this link to our index.html file. Open this file and find the following code ```
<pre lang="html">
<div class="bottom_part"> 
					
					<span class="price">$1.99</span>
					
					<a class="button scroll" href="#bottom_part">Register</a> 
					<div class="clear"></div>
				</div>

``` We want to add our url where the #bottom\_part is and then remove the scroll class so that it ends up looking like this ```
<pre lang="html">
<div class="bottom_part"> 
					
					<span class="price">$1.99</span>
					
					<a class="button" href="http://zunem.com/whmcs/cart.php?a=add&pid=1">Register</a> 
					<div class="clear"></div>
				</div>

``` You will find similar blocks of code for the other two hosting packages where you can add links to your other hosting packages in the same way. This is all for this post but I will write another post later to explain how you can customize your theme even more. [Read my continued post here](http://markustenghamn.com/just-hosting-theme-whmcs "Just Hosting Theme for WHMCS")