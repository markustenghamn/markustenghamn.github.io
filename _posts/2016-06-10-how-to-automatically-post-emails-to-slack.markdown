---
layout: post
title:  "How to automatically post emails to Slack"
redirect_from:
   - /how-to-automatically-post-emails-to-slack
date:   2016-06-10 20:08:24 +0100
categories: best domain registrar
description: .
---

[Slack](https://slack.com) is an amazing service, it is basically a dumbed down (in a good way) and simplified IRC client which may sound bad but it makes it super efficent and great for work. It is also an easier sell to non-IT coworkers who may laugh at the idea of using an IRC server for work. Anyways, sometimes it can be a great idea to post email updates directly to Slack. I have done this for a server which runs CSF & LFD (our server firewall) which sends out notifications when ips get banned, suspicious files are found or if something is just not right with the server. Anyways, to begin you will need to install the email app for your Slack team which can be found here <https://slack.com/apps/A0F81496D-email>. This app is part of slack and will basically create an email like something@yourteam.slack.com which you then can send email to. The email app will work like a bot, so once configured you can set a name and even upload an image for your bot. Once you have your email you can either change the email in the settings of whatever you are sending from or you can go to your email client, preferably the web based version if you have gmail or outlook or something like that. Here you will want to configure a filter or rule that says if these conditions match then I want to forward this email to Slack. For me the conditions was the email that these emails were being sent from. I decided to forward those emails coming from LFD but also keep a copy in the mailbox just in case. If you have attachements like images and what not it should also work as long as the files are not more than 25mb. Now, try sending an email. The email will appear as a snippet usually along with any files you have attached. Now keep on slacking! Hope you enjoyed the post.