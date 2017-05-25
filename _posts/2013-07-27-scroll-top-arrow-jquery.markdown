---
layout: post
title:  "How To Make A Scroll To Top Arrow With jQuery"
redirect_from:
   - /scroll-top-arrow-jquery
date:   2013-07-27 10:22:36 +0100
categories: best domain registrar
description: I wanted to add a scroll to top arrow to a site I was doing for a client recently as I think it looks nice and clever. It is fairly simple to do. To begin make sure you have jQuery added to your page,...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

I wanted to add a scroll to top arrow to a site I was doing for a client recently as I think it looks nice and clever. It is fairly simple to do. To begin make sure you have jQuery added to your page, the following code between your head tags will do the trick.

`<pre lang="php"><br></br><script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js"></script><br></br>`  
 Now to add our arrow up button to the page I will use a 66px by 67px image as seen here.  
  
 (image removed)   
  
 To add this to the page using css I will use the following code between the head tags again.  
```
<pre lang="php"><br></br><br></br><style><br />
.uparrow {<br />
    background: url('../images/uparrow.png');<br />
    cursor: pointer;<br />
    width: 66px;<br />
    height: 67px;<br />
    position: fixed;<br />
    bottom: 30px;<br />
    right:30px;<br />
    display:none;<br />
}<br />
</style><br></br>
```  
 This basically tells the browser that the element with the uparrow class will use the image as a background and be in a fixed position 30 px from the bottom of the window and 30px from the right side of the window. We also use display:none; to hide the element.  
  
 We can then add the element as a div to our page like so.  
`<pre lang="php"><br></br><div class="uparrow" id="uparrow"></div><br></br>`  
 Then to top it off we need to add the jQuery code which will make this thing function. Add this almost anywhere on the page, I stuck mine between the head tags but some prefer to put it at the bottom of the page to make the page load faster.  
```
<pre lang="php"><br></br><script type="text/javascript"><br />
$(document).ready(function(){<br />
    // fade in and fade out<br />
    $(function () {<br />
        $(window).scroll(function () {<br />
            if ($(this).scrollTop() > 50) {<br />
                $('#uparrow').fadeIn();<br />
            } else {<br />
                $('#uparrow').fadeOut();<br />
            }<br />
        });<br />
<br />
        // scroll body to 0px on click<br />
        $('#uparrow').click(function () {<br />
            $('body,html').animate({<br />
                scrollTop: 0<br />
            }, 800);<br />
            return false;<br />
        });<br />
    });<br />
<br />
});<br />
</script><br></br>
```  
 If it is not working and you need to debug it, try using [Firebug](http://getfirebug.com "Firebug").