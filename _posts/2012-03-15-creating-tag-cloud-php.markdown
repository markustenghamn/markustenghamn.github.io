---
layout: post
title:  "Creating a Tag Cloud with PHP"
redirect_from:
   - /creating-tag-cloud-php
date:   2012-03-15 01:43:56 +0100
categories: best domain registrar
description: I have previously uploaded this code to SourceCodeDB.com http://sourcecodedb.com/Create-a-tag-cl
---

I have previously uploaded this code to SourceCodeDB.com [http://sourcecodedb.com/Create-a-tag-cloud-with-php.html](http://sourcecodedb.com/Create-a-tag-cloud-with-php.html "Create a tag cloud with php") I am using and modifying code originally from [Steven York](http://stevenyork.com/tutorial/creating_accessible_tag_cloud_in_php_css_mysql), he has written most of the code in this example. I had to modify pieces of his code due to the fact that the tags stored on sourcecodedb.com are strings like this "C#, loop, code" instead of storing each tag as its own word. Below is the code I am using with plenty of comments to explain what I am doing.

```
<pre lang="php">
<?php function createTagCloud($tags)  
    {    
        //I pass through an array of tags  
        $i=0;  
        foreach($tags as $tag)  
        {  
            if ($i < 61) {
             //the tag id, passed through  
            $name = $tag; //the tag name, also passed through in the array
            // I removed tag id since we dont have any tag ids here
            //using the mysql count command to sum up the tutorials tagged with that id  
            $sql = "SELECT COUNT(*) AS totalnum FROM codetable WHERE Tags LIKE '%".$name."%' AND Moderated = '1'";  
             
            //create the resultset and return it  
            $res = mysql_query($sql);  
            $res = mysql_fetch_assoc($res);  
             
            //check there are results ;)
            if($res)  
            {  
                //build an output array, with the tag-name and the number of results  
                $output[$i]['tag'] = $name;  
                $output[$i]['num'] = $res['totalnum'];  
            }  
            }
            $i++;
           
        }  
         
        /*this is just calling another function that does a similar SQL statement, but returns how many pieces of content I have*/  
        $total_tuts = 50;  //I set this to 50 in the example, we could use a sql query to get the actual nummber of codes
         
        //ugh, XHTML in PHP?  Slap my hands - this isnt best practice, but I was obviously feeling lazy  
        $html = '<ul class="tagcloud"?>';  
        //iterate through each item in the $output array (created above)  
        foreach($output as $tag)  
        {  
            //get the number-of-tag-occurances as a percentage of the overall number  
            $ratio = (100 / $total_tuts) * $tag['num'];  
             
            //round the number to the nearest 10  
            $ratio =  round($ratio,-1);  
             
            /*append that classname onto the list-item, so if the result was 20%, it comes out as cloud-20*/  
            $html.= '
```3. ['.$tag\['tag'\].'](http://sourcecodedb.com/tags.php?tag='.$tag['tag'].')
'; } //close the UL $html.= ''; return $html; } $gettags = mysql\_query("SELECT Tags FROM codetable WHERE Moderated='1' AND Published='1'"); //We get the tags which are written like: php, code, at, sourcecodedb while ($thetags = mysql\_fetch\_array($gettags)) { if ($thetags\['Tags'\] != null) { // We dont want any empty tags of course $newtag\[\] = $thetags\['Tags'\]; // Each group of tags is put in the array } } foreach ($newtag as $thetag) { $tags\[\] = explode(", ", $thetag); // Each group is seperated into individual tags and put into a new array } function array\_flatten\_recursive($array) { if (!$array) return false; $flat = array(); $RII = new RecursiveIteratorIterator(new RecursiveArrayIterator($array)); foreach ($RII as $value) $flat\[\] = $value; return $flat; } $finaltag = array\_flatten\_recursive($tags); //We flatten the array, dont want it to be multidimensional $finaltag2 = array\_unique($finaltag); //We remove any duplicate tags we have in our array echo createTagCloud($finaltag2); //Then the modified createTagCloud function is called ?> To show the tag cloud in the footer or anywhere else on your php page simply include the file with the code above into your file using PHP's include functions. Feel free to comment if you have any questions!