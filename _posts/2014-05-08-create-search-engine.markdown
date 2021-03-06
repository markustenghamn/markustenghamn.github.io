---
layout: post
title:  "How to create a search engine"
redirect_from:
   - /create-search-engine
date:   2014-05-08 09:37:23 +0100
description: (image removed) This is a quick guide on how you can create your own search engine similar...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

[(image removed)](http://markustenghamn.com/?iproduct=5247)  
  
 This is a quick guide on how you can create your own search engine similar [Google](http://google.com "Google"), [Yahoo](http://yahoo.com "Yahoo") or [Bing](http://bing.com "Bing"). We will be using PHP and MySQL to create a search page, a results page and a simple parser to index pages. We will begin by creating the basic structure for the site as seen in the image below.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/1.png)  
  
 Here I have created a folder called "searchengine" with two php files called "index.php" and "results.php". The index file simply contains the search form which will make a post request to the results page which will in turn display any relevant search results. I have also added an assets folder with a css folder which holds a stylesheet to format our pages. Let's get started, below I have created a basic layout for the main page.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/2.png)  
  
 We simply have a heading which says "My Search Engine" and then our search box which is a form with a text field and a submit button. Clicking the button will post the search value to our results page and allow us to use it. Below I have created the basic structure for the results page.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/3.png)  
  
 Here we have an array of errors and an array of results. The results array is untouched and empty for now, but the errors array will contain any errors that we may need to show in case no search term is entered or if the user accesses the page in the wrong way.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/4.png)  
  
 In the picture above you will see more of the code for the results page. Here we check for any errors, if there are errors we display them, otherwise we display any search results. If there are no search results we will have no output on this page at the moment.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/6.png)  
  
 We can add an example search result to our array using the code shown above. If we refresh the page you will see the result displayed.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/7.png)  
  
 At this time I have gone ahead and added some simple formatting to the site in order to make results more readable. For this example it will be enough. If you are thinking about launching a search engine you would probably want to consider hiring a designer to make something really nice.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/8.png)  
  
 I have created a classes folder with a Database class. This will allow us to both store web pages and then retrieve them when a user searches for the relevant terms.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/9.png)  
  
 Above you will see the basic structure for the database, in this example I will only store the title, description and url of each page. A large search engine like Google will most likely store many more parameters in order to provide users with better results.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/10.png)  
  
 Above I have used a simple MySQL query to create a sample result so that we can test the current version of our script.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/11.png)  
  
 Now if we do a simple search for the word "title" you will see that we get the search result we just added to our database. If you perform a search for another term you will find that the results page will not display any results.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/12.png)  
  
 In the same way as we did the search page we will now create a parser page which will allow us to index urls. I will once again create a form with one text field where an url can be added. Below is the code that will handle the added url and parse the title and description and insert it into the database.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/13.png)  
  
 Here we use the url along with curl to grab the content of the page, which is the DOM or HTML. We then use DOMDocument along with XPath to parse the content for the information we want. After we have found it we insert the information and the url into our database so that it can be searched at a later time. If you would like to expand on this example in the future I would recommend using a cron job which picks a url from the database and finds all the links on the page and then indexes those urls. In this way you keep indexing new pages and adding them to your results, eventually you could index every single page out there. This is the basic concept of a search engine spider.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/14.png)  
  
 Above is an example of the code used to insert our parsed page information into our database. This query is done with PDO.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/15.png)  
  
 So what we can do now is start entering urls that will be indexed. I will use some urls from my blog to add some search results.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/16.png)  
  
 As you can see the database is now populated with 3 of my links.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/05/17.png)  
  
 And we end up with this, our results page after a search for "Markus Tenghamn". You can try the live search engine (only populated with these three results) at the following url: [http://markustenghamn.com/searchengine/](http://markustenghamn.com/searchengine/ "Markus Tenghamn Search Engine")  
  
 Get the complete source code along with the database structure for only $5  
  
[(image removed)](http://markustenghamn.com/?iproduct=5247)