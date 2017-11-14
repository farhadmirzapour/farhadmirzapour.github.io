---
layout: documentation
title:  Interface Segregation Principle
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا (OOD)
date:   2017-11-14 17:43:42 +0330
jdate: سه شنبه 23 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
# permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: https://code.tutsplus.com/tutorials/solid-part-3-liskov-substitution-interface-segregation-principles--net-36710 <br> http://pikneek.com/programming/مفهوم-solid-در-برنامه-نویسی-isp/ <br> http://roocket.ir/articles/solid-principles-in-object-oriented-programming-part-iv-the-principle-of-interface-separation <br> http://alihossein.ir/tutorials/آموزش-interface-segregation-principle-solid <br> https://github.com/wataridori/solid-php-example/blob/master/4-interface-segregation-principle.php <br> http://chistio.ir/مفهوم-جدایی-واسط-ها-interface-segregation-principle-مهندسی-نر/
---
<h3>مفهوم Interface Segregation Principle</h3>
<p>
lsp  به طور خلاصه این مفهوم را می رساند :
</p>

<blockquote>
<p>
کلاینت‌ها نباید وابسته به متدهایی باشند که آنها را پیاده سازی نمی کنند و یا نباید به روش هایی که از آن ها استفاده نمی کنند وابسته باشند.
</p>
</blockquote>

<p>
مفهوم جدایی واسط ها در مهندسی نرم افزار بدین معنی است که هیچ شخص یا کدی نباید به ماژولی وابسته باشد که به آن احتیاج ندارد.
</p>

<p>
برای استفاده از اینترفیس ها آنها را باید به اجزای کوچکتری تقسیم کرد. وقتی یک کلاس از یک اینترفیس بزرگ استفاده میکند ممکن است برخی از این متد ها در کلاس مورد نظر قابل استفاده نباشند. اما وقتی یک اینترفیس بزرگ به چند اینترفیس کوچک تقسیم می شود هر کلاس میتواند در صورتی که به اینترفیس خاصی نیاز داشت از آن استفاده نماید. با این امکان اگرچه تعداد اینترفیس ها بیشتر می شوند و ممکن است تکرار رخ دهد اما به دلیل اینکه منطق برنامه ما در اینترفیس ها اجرا نمی شود میتوان این مسئله را نادیده گرفت. در نهایت با رعایت این اصل امکان دیباگ و بررسی کد ها سرعت بیشتری خواهد داشت.
</p>

<p>
تصویر زیر را در نظر بگیرید :‌
</p>

<div align="center">
<img src="/images/post/oneInterfaceManyClients.png" alt="{{page.title}}" />
</div>


<p>
اینترفیس VEHICLE برای کلاس های مرتبط با حمل و نقل ایجاد شده است. این در صورتی است که کلاس هایی مانند HighWay و BusStation که از این اینترفیس استفاده میکنند نیازی به متد هایی مانند stopRadio و یا brake ندارند. اصل چهارم SOLID تاکید بر این موضوع دارد که اینترفیس های بزرگ به اینترفیس های کوچک تر تبدیل شوند. تصویر زیر را در نظر بگیرید :
</p>

<div align="center">
<img src="/images/post/segregatedInterfaces.png" alt="{{page.title}}" />
</div>

<p>
در تصویر بالا به جای یک اینترفیس بزرگ دو اینترفیس کوچک ایجاد کردیم. گرچه برخی از متد ها تکرار شده اند اما همانطور که در ابتدای موضوع مطرح شد این اینترفیس ها قرار نیست منطق برنامه ما را تشکیل دهند لذا اصل اول SOLID را نقض نمی کند. استفاده از اینترفیس های کوچک به ما کمک میکند تا مشکلات را سریع تر شناسایی کنیم. راحت تر تست بگیریم و راحت تر کد های نوشته شده را درک کنیم.
</p>


<p>
جهت مفهوم این مطلب به مثال زیر توجه کنید :
</p>

<pre><code class="language-php   line-numbers">interface Workable
{
    public function code();
    public function test();
}
class Programmer implements Workable
{
    public function canCode()
    {
        return true;
    }
    public function code()
    {
        return 'coding';
    }
    public function test()
    {
        return 'testing in localhost';
    }
}
class Tester implements Workable
{
    public function canCode()
    {
        return false;
    }
    public function code()
    {
         throw new Exception('Opps! I can not code');
    }
    public function test()
    {
        return 'testing in test server';
    }
}

public function processCode(Workable $member)
{
    if ($member->canCode()) {
        $member->code();
    }
}

</code></pre>

<p>
در کد بالا اینترفیسی به نام Workable تعریف کردیم که دو کلاس Programmer و Tester  ملزم به پیاده سازی متدهای code و test  درون خود می باشند.
</p>

<p>
به کلاس Tester  دقت کنید که این کلاس نیازی به پیاده سازی متد code ندارد اما به دلیل implements شدن از اینترفیس Workable مجبور به پیاده سازی آن شده است و در نتیجه در  تابع processCode واقع در خط 39 ابتدا بررسی می گردد که شی دریافت شده توانایی اجرای متد code  را دارد و یا خیر .
</p>

<p>
اصل Interface segregation principle مخالف این قضیه هست و می گوید اگر متدهایی دارین که در برخی از کلاس ها لازم است و در برخی خیر , پس آنها را داخل interface های مختلفی بگذارید و هر موقع لازم شد آنها را implement کنید .
</p>

<p>
بنابراین به جای اینترفیس Workable از دو اینترفیس Codeable و Testableاستفاده می کنیم و کلاس Programmer چون هر دو متد code و test بهره می برد از اینترفیس های  Codeable و Testable و کلاس Tester تنها از اینترفیس Testable پیاده سازی می گردند :
</p>
<pre><code class="language-php   line-numbers">interface Codeable
{
    public function code();
}
interface Testable
{
    public function test();
}
class Programmer implements Codeable, Testable
{
    public function code()
    {
        return 'coding';
    }
    public function test()
    {
        return 'testing in localhost';
    }
}
class Tester implements Testable
{
    public function test()
    {
        return 'testing in test server';
    }
}


public function processCode(Codeable $member)
{
    $member->code();
}
</code></pre>

