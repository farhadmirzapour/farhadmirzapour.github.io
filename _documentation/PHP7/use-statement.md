---
layout: documentation
title:  گروه بندی use ها
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 20:43:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://bhiranian.ir/articles/59-آموزش/learn-programming/php7-training/572-php7-use-statement.html
---
<h3>گروه بندی use ها</h3>
<p>
در PHP 7  می توان use  ها را گروهبندی نمود :
</p>

<p>
برای مثال :
</p>

 <pre><code class="language-php  line-numbers">
   // Before PHP 7
   use com\tutorialspoint\ClassA;
   use com\tutorialspoint\ClassB;
   use com\tutorialspoint\ClassC as C;

   use function com\tutorialspoint\fn_a;
   use function com\tutorialspoint\fn_b;
   use function com\tutorialspoint\fn_c;

   use const com\tutorialspoint\ConstA;
   use const com\tutorialspoint\ConstB;
   use const com\tutorialspoint\ConstC;

   // PHP 7+ code
   use com\tutorialspoint\{ClassA, ClassB, ClassC as C};
   use function com\tutorialspoint\{fn_a, fn_b, fn_c};
   use const com\tutorialspoint\{ConstA, ConstB, ConstC};
</code></pre>   

