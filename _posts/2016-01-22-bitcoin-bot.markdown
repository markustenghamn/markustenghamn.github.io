---
layout: post
title:  "Bitcoin bot"
redirect_from:
   - /bitcoin-bot
date:   2016-01-22 00:26:13 +0100
categories: bitcoin bot
description: So tonight I took a little break to work on an idea that I have had for a while. Initially I wanted to look at social media, analyze the text to try to put a positive or negative number which would ho...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

So tonight I took a little break to work on an idea that I have had for a while. Initially I wanted to look at social media, analyze the text to try to put a positive or negative number which would hopefully give an indication on where the stock price would go. Now tracking stocks can be complicated due to delays in APIs and things like that and you would also need to decide on which stocks to pick. I guess this turned me off a bit as I wanted to make something quick and simple that would give me results. That gave me the idea of trying something different like bitcoin (also litecoin and dogecoin because why not?). What if I could use my bot to predict when the price of bitcoin would go up or down, a simple bitcoin bot? I could very easily get several indicators for my bot and then tell it when to buy or sell. Tonight I got started on the bot which currently gets a live feed of every single tweet metioning bitcoin as a live feed, check it out :D  
  
 \[caption id="attachment\_6066" align="aligncenter" width="720"\][(image removed)](http://tenghamn.com/wp-content/uploads/2016/01/8339bbb45925190c667a0804cd9411c3.png) Bitcoin twitter live feed\[/caption\]  
  
 I can now build a dictionary for positive and negative words which I can then compare and tweak to try to match the price. I also thought of two more indicators while I worked on this which are the amount of tweets (popularity) and also the amount of transactions (buys and sells, aka demand). Hopefully this project can turn into something useful and be a start for a similar bot that predicts stock prices.