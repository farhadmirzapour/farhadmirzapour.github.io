---
layout: documentation
title: عملگر Null coalescing 
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 19:49:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://lydaweb.com/article/courses/php-7-new-features/12/عملگر-null-coalescing-در-php-7
---
<h3>عملگر Null coalescing </h3>
<p>
در 7 php  یک قابلیت جدید به نام  <strong>Null coalescing operator  (??)</strong> اضافه شده  که  برای جایگزینی عملیات سه گانه در ارتباط با عملکرد ()isset استفاده می شود. 
</p>

<p>
<strong>Null coalescing operator</strong> به این صورت عمل می کند که در عمق اول اگر متغیر وجود داشت و پوچ نبود خود را <span style="color:#999999">return </span>می کند  درغیر این صورت مقدار عمق دوم خود را <span style="color:#999999">return </span>می کند .
</p>


<p>
برای مثال :
</p>

<pre><code class="language-php  line-numbers">// fetch the value of $_GET['user'] and returns 'not passed'
   // if username is not passed
   $username = $_GET['username'] ?? 'not passed';
   print($username);
   print("<br/>");

   // Equivalent code using ternary operator
   $username = isset($_GET['username']) ? $_GET['username'] : 'not passed';
   print($username);
   print("<br/>");
   // Chaining ?? operation
   $username = $_GET['username'] ?? $_POST['username'] ?? 'not passed';
   print($username);
</code></pre>   

<p>خروجی :</p>

<pre><code class="language-php">
	not passed
    not passed
    not passed
</code></pre>
