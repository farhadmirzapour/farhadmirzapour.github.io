---
layout: documentation
title:  Scalar type های جدید در php7
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 19:37:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://bhiranian.ir/articles/59-آموزش/learn-programming/php7-training/561-php7-scalar-type-declarations.html
---
<h3>Scalar type های جدید در php7</h3>
<p>
در نسخه 7 (php) ، یک قابلیت جدید به نام اعلان های نوع عددی (Scalar Type Declarations) ، معرفی شده است . اعلان های نوع عددی دو گزینه دارند :
</p>

<p>
<ul>
<li>اجباری – حالت پیش فرض است و نیازی به مشخص کردن نیست</li>
<li>سخت گیرانه – باید با صراحت تمام اشاره شود</li>
</ul>
</p>
<p>
انواع داده شده برای پارامتر های عملکرد می تواند با حالت های بالا ، اجرا شود .
</p>

<p>
<ul>
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
مثال برای حالت اجباری :
</p>

<pre><code class="language-php  line-numbers">// Coercive mode
   function sum(int ...$ints) {
      return array_sum($ints);
   }
   print(sum(2, '3', 4.1));
</code></pre>

<p>خروجی :</p>

<pre><code class="language-php">9</code></pre>

<p>
مثال برای حالت سخت گیرانه :
</p>

<pre><code class="language-php  line-numbers">// Strict mode
   declare(strict_types=1);
   function sum(int ...$ints) {
      return array_sum($ints);
   }
   print(sum(2, '3', 4.1));
</code></pre>

<p>خروجی :</p>

<pre><code class="language-php  line-numbers">Fatal error: Uncaught TypeError: Return value of returnIntValue() must be of the type integer, float returned...
</code></pre>