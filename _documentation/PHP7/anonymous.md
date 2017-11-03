---
layout: documentation
title:  کلاس های anonymous
cattitle: معرفی امکانات جدید PHP 7
date:   2017-11-03 20:10:42 +0330
jdate: جمعه 12 آبان 1396
caturl: 2017/11/03/php7.html
# permalink: /:categories/:title.html
thumbnail: php7.jpg
refrence: http://hiranian.ir/articles/59-آموزش/learn-programming/php7-training/566-php7-anonymous-classes.html
---
<h3>کلاس های anonymous</h3>
<p>
حالا کلاس های بی نام می توانند با استفاده از کلاس های جدید ، تعریف شوند . کلاس های بی نام می توانند در مکانی که پر از تعاریف کلاس است ، استفاده شوند .
</p>


<p>
برای مثال :
</p>

 <pre><code class="language-php  line-numbers">
   interface Logger {
      public function log(string $msg);
   }

   class Application {
      private $logger;

      public function getLogger(): Logger {
         return $this->logger;
      }

      public function setLogger(Logger $logger) {
         $this->logger = $logger;
      }  
   }

   $app = new Application;
   $app->setLogger(new class implements Logger {
      public function log(string $msg) {
         print($msg);
      }
   });

   $app->getLogger()->log("My first Log Message");
</code></pre>     
 
   <p>خروجی :</p>

<pre><code class="language-php">
My first Log Message
</code></pre>      