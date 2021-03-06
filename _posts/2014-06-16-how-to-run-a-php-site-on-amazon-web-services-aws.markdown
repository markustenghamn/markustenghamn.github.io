---
layout: post
title:  "How To Run A PHP Site On Amazon Web Services (AWS)"
redirect_from:
   - /how-to-run-a-php-site-on-amazon-web-services-aws
date:   2014-06-16 13:00:07 +0100
description: The other day I got started with AWS for the first time and it was much easier than I thought but still a little pricy if you are only looking for regular web hosting. However, if you need something t...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post manually so it may be hard to read but I have left it here as a reference.**

The other day I got started with AWS for the first time and it was much easier than I thought but still a little pricy if you are only looking for regular web hosting. However, if you need something that will scale quickly [AWS](http://aws.amazon.com/ "AWS") can be a great option. My initial cost is nothing since I am getting Free Tier pricing which will give me 750 hours free, or about one month. Estimated costs after the free tier was $34 per month.  
  
 So how do we get started? [AWS](http://aws.amazon.com/ "AWS") has a lot of different things to choose from, do you need EC2? S3? And what is Glacier?  
  
 To be honest I am still not an expert on any of the different services but what I do know is that you will most likely need more than one, this is where [Elastic Beanstalk](http://aws.amazon.com/elasticbeanstalk/ "Elastic Beanstalk") comes into play! This is basically an all in one package for all the services you will need, to me it is easier than getting started with a regular web hosting plan. You simply start your instance and upload a zip file with the files for your application. Here is a description from the AWS website:

  
> AWS Elastic Beanstalk is an easy-to-use service for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, and Docker on familiar servers such as Apache HTTP Server, Apache Tomcat, Nginx, Passenger, and IIS 7.5/8.  
>   
>  You can simply upload your code and Elastic Beanstalk automatically handles the deployment, from capacity provisioning, load balancing, auto-scaling to application health monitoring. At the same time, you retain full control over the AWS resources powering your application and can access the underlying resources at any time.  
>   
>  There is no additional charge for Elastic Beanstalk - you pay only for the AWS resources needed to store and run your applications.

  
 So it will basically host pretty much any kind of web application that you want to host unless you are looking for something special. The downside is that you may be limited to how much you can customize your environment, but there is probably a way to do this after everything is running, I just haven't looked in to it yet. So to get started we will create our AWS account, simply[click here](http://aws.amazon.com/ "AWS") and then click the sign up button in the top right to sign up!  
  
 Once you have signed up, login to your control panel and select Elastic Beanstalk from the services menu. Then you will be presented with the "Welcome to AWS Elastic Beanstalk" page. Here you should select the location where you would like to run your application, it doesn't matter too much but I would suggest that you pick the location closest to your customers or readers, it might save them a few milliseconds. After that simply click "Create New Application".  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/1.jpg)  
  
 Next we will need to fill out the name and description for your application. This is mostly for your own reference. I will call this my MarkusTenghamn Test App.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/2.jpg)  
  
 Then we proceed to the environment type. Don't worry too much about workers and what they are. We will need to select Web Server and then the predefined configuration which will be PHP. You can pick between load balancing/autoscaling or a single instance. You can pick either for this example but which you choose would be based on what you want to do with your app.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/3.jpg)  
  
 Click next and we get to the application version. This is how you upload your files and make changes to your application. What you need to do is make a zip file with the files for your website. In my case this is simply an index.php file which will then be unzipped and published to the root directory of your new application. What is nice here is that you can change back to previous uploads in case you should make a mistake or something. If you don't have a zip file with files you can pick "Sample application" for now, you will be able to upload your files later.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/4.jpg)  
  
 Next up is the Environment Information. This is the environment name and url as well as description. The url below is what you will use to access your application unless you are going to point your own domain to it.  
  
   
  
   
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/5.jpg)  
  
 Then we will proceed to the configuration details. Most likely and if you want to utilize the free tier, then you will select a t1.micro instance. However if you are in need of something larger then you are of course free to pick something with more power. The different types of instances can be found [here](http://aws.amazon.com/ec2/instance-types/ "Instance Types"). All you need to do then is to add an email adress and click next. Skip the last step and your application will start launching.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/6.jpg)  
  
 The launch will probably take a few minutes so you can now go and get a cup of coffé or whatever it is that you drink.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/7.jpg)  
 And bam! Your application is ready to go. You will se your app url in the top left corener. My url was markustenghamn-env.elasticbeanstalk.com for example. If you want to change something you simply make the changes to your files, zip them up and then upload the zip file with the "Upload and Deploy" button.  
  
[(image removed)](http://markustenghamn.com/wp-content/uploads/2014/06/8.jpg)  
  
 That's it, you now have your first PHP application running on AWS! Elastic Beanstalk has created all of the services needed to run your PHP application. This prevents you from having to do most of the configuration yourself. Feel free to leave a comment with any questions or feedback.