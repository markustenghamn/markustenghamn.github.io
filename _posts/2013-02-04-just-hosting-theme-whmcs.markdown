---
layout: post
title:  "Just Hosting Theme for WHMCS"
redirect_from:
   - /just-hosting-theme-whmcs
date:   2013-02-04 00:38:07 +0100
categories: best domain registrar
description: This post continues explaining how to setup the Just Hosting Theme for WHMCS going from my original post:
---

This post continues explaining how to setup the Just Hosting Theme for WHMCS going from my original post: [Getting Started With The Just Hosting Theme for WHMCS](http://markustenghamn.com/started-hosting-theme-whmcs "Getting Started With The Just Hosting Theme for WHMCS") Here is a recap of what is needed to set everything up

### What you will need

- [WHMCS](http://www.whmcs.com/members/aff.php?aff=3089 "WHMCS")
- [Just Hosting WHMCS Theme - http://themeforest.net/item/just-hosting/408834](http://themeforest.net/item/just-hosting/408834?ref=bigideaguy "Just Hosting WHMCS Theme")
- [Hosting and a domain](http://zunem.com "Zunem Hosting")
 
  
 Sponsor: <script language="javascript" src="http://www.anrdoezrs.net/placeholder-6786931?target=_top&text=%3Clink%3EWeb+Hosting+for+only+1+Penny%21%3C%2Flink%3E&mouseover=N" type="text/javascript"></script>  
  
 So going from where we were in our previous post the next thing we will need to do is change the logo, unless of course you want to call your site just hosting. The logo is located under images/logo.png and we will also find the small version of the logo under images/logo\_small.png. I am not sure of the exact font used, it may be in the documentation but I did not look for very long. I used a similar font called Banque Gothique RR Extra Bold Extra Condensed and applied a text reflection, if you don't know how to apply a reflection you can follow [this free photoshop tutorial](http://www.photoshopessentials.com/photoshop-text/text-effects/text-reflection/ "Creating Text Reflections With Photoshop") which should also be similare to what you would do in gimp or any other image editor. For the small logo I changed the size of the logo.png to 107px wide and kept the proportions. Now I finally have a logo that matches my domain, but I need to change the title of the page that still says Just Hosting. Opening up our index.html file we will find the title tags on line 5 and in between these tags you will see "Just Hosting", I changed this to say "Zunem - Professional hosting services at affordable prices", adding the name of my site and adding the slightly ajusted version of the slogan that comes with the template. If this is going to be a professional company you may want to consider using a different slogan that sets you apart from the template and anyone else who might be using it. As I am doing this I am simply going from top to bottom in the index.html file. After fixing the title I scrolled down to the bottom of the area and noticed some stylesheets and some where highlighted by my IDE letting me know that they were missing. Here is what I was looking at. ```
<pre lang="html">
<link href="reset.css" rel="stylesheet" type="text/css"></link><link href="screen.css" rel="stylesheet" type="text/css"></link><link href="fancybox.css" rel="stylesheet" type="text/css"></link>
``` The reset.css and fancybox.css were not in the folder I am using for the theme and they did not seem to be in the template folder in whmcs. By the way I am using the [PHPStorm IDE from Jetbrains](http://www.jetbrains.com/phpstorm/ "PHPStorm from Jetbrains") which is an awesome tool when working with PHP. After removing the stylesheets I was simply left with this. ```
<pre lang="html">
<link href="screen.css" rel="stylesheet" type="text/css"></link>
``` After this you should probably upload these and commit your changes to make sure it didn't affect your site in any way, if it does then add the code back again and don't worry about removing them. Next I find the block of code which defines the header of my page and defines the text which is shown there. This is what the code should look like: ```
<pre lang="html">
<div class="holder">
		<h1><a href="/">Just Hosting</a></h1>
		<h2>Professional hosting services affordable prices</h2>
		<p>Sales enquiry: +44 0123 456 789</p>
		<div class="clear"></div>
</div>

``` I will simply update this with the information for my website and since I don't have a sales phone number yet I will change the number to my email instead. At this time I'm not worrying about getting spam but if you want to prevent bots from getting your email you can read my earlier post: [Hide your email from parsers and email spiders](http://markustenghamn.com/hide-email-parsers-email-spiders "Hide your emial from parsers and email spiders"). Here is the code I have after changing the text. ```
<pre lang="html">
	<div class="holder">
		<h1><a href="/">Zunem</a></h1>
		<h2>Professional hosting services at affordable prices</h2>
		<p>Sales enquiry: sales@zunem.com</p>
		<div class="clear"></div>
	</div>

``` After this we will find the boxes or plans which we changed in the previous guide, these are commented to make it easy to understand. I like the plans the way they are and will leave the text the way it is. If you are new to html you should simply be able to edit the text between the tags and not have to worry about messing up the layout. Here is the code for the first block, the other two blocks are pretty much the same. ```
<pre lang="html">
<div class="box" id="one">

				<h3>STARTER</h3>
				<ul><li title="item-a">100 MB space</li>
					<li title="item-b">1GB Monthly Transfer</li>
					<li title="item-c">2 SQL databeses</li>
					<li title="item-d">Unlimited email accounts</li>
					<li title="item-e"><strong>FREE!</strong> for first 3months</li>
				</ul><div class="bottom_part"> 
					
					<span class="price">$2.00</span>
					
					<a class="button" href="http://zunem.com/whmcs/cart.php?a=add&pid=1">Register</a> 
					<div class="clear"></div>
				</div>
				
			</div>

``` That sums up most of what is needed to be edited, there is more text further down on the page which can easily be found and changed in the same matter as we have done so far. You can find the text by viewing your template as a normal website and copying the text you want to edit and then using the find function in your editor to find where the text is in the HTML. Also don't forget to change the text in your footer. Getting the forms working
-------------------------

 So there are two forms at the bottom of the page. The registration form will send you an email via the register.php file and the login for customers does nothing. Unfortunately the registration form doesn't really work with whmcs but we can change it slightly to accommodate for this. I removed the username and email fields and left the domain field. Then I changed it so that the form would take the customer to the order form for the starter package. Let me explain what I did, here is the code I started with. ```
<pre lang="html">
<h4>REGISTER NEW ACCOUNT</h4>
		
```<form action="#" id="register" method="post"> <input id="plan" name="plan" type="hidden" value="starter"></input><div class="input_section">  <input id="domain" name="domain" onblur="if(this.value==''){this.value='Please enter domain'}" onfocus="if(this.value=='Please enter domain'){this.value=''}" type="text" value="Please enter domain"></input>if you don´t provide extension we use our domain

<div class="clear"></div> </div><div class="input_section">  <input id="username" name="username" onblur="if(this.value==''){this.value='Choose username'}" onfocus="if(this.value=='Choose username'){this.value=''}" type="text" value="Choose username"></input>This will be your identification in our system

<div class="clear"></div> </div><div class="input_section">  <input id="email" name="email" onblur="if(this.value==''){this.value='Your email'}" onfocus="if(this.value=='Your email'){this.value=''}" type="email" value="Your email"></input>We wan´t use it for marketing purposes

<div class="clear"></div> </div><div class="last_one"> <input class="button" id="register_button" name="register_button" type="submit" value="Submit"></input>By registering you accept terms and conditions

 Please Enter Valid Data

  Registration successful

<div class="clear"></div> </div> </form> To begin with I opened up the javascript file which currently prevents the form from going to a new page, this is found under js/site.js and I removed the following code: ```
<pre lang="html">
$(function() {
	$("#register_button").click(function() {
	
		
		var domain = $("#domain").val();
		var username = $("#username").val();
		var email = $("#email").val();
		var plan = $("#plan").val();
		var dataString = '&domain='+ domain + '&username=' + username + '&email=' + email + '&plan=' + plan;
		
		
		if(domain=='' || username=='' || email=='' || domain=='Please enter domain' || username=='Choose username' || email=='Your email')
					{
						$('.register_success').fadeOut(200).hide();
						$('.register_error').fadeOut(200).show();
						$('.register_terms').fadeOut(200).hide();
					}
		else
			{
				$.ajax({
				type: "POST",
				url: "register.php",
				data: dataString,
				success: function()
				
				{
					$('.register_success').fadeIn(200).show();
					$('.register_error').fadeOut(200).hide();
					$('.register_terms').fadeOut(200).hide();
				}
			});
	}
return false;
});
	});

``` By removing this we can now submit our form like we want. I then went ahead and changed the form action on the index.html page, here is my current code for the form which now has my whmcs url. ```
<pre lang="html">

```<form action="http://zunem.com/whmcs/cart.php?a=add&pid=1" id="register" method="post"> Then for the domain field I changed the name to sld as this is what whmcs used to get the domain when entered through a form. Unfortunately my solution won't account for someone typing in a domain extension, this would require either some javascript or php. Here is my domain field with the updated name. ```
<pre lang="html">
<input id="domain" name="sld" onblur="if(this.value==''){this.value='Please enter domain'}" onfocus="if(this.value=='Please enter domain'){this.value=''}" type="text" value="Please enter domain"></input>
``` I also removed some of the errors and success messages at the bottom, the code I ended up with looked like this. ```
<pre lang="html">
<div class="content_box" id="register_box">
		<h4>REGISTER NEW ACCOUNT</h4>
		<form action="http://zunem.com/whmcs/cart.php?a=add&pid=1" id="register" method="post">
		<input id="plan" name="plan" type="hidden" value="starter"></input><div class="input_section">
				
				<input id="domain" name="sld" onblur="if(this.value==''){this.value='Please enter domain'}" onfocus="if(this.value=='Please enter domain'){this.value=''}" type="text" value="Please enter domain"></input><p class="note">if you don´t provide extension we use our domain</p>
				<div class="clear"></div>
			</div>
			<div class="last_one">
				<input class="button" id="register_button" name="register_button" type="submit" value="Submit"></input><p class="note register_terms">By registering you accept terms and conditions</p>

				<div class="clear"></div>
				
			</div>
		</form>
	</div>

``` Now to fix our login form, this will be a little easier as it matches with the input fields that whmcs and has no javascript added to it. This is the code I start with in the index.html file. ```
<pre lang="html">
<div class="content_box" id="login_box">
		<h4>EXISTING CUSTOMERS</h4>
			<form action="#" method="post">
				
				<div class="input_section">
					
					<div class="input_fields">
						<input id="login_username" name="login_username" onblur="if(this.value==''){this.value='Username'}" onfocus="if(this.value=='Username'){this.value=''}" type="text" value="Username"></input></div>
					<div class="clear"></div>
				</div>
				<div class="input_section">
					
					<div class="input_fields">
						<input id="password" name="password" onmousedown="javascript:this.value='';" type="password" value="passworde"></input></div>
					<div class="clear"></div>
				</div>
					<div class="input_section">
					<p class="note">If you forgot your passoword just enter your email and click <a href="#">here</a></p>
					<div class="clear"></div>
					</div>
					<div class="last_one">
						
					<input class="button" id="login" name="login" type="submit" value="Submit"></input><div class="clear"></div>
					</div>
					
			
			</form>	
		</div>

``` All I have to do here is change the form action from # to link to the dologin.php file which handles the login in whmcs. The link I end up with is http://zunem.com/whmcs/dologin.php and here is the final code. ```
<pre lang="html">
<div class="content_box" id="login_box">
		<h4>EXISTING CUSTOMERS</h4>
			<form action="http://zunem.com/whmcs/dologin.php" method="post">
				
				<div class="input_section">
					
					<div class="input_fields">
						<input id="login_username" name="login_username" onblur="if(this.value==''){this.value='Username'}" onfocus="if(this.value=='Username'){this.value=''}" type="text" value="Username"></input></div>
					<div class="clear"></div>
				</div>
				<div class="input_section">
					
					<div class="input_fields">
						<input id="password" name="password" onmousedown="javascript:this.value='';" type="password" value="passworde"></input></div>
					<div class="clear"></div>
				</div>
					<div class="input_section">
					<p class="note">If you forgot your passoword just enter your email and click <a href="#">here</a></p>
					<div class="clear"></div>
					</div>
					<div class="last_one">
						
					<input class="button" id="login" name="login" type="submit" value="Submit"></input><div class="clear"></div>
					</div>
					
			
			</form>	
		</div>

``` That's all the customization you need to get up and going with your site. I hope the guide has helped, please leave a comment with any feedback or questions! </form>