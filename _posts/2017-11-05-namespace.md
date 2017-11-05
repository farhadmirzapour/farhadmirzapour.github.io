---
layout: post
title:   مفهوم namespace  در PHP
date:   2017-11-05 20:35:42 +0330
jdate: یکشنبه 14 آبان 1396
categories: articles
#  permalink: /:categories/:title.html
thumbnail: namespaces-1024x413-compressor.jpg
refrence: http://aparnet.ir/3021-namespaces-in-php-part-1
---
<p>
یکی از ویژگی های مهمی که در 5.3 PHP اضافه شد، namespace بود. برنامه نویس های #C و جاوا با این ویژگی آشنا هستند.

namespace باعث بهبود ساختار اپلیکیشن‌های PHP میشود به طوری که مشکل نام گذاری‌های یکتا حل، همچنین امکان بخش بندی کدها را به توسعه دهندگان می‌دهد، و سازماندهی و پکیج بندی کدها در آن خیلی شبیه به ساختار دایرکتوری‌ها در فایل سیستم هست.

علاوه بر موارد بالا شما را قادر می سازد تا از تمام مزایای autoloaderهایی که از جدیدترین استانداردها پیروی می کنند، که شامل اتولودر کامپوزر (Composer’s autoloader) هم می‌شود بهره ببرید.
</p>


<h3 >مفهوم و مقدمات namespace</h3>

<p>از مزایای namespace گفتیم، اما بیایید کمی دقیق‌تر به موضوع نگاه کنیم. چه زمانی به استفاده از آنها نیاز پیدا می کنیم؟</p>

<p>
فضای پیش فرض، که شما در آن به نوشتن کد PHP می‌پردازید فضای سراسری یا global space نام دارد در این فضا شما اجازه تعریف دو کلاس با نام یکسان را ندارید و اگر این کار را انجام دهید با Fatal error روبرو میشوید. این موضوع برای نام تابع‌ها و ثابت‌ها نیز صدق می‌کند.
</p>

<p>
مثال مشابه برای روشن‌تر شدن موضوع، ساختار دایرکتوری‌ها در سیستم عامل‌ها است که امکان ندارد در یک مسیر واحد دو فایل foo.txt ایجاد کرد ولی برای گروه بندی فایل‌های مرتبط می‌توان دایرکتوری دیگری تعریف کرد و یک فایل foo.txt در یک دایرکتوی و فایل foo.txt دیگر را در دایرکتوری دیگر ایجاد کرد.
</p>

<p>
وقتی پروژه گسترده می‌شود این احتمال بالاتر می‌رود که دوباره بخواهیم از نام تابع یا کلاسی که قبلا تعریف شده برای تعریف مجدد استفاده کنید. اوضاع وقتی بدتر می‌شود که بخواهید کامپوننت یا پلاگین شخص دیگری را به پروژه اضافه کنید.
</p>

<p>
این احتمال وجود دارد که در کامپوننتی که میخواهید به پروژه اضافه کنید هم، نام چند تا از کلاسها با نام کلاس‌هایی که شما انتخاب کردید یکی باشد.
</p>

<p>
بنابراین پیامدهایی که بوجود می‌آید شامل موارد زیر هستند:
</p>

<p>
<ul>
<li>تداخل نام بین کدهایی که شما ایجاد کردید، و کلاس ها و ثابت ها و توابع داخلی PHP و یا کلاس ها و ثابت ها و توابع یک کامپوننت&nbsp;دیگر.</li>
<li>برای حل مشکل اول از نام های طولانی و توصیفگر یا پیشوند گذاری قبل بعضی نام ها استفاده می‌شد. که این کار خودش به نوعی کار برنامه‌نویس را سخت‌تر می‌کند.</li>
</ul>
</p>

<p>
اما حالا براحتی می‌توانیم فضای نام یا namespace تعریف کنیم که با این کار به نوعی کلاس ها و تابع ها و ثابت‌هامون رو بخش بندی خواهیم کرد و دیگر خبری از تداخل نیست.
</p>

<p>
نکته: داخل یک namespace هم طبیعتا نمی توان کلاس‌ها یا تابع‌ها یا ثابت‌های هم نام تعریف کرد.
</p>



<h3>تعریف namespace</h3>

<p>
خط تعریف namespace باید اولین دستور در بالای کدهایتان و قبل از هر کد دیگری باشد. طبق استاندار PSR-2 یک خط خالی بعد از تعریف آن با بقیه کدها باید وجود داشته باشد.
</p>



<pre><code class="language-php  line-numbers"><?php
namespace FooProject;

const CONNECT_OK = 1;
class Connection { /* ... */ }
function connect() { /* ... */  }
</code></pre>




<h3>تعریف Sub-namespaces</h3>

<p>
مانند فایل‌ها و دایرکتوری‌ها، میتوانید یک ساختار سلسله مراتبی و تو در تو برای کدهایتان درست کنید که با کاراکتر backslash (\)  از هم جدا میشوند.
</p>

<pre><code class="language-php  line-numbers"><?php
namespace MyProject1;

class Foo {
	public function Bar()
	{
		echo 'Bar method for MyProject1, Foo class. <br />';
	}
}
$objFoo1=new Foo;
$objFoo1->bar();

namespace MyProject2;

class Foo {
	public function Bar()
	{
		echo 'Bar method for MyProject2, Foo class. <br />';
	}
}
$objFoo2=new Foo;
$objFoo2->bar();
</code></pre>

<p>
در مثال بالا ما دو namespace در یک فایل PHP تعریف کردیم که اینکار امکان پذیر است، اما هرگز توصیه نمی‌شود و برای هر فایل بایستی یک namespace تعریف کرد.
</p>

<p>
نکته: اگر در یک فایل بخواهید از کد namespace شده و کد namespace نشده (Global code) استفاده کنید. باید از ساختار براکت شده مثل زیر استفاده کنید:
</p>

<pre><code class="language-php  line-numbers"><?php
namespace MyProject { // MyProject namespace code
}

namespace { // global code
}
</code></pre>


<h3>
فراخوانی کدهای Namespace شده
</h3>

<p>
در فایل lib1.php، یک ثابت و یک تابع و یک کلاس تعریف کرده ایم و namespace آنرا App\Lib1 قرار داده ایم:
</p>

<pre><code class="language-php  line-numbers"><?php
// application library 1
namespace App\Lib1;

const MYCONST = 'App\Lib1\MYCONST';

function MyFunction() {
	return __FUNCTION__;
}

class MyClass {
	static function WhoAmI() {
		return __METHOD__;
	}
}
</code></pre>

<p>
حال برای صدا زدن (call) کدهای بالا در فایل دیگر برای مثال فایل myapp.php یک روش استفاده از کدی مشابه کد زیر است:
</p>


<pre><code class="language-php  line-numbers"><?php
header('Content-type: text/plain');
require_once('lib1.php');

echo \App\Lib1\MYCONST . "\n";
echo \App\Lib1\MyFunction() . "\n";
echo \App\Lib1\MyClass::WhoAmI() . "\n";
</code></pre>


<p>
بسیارخب، در کد myapp.php هیچ namespaceایی تعریف نشده، بنابراین در فضای global هستیم. از آنجاییکه MYCONST و MyFunction و MyClass در فضای نام App\Lib1 تعریف شده‌اند شما به طور مستقیم قادر به فراخوانی آنها نیستید و بایستی پیشوند \App\Lib1 را اضافه کنید تا یک نام fully-qualified داشته باشید. در نهایت خروجی زیر را خواهید داشت:
</p>

<pre><code class="language-php  line-numbers"><?php
App\Lib1\MYCONST
App\Lib1\MyFunction
App\Lib1\MyClass::WhoAmI
</code></pre>

<p>
اما نام های fully-qualified خیلی طولانی هستند و مزیت زیادی نسبت به مثلا نامگذاری کلاس به صورت App-Lib1-MyClass ندارند.
قبل از اینکه بخواهید استفاده از namespaceها را یاد بگیرید مهم است که بدانید PHP چطور تشخیص میدهد که کدام بخش از کد namespace شده درخواست شده است. یک قیاس ساده بین namespaces در PHP و فایل سیستم می‌تواند مثال خوبی باشد.
</p>

<p>
در فایل سیستم 3 راه برای دسترسی به یک فایل داریم:
</p>

<p>
<ul>
<li>نام فایل به صورت نسبی (Relative) باشد مانند <span class="en-words">foo.txt</span>. که این مسیر به <span class="en-words">currentdirectory/foo.txt</span> تبدیل می‌شود که <span class="en-words">currentdirectory</span> همان دایرکتوری جاری است که در آن قرار داریم.</li>
<li>آدرس دهی نسبی مثل <span class="en-words">subdirectory/foo.txt</span> که به <span class="en-words">currentdirectory/subdirectory/foo.txt</span> تبدیل میشود.</li>
<li>آدرس دهی مطلق مانند <span class="en-words">main/foo.txt/</span>&nbsp;که تبدیل می‌شود&nbsp;به <span class="en-words">main/foo.txt/</span>&nbsp;(یعنی به خودش).</li>
</ul>
</p>

<p>
همین قاعده نیز برای عناصر namespace شده PHP کاربرد دارد. مثلا نام کلاس به سه روش زیر معرفی شده:
</p>

<div align="center">
<img src="/images/post/2_resolve_name-compressor.png" alt="{{page.title}}" />
</div>

<p>
1. Unqualified name یا نام کلاس بدون پیشوند مثل :
</p>

<pre><code class="language-php  line-numbers">$a = new foo();</code></pre>

<p>
یا
</p>

<pre><code class="language-php  line-numbers">foo::staticmethod();</code></pre>


<p>
اگر namespace جاری currentnamespace باشد، تبدیل می‌شود به currentnamespace\foo اگر هم بدون namespace باشد تبدیل می شود به foo.
</p>

<p>
نکته: وقتی علامت \ را قبل از نام تابع یا ثابت قرار بدهیم تابع یا ثابت global هدف قرار میگیرد.
</p>


<pre><code class="language-php  line-numbers"><?php
namespace A\B\C;

function strlen($str)
{
	return 'ok';
}

echo strlen('hi'), "<br />"; // prints "ok"
echo \strlen('hi'), "<br />"; // prints "2"
</code></pre>

<p>
2. Qualified name یا نام کلاس همراه با پیشوند مثل :
</p>

<pre><code class="language-php  line-numbers">$a = new subnamespace\foo();
</code></pre>

<p>
یا
</p>

<pre><code class="language-php  line-numbers">subnamespace\foo::staticmethod();
</code></pre>


<p>
اگر namespace جاری currentnamespace باشد تبدیل به currentnamespace\subnamespace\foo می‌شود و اگر  بدون namespace باشد، subnamespace\foo اجرا می‌شود.
</p>

<p>
3. Fully qualified name یا نام پیشوند گذاری شده با پیشوند global \ مثل :
</p>

<pre><code class="language-php  line-numbers">new \currentnamespace\foo();
</code></pre>

<p>
یا
</p>

<pre><code class="language-php  line-numbers">\currentnamespace\foo::staticmethod();
</code></pre>

<p>
همیشه تبدیل می‌شود به همان نام مشخص شده یعنی خودش
</p>

<pre><code class="language-php  line-numbers">currentnamespace\foo
</code></pre>