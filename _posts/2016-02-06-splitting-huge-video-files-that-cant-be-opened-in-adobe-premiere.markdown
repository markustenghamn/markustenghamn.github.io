---
layout: post
title:  "Splitting huge video files that can't be opened in Adobe Premiere"
redirect_from:
   - /splitting-huge-video-files-that-cant-be-opened-in-adobe-premiere
date:   2016-02-06 15:02:00 +0100
categories: splitting huge video files
description: So last night was game night and I was recording the newly released XCOM 2 with BigSwedish for several hours using...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

So last night was game night and I was recording the newly released [XCOM 2](https://xcom.com/) with [BigSwedish](http://bigswedish.com/) for several hours using [OBS](https://obsproject.com/). OBS allows me to record on multiple audio channels so that I have myself speaking on one channel, Skype and whoever I am talking to on one channel and the actual game audio on a third channel. This makes it easier to edit in Premiere but it also makes for some very big files. This morning I wake up to begin editing my file and notice that its 40GB and importing it to Premiere either seems to freeze my computer completely or simply crash the program. Thus I realized that I needed to cut this file into smaller pieces but I had no idea what program to use. Splitting the file would not be as easy as I had hoped.  
  
 One option is to use VLC media player to record the video that is playing and use the recorded file, however this did **NOT** work for me as VLC did not preserve the video quality and also seemed to mess upp my audio tracks. I kept searching and tried many different programs, some programs I skipped as they seemed to be malware/spyware (Be careful when searching for free stuff). Finally looking at some forum posts got me going in the right direction to find a solution to my problem.  
  
**How I solved it**  
  
[Avidemux](http://www.avidemux.org/) solved all of my problems. It basically gives you the same capabilities as cutting video with premiere pro but in a much simpler interface. This program lagged for a few seconds but had no trouble loading my video file. I could also drag the slider to the needed position and set the markers for when I wanted to begin my cut and end it (just like Premiere). Once you have set your markers all you need to do is save your video and you are done! I love that Avidemux has the ability to simply copy the video and audio output. Avidemux should run fine and be perfect for splitting video on Unix, Mac or Windows. I hope this post helps some users who may be stuck with the same problems I had.  
  
 If you know of an even better way of doing video splitting please do let me know in the comments below!