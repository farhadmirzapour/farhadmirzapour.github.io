---
layout: documentation
title:  interface در PHP
cattitle: آموزش شی گرایی در php
date:   2017-11-02 21:06:42 +0330
jdate: پنج شنبه 11 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
thumbnail: phpOOPTutorialPAGE.png
refrence: http://haamid.ir/مفهوم-و-کاربرد-اینترفیس-interface-در-php/ <br> http://alihossein.ir/tutorials/آموزش-interface
---
<h3> مفهوم و کاربرد اینترفیس – interface در PHP</h3>
<p>
قبلا در مورد کلاسهای abstract در PHP مطلبی نوشتم ، کلاسهای انتزاعی یا abstract کلاسهایی هستند که قابل نمونه گیری نیستند و میتونیم درون کلاسهای انتزاعی متد هایی بنویسیم و در کلاسهایی که از اون مشتق شدن ازش استفاده کنیم. اینترفیس ها دقیقا مثل کلاسهای انتزاعی هستند ، با این تفاوت که در اینترفیس ها هیچ متدی نمیتونه دارای بدنه باشه. اولش خیلی عجیب به نظر میرسه… وقتی متدها بدنه نداشته باشن ، به چه دردی میخودن؟
</p>

<p>
به خاطر داشته باشید که شما با تعریف یک متد در کلاس abstract نویسنده کلاسهای دیگه (که از این کلاس مشتق شدن) رو وادار نمیکردید که متدهای کلاس پدر رو بازنویسی کنه. ولی در اینترفیس ها اینطور هست ، یعنی هر کلاسی که یک اینرفیس رو implement کنه ، باید تمام متدهای کلاس پدر رو بازنویسی کنه.
</p>


<p>
<ul>
<li>interface ها نباید  شامل هیچ بدنه یک تابع باشند و تابع های درون آن باید بدون بدنه باشند همگی. در صورتی که  abstract می توانستند ادغامی از توابع معمولی و توابع abstract (بدون بدنه) شوند.
</li>
<li>از interface ها مانند  abstract نمیتوان نمونه یا شی ای ایجاد کرد.
</li>
<li>interface به کلمه ی کلیدی extends از کلاس interface دیگری مشتق میگیرد.
</li>
<li>
کلاس معمولی با کلمه ی implements  از interface دیگری مشتق میگیرد.
</li>
</ul>
</p>

<h3>نحوه ی تعریف interface ها در PHP</h3>

<p>
برای تعریف اینترفیس ها از کلمه کلیدی interface استفاده میکنیم :
</p>
<pre><code class="language-php  line-numbers">interface abc
{
public function xyz($b);
}
</code></pre>

<p>
در مثال بالا کلاسها رو مجبور میکنیم که متد xyz رو بنویسن. برای ارث بردن یا به اصطلاح تکمیل کردن یک اینترفیس از کلمه کلیدی implements استفاده میکنیم :
</p>

<pre><code class="language-php  line-numbers">class test implements abc
{
public function xyz($b)
{
//your function body
}
}
</code></pre>


<p>
نکته قابل توجه اینه که تمام متد ها و پراپرتی هایی که درون یک اینترفیس تعریف میشن باید بصورت عمومی یا public تعریف بشن ، در غیر اینصورت با اررور مواجه میشیم.
</p>

<p>
یک نکته دیگه اینکه یک کلاس میتونه بیش از یک اینترفیس رو implement کنه و همچنین یک اینترفیس میتونه از چند اینترفیس دیگه مشتق بشه.
</p>

<br>
<hr>

<p>
مثال :
</p>

<pre><code class="language-php  line-numbers"><?php
interface a
{
    public function foo();
}

interface b extends a
{
    public function baz(Baz $baz);
}

// This will work
class c implements b
{
    public function foo()
    {
    }

    public function baz(Baz $baz)
    {
    }
}
</code></pre>


<p>
<ul>
<li>&nbsp;در خط 2 یک&nbsp;interface تعریف شده است.</li>
<li>در خط 4 متدی تعریف شده به نام&nbsp;foo . چون داخل کلاسی از نوع Interface است پس نباید بدنه داشته باشد.</li>
<li>در خط 7&nbsp;یک&nbsp;Interface تعریف شده و طبق قانون گفته شده با کلمه ی کلیدی extends از a مشتق شده است.</li>
<li>دز خط 13 کلاس معمولی c طبق قانون گفته شده &nbsp;با کلمه کلیدی&nbsp;implements از b&nbsp;مشتق شده است.</li>
<li>در خط 15 و 19 توابعی تعریف شدن به همراه بدنه. زیرا کلاس c از دو interface مشتق شده است و باید بدنه های اون کلاس رو تکمیل کنه حتما.</li>
</ul>
</p>

<br>
<hr>

<p>
مثال :
</p>

<pre><code class="language-php  line-numbers"><?php
interface a
{
    public function foo();
}

interface b
{
    public function bar();
}

interface c extends a, b
{
    public function baz();
}

class d implements c
{
    public function foo()
    {
    }

    public function bar()
    {
    }

    public function baz()
    {
    }
}
</code></pre>


<p>
<ul>
<li>
     نکته مهم اش اینه که در خط 12 کلاس c تعریف شده که از دو اینترفیس مشتق شده و درون اون 3 متد تعریف شده ، برای اینکه کلاس هایی که ازشون متشق گرفته دارای این 3 متد بودن.
</li>
</ul>
</p>