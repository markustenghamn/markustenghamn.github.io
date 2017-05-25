---
layout: post
title:  "How to configure and setup WHMCS Licensing Addon [Review] [How to]"
redirect_from:
   - /how-to-configure-and-setup-whmcs-licensing-addon-review-how-to
date:   2015-03-27 13:49:38 +0100
categories: best domain registrar
description: Almost 3 years ago I wrote an article regarding WHMCS Licensing Addon...
---

Almost 3 years ago I wrote an article regarding [WHMCS Licensing Addon](http://anve.to/cHPe1 "WHMCS Licensing Addon") and how to set it up. Back then I was new to [WHMCS](http://anve.to/cHPe1 "WHMCS") and not as good at PHP as I am today. Due to the articles popularity I have decided to revisit this topic and make a new, more detailed post on how to get going with the addon. I will link to this article in the old guides as this article should be more relevant and is made for [WHMCS](http://anve.to/cHPe1 "WHMCS") version 5.3.12 and [Licensing Addon](http://anve.to/cHPe1 "WHMCS Licensing Addon") version 3.0. I have also made an easy to follow video to go along with this guide which you will find [here](http://anve.to/vu4Dt "How to configure and setup WHMCS Licensing Addon [Review] [How to]") on YouTube. If you need help integrating the Licensing Addon to a php script this is something we offer at Anveto, click [here](http://anve.to/MQTyb "WHMCS Licensing Addon Integration by Anveto") to read more and order!

## Review

 As for my review, I can say that this has been the easiest and most secure way to protect my php scripts. Sure it is not fool proof but it is very good compared to other free alternatives. I also like the use of a local key to keep the script from having to check with the server every time it is used, instead it will make one check ever 15 days or so, you can of course change how often the check is done. Inside of your [WHMCS](http://anve.to/cHPe1 "WHMCS") installation you will also see when the last check was made, where the script is being used, and what the status is. [WHMCS](http://anve.to/cHPe1 "WHMCS") itself allows you to automatically suspend licenses if an invoice is overdue which is very convenient and clients can see the status of their services/licenses inside the client area if needed. ## Configure and Setup

 Let's get to the configuration and setup which is now easier than ever as the licensing addon is included when you download the latest version of [WHMCS](http://anve.to/cHPe1 "WHMCS"). You will of course need to go to Setup->Addon Modules and activate the licensing addon making sure to set the Access Control to Administrator or whichever group you would like to have access to it. Now that that is taken care of you should see the licensing module when creating a product. So either create a new product under Setup->Products/Services or edit an existing one. Then in the tabbed menus navigate to Module Settings. Click the dropdown and select Licensing as the Module Name. This will present you with several options, the first being key length. You can set the key length to any number, I used 35 in my example as I find it to be a good length. If you plan on having a few licenses then a short length is fine, however if you plan on issuing millions of licenses you may want to consider a key length closer to 35 as I have done. I checked the box to allow reissue, this basically means that clients can move the script to a new domain or path without having to open a support ticket and have me do it manually. Next is the Key Prefix which can be good if you are trying to identify which product a license is for, perhaps wordpressplugin1- is a good identifier, pick anything. Now for the secret key I simply generated a password which is unique for this product, you can enter any string here but I would recommend using some form of password generator. There are a few other options here but I am not going to cover it in this guide. Now we can navigate to the links tab, copy the link to the product and perform an order to see if everything works and also to get our license key that we want to test with. After the order has been made on your test account and after accepting the order in the admin area, you should find your license key in the test accounts client area under services. Keep this as we will need it later in this guide. Next we need to find the code that needs to be included in our script, the license check code. WHMCS also includes this in the latest releases and we can find it using an FTP client by navigating from the WHMCS root to modules->servers->licensing->check\_sample\_code.php. We need to copy this code and then head over to our PHP script which we want to add the license to. Now I don't want to clutter things up so I will be making a new project, however you could create the same files and later integrate them into your script. To begin with I will create a file called license.php, this code will hold all the code that I just copied from the example file. Once we have pasted the code we can make some small modifcations right after "function yourprefix123\_check\_license". You can change the $whmcsurl to your WHMCS installation url making sure to keep the trailing slash. You also need to add the $licensing\_secret\_key which we added to our product earlier. If you don't remember this step please see my [video](http://anve.to/vu4Dt "How to configure and setup WHMCS Licensing Addon [Review] [How to]") as this clearly shows where to find this key. You can also modify the $localkeydays and $allowcheckfaildays if needed but I think the default values are pretty good to use. Now that is all we need to edit inside that function, the rest of the code is what does the actual checking of the license which we never need to modify. If you scroll down to the bottom where this function ends you should see an example $licensekey and $localkey. We won't modify these right away as I am going to use a text file for this example, however you could input your licensekey here and just use this file, however without storing a local key in a file or database then you will be checking that the license is valid on every page load which is not good. Next, we will create a file called addlicense.php which will be used to store our license key inside a text file. This will be a basic form which will write to our text file when the form is submitted. At this time we can also go ahead and create the license.txt file which will hold our license and local keys once everything works. ```
<?php
$success = false;
if (isset($_POST['license'])) {
    $license = $_POST['license'];
    $base = __DIR__;
    $textfile = fopen($base."/license.txt", "w") or die("Unable to open file!");
    $contents = $license."\n";
    fwrite($textfile, $contents);
    fclose($textfile);
    $success = true;
}
?>
<html>
<head>
    <title>License Test</title>
</head>
<body>
<div class="wrapper">
    <div class="form">
        <?php
        if ($success) {
            ?>
            <p>License added!</p>
            <?php
        } else {
        ?>
        <form method="post" action="">
            Enter your license <input name="license">
            <input type="submit" value="Add">
        </form>
        <?php } ?>
    </div>
</div>
</body>
</html>
``` This is my addlicense.php file. Going from top to button we begin by setting a bool called $success which is always false unless a license key has been submitted. We don't do any validation here as it is an example but when using this in a real script you probably want to make sure that at least the length matches to make sure your user found their key. The $\_POST\['license'\] is the text field from the form and if it is set, if the form has been submitted, we try to write whatever was in this field to our textfile, license.txt. $base gets our current directory so that we can access the text file in the same directory as the addlicense.php file. We then open our textfile, write our license key to it with a line break and then close the file. If we were unable to open the textfile then the script will stop and output "Unable to open file!". If this happens you need to make sure that you added the text file and that you have set file permissions so that apache/web can write to your text file. Next we load the actual html for the page, if $success is true it means that the form was submitted and since we got this far then we know that the license was added and we say "License added!". If not then the form has not been submitted and we show the form where the user can enter the license. In a real world example you would probably check if the license has been added and if it has been validated already and display this information to the user. This form might also be added inside of an admin area or something like that. If you wanted to store the key using MySQL you would simply replace the text file writing at the top with your MySQL query, updating the license key in the database. Now let's head back to our license.php file where we pasted all that sample code. From the top, make sure you have removed the "exit;" as this will cause you to only see a white page when testing your code. Next we go down to our function, yourprefix123\_check\_license, which you can change to pretty much anything, I changed mine to say licensetest\_check\_license but you could call your function ketchup($licensekey, $localkey='') if you wanted to. Now scroll all the way to the bottom and find your $licensekey and $localkey. In this area you should see yourprefix123\_check\_license, rename this to the same thing as you did above. Now I make several changes to the code and I will go through it shortly. Below you will see how I have changed the code in the license.php file under the check license function which we just renamed. ```
// Get the license key and local key from storage
// These are typically stored either in flat files or an SQL database

$licensekey = "";
$localkey = "";
$base = __DIR__;
$handle = fopen($base."/license.txt", "r");
if ($handle) {
    $count = 0;
    while (($line = fgets($handle)) !== false) {
        // process the line read.
        if ($count == 0) {
            $licensekey = trim($line);
        } else if ($count == 1) {
            $localkey = trim($line);
            break;
        }
        $count++;
    }

    fclose($handle);
} else {
    die("Could not read license file. Please contact support.");
}

echo $licensekey."<br/>";
echo $localkey."<br/>";

// Validate the license key information
$results = licensetest_check_license($licensekey, $localkey);

// Raw output of results for debugging purpose
echo '<textarea cols="100" rows="20">' . print_r($results, true) . '</textarea>';

// Interpret response
switch ($results['status']) {
    case "Active":
        // get new local key and save it somewhere
        $localkeydata = str_replace(' ','',preg_replace('/\s+/', ' ', $results['localkey']));
        $handle = fopen($base."/license.txt", "r");
        if ($handle) {
            $count = 0;
            while (($line = fgets($handle)) !== false) {
                // process the line read.
                if ($count == 0) {
                    $licensekey = trim($line);
                    break;
                }
                $count++;
            }
            fclose($handle);
            if (isset($results['localkey'])) {
                $textfile = fopen($base . "/license.txt", "w") or die("Unable to open file!");
                $contents = $licensekey . "\n" . $localkeydata . "\n";
                fwrite($textfile, $contents);
                fclose($textfile);
            }
        } else {
            die("Could not read license file. Please contact support.");
        }
        break;
    case "Invalid":
        die("License key is Invalid");
        break;
    case "Expired":
        die("License key is Expired");
        break;
    case "Suspended":
        die("License key is Suspended");
        break;
    default:
        die("Invalid Response");
        break;
}
``` Basically what we are doing is checking for a license key and local key and reading it from the text file. If the license is valid we store the new local key so that we don't have to run the check again for another 15 days. Now there is a lot of code to go through here but we will go through it just like we did before, from top to bottom. So what I did first was to remove the example $licensekey and $localkey and leave these blank. If there is no key in the text file then the blank key will be checked and be returned as invalid. After these two variables we set our base directory with $base so that we now that text file we are reading is in the same directory. We then try to open the text file and read it line by line. Because of the way we store the licensekey and localkey we know that the first line is always the licensekey and the second line is always the localkey. If the license has never been checked the $localkey will remain blank, all this does is force the license check function to try to make a call to the license server (your WHMCS install). So once we are done reading the text file we close the $handle. I decided to echo my licensekey and localkey here so that I could see what was stored in my text file, you can should probably remove this echo and your textarea echo when running this on an actual script. Next we call our license function with $results = licensetest\_check\_license($licensekey, $localkey), remember that you may have changed the name of this to something else. After this i print out the returned $results in a textarea and then I proceed to the provided switch case which looks at the status key of the $results array. If this key is equal to "Active" we then know that the license was validated and that a localkey should have been returned if this was a remote check. One important thing here is to remove the spaces in the $results\['localkey'\] as it can mess things up if you are reading a text file line by line. However if the validation was done via the local key, as in no call was made to the license server, then there will be no local key returned. This is why I check to see if the $results\['localkey'\] is set further down so that I don't overwrite an existing local key with a blank key. I also copied the earlier code and once again read the text file line by line to get our license key from storage once again. If the local key was returned in the results array I then proceed to write the license and local key to the text file.  You will see other cases in this switch case statement which I have not modified. In these other cases you may take some kind of action such as displaying an error message to the user saying that his invoice is unpaid and that he should contact support. Although please keep in mind that your clients users may also see the message so I would recommend showing a general error message as to not embarrass your client with a personal message. Thanks for reading the guide, I hope it helps. Feel free to leave and comments, questions or suggestions in the comments below! Remember, if you need help integrating the Licensing Addon to a php script this is something we offer at Anveto, click [here](https://anveto.com/members/cart.php?a=add&pid=15 "WHMCS Licensing Addon Integration by Anveto") to read more and order!