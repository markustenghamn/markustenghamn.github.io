---
layout: post
title:  "WHMCS Licensing Addon C# Sample Code"
redirect_from:
   - /whmcs-licensing-addon-c-sample-code
date:   2013-06-24 02:36:04 +0100
categories: best domain registrar
description: Here is some sample code I wrote recently due to many requests after previously writing many articles about the WHMCS Licensing Addon. This code will call your WHMCS installation and reutrn a dicti
---

Here is some sample code I wrote recently due to many requests after previously writing many articles about the WHMCS Licensing Addon. This code will call your WHMCS installation and reutrn a dictionary with the key/value pairs as the php check license function does. This code is not made to work with a local key. See comments in the code for more details.

 

```
<pre lang="php">
using System;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Net;
using System.Net.Sockets;
using System.Security.Cryptography;
using System.Text;
using System.Text.RegularExpressions;
using System.Windows.Forms;

namespace WHMCSLicenseCheck
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        //WHMCS License Check in C# by Markus Tenghamn
        //
        // http://MarkusTenghamn.com
        //
        //The following code will return the result from
        //a license check to your whmcs installation in
        //the form of a dictionary.
        //
        //See the button1_Click for sample use
        //
        //Release under GPLv3

        private void button1_Click(object sender, EventArgs e)
        {
            Dictionary<string string=""> myDic = checkLicense("MyLicenseKeyGoesHere");
        }

        public Dictionary<string string=""> checkLicense(string licensekey)
        {
            Random rand = new Random();
            Dictionary<string string=""> results = new Dictionary<string string="">();
            string whmcsUrl = "http://www.yourdomain.com/whmcs/";
            string licensingSecretKey = "abc123"; // Unique value, should match what is set in the product configuration for MD5 Hash Verification
            string checkToken = DateTime.Now + CalculateMD5Hash(rand.Next(100000000, 999999999) + licensekey);
            string clientIP = currentIP();

            WebClient WHMCSclient = new WebClient();

            NameValueCollection form = new NameValueCollection();
            form.Add("licensekey", licensekey);
            form.Add("domain", "yourdomain.com"); //this may not apply, a placeholder domain could be used
            form.Add("ip", clientIP);
            form.Add("dir", "yourdir"); //dir should probably not be applied either
            form.Add("check_token", checkToken);

            // Post the data and read the response
            Byte[] responseData = WHMCSclient.UploadValues(whmcsUrl + "modules/servers/licensing/verify.php", form);

            // Decode and display the response.
            textBox1.AppendText("Response received was " + Encoding.ASCII.GetString(responseData));

            Match match = Regex.Match(Encoding.ASCII.GetString(responseData), @"([^\^\]+)");
            
            while (match.Success)
            {
                string temp = match.Value;
                match = match.NextMatch();
                results[temp] = match.Value;
                match = match.NextMatch();
                match = match.NextMatch();
            }

            if (results.ContainsKey("md5hash"))
            {
                if (results["md5hash"] != CalculateMD5Hash(licensingSecretKey + checkToken))
                {
                    results["status"] = "Invalid";
                    results["description"] = "MD5 Checksum Verification Failed";
                    return results;
                }
            }

            return results;
        }


        public string CalculateMD5Hash(string input)
        {
            // step 1, calculate MD5 hash from input
            MD5 md5 = System.Security.Cryptography.MD5.Create();
            byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
            byte[] hash = md5.ComputeHash(inputBytes);

            // step 2, convert byte array to hex string
            StringBuilder sb = new StringBuilder();
            for (int i = 0; i 
<p><br></br><br></br><br></br><br></br>
Please let me know if you find it useful or if you have any questions!</p></string></string></string></string>
```