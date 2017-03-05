---
layout: post
title:  "Post updates to Slack with PHP"
redirect_from:
  - /post-updates-to-slack-with-php
date:   2016-09-14 20:42:47 +0100
categories: slack php
description: A guide on how to use PHP to post automated updates directly to slack. Example code and images are provided to help the user.
---

## What is Slack

So if you haven't heard of Slack yet then you should probably [check it out](http://slack.com). I use it for work to communicate more easily with colleauges and also organize information better, I even set one up for myself at tenghamn.slack.com. Slack is free to use with premium features starting at $8 per user.

## How to create a Slack bot

You will need to pay for Slack in order to create bots I believe. Bots are referred to as integrations on Slack and are managed in the same way as all of your other integrations. Simply go to your [custom integrations page](http://slack.com/apps/manage/custom-integrations) and click on Bots to get started.

Once you get to the next page simply click on "Add Configuration" on the left hand side in order to create your first bot.

![Create a Slack Bot](/assets/images/posts/2016-09-14-post-updates-to-slack-with-php/slackbots-with-php-1-min.jpeg)

I'm just going to name my bot @myfirstbot, this is the username that will be used to refer to your bot in channels and will be the username that your messages are posted from (with some exceptions as you get into more advanced stuff which we won't cover in this post).

![slackbots-with-php-2](/assets/images/posts/2016-09-14-post-updates-to-slack-with-php/slackbots-with-php-2-min.jpeg)

You will then get to the settings page for your bot that you just created. You can give your bot a name here and upload an icon. I will just go ahead and keep mine the way it is. What we are most interested in here is the **API Token** which we will be using to authorize our requests as our bot. Please keep in mind that this API Token could be used to read anything that is posted to your channels and even post messages and change settings, keep it safe just like you would with a username and password.

![slackbots-with-php-3](/assets/images/posts/2016-09-14-post-updates-to-slack-with-php/slackbots-with-php-3-min.jpeg)

Now the final step, let's go ahead and invite this bot to one of our channels in the same way that we would add a normal user. You will see that the bots name now shows up when you type **/invite**.

![Invite a Slack bot to a channel](/assets/images/posts/2016-09-14-post-updates-to-slack-with-php/slackbots-with-php-4-min.jpeg)

## How to post to Slack with PHP

So lets get to the good stuff. How do we make this bot post a message to Slack?

First think we need to do is enter our message and set up our array of post variables that we want to send to Slack.

```
// The message you would like to send

$message = "Hello this is a test!";

$parameters = array(

    // Simply type your channel name, make sure your bot is invited

    'channel' => 'testing',

    // Good idea to decode

    'text' => html_entity_decode($message),

    'as_user' => 'true'

);

```

Channel here is simply the name of the channel you created and invited your bot to. Text is the message you want to send, I try to make sure to always html_entity_decode my message just to make sure its formatted correctly. The as_user variable is an optional parameter and all it says is that we want to post as our bot called @myfirstbot.

Now on to the next part, we will need to enter the Slack URL which is always the same with the exception of the last part which is the method you want to call.

<pre class="language-php">// Use groups.list if you run into channel_not_found

// issues with private channels, make sure your bot is invited

// chat.postMessage is the method

$url = "https://slack.com/api/"."chat.postMessage";

// replace MY-API-TOKEN with the API Token for your bot

$parameters['token'] = "MY-API-TOKEN";</pre>

In this case we want to call the [chat.postMessage](https://api.slack.com/methods/chat.postMessage) method. I am also adding our **API Token** which we discussed earlier in this article. Simply replace MY-API-TOKEN with the token that you got when creating your bot.

Finally we will need to actually make the request to the Slack API, for this I have created a method which will use Curl if your installation supports it and fall back to file_get_contents if Curl isn't available. This should work on most web hosts.

```
if (function_exists('curl_version')){

    // Use curl if we have it installed

    $ch = curl_init();

    curl_setopt($ch, CURLOPT_URL, $url);

    curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);

    curl_setopt($ch, CURLOPT_TIMEOUT, 30);

    curl_setopt($ch, CURLOPT_POST, true);

    curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);

    curl_setopt($ch, CURLOPT_POSTFIELDS, $parameters);

    $result = curl_exec($ch);

    curl_close($ch);

} else {

    // If we don't have curl we use file_get_contents

    $post_data = http_build_query($parameters);

    $result = file_get_contents($url, false, stream_context_create(array(

        'http' => array(

            'content' => $post_data,

            'method' => 'POST',

            'protocol_version' => 1.1,

            'header' => "Content-type: application/x-www-form-urlencoded\r\n" .

                "Content-length: " . strlen($post_data) . "\r\n" .

                "Connection: close\r\n"

        ),

    )));

}

// Echo our result, this is returned as a

// json encoded string. Use json_decode($result, true) to turn it into an array

echo $result;

```

The if statement here checks if we have curl activated. If we have curl we make a curl request to the Slack API, if we don't have curl we do the same thing but with file_get_contents instead. Both methods work in pretty much the same way and you should get the same result. The $result variable now holds the response from the Slack API. If it worked you should see something like the following.

```

{"ok":true,"channel":"XXXXXXXXX","ts":"1473880390.000004","message":{"type":"message","user":"UXXXXXXXX","text":"Hello this is a test!","bot_id":"XXXXXXXXX","ts":"1473880390.000004"}}

```

If you get a different result like channel_not_found please make sure you have followed all the steps carefully. You may have missed inviting your bot to the Slack channel or another step. I have tried to comment the code in order to make it easier to understand and debug.

A message from your bot should now be in the channel you created earlier, success!

![A bot posted a message to Slack](/assets/images/posts/2016-09-14-post-updates-to-slack-with-php/slackbots-with-php-5-min.jpeg)

The things you could use this for are endless. I personally use it to keep track of things so that I can get instant notifications. It ranges from everything from when I get an important email to when my server starts getting a bit hot to when a new device has connected to my wifi network. Slack notifications are easy to configure and you are not limited to one way messages. I have made bots that communicate with users and ask questions, store answers and process them on a server. The things you can do with Slack bots are endless, especially now that Slack has added buttons to the messages that bots can post to Slack.

You will find all the source code on **GitHub** here: [https://gist.github.com/markustenghamn/948a36d9ffffca8a1e0212f901bf8c8e](https://gist.github.com/markustenghamn/948a36d9ffffca8a1e0212f901bf8c8e)

If you are interested in exploring the Slack API and everything it can do you will find all of the [documentation for Slack here](https://api.slack.com/methods).

This was also posted on Steemit by me (@tenghamn) [here](https://steemit.com/php/@tenghamn/post-updates-to-slack-with-php#comments).

