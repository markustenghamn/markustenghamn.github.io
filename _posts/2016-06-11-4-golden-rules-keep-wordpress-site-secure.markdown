---
layout: post
title:  "4 Golden Rules To Keep Your Wordpress Site Secure"
redirect_from:
   - /4-golden-rules-keep-wordpress-site-secure
date:   2016-06-11 17:36:01 +0100
categories: wordpress site secure
description: I have worked with Wordpress for about 10 years, with my own websites and clients websites. My sites have been hacked in the past. While working at one of Sweden's largest web hosts I would fix broken...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

I have worked with Wordpress for about 10 years, with my own websites and clients websites. My sites have been hacked in the past. While working at one of Sweden's largest web hosts I would fix broken and hacked Wordpress sites on a daily basis. Today I work as an IT Security Manager where I do everything from keeping our companys Wordpress site secure to securing several servers and networks that we have at more than 5 different physical locations. From all of this experience I have learned a lot and this has led me to create a list of 4 golden rules which will keep your Wordpress site safe and secure.

### 1. Update

  
 Not updating Wordpress or the themes and plugins installed in Wordpress is by far the most common reason why most users get hacked. Personally I keep a todo list that along with a reminder every two weeks to go through all my installations (it can be Wordpress, Servers, other websites, etc.) and just check on things and do all the needed updates. In the past I have also used [InfiniteWP](https://infinitewp.com/) to keep track of many Wordpress installations at the same time, for its basic features it is free to use.  
### 2. Use popular plugins and themes

  
 This is related to the above point about updating. There are lots and lots of themes and plugins out there that are old and that may be unmaintained. A large user base will keep you safer as it is more likely to be supported (check when the last update was made) and there is a greater chance that if a user finds a flaw it will be widely reported. When getting a new plugin or theme I always recommend googling the plugin or theme name like "Woocommerce Wordpress" to see if there is anything negative that I should know about before I install it.  
### 3. Only use the plugins or themes that you need

  
 Think of your Wordpress installation as you think of your computer. The Wordpress.org repository of plugins and themes (along with third party websites) is the internet and whenever you install something you are taking a risk that it could be something malicious. This goes with #2, popular themes and plugins are generally less risky. Also keep in mind that there are malicious users who upload bad plugins and themes on purpose to hack websites or steal data (these usually get reported but there is always a chance with plugins or themes that nobody else has looked at). Another reason why you don't want a bunch of themes and plugins clogging up your Wordpress site is that it gives you more things to update and more ways that things could go wrong, more doorways that an attacker could use to get into your system. Security plugins for Wordpress is an example of an extra plugin you don't need, all these do is make it harder to find login pages and what not but in reality you are most likely taking more of a risk installing this addon as it could have potential vulnerabilities. Wordpress is used by companies like Spotify, if you keep it updated there is no need for silly security plugins which are just a waste of time and money.  
### 4. Remove the admin user

  
 A simple way to attack a Wordpress site is to brute force by guessing the administrator username and password. Having a username like admin and a password like "strongpass1234" is not really that safe. I highly recommend using a long username that is not admin or administrator, and preferably a username that contains a space. So for my Wordpress site I might use the username "Markus Tenghamn" as it is long and then I like to generate a random password which is 15-20 characters long via this [password generator](http://strongpasswordgenerator.com/) website. Most of these bruteforce attacks are automated using the most common usernames, having a uncommon username along with a strong password makes your site extra safe. Keep in mind that I put this as my 4th rule, keeping your Wordpress site updated is more important.  
  
 Stick to these 4 golden rules and you will have a safe and secure Wordpress site. I hope the article was useful and I would love to hear your feedback in the comments section below.