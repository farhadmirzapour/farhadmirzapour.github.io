---
layout: documentation
title:  ساختار کلاس در PHP
cattitle: آموزش شی گرایی در php
date:   2017-11-01 20:53:42 +0330
jdate: چهارشنبه 10 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
thumbnail: phpOOPTutorialPAGE.png
refrence: https://roocket.ir/articles/object-oriented-programming-in-php-part-1 <br> https://bshafiei.ir/Article_view/index/1XvocDexH9mhWT/برنامه-نویسی-شی-گرا-در-PHP
---
<h3>ساختار کلاس ها</h3>
<p>در php یک کلاس با کلمه کلیدی (<span style="font-size: 16px;"><strong><span class="notespan">class</span></strong></span>) بوجود میاد و با یک اسپیس و تایپ یک اسم، شما اسم اون کلاس رو تعریف میکنید و در نهایت با قرار دادن براکت های باز و بسته ( <span style="font-size: 16px;"><strong><span class="notespan">{ }</span></strong></span>&nbsp;) کار یک class رو شروع میکنید . دقیقا مثل مثال زیر :
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  // class propertys and methods go here;
}
</code></pre>

<p>
بعد از به وجود آوردن کلاس ما با استفاده از کلمه کلیدی new می تونیم از اون کلاس استفاده کنیم و یک شی (object) با همون کلاس بسازیم . در زیر میتونید این روش رو ببینید:
</p>
<pre><code class="language-php  line-numbers">$obj = new MyClass;</code></pre>

<p>
شما با قرار دادن شی (obj$) در داخل var_dump میتونید محتوای کلاس رو مشاهده کنید:
</p>
<pre><code class="language-php  line-numbers">var_dump($obj);</code></pre>


<h3>معرفی property ها</h3> <p>برای اضافه کردن اطلاعات در کلاس ها از <span style="font-size: 16px;"><strong><span class="notespan">property </span></strong></span>ها استفاده میشه . کار اونها دقیقا شبیه متغیرها در php معمولیه و تنها تفاوتشون اینکه قبل از تایپ اسم <span class="notespan"><span style="font-size: 16px;"><strong>property </strong></span></span>از کلمات کلیدی <strong><span class="notespan"><span style="font-size: 16px;">private </span></span></strong>,&nbsp;<span style="font-size: 16px;"><span class="notespan"><strong>protected </strong></span></span>و <strong><span class="notespan"><span style="font-size: 16px;">public </span></span></strong>استفاده میشه ، این کلمات کلیدی رو در کپسوله سازی (پنهان سازی) بطور کامل توضیح می دم فقط فعلا در همین حد بدونید که این کلمات باید برای تعریف <span style="font-size: 16px;"><strong><span class="notespan">property </span></strong></span>ها و <span style="font-size: 16px;"><strong><span class="notespan">method </span></strong></span>ها قبل از اسم اونها قرار بگیرند:
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $name = 'john doe';
}

$obj = new MyClass;

var_dump($obj);
</code></pre>

<p>
در بالا با استفاده از کلمه public تعیین کردیم که property مون برای استفاده در یک object قابل مشاهدست و همینطور property به اسم name$ تعریف و بعد اون رو مقدار دهی کردیم و بعد با تعریف یک شی و قرار دادن اون در var_dump اطلاعات کامل رو برگشت دادیم .
</p>

<p>
شما به راحتی میتونید بعد از تعریف شی دوباره property رو مقداردهی کنید البته تنها در حالتی که اون property از نوع public باشه و همینطور به راحتی میتونید اون رو با استفاده از echo چاپ کنید . البته برای چاپ یا مقداردهی دوباره ، نیاز به دسترسی به اون property از طریق object دارید برای اینکار بعد از تایپ اسم object با قرار دادن یک فلش ( <- ) و تایپ اسم property میتونید به اون دسترسی داشته باشید . به مثال زیر دقت کنید:
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $name = 'John Doe';
}

$obj = new MyClass;

echo $obj->name . '</br >';

$obj->name = 'Hesam Mousavi';

echo $obj->name ;
</code></pre>


<h3>معرفی method ها</h3><p><strong><span class="notespan"><span style="font-size: 16px;">method</span></span></strong> ها دقیقا کار توابع رو در کلاس ها انجام میدن یعنی تفاوتی چندانی با هم ندارن <span class="notespan"><span style="font-size: 16px;"><strong>method </strong></span></span>ها هم با قرار گرفتن کلمه کلیدی&nbsp;<strong><span class="notespan"><span style="font-size: 16px;">private&nbsp;</span></span></strong>,&nbsp;<span style="font-size: 16px;"><span class="notespan"><strong>protected&nbsp;</strong></span></span>و&nbsp;<strong><span class="notespan"><span style="font-size: 16px;">public</span></span></strong> قبل از <span style="font-size: 16px;"><strong><span class="notespan">function </span></strong></span>تعریف میشن . یک <span style="font-size: 16px;"><span class="notespan"><strong>method</strong></span></span> میتونه به شی ها کمک کنه که در داخل کلاس ها عملیاتی رو انجام بدن البته این عملیات توسط متدها مشخص میشه .&nbsp;
</p>

<p>
برای مثال متدهایی برای set و get کردن اطلاعات property داخل کلاس می نویسیم . به کد زیر دقت کنید:
</p>


<pre><code class="language-php  line-numbers">class MyClass
{
  public $name = 'John Doe';

  public function setProperty($newval)
  {
     $this->name = $newval;
  }

  public function getProperty()
  {
     return $this->name . "</br >";
  }

}

$obj = new MyClass;

echo $obj->name;
</code></pre>

<p>
نکته : در کد بالا ما در دو جا از this$ استفاده کردیم و بعد با یک فلش و قرار دادن اسم property بهش دسترسی پیدا کردیم . در اصل این طریقه دسترسی به property ها و  method ها در داخل یک  method است . چون بطور معمولی شما نمی تونید با تایپ فقط اسم property یا method بهش دسترسی داشته باشید تنها زمانی که از this$ و با روش بالا عمل کنید میتونید به یک  property و  method از یک کلاس داخل یک method دسترسی پیدا کنید .
</p>

<p>
در کد بالا من فقط با قرار دادن obj->name$ اومدم مقدار این  property رو چاپ کردم اما در مثال زیر من ابتدا من با استفاده از متد getProperty میام مقدار فعلی name$ رو چاپ میکنم و بعد در مرحله بعدی با استفاده از متد setProperty و ارسال یک مقدار به عنوان آرگومان میام یک مقدار جدید برای name$ تعیین میکنم و بعد دوباره با چاپ کردن متد getProperty میام مقدار فعلیش رو چاپ می کنیم . این یک روش مهم برای set و get کردن  property هاست که به زودی در قسمت بعد دلیلش رو هم میفهمید ولی فعلا از دید امتحان کردن یک متد بهش نگاه کنید:
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $name = "John Doe";

  public function setProperty($newval)
  {
      $this->name = $newval;
  }

  public function getProperty()
  {
      return $this->name . "<br />";
  }
}

$obj = new MyClass;

echo $obj->getProperty(); // Get the property value

$obj->setProperty("Hesam Mousavi"); // Set a new one

echo $obj->getProperty(); // Read it out again to show the change
</code></pre>

<p>
نتیجه زیر حاصل از اجرای کد بالاست:
</p>


<pre><code class="language-php ">John Doe
Hesam Mousavi
</code></pre>

<h3>ثابت ها</h3>

<p>
یک ثابت چیزی شبیه به یک متغیر است، که می تواند یک مقدار را در خود نگاه دارد. یک بار که شما یک ثابت را تعریف کنید آن ثابت دیگر تغییر نخواهد کرد.
</p>


<pre><code class="language-php ">class MyClass {
    const requiredMargin = 1.7;

    function __construct($incomingValue) {
        // Statements here run every time
        // an instance of the class
        // is created.
    }
}
</code></pre>

<p>
در این کلاس، requiredMargin یک ثابت است. این ثابت با استفاده از کلمه کلیدی const اعلان شده است و تحت هیچ شرایطی مقدار آن بع غیر از 1.7 تغییر نخواهد کرد. توجه داشته باشید که نام ثابت پیشوند $ مشابه آنچه در مورد متغیرها به کار می رود ندارد.
</p>

<h3>
کلمه کلیدی static
</h3>

<p>
اعلان اعضا یا متدهای کلاس به صورت static آنها را بدون نیاز به نمونه سازی از کلاس در دسترس می کند. یک عضو اعلان شده به صورت static نمی تواند با یک شی کلاس نمونه سازی شده در دسترس باشد(البته از طریق یک متد استاتیک می تواند).
</p>


<pre><code class="language-php "><?php
class Foo {
    public static $my_static = 'foo';

    public function staticValue() {
        return self::$my_static;
    }
}

print Foo::$my_static . "\n";
$foo = new Foo();

print $foo->staticValue() . "\n";
</code></pre>