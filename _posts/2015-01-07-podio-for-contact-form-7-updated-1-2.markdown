---
layout: post
title:  "Podio For Contact Form 7 Updated To 1.2"
redirect_from:
   - /podio-for-contact-form-7-updated-1-2
date:   2015-01-07 18:14:20 +0100
categories: best domain registrar
description: I recently updated my Wordpress plugin Podio For Contact Form 7. T...
---

I recently updated my Wordpress plugin [Podio For Contact Form 7](http://anve.to/9qwgJ "Podio For Contact Form 7"). This latest update fixes a few issues which broke the previous version due to Contact Form 7 updates. I also had many questions on how to find API details so I have added a detailed description on how to find this. I have also made debugging easier. In addition to all this I have also added a github repository for anyone who wants to contribute and help with the plugin.

<div class="block-content">### Podio API Details

 To get your API details you will need to go to [https://podio.com/settings/api?force\_locale=en\_US.](http://anve.to/VXKWj)Then you need to find the numeric id of the application. Navigate to your newly installed app inside Podio, click the little wrench next to the application name in the left sidebar and click on the 'Developer' option in the menu. On the following page the application id is listed along with other useful data. The podio app url is formatted like this 'https://podio.com/workspace/appname/' I use the simple lead handling example app to get this to work. ### Podio Debugging

 The debugging function is there to help users solve any typ of issue that may arise. Fields must be of the type location or text. This plugin does not support contact ids or sales agent ids for example. This may be something that could be implemented in future versions. In this version I have made it so that an error will display as Error! - (error infor from podio) If everything seems to be working the debugging information will display all the fields from the app you have configured podio to use. The names of these fields should match the names in your contact form 7 configuration as this addon simply tries to match fields of the same name and input it into your Podio app. You should also have an extra field under "Extra info field external id", I typically make this the "notes" field, any information not matched will be stored in this field. If debugging is enabled then any Podio errors will be appended to the message body of the Podio message. ### Development

 You will find the official Github repository here [https://github.com/markustenghamn/Podio-For-Contact-Form-7](http://anve.to/tuiMr)</div>