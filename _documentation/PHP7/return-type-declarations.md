---
layout: documentation
title:  مشخص کردن نوع داده بازگشتی
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 19:42:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://lydaweb.com/article/courses/php-7-new-features/11/مشخص-کردن-نوع-داده-بازگشتی-در-php-7
---
<h3>مشخص کردن نوع داده بازگشتی</h3>
<p>در php 7 یک قابلیت بسیارخوب اضافه شده که میتوانیم نوع داده های برگشتی از متدهامون رو مشخص کنیم . نوع داده های برگشتی که می توانیم مشخص کنیم به شرح زیر است :
</p>

<p>
<ul dir="rtl">
<li>int</li>
<li>float</li>
<li>bool</li>
<li>string</li>
<li>interfaces</li>
<li>array</li>
<li>callable</li>
</ul>
</p>

<p>
مثال برای نوع بازگشتی معتبر :
</p>

<pre><code class="language-php  line-numbers">
   declare(strict_types = 1);
   function returnIntValue(int $value): int {
      return $value;
   }
   print(returnIntValue(5));
</code></pre>   

<p>خروجی :</p>

<pre><code class="language-php">5</code></pre>

<p>
مثال برای نوع بازگشتی نامعتبر :
</p>

<pre><code class="language-php  line-numbers">
   declare(strict_types = 1);
   function returnIntValue(int $value): int {
      return $value + 1.0;
   }
   print(returnIntValue(5));
</code></pre>   

<p>خروجی :</p>

<pre><code class="language-php">Fatal error: Uncaught TypeError: Return value of returnIntValue() must be of the type integer, float returned...
</code></pre>
