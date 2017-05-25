---
layout: post
title:  "How to configure and setup WHMCS Licensing addon"
redirect_from:
   - /configure-setup-whmcs-licensing-addon
date:   2012-05-18 22:00:35 +0100
categories: best domain registrar
description: .
---

**Please see my updated guide on [How to setup and configure the WHMCS Licensing Addon](http://markustenghamn.com/how-to-configure-and-setup-whmcs-licensing-addon-review-how-to "How to configure and setup WHMCS Licensing Addon [Review] [How to]")** I get a lot of questions in regards to how to use the WHMCS licensing addon which is why I wrote a post a while back which described how to get the code working. That guide is outdated and did not show any code which is why I want to write a new guide with a little bit of code, but not enough to disclose how the whmcs licensing addon works. When you purchase the [WHMCS licensing addon](http://anve.to/i5irP "WHMCS licensing addon") you will receive several files but you will only have to worry about one for this guide which is the check\_sample\_code.php file. This is an example by WHMCS to show you how the code should be implemented in your script, plugin or theme. I will now walk through the example file and show you which parts are important and what they do. inside the function check\_license() we have the following code. ```
<pre lang="php">$whmcsurl = "http://www.yourdomain.com/whmcs/";
    $licensing_secret_key = "abc123";
``` These almost explain themselves and WHMCS even has some commenting to explain how to modify them. The first variable $whmcsurl should be equal to your whmcs installation and the other variable $licensing\_secret\_key is your secret key that you use when creating the product that you will be licensing. those are all the changes you should make to the check\_license() function. You could basically copy the check license function (all of it) and paste it into your script, plugin, or theme. The header file is always a good place to either have the code or include the file which contains the code for the function check\_license(). Check [php's include function](http://anve.to/iAOaM "php include function") for more information on how to do this. Now, let's go past the check\_license() function and move down to $licensekey, this is very important and should always be equal to the license you generate in WHMCS. This is the key used to check if a license is valid or not. Next is the local key ($localkey), don't worry about this, its a local key used for logging and you could change it up if you would like, but i wouldn't touch it. This line actually calls the function that will talk to your WHMCS and check if the license is valid or not. `<pre lang="php">$results = check_license($licensekey,$localkey);` You need to include this in your php file after the $licensekey and $localkey are defined. If you do not include this function then your script won't check if the license is valid. Now the results of the function check\_license() is stored in the variable $results as an array. The status of the license will be stored in this key. `<pre lang="php">$results["status"]` We can now check if the license was valid using the included else if statement WHMCS uses. ```
<pre lang="php">if ($results["status"]=="Active") {
    # Allow Script to Run
    if ($results["localkey"]) {
        # Save Updated Local Key to DB or File
        $localkeydata = $results["localkey"];
    }
} elseif ($results["status"]=="Invalid") {
    # Show Invalid Message
} elseif ($results["status"]=="Expired") {
    # Show Expired Message
} elseif ($results["status"]=="Suspended") {
    # Show Suspended Message
}
``` This is very simple but can get confusing for some so I will make a quick example of my own script which I want to use the license check on. So here is my script which I want to license. <form action="#" method="POST"><input name="add1" type="text"></input> plus <input name="add2" type="text"></input><input type="submit" value="Solve"></input></form>It simply takes two numbers and adds them. Now I want to show my script only if the license is valid, so I can add an if statement to make this check. I also need to include the check\_license() function which I added in a file called licensefunction.php. <form action="#" method="POST"><input name="add1" type="text"></input> plus <input name="add2" type="text"></input><input type="submit" value="Solve"></input></form>Now the script will only show if there is a valid license otherwise the page will be blank, but I want to show an error if the license is invalid so that the user doesn't get confused. <form action="#" method="POST"><input name="add1" type="text"></input> plus <input name="add2" type="text"></input><input type="submit" value="Solve"></input></form>This will echo the error if the license was not valid, I could of course add different errors for an invalid, suspended, or expired license but this will be ok for my script, the user will know something is wrong. For larger scripts I could terminate the loading of a page by simply adding a die() function at the top. <form action="#" method="POST"><input name="add1" type="text"></input> plus <input name="add2" type="text"></input><input type="submit" value="Solve"></input></form> ```
<pre lang="php">
?>
``` Now the script will stop loading at the die() function if the license is not active, so none of the code below the die statement will be presented. If the license is active the die function will never be called and the script will continue to run normally. To make your licensed script, plugin, or theme even more secure I would recommend using something like IonCube loader which WHMCS uses on their own encrypted files.