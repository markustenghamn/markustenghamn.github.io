---
layout: post
title:  "How To Find Twitter Followers Who Don't Follow Back With PHP"
redirect_from:
   - /find-twitter-followers-dont-follow-back-php
date:   2014-12-24 07:09:46 +0100
categories: best domain registrar
description: I was working on Anveto and decided to implement a feature which will display all users who you follow but who don't follow you back on t...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

I was working on [Anveto](http://anveto.com "Anveto") and decided to implement a feature which will display all users who you follow but who don't follow you back on twitter. This can be useful if you want to know which of your twitter followers who are not returning the favor and following you or who are simply not interested in you. If you don't know how to program you can sign up for this service over at [http://anveto.com](http://anveto.com "Anveto.com").  
  
 Let's begin! In order to do this you will need to download [twitteroauth](https://github.com/abraham/twitteroauth "Twitteroauth") and upload these files to your server. You will also need to create a twitter app and get your credentials [here](https://apps.twitter.com/ "Create a twitter app"). Once you have these you will be able to follow this guide. What I did was to create a SOCIAL class which will hold all of my functions. In this file you will need to include the twitter files with a statement like this, you may have to modify the path.

```
require_once('twitter/twitteroauth/twitteroauth.php');<br></br>
require_once('twitter/config.php');<br></br>
```  
  
 Then we can go ahead and create our first method which will get all of the ids of the people which are following the user. We can do this with the following method.  
  
```
<br></br>
function getTwitterFollowers($username) {<br></br>
        $connection = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET);<br></br>
        $res = $connection->get('https://api.twitter.com/1.1/followers/ids.json', array('cursor' => '-1', 'screen_name' => $username));<br></br>
        return $res->ids;<br></br>
}<br></br>
```  
  
 And in the same way we can create a method to get the ids of all of the people which the user follows. All we are changing here is the url being used.  
  
```
<br></br>
function getTwitterFollowers($username) {<br></br>
        $connection = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET);<br></br>
        $res = $connection->get('https://api.twitter.com/1.1/friends/ids.json', array('cursor' => '-1', 'screen_name' => $username));<br></br>
        return $res->ids;<br></br>
}<br></br>
```  
  
 Finally we can make a method which will take both of these results and find the people whom are not following us back.  
  
```
<br></br>
function getTwitterNotFollowingBack($username) {<br></br>
        $followers = $this->getTwitterFollowers($username);<br></br>
        $following = $this->getTwitterFollowing($username);<br></br>
        $tounfollow = array();<br></br>
        foreach ($following as $f) {<br></br>
            if (!in_array($f, $followers)) {<br></br>
                $tounfollow[] = $f;<br></br>
            }<br></br>
        }<br></br>
        return $tounfollow;<br></br>
}<br></br>
```  
  
 Now we are basically done, we can call this method and it will give us all of the ids of the people we want to unfollow. We could then make a request with these ids to make an unfollow request or we could simply present these to the user and let them click a button to do the unfollowing manually as there are most likely some people who you would still want to follow even if they don't follow back. To see the list of ids you could do this.  
  
```
<br></br>
$social = new SOCIAL();<br></br>
$res = $social->getTwitterNotFollowingBack('markustenghamn');<br></br>
echo '';<br></br>
print_r($res);<br></br>
echo '';<br></br>
```  
  
 Finally we could make a method which will get the details by a users id, such as their screen name, with a method like this. Please not that this method takes an array of ids and not a string. Basically the above $res variable could be used in order to return the information of all users who don't follow back.  
  
```
<br></br>
function getTwitterUserDetails($userids = array()) {<br></br>
            $string = "";<br></br>
            $first = true;<br></br>
            foreach ($userids as $id) {<br></br>
                if ($first) {<br></br>
                    $string = $id;<br></br>
                    $first = false;<br></br>
                } else {<br></br>
                    $string .= ",".$id;<br></br>
                }<br></br>
            }<br></br>
            $connection = new TwitterOAuth(CONSUMER_KEY, CONSUMER_SECRET);<br></br>
            $res = $connection->get('https://api.twitter.com/1.1/users/lookup.json', array('cursor' => '-1', 'user_id' => $string));<br></br>
            return $res;<br></br>
}<br></br>
```