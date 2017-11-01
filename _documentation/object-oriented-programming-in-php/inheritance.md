---
layout: documentation
title:  Inheritance (وراثت)
cattitle: آموزش شی گرایی در php
date:   2017-11-01 20:57:42 +0330
jdate: چهارشنبه 10 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
refrence: https://roocket.ir/articles/object-oriented-programming-in-php-part-4
---
<div align="center">
<img src="/images/original/php-oop-course.png" alt="{{page.title}}" />
</div>
<h3>وراثت (Inheritance)</h3>
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