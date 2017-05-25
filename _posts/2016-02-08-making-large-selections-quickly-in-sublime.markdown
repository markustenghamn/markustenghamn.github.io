---
layout: post
title:  "Making large selections quickly in Sublime"
redirect_from:
   - /making-large-selections-quickly-in-sublime
date:   2016-02-08 02:15:36 +0100
categories: best domain registrar
description: So tonight I was editing a large sql file, a few million lines and the import got stuck about half way through. The easiest solution would be to edit the sql dump and I needed a quick way to remove ha...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

So tonight I was editing a large sql file, a few million lines and the import got stuck about half way through. The easiest solution would be to edit the sql dump and I needed a quick way to remove half the text in the file. At first I tried finding the line and manually selecting the lines but realized that it would take forever. So how in the world do I make large selections quickly?  
  
 Thats when I headed to Google to find the best way of doing this. After a bit of searching I stubled across a thread called [Large Selection/Highlight or Select Range by Line Number](https://forum.sublimetext.com/t/large-selection-highlight-or-select-range-by-line-number/13879). With a nice answer regarding markers in Sublime.  
  
 So basically all you have to do is scroll to the line where you want to make your selection from and click Edit->Mark->Set mark.  
  
[(image removed)](http://tenghamn.com/wp-content/uploads/2016/02/sublime-mark.jpg)  
  
 Then scroll to the line where you want your selection to go to and click Edit->Mark->Select to mark