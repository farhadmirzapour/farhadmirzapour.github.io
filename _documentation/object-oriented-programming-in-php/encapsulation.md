---
layout: documentation
title:  Encapsulation (کپسوله سازی)
cattitle: آموزش شی گرایی در php
date:   2017-11-01 20:56:42 +0330
jdate: چهارشنبه 10 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
thumbnail: phpOOPTutorialPAGE.png
refrence: https://roocket.ir/articles/object-oriented-programming-in-php-part-3
---
<h3>کپسوله سازی (Encapsulation)</h3>
<p>
کپسوله سازی همون پنهان سازی اطلاعاته اما ما چرا باید اطلاعاتی رو پنهان سازی کنیم . در جلسه قبل اگه یادتون باشه من دوتا method درست کردم به اسم های set و get که هر کدوم کار خودشون رو انجام می دادن یعنی یکی مقداردهی property مون رو انجام میداد و یکی مقدار property رو برامون بر میگردوند اما چرا باید اینطوری باشه . این سوالیه که منم داشتم چون ما به راحتی میتونیم از خود property استفاده کنیم و مقداردهی و چاپش کنیم اما این درست نیست . گاهی property ها و method های حساسی وجود داره که قابل استفاده در object ها نیستن ! چرا نیستن ؟ چون پنهان سازی شدن . اگه یادتون باشه در جلسه قبلی سه کلمه کلیدی public , private و protected رو معرفی کردم اما فقط از public استفاده کردم و گفتم تو این جلسه میگم اینا به چه کاری میان بزارین با تعریف کردن هر کدوم اینا به نتیجه برسیم .
</p>

<p>
protected  : اگر property یا method ای قبلش از این کلمه استفاده بشه به این معنیه که شما از اون property و method  فقط در کلاس ها میتونید استفاده کنید و اصلا نمی تونید در object ای که میسازید مورد استفاده قرارش بدید . [ البته با روش های خاص میشه ]
</p>

<p>
private : اگر property یا method ای قبلش از این کلمه استفاده بشه به این معنیه که شما از اون property و method فقط و فقط میتونید در داخل همون کلاس استفاده کنید و پس یعنی قابلیت استفاده در object رو هم ندارید . private شبیه protected  اما استفاده نشدن در کلاس های دیگه بین اونا فرق میزاره .
</p>

<p>
و در نهایت public : اگر property یا method ای قبلش از این کلمه استفاده بشه به این معنیه که شما از اون property و method به راحتی می تونید در کلاس ها و object ها استفاده کنید . به همین سادگی .
</p>

<p>
خب حالا شما میگین اینا فقط تعریف بودن اما هنوز کپسوله سازی رو دقیقا نفهمیدم که چی هست . شما گاهی میخواین اطلاعاتی رو به نسبت حساسیتش از object یا کلاس های دیگه مخفی کنید . برای همین به نسبت کاری که قراره انجام بدید در از private یا protected استفاده میکنید تا دیگه در object ها قابلیت استفاده نداشته باشن .
</p>

<p>
خب حالا فکر کنم باید متوجه شده باشید چرا از method های set و get استفاده کردیم ولی هر موضوعی با مثال واضح تر میشه پس به مثال های زیر دقت کنید تا بیشتر براتون این موضوع جا بیوفته .
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

<p>
این همون مثال جلسه قبلیه در این مثال property ما از نوع public برای همین با ساخت object به راحتی می تونید از خود object هم عمل مقدار دهی دوباره و هم مقدار فعلیش رو برگشت بدید . حالا به مثال زیر هم دقت کنید .
</p>


<pre><code class="language-php  line-numbers">class MyClass
{
  protected $name = 'John Doe';
}

$obj = new MyClass;

echo $obj->name;
</code></pre>

<p>
در بالا property ما از نوع protected برای همین در object نه میتونید مقدار دهی کنید و نه میتونید مقدار فعلی رو بر گردونید در واقع اگه کد بالا رو اجرا کنید بهتون ارور میده .
</p>
<p>
اما در مثال زیر با استفاده از متدهای get و set به راحتی یک property ای که از نوع protected باشه رو مقدار دهی یا مقدار فعلی رو برگشت میدیم .
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  protected $name = "John Doe";

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
حالا میبینید که در کد بالا به راحتی با استفاده از method ها تونیستم عمل مقدار دهی و همینطور برگشت مقدار فعلی یک property از نوع protected رو انجام بدیم . برای کلمه private هم همین وضعیت بالا (protected) برقراره اما یک ویژگی دیگه ای که private داره اینکه در کلاس های دیگه قابل استفاده نیست .
</p>