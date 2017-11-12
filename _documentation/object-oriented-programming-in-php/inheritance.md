---
layout: documentation
title:  Inheritance (وراثت)
cattitle: آموزش شی گرایی در php
date:   2017-11-01 20:57:42 +0330
jdate: چهارشنبه 10 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
thumbnail: phpOOPTutorialPAGE.png
refrence: https://roocket.ir/articles/object-oriented-programming-in-php-part-4 <br> http://bshafiei.ir/Article_view/index/1XvocDexH9mhWT/برنامه-نویسی-شی-گرا-در-PHP <br>  http://w3-farsi.ir/?p=2432 <br> http://python.coderz.ir/lessons/l05.html
---
<h3>وراثت (Inheritance)</h3>
<p>
وراثت یکی از شکل‌های «قابلیت استفاده مجدد» کد بوده که برنامه‌نویس را قادر می‌سازد تا با ارث‌بری صفات و متدهای یک یا چند کلاس موجود، کلاس‌های جدیدی را ایجاد نماید.
</p>

<p>
برای نمونه فرض کنیم صاحب کلاس کارخانه خودروسازی مثال پیش، قصد تولید یک مدل خودرو جدید با رویکرد باربری دارد؛ بنابراین می‌بایست کلاسی جدید برای تولید آن تهیه نماید. ولی کلاس جدید علاوه‌بر صفات (ظرفیت بارگیری و..) و متدهای (انجام بارگیری، تخلیه بار و...) خاص خودش به صفات (رنگ بدنه، ظرفیت باک و...) و متدهای (راندن، سوخت گیری، توقف و...) مشابه در کلاس قبل هم نیاز دارد؛ در این حالت نیازی به تعریف مجدد آن‌ها نیست و می‌توان صفات و متدهای کلاس پیش را در کلاس جدید به ارث برد.
</p>

<p>
به کلاسی که از آن ارث‌بری می‌شود ”Parent Class“ یا ”Base Class“ (کلاس پایه) یا ”Superclass“ و به کلاسی که اقدام به ارث‌بری می‌کند ”Child Class“ (کلاس فرزند) یا ”Derived Class“ یا ”Subclass“ گفته می‌شود.
</p>

<p>
ارث‌بری توسط «نسبت هست-یک» (IS-A Relationship) بیان می‌شود؛ این نسبت می‌گوید کلاس فرزند یک نوع از چیزی است که کلاس پایه هست. کلاس A از کلاس B ارث‌بری دارد؛ در این حالت می‌گوییم: A is a type of B، یعنی درست است اگر بگوییم: «سیب» یک نوع «میوه» است یا «خودرو» یک نوع «وسیله نقلیه» است ولی توجه داشته باشید که این یک ارتباط یک‌طرفه از کلاس فرزند به کلاس پایه است و نمی‌توانیم بگوییم: «میوه» یک نوع «سیب» است یا «وسیله نقلیه» یک نوع «خودرو» است.
</p>

<p>
کلاس‌ها می‌توانند مستقل باشند ولی هنگامی که وارد رابطه‌های وراثت می‌شوند، یک ساختار سلسله مراتب (Hierarchy) به شکل درخت را تشکیل می‌دهند. برای نمونه به ساختار سلسله مراتب وراثت پایین که مربوط به برخی اشکال هندسی است توجه نمایید، پیکان‌ها نشانگر نسبت is-a هستند.
</p>

<div align="center">
<img src="/images/post/l05-Inheritance-Hierarchy-Sample.png" alt="{{page.title}}" />
</div>


<p>
در برنامه‌نویسی شی‌گرا نسبت دیگری نیز با عنوان «نسبت دارد-یک» (HAS-A Relationship) وجود دارد که بیانگر مفهومی به نام «ترکیب» (Composition) است که شکل دیگری از قابلیت استفاده مجدد کد می‌باشد ولی مفهومی متفاوت با وراثت دارد. این نسبت زمانی بیان می‌شود که درون یک کلاس (مانند: C) از کلاس دیگری (مانند: D) نمونه‌سازی شده باشد؛ یعنی شی کلاس C درون خودش شی‌ای از کلاس D را داشته باشد؛ در این حالت می‌گوییم: C has a D. به یاد دارید خواندیم کلاس خودرو از کلاس‌های کوچکتری ساخته شده است؛ مثلا کلاس موتور - یعنی درون این کلاس یک شی از کلاس موتور ایجاد شده است، اکنون می‌توانیم بگوییم: «خودرو» یک «موتور» دارد.
</p>


<div align="center">
<img src="/images/post/l05-has-a-Sample.png" alt="{{page.title}}" />
</div>


<h3>
وراثت در PHP
</h3>

<p>
در نظر بگیرید شما باید یک متدی با یک وظیفه ای خاصی بنویسید اما دقیقا این متد در پدر یا پدر پدر کلاس فعلیتون وجود داره خوب بنظرتون باید دوباره اون method رو بنویسید یا از اون متدی که در کلاس های پدر هست استفاده کنید . اول اینکه دوباره نویسی کدهاتون فوق العاده کم میشه و بعدش اینکه مدیریت روی کدهاتون به راحتی بالا میره . کیه که این روش کد نویسی رو نخواد . چون واقعا کار رو راحتتر میکنه .
</p>
<p>
بزارید یک مثال ساده بزنم . در پایین من یک کلاس معمولی به اسم Father میسازم و یک method توش قرار میدم .
</p>

<pre><code class="language-php  line-numbers">class father
{
    public function getEyeCount() {
        return 2 ;
    }
}
</code></pre>

<p>
خب حالا که کلاس پدر رو ساختیم میخوام یک کلاس دیگه مثل زیر بسازم به اسم child و به father متصل کنم.
</p>

<pre><code class="language-php  line-numbers">class child extends father
{

}

$obj = new child;
echo $obj--->getEyeCount; // 2
</code></pre>

<p>
در بالا ما با کمک کلمه کلیدی extends تونستیم کلاس child رو به کلاس father مرتبط کنیم و با استفاده از متدی که در کلاس father هست مقداری رو در کلاس فرزند برگردونیم .
</p>

<p>
نکته : به یاد داشته باشید property ها و method های که از نوع private باشن قابلیت ارث بری ندارن و نمیشه در کلاس های فرزند از این نوع method ها و property ها استفاده کرد .
</p>

<p>
در اینجا چند مسئله پیش میاد که شاید برای شما هم سوال شده باشه ! مسئله اول اینکه یک کلاس که از کلاس پدر ارث می بره خودش (فرزند) میتونه ویزگی های ( method ها و peroperty های ) خودش رو داشته باشه ؟ مسئله دوم اینکه آیا میشه method ها و property های که در کلاس پدر هست در کلاس فرزند هم دوباره نویسی کرد با یک ویژگی خاص دیگه ؟ بزارید اینجا به این دو مسئله جواب بدیم تا دیگه سوالی در موردش نباشه .
</p>

<p>
یک کلاس که از کلاس پدر ارث می بره خودش (فرزند) میتونه ویزگی های خودش رو داشته باشه ؟
</p>

<p>
جواب این مسئله بله است . چرا ؟ این دفعه بزارید با یک سوال از شما به چرایی این موضوع پی ببریم . آیا شمایی که ویژگی های رو از والدینتون به ارث میبرید . آیا خودتون اخلاق و ویژگی های خاص خودتون رو ندارید ؟ فکر کنم فهمیده باشید داستان چیه . چون مسئله سختی نیست . ولی با این حال

به کد زیر توجه کنید که در داخل کلاس فرزند یک متد جدید میسازیم و به راحتی ازش استفاده میکنیم .
</p>


<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1;
  }
}

class MyOtherClass extends MyClass
{
  public function newMethod()
  {
      echo "From a new method in" . __CLASS__ ;
  }
}

// Create a new object
$newobj = new MyOtherClass;

// Output the object as a string
echo $newobj->newMethod();

// Use a method from the parent class
echo $newobj->getProperty();
</code></pre>

<p>
 نتیجه کد بالا بصورت زیر به نمایش در میاد اما شاید براتون سوال شده باشه که __CLASS__ دقیقا چیه این اسم کلاس رو برامون بر میگردونه .
</p>

<pre><code class="language-php ">From a new method in MyOtherClass.
I'm a class property!
</code></pre>

<h3>مفهوم Overriding</h3>

<p>
 آیا میشه method ها و property های که در کلاس پدر هست در کلاس فرزند هم دوباره نویسی کرد با یک ویژگی دیگه ؟
</p>


<p>
جواب این مسئله هم بله است ، شما به راحتی مثل کد زیر می تونید همون متدی که در کلاس پدر هست با همون اسم در کلاس فرزند بسازین و دوباره نویسی کنید با ویژگی های جدید اینطوری اول چک میکنه که اون method در کلاس فزرند هست یا خیر اگر بود که برگشت داده میشه و اگر نبود به کلاس پدر میره و دنبال اون method میگرده و اگر بود برمیگردونه.
</p>

<pre><code class="language-php  line-numbers">class Foo
{
    public function printItem($string)
    {
        echo 'Foo: ' . $string . PHP_EOL;
    }

    public function printPHP()
    {
        echo 'PHP is great.' . PHP_EOL;
    }
}

class Bar extends Foo
{
    public function printItem($string)
    {
        echo 'Bar: ' . $string . PHP_EOL;
    }
}

$foo = new Foo();
$bar = new Bar();
$foo->printItem('baz'); // Output: 'Foo: baz'
$foo->printPHP();       // Output: 'PHP is great'
$bar->printItem('baz'); // Output: 'Bar: baz'
$bar->printPHP();       // Output: 'PHP is great'
</code></pre>

<h3>کلمه کلیدی Final</h3>

<p>
در PHP5 کلمه کلیدی final معرفی شد که از همپوشانی یک متدی که به صورت final‌تعریف شده توسط کلاس های فرزند جلوگیری می کند. اکر کلاس خودش به صورت final تعریف شود آنگاه نمی تواند ارث بری شود.
</p>



<pre><code class="language-php  line-numbers"><?php
class BaseClass {
    public function test() {
        echo "BaseClass::test() called<br>";
    }

    final public function moreTesting() {
        echo "BaseClass::moreTesting() called<br>";
    }
}

class ChildClass extends BaseClass {
    public function moreTesting() {
        echo "ChildClass::moreTesting() called<br>";
    }
}
</code></pre>

<p>
مثال بالا منجر به خطای زیر شده است :
</p>

<pre><code class="language-php">
Cannot override final method BaseClass::moreTesting()
</code></pre>


<h3>عملگرهای parent و self</h3>
<p>
parent و self در PHP دو کلمه کلیدی هستند که کدنویسی را در زمان نوشتن برنامه های شیء گرا راحت می کنند. از کلمه کلیدی parent برای دسترسی به سازنده و متدهای کلاس والد و از کلمه کلیدی self برای دسترسی به کلاس جاری و استفاده از اعضا و متدهای استاتیک و همچنین ثابت های کلاس استفاده می شود. نحوه استفاده از این دو کلمه برای دسترسی به اعضا و متدها به صورت زیر است :
</p>

<pre><code class="language-php">parent :: class member
self :: class member
</code></pre>

<p>
یعنی مثلا اگر بخواهیم از یک ثابت در یک کلاس استفاده کنیم کافیست کلمه self و بعد از آن دو نقطه و سپس نام ثابت را بنویسیم. در کد زیر نحوه استفاده از این دو کلمه کلیدی آمده است :
</p>

<pre><code class="language-php   line-numbers"><?php
     class ParentClass
     {
         const NAME = "ParentClass";
         function __construct()
         {
             echo "In " . self::NAME . " constructor" . "<br/>";
         }
     }

     class Child extends ParentClass
     {
         const NAME = "Child";
         function __construct()
         {
             parent::__construct();
             echo "In " . self::NAME . " constructor" . "<br/>";
         }
     }

     $child = new Child();
 </code></pre>

 <p>خروجی : </p>
 <pre><code class="language-php">
 In ParentClass constructor
 In Child constructor
  </code></pre>

  <p>
  همانطور که احتمالا متوجه شده اید برای دسترسی به اعضا، متدها و ثابت ها بعد از این دو کلمه کلیدی علامت دو نقطه (::) می گذاریم. کلمه کلیدی self در خط 7 به کلاس ParentClass و در خط 17 به کلاس Child اشاره دارد. در همین دو خط علامت دو نقطه و سپس نام ثابت های این دو کلاس یعنی NAME را نوشته ایم و این بدین معنی است که می خواهیم از این ثابت ها استفاده کنیم. در خط 16 برای اینکه از تمام کدهای سازنده کلاس پدر استفاده کنیم، به راحتی کلمه parent و بعد دو نقطه و در نهایت نام سازنده یعنی ()constract__ را می نویسیم. این کار باعث می شود تمام کدهای موجود در سازنده کلاس پدر در داخل کلاس فرزند اجرا شوند. برای همین است که وقتی یک شیء از کلاس فرزند ایجاد می کنیم کدهای سازنده کلاس پدر (خط 7) اجرا می شوند.
  </p>