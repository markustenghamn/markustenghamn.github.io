---
layout: post
title:  "How To Make A Scroll To Top Arrow With jQuery"
redirect_from:
   - /scroll-top-arrow-jquery
date:   2013-07-27 10:22:36 +0100
categories: best domain registrar
description: I wanted to add a scroll to top arrow to a site I was doing for a client recently as I think it looks nice and clever. It is fairly simple to do. To begin make sure you have jQuery added to your page,
---

I wanted to add a scroll to top arrow to a site I was doing for a client recently as I think it looks nice and clever. It is fairly simple to do. To begin make sure you have jQuery added to your page, the following code between your head tags will do the trick.

```
<pre lang="php">
    <script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.1/jquery.min.js"></script>
``` Now to add our arrow up button to the page I will use a 66px by 67px image as seen here.  
 (image removed)   
 To add this to the page using css I will use the following code between the head tags again. ```
<pre lang="php">

<style>
.uparrow {
    background: url('../images/uparrow.png');
    cursor: pointer;
    width: 66px;
    height: 67px;
    position: fixed;
    bottom: 30px;
    right:30px;
    display:none;
}
</style>
``` This basically tells the browser that the element with the uparrow class will use the image as a background and be in a fixed position 30 px from the bottom of the window and 30px from the right side of the window. We also use display:none; to hide the element. We can then add the element as a div to our page like so. ```
<pre lang="php">
<div class="uparrow" id="uparrow"></div>

``` Then to top it off we need to add the jQuery code which will make this thing function. Add this almost anywhere on the page, I stuck mine between the head tags but some prefer to put it at the bottom of the page to make the page load faster. ```
<pre lang="php">
<script type="text/javascript">
$(document).ready(function(){
    // fade in and fade out
    $(function () {
        $(window).scroll(function () {
            if ($(this).scrollTop() > 50) {
                $('#uparrow').fadeIn();
            } else {
                $('#uparrow').fadeOut();
            }
        });

        // scroll body to 0px on click
        $('#uparrow').click(function () {
            $('body,html').animate({
                scrollTop: 0
            }, 800);
            return false;
        });
    });

});
</script>
``` If it is not working and you need to debug it, try using [Firebug](http://getfirebug.com "Firebug").