---
layout: post
title:  "WHMCS Licensing Addon - Storing the local key with MySQL"
redirect_from:
   - /whmcs-licensing-addon-storing-the-local-key
date:   2012-08-07 03:33:42 +0100
categories: best domain registrar
description: .
---

**Please see my updated guide on [How to setup and configure the WHMCS Licensing Addon](http://markustenghamn.com/how-to-configure-and-setup-whmcs-licensing-addon-review-how-to "How to configure and setup WHMCS Licensing Addon [Review] [How to]")** This post will discuss how the local key is returned and what it does. If you need help configuring the WHMCS licensing Addon please read my previous post [How to configure and setup WHMCS Licensing addon](http://markustenghamn.com/configure-setup-whmcs-licensing-addon). So let's get started, this is how WHMCS explains the local key in their FAQ section. > The local key is the thing that stops the license checking code having to call your server on every page load. It's basically just an encrypted version of the license check data, so that when it's present and valid, the licensing addon doesn't have to make a remote call which would slow down your application/code, thus improving performance. The local key will always be empty on the first check your client makes, but then with every successful remote check, a local key value is returned (sample code provided) which you then just need to store and pass into any future license check calls. We recommend storing into a database for ease of updating and retrieval, but flat files can work just as well.

 From the [documentation for the WHMCS Licensing Addon](http://anve.to/npIyP "documentation for the WHMCS Licensing Addon"). Looking at the check\_sample\_code.php file we can see that the function which checks the license accepts a $localkey but if it is not give in sets it to nothing and calls the server. If the local key is invalid it will call the licensing server. `<pre lang="php">check_license($licensekey,$localkey="");` We also have this sample license and sample local key. ```
<pre lang="php"># Get Variables from storage (retrieve from wherever it's stored - DB, file, etc...)
$licensekey = "WHMCS-c5adf50c9a";
$localkey = '9tjIxIzNwgDMwIjI6gjOztjIlRXYkt2Ylh2YioTO6M3OicmbpNnblNWasx1cyVmdyV2ccNXZsVHZv1GX
zNWbodHXlNmc192czNWbodHXzN2bkRHacBFUNFEWcNHduVWb1N2bExFd0FWTcNnclNXVcpzQioDM4ozc
7ISey9GdjVmcpRGZpxWY2JiO0EjOztjIx4CMuAjL3ITMioTO6M3OiAXaklGbhZnI6cjOztjI0N3boxWY
j9Gbuc3d3xCdz9GasF2YvxmI6MjM6M3Oi4Wah12bkRWasFmdioTMxozc7ISeshGdu9WTiozN6M3OiUGb
jl3Yn5WasxWaiJiOyEjOztjI3ATL4ATL4ADMyIiOwEjOztjIlRXYkVWdkRHel5mI6ETM6M3OicDMtcDM
tgDMwIjI6ATM6M3OiUGdhR2ZlJnI6cjOztjIlNXYlxEI5xGa052bNByUD1ESXJiO5EjOztjIl1WYuR3Y
1R2byBnI6ETM6M3OicjI6EjOztjIklGdjVHZvJHcioTO6M3Oi02bj5ycj1Ga3BEd0FWbioDNxozc7ICb
pFWblJiO1ozc7IyUD1ESXBCd0FWTioDMxozc7ISZtFmbkVmclR3cpdWZyJiO0EjOztjIlZXa0NWQiojN
6M3OiMXd0FGdzJiO2ozc7pjMxoTY8baca0885830a33725148e94e693f3f073294c0558d38e31f844
c5e399e3c16a';
``` Now we could store these in a database or a text file and update the local key as needed to prevent excessive checks to the remote server. I am going to store mine in a mysql database where I have a table called "keys" with the fields license\_key and local\_key. I will also connect to the mysql database, to see an example of this please see [How to connect to mysql and select a database](http://anve.to/PpEXC " How to connect to mysql and select a database"). Now i will add the license\_key manually to the database, you could do this with an install script or also do it manually, and then leave the local\_key blank, this will be updated automatically during checks. So to begin I change the above code to the following ```
<pre lang="php"># Get Variables from storage (retrieve from wherever it's stored - DB, file, etc...)
$licensekey = mysql_result(mysql_query("SELECT license_key FROM keys LIMIT 1"), 0); //Gets the license key from the database
$localkey = mysql_result(mysql_query("SELECT local_key FROM keys LIMIT 1"), 0); //Gets the local key from the database
``` Now the local key and license key may be blank or actually contain data, it wont create problems for your script either or. However a blank or invalid license key will not activate of course, and a blank or wrong local key will simply cause the remote server to be called and then we will use the returned info to update our local key. The local key should be blank on the first run. Now in our check\_sample\_code.php file you will see the following lines ```
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
``` Here is the information we need to update our local key, pay attention to the variable $results\["localkey"\], this will contain your local key that you need to store. So to make the update I simply change the code to the following ```
<pre lang="php">if ($results["status"]=="Active") {
    # Allow Script to Run
    if ($results["localkey"]) {
        # Save Updated Local Key to DB or File
        $localkeydata = $results["localkey"];
        mysql_query("UPDATE keys SET local_key = '$localkeydata'");
    }
} elseif ($results["status"]=="Invalid") {
    # Show Invalid Message
} elseif ($results["status"]=="Expired") {
    # Show Expired Message
} elseif ($results["status"]=="Suspended") {
    # Show Suspended Message
}
``` That's all you have to do to make the local key check work. Easy right? Have any questions or need me to configure something for you feel free to get in touch or leave a comment! **DISCLAIMER: This guide does not focus on security and my mysql statements are considered unsafe and open to SQL injection. For real use please be very careful with this and secure the script especially if it is being used on a clients database.** If you need help configuring the WHMCS licensing Addon please read my previous post [How to configure and setup WHMCS Licensing addon](http://markustenghamn.com/configure-setup-whmcs-licensing-addon).