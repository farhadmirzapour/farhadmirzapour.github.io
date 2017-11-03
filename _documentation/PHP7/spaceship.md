---
layout: documentation
title:  عملگر spaceship
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 19:58:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://lydaweb.com/article/courses/php-7-new-features/13/عملگر-spaceship-در-php-7
---
<h3>عملگر spaceship</h3>
<p>
 در php 7 عملگر جدیدی به نام <strong>&nbsp;(&lt;=&gt;) </strong>Spaceship &nbsp;اضافه شده که برای مقایسه دو عبارت استفاده می شود , نتایج آن به این صورت است که اگر عبارت کوچکتر باشد <strong>1-&nbsp;</strong>&nbsp;بر میگرداند , اگر برابر باشند <strong>0 </strong>بر می گرداند و اگر بزرگتر باشد <strong>1</strong>&nbsp;را بر میگرداند .
 </p>

<p>
برای مثال :
</p>

 <pre><code class="language-php  line-numbers">
    //integer comparison
   print( 1 <=> 1);print("<br/>");
   print( 1 <=> 2);print("<br/>");
   print( 2 <=> 1);print("<br/>");
   print("<br/>");
   //float comparison
   print( 1.5 <=> 1.5);print("<br/>");
   print( 1.5 <=> 2.5);print("<br/>");
   print( 2.5 <=> 1.5);print("<br/>");
   print("<br/>");
   //string comparison
   print( "a" <=> "a");print("<br/>");
   print( "a" <=> "b");print("<br/>");
   print( "b" <=> "a");print("<br/>");
</code></pre>   

<p>خروجی :</p>

<pre><code class="language-php">
        0
       -1
        1

        0
       -1
        1

        0
       -1
        1
</code></pre>   