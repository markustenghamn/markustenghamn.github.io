---
layout: post
title:  "Hide your email from parsers and email spiders"
redirect_from:
   - /hide-email-parsers-email-spiders
date:   2013-01-03 05:38:21 +0100
description: Email spiders and parsers are constantly at work scanning pages for any valuable emails that they can find. Usually your email will be used to send you some kind of advertisement or scam. This is a hu...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

Email spiders and parsers are constantly at work scanning pages for any valuable emails that they can find. Usually your email will be used to send you some kind of advertisement or scam. This is a huge problem if you want to avoid these emails.  
  
 A simple solution which is very common is to replace the @ sign in the email with a # or simply (at). This will deter most email spiders, however there are smarter spiders programmed to recognize common replacements of the @ sign and still find your email.  
  
 One of the best solutions if you want to display your email somewhere is to use javascript to generate it when the page has loaded or simply create an image of your email and use it instead. Why is javascript a good solution? Most spiders will download the html content of your page and then parse it for any emails. Javascript will not execute when this is done and therefore your email will most likely not be captured.  
  
 This can all be done with a very few lines of code, especially if you are using jquery on your site. Here is an example of how it could be done.

```
<pre lang="html"><br></br><script type="text/javascript"><br />
$(document).ready(function(){<br />
    var a = "web";<br />
    var b = "master";<br />
    var c = "marks";<br />
    var d = "pixel";<br />
    $('#email').text(a + b + "@" + c + d + ".com");<br />
});<br />
</script><br></br><br></br><span id="email"></span><br></br><br></br>
```  
  
 This code will print out webmaster@markspixel.com where I have a span with the id email. The code is executed when the document is ready which will not execute for a email spider. I have split up my email into several strings and then i am simply adding the strings back together and telling the page that my combied string should be the text for the element with id email.  
  
 If this is not working for you then you may need to include jquery. You can do this by adding the following code between the tags on your page.  
  
`<pre lang="html"><br></br><script src="//ajax.googleapis.com/ajax/libs/jquery/1.4.3/jquery.min.js"></script><br></br>`