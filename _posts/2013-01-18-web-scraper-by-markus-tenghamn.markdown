---
layout: post
title:  "A Web Scraper - By Markus Tenghamn"
redirect_from:
   - /web-scraper-by-markus-tenghamn
date:   2013-01-18 07:30:53 +0100
description: I recently finished the web applications course att Mälardalens University and decided to do the course project, which is usally for a group of 4-6 people, on my own. The course is part time spanning...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

I recently finished the web applications course att Mälardalens University and decided to do the course project, which is usally for a group of 4-6 people, on my own. The course is part time spanning about 8 weeks in total. The project was done for a company from Eskilstuna Sweden which is called Cleaning Services. Below is my report for the project as well as the presentation. I will also explain how the parsers were created. The overall project was a success and the company stated that it went beyond their expectations. I would be happy to receive any positive or constructive feedback!  
  
 Presentation slides: [https://docs.google.com/file/d/0B1Xh2EtX2fzCSzhFZXBUTWVPbVE/edit](https://docs.google.com/file/d/0B1Xh2EtX2fzCSzhFZXBUTWVPbVE/edit "Presentation Slides - Web Scraper by Markus Tenghamn")  
 Report: [https://docs.google.com/file/d/0B1Xh2EtX2fzCWnpKMGV5cU5qUVk/edit](https://docs.google.com/file/d/0B1Xh2EtX2fzCWnpKMGV5cU5qUVk/edit "Report - A Web Scraper by Markus Tenghamn")

## How do the parsers work?

  
 The parsers are written in PHP and utilize Curl and Xpath to find the information needed on the various web pages. My project required me to parse 5 different web pages with different structures, where the goal was to pull information from real estate listings which the company could then use for marketing. For this I created 5 different parsers which would all parse their own website, I could have made one dynamic parser to handle all the websites but felt that it may get a bit too complicated with the limited time I had to work on the project. As an example I will do a quick demonstration on how a similar parser might have worked on a site like craigslist which most people are most likely familiar with or have at least heard of it.  
  
 In this case I would start by going to craigslist.org and analyzing the URL, to make things easier I go to the city I want to parse information from, which is Houston, and select the category I want to parse which is computers for sale. I navigated there and copied the URL.  
> http://houston.craigslist.org/sya/

  
 This is the url I want my parser to start at. So I will now go ahead and write the code to initialize curl, connect to the specified url and return the html content of the page.  
  
```
<pre lang="php"><br></br><?php <br /?>
$ch = curl_init();<br></br><br></br>
curl_setopt($ch, CURLOPT_URL, 'http://houston.craigslist.org/sya/');<br></br>
curl_setopt($ch, CURLOPT_CUSTOMREQUEST, 'GET');<br></br><br></br>
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);<br></br>
curl_setopt($ch, CURLOPT_TIMEOUT, '30');<br></br><br></br>
if (curl_exec($ch) === false) {<br></br>
    echo 'Curl error: ' . curl_error($ch);<br></br>
} else {<br></br>
    $content = trim(curl_exec($ch));<br></br>
}<br></br>
```  
  
  
 After this code has run the content of the page will be stored in the $content variable and I can now begin to analyze the content with xpath. In this case I would want to find each listing link on the page on follow that link with a curl request. This can be done with the following code.  
  
```
<pre lang="php"><br></br>
$dom_document = new DOMDocument();<br></br>
@$dom_document->loadHTML($content);<br></br>
$dom_xpath = new DOMXpath($dom_document);<br></br>
$elements = $dom_xpath->query("//p[@class='row']//a/@href");<br></br><br></br>
foreach ($elements as $element) {<br></br>
    $url = $element->nodeValue;<br></br>
}<br></br>
```  
  
  
 After looking at the html structure of the site we can see that each listing url was inside a p element with the class row, thus I was able to use the following query in xpath to get all of the links on the page //p\[@class='row'\]//a/@href. Then we can loop through all of these elements one at a time and output the url which is the node value.  
  
 That is the basics of how the parsers work. The next step would be to make another curl request inside the foreach loop which would then parse the url found in the same manner and find the title, price, description and possibly the sellers information using xpath queries and possibly som regex depending on how the listing is formatted.  
  
 Thanks for reading, feel free to comment with any questions, criticism or feedback!