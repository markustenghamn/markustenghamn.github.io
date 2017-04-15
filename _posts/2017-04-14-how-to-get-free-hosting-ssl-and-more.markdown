---
layout: post
title:  "How to get Free Hosting, SSL and more"
date:   2017-04-14 18:15:00 +0100
categories: Hosting SSL free Domains DNS
description: During the past month I have been shutting down servers and reducing costs as I realized how much
             I actually spent on my hobby projects that I love to work on. There is a cheaper alternative to
             everything I was doing and my blog is a great example of one such freebie.
---

During the past month I have been shutting down servers and reducing costs as I realized how much
I actually spent on my hobby projects that I love to work on. There is a cheaper alternative to
everything I was doing and my blog is a great example of one such freebie.

## Free DNS

So many of you have probably heard of [CloudFlare](https://www.cloudflare.com/) which can not only be used
as a free dns manager but it can also provide security, SSL encryption, and reliability (all for free). My
blog uses it at the moment and I love it. If the server that I am currently hosting my blog on were to go down
then CloudFlare will show a cached backup of my website until it comes back up again. It also reduces the load
time for all my visitors which is another big plus.

If you want to have a free DNS manager but don't want to use CloudFlare then I would highly recommend registering
domains with [NameSilo](https://www.namesilo.com/pricing.php?rid=ee81e92mn) where domains are not only cheaper
than any other place that I have found but they also provide free DNS.

## Free SSL

So once again I am going to bring up [CloudFlare](https://www.cloudflare.com/), once you add your domain you can
have it use https traffic for port 80 on your site (normal HTTP traffic). Now you might say that this only protects
the client traffic from the client to CloudFlare and not the traffic between CloudFlare and the website. You would be
completely correct in saying that and that is why I have another alternative that you can use on it's own or with CloudFlare.

Head over to [LetsEncrypt](https://letsencrypt.org/) where you can get a free SSL certificate that you can install on your
server. This is offered completely free and will work with pretty much all modern browsers and devices, see the compatibility list
[here](https://letsencrypt.org/docs/certificate-compatibility/).

## Free Hosting

If you want to run a blog like I do or any other static web page for that matter you can do what I have done. I don't pay
anything at all for my blog because I use [Github Pages](https://pages.github.com/). It is a bit more complicated than your
average Wordpress site but not much more complicated. All it is is a different interface and a different way of publishing
your website. If you know how to write HTML or Markup then all you have to do is learn how to use [Github Desktop](https://desktop.github.com/)
and you are good to go, even my mother can use Github Pages!

Now you may need something more complicated, with a database, maybe some server side processing. Well there is nothing
completely free that I can offer you, but almost. Did you know that both Google and Amazon offer free tier pricing on their
cloud solutions? You can read more about [Google Free Tier](https://cloud.google.com/free/) which won't really cost you anything
if you are just building test applications with a low amount of traffic, like me. So instead of paying $60+ per month for a dedicated
server I will now be moving to Google where I will pay nothing for at least 12 months. I also like [Amazons Free Tier](https://aws.amazon.com/free/)
but they can get a bit pricier when testing stuff as Amazon charges by the started hour and Google charges by the minute. Both
companies offer great support if you run into trouble when trying to configure your apps.

## Free Domains

You can also get free domain names with extensions like .tk which you can register at [dot.tk](http://www.dot.tk/). You can keep using it as long
 as you get 25 hits to your domain in 90 days which should not be hard if you are adding content to it and telling others about it. Personally I think
it's better to just get a .com for $8.99 per year with a registrar like [NameSilo](https://www.namesilo.com/pricing.php?rid=ee81e92mn). A .tk will work fine
and may be good for a personal blog but a .com is more professional and easier to remember. Since .tk is a free domain some may
connect the .tk extension with abuse/scams but it could just be something I have associated with .tk in the past.
