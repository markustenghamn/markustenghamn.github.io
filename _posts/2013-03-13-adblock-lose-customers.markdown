---
layout: post
title:  "Why Adblock Plus Could Cause You To Lose Customers"
redirect_from:
   - /adblock-lose-customers
date:   2013-03-13 20:56:03 +0100
categories: best domain registrar
description: I was recently writing a review on Swedish web hosts over on my Swedish version of this blog and realized that Adblock Plus was preventing me from mak...
---

I was recently writing a review on Swedish web hosts over on my Swedish version of this blog and realized that Adblock Plus was preventing me from making a purchase at [Surftown](http://surftown.se/ "Surftown"), one of the swedish hosts. The problem occured because Surftown uses a site called adform.net to track events on their site it seems and since this ad network, or whatever adform.net is, also displays ads it is then blocked by Adblock Plus. This resulted in a error on their checkout page which made it seem like nothing happened when I clicked the order button. The page was just blank and nothing changed, no error, nothing. At first I thought, oh well, I can move on to another host and just skip my review of Surftown but the error got me interested and I had to go back. I suspected the error might be caused by Adblock Plus and tried disabling it for the checkout page and then everything worked. I then went back to see what errors I received in my web console when nothing happened. It seems like the page tried to create a object from the adform.net page and since it could not reach that page the object failed to initialize causing the errors. A good implementation of this would have a fallback for this but Surftown does not, which causes them to lose any customer using Adblock Plus who doesn't know why it's not working and doesn't bother to find out why. I put the blame here on adform.net however since I assume they either did the implementation or provided the code, but I have no idea if I am right and it was hard to find anything concrete on what adform.net is. (image removed)

How does this affect you?
-------------------------

 If you run an ecommerce site which uses ajax during checkout which has implemented tracking or any calls to a site which might be blocked by Adblock Plus you may want to take a look at your code just to make sure you are not also losing customers. If you see the same issues on your site you need to consider adding a fallback for when the tracking doesn't work so that your checkout page always works.