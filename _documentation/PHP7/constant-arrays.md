---
layout: documentation
title:  آرایه های ثابت 
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 20:03:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://lydaweb.com/article/courses/php-7-new-features/17/آرایه-های-ثابت-php-7
---
<h3>آرایه های ثابت </h3>
<p>آرایه های ثابت می توانند با استفاده از تابع <strong>()define</strong> تعریف شوند .</p>

<p>این کار در php 5.6  فقط با استفاده از <strong>const</strong> قابل انجام بود .<code> </code></p>


<p>
برای مثال :
</p>

 <pre><code class="language-php  line-numbers">
   //define a array using define function
   define('animals', [
      'dog',
      'cat',
      'bird'
   ]);
   print(animals[1]);
</code></pre>     
 
   <p>خروجی :</p>

<pre><code class="language-php">
cat
</code></pre>   





