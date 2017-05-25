---
layout: post
title:  "WHMCS Licensing Addon - Storing the local key in a text file"
redirect_from:
   - /whmcs-licensing-addon-storing-local-key-text-file
date:   2012-08-09 22:28:44 +0100
categories: best domain registrar
description: Please see my updated guide on...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

**Please see my updated guide on [How to setup and configure the WHMCS Licensing Addon](http://markustenghamn.com/how-to-configure-and-setup-whmcs-licensing-addon-review-how-to "How to configure and setup WHMCS Licensing Addon [Review] [How to]")**  
  
 This post will attempt to explain how to use a text file to store the license key and local key when check license checks are performed preventing your whmcs licensing addon script from calling remotely too often and only when needed.  
  
 Need help getting the WHMCS licensing addon working?[Read my other post](http://markustenghamn.com/configure-setup-whmcs-licensing-addon "How to configure and setup WHMCS licensing addon")  
  
 At the end of our check license script we have some code that looks like this which will return the state of the license.  
```
<pre lang="php">$results = check_license($licensekey, $localkey);<br></br><br></br>
if ($results["status"]=="Active") {<br></br>
    # Allow Script to Run<br></br>
    if ($results["localkey"]) {<br></br>
        # Save Updated Local Key to DB or File<br></br>
        $localkeydata = $results["localkey"];<br></br>
    }<br></br>
} elseif ($results["status"]=="Invalid") {<br></br>
    # Show Invalid Message<br></br>
} elseif ($results["status"]=="Expired") {<br></br>
    # Show Expired Message<br></br>
} elseif ($results["status"]=="Suspended") {<br></br>
    # Show Suspended Message<br></br>
}
```  
 Well to change this up I will remove the $licensekey and $localkey declarations above that code and replace the code shown above with the following code.  
```
<pre lang="php">// End Check Function<br></br>
# Get Variables from storage (retrieve from wherever it's stored - DB, file, etc...)<br></br>
//I pull the license and local key from the text file.<br></br>
$file = 'license.txt';<br></br>
if (file_exists($file)) {<br></br>
$lines = explode("\n", file_get_contents($myfile));;<br></br>
$licensekey = $lines[0];<br></br>
$localkey = $lines[1];;<br></br><br></br>
}<br></br>
else {<br></br>
$licensekey = "":<br></br>
$localkey = "";<br></br>
}<br></br><br></br>
$results = check_license($licensekey, $localkey);<br></br><br></br>
    if ($results["status"] == "Active") {<br></br>
        # Allow Script to Run<br></br>
   if ($results["localkey"]) {<br></br>
        # Save Updated Local Key to DB or File<br></br>
        $localkeydata = $results["localkey"];<br></br>
    }<br></br>
        # Show Invalid Message<br></br>
        $fp = fopen("license.txt", "w");<br></br><br></br>
        fwrite($fp, $licensekey);<br></br><br></br>
        fwrite($fp, $localkey);<br></br><br></br>
        fclose($fp);<br></br>
    } elseif ($results["status"] == "Invalid") {<br></br><br></br>
	echo 'License invalid';<br></br><br></br>
    } elseif ($results["status"] == "Expired") {<br></br><br></br>
	echo 'License expired';<br></br><br></br>
    } elseif ($results["status"] == "Suspended") {<br></br><br></br>
	echo 'License suspended';<br></br><br></br>
    }<br></br>
}
```  
 You will also need to create a text file which is called license.txt and add the license key to the first line.  
  
 The code which I have written will open the text file and read the first line (license key) and if there is a second line it will read that as the local key. Leave the second line blank when you add the text file, the script adds this line automatically upon license validation. Every time the license is validated the license key (stays the same) and the local key is updated and rewritten to the text file. As long as the local key is valid there will be no need for your script to do the remote check.  
  
 Hope that helps, please feel free to ask any questions by sending me an email or commenting.