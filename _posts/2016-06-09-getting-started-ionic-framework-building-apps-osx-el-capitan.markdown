---
layout: post
title:  "Getting started with Ionic framework for building apps on OSX El Capitan"
redirect_from:
   - /getting-started-ionic-framework-building-apps-osx-el-capitan
date:   2016-06-09 19:50:47 +0100
categories: ionic
description: First thing you need to do if you haven't already done so is to download and install Node.js. You will find Node.js via their website
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

First thing you need to do if you haven't already done so is to download and install Node.js. You will find Node.js via their website here: <https://nodejs.org/en/>. I picked the recommended option as it is most likely more stable but you are free to try either one. Both will probably work with Ionic framework.  
  
 Once Node.js is installed you will need to open terminal, cmd + space and then type "terminal" to open it quickly. After this I will be following the getting started guide from the Ionic website which can be found here <http://ionicframework.com/getting-started/>.  
  
 So first thing we need to do is install the Cordova and Ionic command line tools, simply type "`bash npm install -g cordova ionic`" in the terminal and you should see a spinning line and then a loading bar which means it is loading.  
  
**If you run into an error here:** You may get an error like "`<span class="typ">Error</span><span class="pun">:</span><span class="pln"> EACCES</span><span class="pun">,</span><span class="pln"> permission denied</span>`" and then it tells you to try running the command as an administrator. Usually the error is related to moving files or not being able to create symlinks. You could do sudo but I prefer to run this as a regular user. So this error basically means we have a permission issue. To fix the issue start by running "sudo npm update -g", this will ask for your password, just type it in and it will update node. Then we clear the cache so we start of nice and cleanly using the command "npm cache clean". Now let's update our permissions, let's run "sudo chown -R $USER:$GROUP /usr/local/lib/node\_modules/" and also for the symlinks run "sudo chown -R $USER:$GROUP /usr/local/bin". Then run "npm update -g" to see if things work and finally run "`bash npm install -g cordova ionic`" and things should now install without errors.  
  
 Now we can create a project, simply cd to the directory where you want your project, im just going to put it in my users folder so I type "cd --". Then run "`bash ionic start myApp tabs`" to start our first project. You may get a promt to install command line tools, if you get this then you should install it. It may also ask if you want an ionic.io account, you can create one if you want.  
  
 Now that your project is created we can cd to your app directory using "`bash <span class="nb">cd </span>myApp`". Add the ios platform to ionic using "`bash ionic platform add ios`".  
  
**If you get an error:** You may get an error like "xcode-select: error: tool 'xcodebuild' requires Xcode, but active developer directory '/Library/Developer/CommandLineTools' is a command line tools instance" which basically means you need to install xcode to run the app. Luckily Xcode is completely free to download, simply go here and proceed to download and install: <https://developer.apple.com/xcode/download/>. Once this is done you are ready to run "`bash ionic platform add ios`" again and follow the next step in the guide.  
  
 Now we are ready to run our sample app. Simply type "`bash ionic emulate ios`" and the app will launch. You can navigate to the myApp folder in finder to view the files. Since Ionic uses Angular the functions are javascript based which you will find in the .js files. You can actually launch pages in the app by opening the .html files in your browser. This allows you to inspect css elements and make changes quickly with a visual guide before you actually build the app. When you need to make a new project simply open terminal, cd to your projects directory and then run "`bash ionic start myApp2 tabs`" where myApp2 is the name of the folder or project and tabs is the base build. You can change out tabs with blank or side menu depending on what type of app you want to create.``  
  
 I hope this guide has helped you to get started with the Ionic framework!