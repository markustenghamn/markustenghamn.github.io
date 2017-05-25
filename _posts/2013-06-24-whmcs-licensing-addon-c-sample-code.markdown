---
layout: post
title:  "WHMCS Licensing Addon C# Sample Code"
redirect_from:
   - /whmcs-licensing-addon-c-sample-code
date:   2013-06-24 02:36:04 +0100
categories: best domain registrar
description: Here is some sample code I wrote recently due to many requests after previously writing many articles about the WHMCS Licensing Addon. This code will call your WHMCS installation and reutrn a dicti...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

Here is some sample code I wrote recently due to many requests after previously writing many articles about the WHMCS Licensing Addon. This code will call your WHMCS installation and reutrn a dictionary with the key/value pairs as the php check license function does. This code is not made to work with a local key. See comments in the code for more details.

  
 

  
```
<pre lang="php"><br></br>
using System;<br></br>
using System.Collections.Generic;<br></br>
using System.Collections.Specialized;<br></br>
using System.Net;<br></br>
using System.Net.Sockets;<br></br>
using System.Security.Cryptography;<br></br>
using System.Text;<br></br>
using System.Text.RegularExpressions;<br></br>
using System.Windows.Forms;<br></br><br></br>
namespace WHMCSLicenseCheck<br></br>
{<br></br>
    public partial class Form1 : Form<br></br>
    {<br></br>
        public Form1()<br></br>
        {<br></br>
            InitializeComponent();<br></br>
        }<br></br><br></br>
        //WHMCS License Check in C# by Markus Tenghamn<br></br>
        //<br></br>
        // http://MarkusTenghamn.com<br></br>
        //<br></br>
        //The following code will return the result from<br></br>
        //a license check to your whmcs installation in<br></br>
        //the form of a dictionary.<br></br>
        //<br></br>
        //See the button1_Click for sample use<br></br>
        //<br></br>
        //Release under GPLv3<br></br><br></br>
        private void button1_Click(object sender, EventArgs e)<br></br>
        {<br></br>
            Dictionary<string string=""> myDic = checkLicense("MyLicenseKeyGoesHere");<br></br>
        }<br></br><br></br>
        public Dictionary<string string=""> checkLicense(string licensekey)<br></br>
        {<br></br>
            Random rand = new Random();<br></br>
            Dictionary<string string=""> results = new Dictionary<string string="">();<br></br>
            string whmcsUrl = "http://www.yourdomain.com/whmcs/";<br></br>
            string licensingSecretKey = "abc123"; // Unique value, should match what is set in the product configuration for MD5 Hash Verification<br></br>
            string checkToken = DateTime.Now + CalculateMD5Hash(rand.Next(100000000, 999999999) + licensekey);<br></br>
            string clientIP = currentIP();<br></br><br></br>
            WebClient WHMCSclient = new WebClient();<br></br><br></br>
            NameValueCollection form = new NameValueCollection();<br></br>
            form.Add("licensekey", licensekey);<br></br>
            form.Add("domain", "yourdomain.com"); //this may not apply, a placeholder domain could be used<br></br>
            form.Add("ip", clientIP);<br></br>
            form.Add("dir", "yourdir"); //dir should probably not be applied either<br></br>
            form.Add("check_token", checkToken);<br></br><br></br>
            // Post the data and read the response<br></br>
            Byte[] responseData = WHMCSclient.UploadValues(whmcsUrl + "modules/servers/licensing/verify.php", form);<br></br><br></br>
            // Decode and display the response.<br></br>
            textBox1.AppendText("Response received was " + Encoding.ASCII.GetString(responseData));<br></br><br></br>
            Match match = Regex.Match(Encoding.ASCII.GetString(responseData), @"([^\^\]+)");<br></br><br></br>
            while (match.Success)<br></br>
            {<br></br>
                string temp = match.Value;<br></br>
                match = match.NextMatch();<br></br>
                results[temp] = match.Value;<br></br>
                match = match.NextMatch();<br></br>
                match = match.NextMatch();<br></br>
            }<br></br><br></br>
            if (results.ContainsKey("md5hash"))<br></br>
            {<br></br>
                if (results["md5hash"] != CalculateMD5Hash(licensingSecretKey + checkToken))<br></br>
                {<br></br>
                    results["status"] = "Invalid";<br></br>
                    results["description"] = "MD5 Checksum Verification Failed";<br></br>
                    return results;<br></br>
                }<br></br>
            }<br></br><br></br>
            return results;<br></br>
        }<br></br><br></br><br></br>
        public string CalculateMD5Hash(string input)<br></br>
        {<br></br>
            // step 1, calculate MD5 hash from input<br></br>
            MD5 md5 = System.Security.Cryptography.MD5.Create();<br></br>
            byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);<br></br>
            byte[] hash = md5.ComputeHash(inputBytes);<br></br><br></br>
            // step 2, convert byte array to hex string<br></br>
            StringBuilder sb = new StringBuilder();<br></br>
            for (int i = 0; i 
            {<br></br>
                sb.Append(hash[i].ToString("X2"));<br></br>
            }<br></br>
            return sb.ToString();<br></br>
        }<br></br><br></br>
        private string currentIP()<br></br>
        {<br></br>
            IPHostEntry host;<br></br>
            string localIP = "?";<br></br>
            host = Dns.GetHostEntry(Dns.GetHostName());<br></br>
            foreach (IPAddress ip in host.AddressList)<br></br>
            {<br></br>
                if (ip.AddressFamily == AddressFamily.InterNetwork)<br></br>
                {<br></br>
                    localIP = ip.ToString();<br></br>
                }<br></br>
            }<br></br>
            return localIP;<br></br>
        }<br></br><br></br>
    }<br></br>
}<br></br></string></string></string></string>
```  
  
  
  
  
  
  
Please let me know if you find it useful or if you have any questions!