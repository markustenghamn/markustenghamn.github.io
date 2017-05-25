---
layout: post
title:  "Laravel and keeping my bitcoin bot running forever"
redirect_from:
   - /laravel-and-keeping-my-bitcoin-bot-running-forever
date:   2016-01-23 18:12:31 +0100
categories: best domain registrar
description: .
---

So I was trying to figure out how to keep a script running forever as I want to constantly get social media updates and feeds from bitcoin exchanges when I use my bot to trade. Previously I would have done this with a bash script that would check for a tmp file that would be generated when the script was running and removed when it stopped, The script would be accessible as a console command in laraval making it easy to run. However, this could sometimes fail as well. I figured there must be a better way to achieve what I want to do. I headed over to Reddit and posted [my question](https://www.reddit.com/r/laravel/comments/42bato/keep_a_job_running_forever/) where I have gotten a lot of help before and got a really useful answer from [mbdjd](https://www.reddit.com/user/mbdjd). I had overlocked the cron section of the Laravel documentation that mentions the [withoutOverlapping()](https://laravel.com/docs/5.1/scheduling#preventing-task-overlaps) method which would basically start my script whenever it stopped and keep it running at all times.