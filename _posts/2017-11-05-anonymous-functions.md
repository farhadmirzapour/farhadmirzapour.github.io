---
layout: post
title:   مفهوم توابع بی‌نام (anonymous function)
date:   2017-11-05 19:35:42 +0330
jdate: یکشنبه 14 آبان 1396
categories: articles
#  permalink: /:categories/:title.html
thumbnail: anonymous-php.png
refrence: http://aparnet.ir/2917-anonymous-functions
---
<p>
نیمه دوم سال 2009 بود که PHP با ورژن 5.3 با ویژگی‌های زیاد جدیدی که برای برنامه نویسان جذاب بود، انتشار یافت. یکی از ویژگی‌هایی که به PHP در این نسخه اضافه شد، تابع بی‌نام بود.
</p>


<h3 >تابع بی‌نام (anonymous function)</h3>

<p>
توابع ناشناخته که اصطلاحا به آن‌ها تابع روی هوا (on the fly) گفته می شود، به طور ساده تابعی است بدون‌نام. مانند مثال زیر:
</p>


<pre><code class="language-php  line-numbers"><?php
// Anonymous function

function () {
  return "Hello world";
}
</code></pre>


<p>
این مفهوم در PHP تحت قالب Lambda  وClosures پیاده سازی شده است. در ادامه مطلب بیشتر به این مورد خواهیم پرداخت.
</p>

<h3>توابع Lambada</h3>

<p>
یک Lambada تابع ناشناخته‌ای است که به یک متغیر انتساب داده می شود و یا به عنوان پارامتر به تابع دیگر انتساب داده می‌شود.
</p>


<h3>استفاده از Lambada</h3>

<p>
از آن جا که این مدل از تابع‌ها نام ندارند، پس شما نمی‌توانید مثل تابع‌های معمولی آن‌ها را صدا بزنید.
بلکه باید همینطور که قبلا به آن اشاره شد، تابع را به یک متغیر انتساب بدهید و یا به عنوان پارامتر به تابع دیگری ارسال کنید.
</p>

<pre><code class="language-php  line-numbers"><?php
// Anonymous function

// assigned to variable
$greeting = function ($world = '') {
  return "Hello". $world;
};

// Call function
echo $greeting('world');

// Returns "Hello world"
</code></pre>

<p>
نکته: همینطور که در کد بالا هم مشاهده می کنید، سمی کالن (;) را بعد از اتمام تابع قرار می‌دهیم.
</p>

<p>
نکته: همینطور که در کد بالا هم مشاهده می کنید، می‌توانیم پارامتر به صورت دلخواه به تابع بدهیم (world$).
</p>

<p>
به منظور استفاده از تابع ناشناخته، ما این تابع را به یک متغیر انتساب میدهیم و سپس متغیر را به عنوان یک تابع، Call یا صدا میزنیم.
</p>

<p>
شما همچنین می‌توانید Lambadaایی که تعریف کرده‌اید را به عنوان پارامتر به تابع دیگر بفرستید:
</p>

<pre><code class="language-php  line-numbers"><?php
// Pass Lambda to function

function shout ($message) {
  echo $message();
}


// Call function
shout(function() {
  return "Hello world";
});
</code></pre>


<h3>برای چی من باید از Lambada استفاده کنم؟</h3>
<p>شاید الان با خودتان گفتید که این تابع بدون اسم چه زمانی کارایی دارد؟</p>
<p>Lambada از تعریف تابع هایی که معمولا کد کوچک و مختصری دارند و ممکن است شما در طول اجرای برنامه، مثلا، فقط یکبار صدا بزنید، خودداری می کند. اغلب شما، به تابعی نیاز دارید که کاری را برای شما انجام دهد، اما این نیاز را پیدا نمی کنید که آن را داخل global scope برنامه یا حتی به عنوان بخشی از بقیه کدها اضافه کنید.</p>
<p>به جای اینکه تابعی داشته باشید که یک بار استفاده و بعد هم رها شود، میتوانید از Lambada استفاده کنید.</p>
<p>یک مورد پر استفاده دیگر توابع ناشناخته درfunction callbackها است.</p>
<p>اگر مفهوم توابع  callback در PHP را نمی‌دانید، یک جستجو راجع به آن انجام دهید، چرا که شرح کامل آن در این بحث نمی گنجد. اما یک نگاه سطحی با یک مثال به آن می‌اندازیم تا بحث بیشتر برایتان روشن شود.</p>
<p>Callback هر تابعی که صدا زده شده است توسط تابع دیگری که البته آن تابع دیگر، تابع اول را به عنوان پارامتر میگیرد و Call می‌کند. معمولا callback تابعی است که زمانی صدا زده می‌شود که اتفاقی رخ داده باشد، که در دنیای برنامه نویسی به آن اتفاق، رویداد یا event گفته میشود.</p>
<p>مثال زیر یک مثال پایه‌ایی از تابع‌های callback است:</p>
<p>شما می‌توانید تابعی که نامش در یک متغیر ذخیره شده است را call کنید. فقط کافیست به انتهای نام متغیر، پرانتز باز و بسته () اضافه کنید. مثل <span class="en-words">()variable$</span></p>

<pre><code class="language-php  line-numbers"><?php
function thisFuncTakesACallback($callbackFunc)
{

  echo "I'm going to call $callbackFunc!<br />";

  $callbackFunc();

}

function thisFuncGetsCalled()
{
  echo "I'm a callback function!<br />";
}

thisFuncTakesACallback( 'thisFuncGetsCalled' );
</code></pre>

<p>خب در مثال بالا، ما نام تابع <span class="en-words">thisFuncGetsCalled</span> را به تابع <span class="en-words">()thisFuncTakesACallback</span> ارسال کردیم که سپس این تابع، تابعی که به آن فرستاده شده بود رو صدا زد.</p>
<p>از توابع ناشناخته در توابع بومی PHP استفاده شده، که مثال‌های فوق العاده‌ای در این زمینه هستند، از جمله <span class="en-words">array_walk</span>, <span class="en-words">array_map</span>, <span class="en-words">array_reduce</span> و <span class="en-words">array_filter usort</span>.</p>
<p>در مثال زیر نمونه‌ایی از callback function را مشاهده می‌کنید که با Lambada و در تابع <span class="en-words">array_filter</span> پیاده سازی شده است.</p>
<p>نکته: کار <span class="en-words">array_filter</span> به این صورت است که پارامتر اول یک آرایه مثلا <span class="en-words">input$</span> را می‌گیرد و در یک Loop قرار می‌دهد و هر دفعه مقدار فعلی آرایه را به function callback ارسال یا pass میکند. اگرfunction callback مقدار <span class="en-words">true</span> برگرداند، value جاری خانه مورد نظر از آرایه برگردانده می‌شود. در ضمن keyهای آرایه هم تغییری نمی‌کنند.</p>

<pre><code class="language-php  line-numbers"><?php
$input = array(1, 2, 3, 4, 5);

$output = array_filter($input, function ($v) { return $v > 2; });

// $output == array(2 => 3, 3 => 4, 4 => 5)
</code></pre>
<p>در مثال بالا، آرایه ورودی فیلتر می‌شود؛ به طوریکه تمام اعداد بزرگتر از 2 استخراج می‌شوند.</p>
<p>در مثال بالا، عبارت <span class="en-words">{ ;function ($v) { return $v &gt; 2</span> یک Lambada است، که اگر میخواهید قابل استفاده مجدد باشد باید آن را داخل یک متغیر ذخیره کنید.</p>
<p>اطلاعات بیشتر:  پیشتر می‌توانستید از <span class="en-words">create_function</span> در PHP استفاده کنید که اساسا همین کار را انجام میدهد.</p>

<pre><code class="language-php  line-numbers"><?php
// Use create_function

$greeting = create_function('', 'echo "Hello World!";');



// Call function
$greeting();
</code></pre>

<p></p>
<h3>توابع Closure</h3>
<p>یک Closure (کلوژر) اساسا همان Lambada است، علاوه بر اینکه، می‌توانید به متغیرهای خارج از محدوده‌ایی که تعریف شده هم، دسترسی داشته باشد.</p>

<pre><code class="language-php  line-numbers"><?php
// Create a user

$user = "Philip";

// Create a Closure

$greeting = function() use ($user) {

  echo "Hello $user";

};

// Greet the user
$greeting(); // Returns "Hello Philip"
</code></pre>
<p>همان طور که در بالا می بینید Closure می‌تواند به متغیر <span class="en-words">user$</span> دسترسی داشته باشد، به این دلیل که در عبارت <span class="en-words">use</span>، تعریف تابع Closure آورده شده است.</p>
<p>همچنین اگر می‌خواهید مقدار متغیر <span class="en-words">user$</span> را داخل Closure تغییر بدهید و بیرون آن هم این تغییر از بین نرود، باید حتما قبل از متغیر داخل عبارت <span class="en-words">use</span> یک <span class="en-words">&amp;</span> (امپرسند) قرار بدهید تا متغیر به صورت ارسال با مرجع فرستاده شود.</p>
<p>بیایید مثال array_filter را نیز با Closure انجام بدهیم.</p>

<pre><code class="language-php  line-numbers"><?php
$max_comparator = function ($max)
{
  return function ($v) use ($max) { return $v > $max; };
};

$input = array(1, 2, 3, 4, 5);

$output = array_filter($input, $max_comparator(3)); // Array ( [3] => 4 [4] => 5 )
</code></pre>
<p>در مثال بالا دو نکته را دیدید که یکی استفاده از توابع ناشناخته بصورت تودرتو و دیگری اینکه max$ به عنوان یک متغیر خارج از محدوده closure داخلی، مورد استفاده قرار گرفت.</p>
<p>مثالی از Closure در array_walk:</p>

<pre><code class="language-php  line-numbers"><?php
// Set a multiplier

$multiplier = 3;

// Create a list of numbers
$numbers = array(1,2,3,4);

// Use array_walk to iterate

// through the list and multiply

array_walk($numbers, function($number) use($multiplier) {

  echo $number * $multiplier;

});
</code></pre>
<p>در مثال بالا تقریبا این نیاز احساس نمی‌شود که یک تابع بسازید که 2 عدد را در هم ضرب کند در حالی که فقط در یک جا مورد استفاده قرار میگیرد. با استفاده از یک Closure به عنوان callback، ما می‌توانیم تابع را یکبار استفاده کنیم و دیگر آن را فراموش کنیم.</p>
<h3>استفاده از this$ در توابع ناشناخته</h3>
<p>در نیمه اول سال 2012، یعنی زمانی که PHP به نسخه 5.4 خودش رسید، قابلیت جدیدی به توابع ناشناخته اضافه شد. بوسیله این قابلیت قادر هستید به راحتی با استفاده از this$ داخل تابع ناشناخته، به یک نمونه Object دسترسی داشته باشید.</p>

<pre><code class="language-php  line-numbers"><?php
class Foo
{

  function hello() {
    echo 'Hello Nettuts!';
  }

  function anonymous()
  {
    return function() {
      $this->hello(); // $this wasn't possible before
    };
  }

}

class Bar
{

  function __construct(Foo $Foo) // object of class Foo typehint
  {
    $x = $Foo->anonymous(); // get Foo::hello()

    $x(); // execute Foo::hello()
  }

}

new Bar(new Foo); // Hello Nettuts!
</code></pre>
<p></p>
<h3>استفاده در دنیای واقعی</h3>
<p>یک مثال معروف از استفاده از این نوع توابع در routing درخواست ها در فریم ورک های مدرن است.</p>
<p>به عنوان مثال Laravel، به شما اجازه میده که مثل زیر عمل کنید:</p>

<pre><code class="language-php  line-numbers"><?php
Route::get('user/(:any)', function($name) {
  return "Hello " . $name;
});
</code></pre>
<p>کد بالا به سادگی با URLایی مثل <span class="en-words">user/Philip/</span>، تطبیق داده می‌شود.</p>
<p>این یک مثال خیلی ساده بود، اما روشن میکند که چطور یک Closure در شرایطی که به آن نیاز هست می‌تواند مورد استفاده واقع شود.</p>
<h3>در دیگر زبان ها چطور؟</h3>
<p>توابع ناشناخته در خیلی از زبانهای برنامه نویسی وجود دارند (از جمله پایتون، دلفی، جاوا، C و …) و اگر Ruby و Javascript کار کرده باشید حتما به آن‌ها برخوردید، چون در این زبان‌ها بسیار متداول هستند.</p>
<p>البته این نکته نیز ناگفته نماند که کاربرد آن در PHP، دقیقا همان کاربردی نیست که بقیه زبان ها از آن دارند.</p>
<h3>نمونه‌ایی از تابع ناشناخته در jQuery:</h3>

<div align="center">
<img src="/images/post/jquery-callback.jpg" alt="{{page.title}}" />
</div>