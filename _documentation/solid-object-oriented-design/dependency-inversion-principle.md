---
layout: documentation
title:  Dependency Inversion Principle
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا (OOD)
date:   2017-11-14 19:31:42 +0330
jdate: سه شنبه 23 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
# permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: http://pikneek.com/programming/مفهوم-solid-در-برنامه-نویسی-dip/ <br> http://chistio.ir/معکوس-سازی-وابستگی-dependency-inversion-برنامه-نویس/ <br> http://roxo.ir/آموزش-تزریق-وابستگی/
---
<h3>مفهوم Dependency Inversion Principle (اصل معکوس سازی وابستگی‌ها)</h3>
<p>
DIP  به طور خلاصه این مفهوم را می رساند :
</p>

<blockquote>
<p>
<ol>
<li>
ماژول های سطح بالا نباید به ماژول های سطح پایین وابسته باشند. بلکه باید هر دو به یک رابط (Abstraction) وابسته شوند.
</li>

<li>
انتزاع‌ (Abstraction) نباید به جزئیات وابسته باشند. جزئیات باید به انتزاع وابسته باشند. (منظور از انتزاع دید کلی نسبت به یک شیء است مثلا وقتی ما می‌گوییم میز چیزی که در ذهن ما نقش می‌بندد یک شکل کلی است ولی وقتی می‌گوییم میز ناهارخوری دقیقا مشخص می‌کنیم که چه نوع میزی است. در نتیجه انتزاع یک دید کلی از یک شیء بحساب می‌آید)
</li>

</ol>
</p>
</blockquote>


<p>
Dependency Inversion Principle (اصل معکوس سازی وابستگی‌ها) مفهومی است که وابستگی مستقیم کلاس های سطح بالا را به کلاس های سطح پایین منع میکند. به این منظور که اگر کلاس خاصی(high-level) که از کلاس های دیگر(low-level) استفاده می کند وابستگی مستقیمی با کلاس های low-level داشته باشد سبب بروز این مشکل خواهد شد که اگر کلاس low-level دیگری به مجموعه افزوده شود اجبارا کلاس high-level نیز بایستی تغییر کند. DIP برای حل این مشکل به وجود آمده است و این وابستگی باید معکوس شده و همچنین بر اساس Abstraction یا برای مثال استفاده از اینترفیس‌ها صورت گیرد.
</p>

<p>
جهت مفهوم این مطلب به مثال زیر توجه کنید :
</p>

<pre><code class="language-php   line-numbers"><?php
class Mailer
{
    public function send($string)
    {
        // sending mail
    }
}
class SendWelcomeMessage
{
    private $mailer;
    public function __construct(Mailer $mailer)
    {
        $this->mailer = $mailer;
    }
    public function send_string($user_id)
    {
        $string = 'welcome user ' . $user_id;
        $this->mailer->send($string);
    }
}

</code></pre>

<p>
در تکه کد بالا یک کلاس SendWelcomeMessage داریم و در آن یک تابع send_string که توسط آن پیغام خوش آمد گویی را برای کاربری با شماره $user_id میفرستد. این کلاس از یک کلاس Mailer استفاده میکند تا بتواند از تابع send آن ایمیل خود را بفرستد. پس الان یک کلاس SendWelcomeMessage داریم که به وسیله آن میتوانیم یک پیغام خوش آمد گویی را برای کاربران ایمیل کنیم.
</p>

<p>
حال فرض کنید نیازمندی های پروژه عوض می شود و میخواهیم به جای ایمیل کردن این پیام، این پیام را SMS کنیم. قاعدتا بایستی بسیار از قسمت های کد بالا را تغییر داده و کد را دوباره بازنویسی کنیم.
</p>

<p>
معکوس سازی وابستگی یا همان Dependency Inversion اینجا به کمک ما می آید. توجه کنید که کلاس SendWelcomeMessage به کلاس Mailer وابسته است. که این کار در معکوس سازی وابستگی ها خلاف قانون ماست. پس به تکه کد زیر نگاهی بیندازید:
</p>

<pre><code class="language-php   line-numbers"><?php
Interface Sender
{
    public function send($string);
}
class Mailer implements Sender
{
    public function send($string)
    {
        // sending mail
    }
}
class SMSer implements Sender
{
    public function send($string)
    {
        // sending SMS
    }
}
class SendWelcomeMessage
{
    private $sender;
    public function __construct(Sender $sender)
    {
        $this->sender = $sender;
    }
    public function send_string($user_id)
    {
        $string = 'welcome user ' . $user_id;
        $this->sender->send($string);
    }
}
</code></pre>

<p>
در اینجا عملیات Refactoring انجام داده ایم. یعنی کد را به حالتی بازنویسی کرده ایم که از لحاظ مهندسی نرم افزار بهتر باشد. همان طور که میبینید کلاس SendWelcomeMessage دیگر به کلاس Mailer وابسته نیست در عوض به یک واسط(Interface) به اسم Sender وابسته است. دو کلاس دیگر هم داریم Mailer و SMSer که هر دوی آن ها از نوع Sender هستند. حال هر موقع که بخواهیم میتوانیم به کلاس SendWelcomeMessage بگوییم که از کدام کلاس استفاده کند.
</p>
