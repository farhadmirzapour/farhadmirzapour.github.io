---
layout: documentation
title:  نوشتن اولین آزمون
cattitle: اصول Test Driven Development
date:   2017-11-18 22:55:42 +0330
jdate: شنبه 27 آبان 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: https://jtreminio.com/2013/03/unit-testing-tutorial-introduction-to-phpunit/ <br> https://www.telerik.com/blogs/30-days-of-tdd-day-three-your-first-test <br> https://www.telerik.com/blogs/30-days-of-tdd-day-four-making-your-first-test-pass
---
<p>
  در این قسمت می خواهیم روند توسعه نرم افزار براساس TDD  را در قالب یک مثال بیان کنیم.
</p>

<blockquote>
<p>
می خواهیم قطعه کدی نوشته شود که با دریافت یک جمله تعداد کاراکتر مشخص شده ای را شمارش نماید.
</p>
</blockquote>

<p>

</p>

<h3>مفهوم Arrange, Act Assert</h3>

<p>
الگوها در توسعه نرم افزار بسیار مفید هستند. آنها یک راه برای برداشتن یک مشکل پیچیده دارند و آن را به چند مرحله (کوچکتر و  ساده تر) تبدیل می کنند. بهترین الگوها، الگوهایی هستند که به آنها عادت و احساس راحتی کنید، به طوری که «شما به دنبال الگوی دیگری نیستید». در مورد نوشتن آزمونهای واحد ،الگوی Arrange, Act, Assert الگویی استاندارد و منطقی می باشد.
</p>

<p>
AAA بیانگر سه مرحله برای ایجاد تست واحد می باشد:
</p>

<ol>

<li>
<p>
کلمه Arrange اشاره دارد به تنظیم تست شما با تعریف ورودی ها و خروجی های مورد انتظار .
</p>
<p>
برای مثال مفهوم Arrange  را می توان در این تست با تعریف یک جمله انتخابی، کاراکتری که به دنبال آن هستیم و نتیجه مورد انتظار بصورت زیر پیاده سازی کرد :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testShouldBeAbleToCountNumberOfLettersInSimpleSentence()
    {
        $sentenceToScan = "TDD is awesome!";
		$characterToScanFor = 'e';
		$expectedResult = 2;
    }
}
</code></pre>

</li>


<li>
<p>
گام بعد در الگوی AAA عملیلت Act  می باشد. در این مرحله کلاس و متدی که قرار است بر روی آن تست شود را فراخوانی و نتیجه آن را دریافت می کنیم.
</p>
<p>
برای مثال بالا نام کلاس را StringUtils انتخاب می کنیم همچنین به نظر نام findNumberOfOccurences نام توصیفی خوبی برای متدی که قرار است مورد آزمون قرار گیرد باشد. بنابراین در کد زیر در خط 12 نمونه ای از کلاس ایجاد و در خط 13 متد مورد نظر را فراخوانی و نتیجه آن را دریافت می کنیم (توجه کنید در عمل هنوز کلاس و متدهای مربوطه را ایجاد نکرده ایم):
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testShouldBeAbleToCountNumberOfLettersInSimpleSentence()
    {
        $sentenceToScan = "TDD is awesome!";
		$characterToScanFor = 'e';
		$expectedResult = 2;

		$stringUtils = new StringUtils();
		$result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);
    }
}
</code></pre>
</li>

<li>
<p>
 سرانجام به مرحله Assert می رسیم. در این مرحله ما تائید می کنیم که نتیجه دریافتی از متد مربوطه (result$) منطبق با مقدار مورد انتظار (expectedResult$) می باشد یا خیر.
</p>

<p>
 ما این کار را با استفاده از متدهای Assert انجام می دهیم. کلاس Assert یک کلاس استاتیک است که بخشی از چارچوب برنامه PHPUnit می باشد و مجموعه ای از روش های ارزیابی داده ها را فراهم می کند و سیگنال هایی را نشان می دهد که یک آزمون شکست خورده است و یا نتیجه غیرقابل حل بوده است.
</p>

<p>
برای این مثال از متد assertEquals استفاده می کنیم که تساوی دو مقدار را بررسی می کند. چنانچه دو مقدار برابر نباشند برنامه PHPUnit  به ما می گوید که تست شکست خورده است واجرای آزمون با علت شکست گزارش داده شده و متوقف می گردد، در غیر اینصورت اجرای آزمون ادامه پیدا می کند. این به شما این امکان را می دهد تا چند آزمون را در یک آزمون انجام دهید :
</p>
<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testShouldBeAbleToCountNumberOfLettersInSimpleSentence()
    {
        $sentenceToScan = "TDD is awesome!";
        $characterToScanFor = 'e';
        $expectedResult = 2;

        $stringUtils = new StringUtils();
        $result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);

        $this->assertEquals($expectedResult, $result);
    }
}
</code></pre>
</li>
</ol>

<br>
<h3>مرحله اجرای تست</h3>

<p>
تست آماده است، بنابراین اولین چیزی که می خواهیم انجام شود این است که سعی کنیم تستم را اجرا کنیم. این یک بخش کلیدی از گردش کار TDD است؛ یک تست را بنویسید و بلافاصله آن تست را اجرا کنید تا مشاهده کنیم تست شکست خورده است (و باید شکست بخورد).
<p>

<blockquote>
<p>
 حقیقت یک تست شکست است، به خصوص شکستی که برای عملکردی نوشته شده است که هنوز کد نویسی نوشته نشده ،می تواند به ما اطلاعات فراوانی دهد.
</p>
</blockquote>

<p>
اگر من یک تست را برای یک عملکردی که هنوز وجود ندارد (کدنویسی نشده است) بنویسم و آن تست شکست بخورد، به من چند چیز را نشان می دهد :
</p>
<ul>
<li>
اول از همه می تواند به من این تائید (نه به معنای تضمین) را برساند که من آزمون را درست نوشته ام. زیرا اگر تست بلافاصله pass شود، به من این نتیجه را می رساند که قطعه عملکرد صحیح مورد نظر را تست نکرده ام (قطعه کد دیگری به اشتباه تست شده)، به دلیل اینکه عملکردی هنوز وجود ندارد و آزمون نباید pass شود.
</li>

<li>
با مشاهده شکست بعد از اولین آزمون می توانم این اطمینان را حاصل کنم که کار من تکرار کار دیگری نیست. در بعضی موارد، به ویژه در پروژه های بزرگتر، ممکن است برای ایجاد قطعه ای از برنامه به اشتباه دوبار برنامه ریزی صورت گرفته باشد. در این مورد زمانیکه تست pass شود می تواند نشاندهنده این باشد که من دارم کاری که دیگری انجام داده را دوباره انجام می دهم.
</li>
</ul>

<p>
بسیار خب، تست مثال بالا را با اجرای دستور phpunit  در ترمینال اجرا می کنیم:
</p>

<div align="center">
<img src="/images/post/pass1.jpg" alt="{{page.title}}" />
</div>

<p>
همانطور که مشاهده می کنید دلیل شکست خوردن تست ما این است که کلاس StringUtils و متد findNumberOfOccurences یافت نشده است.
</p>

<p>بنابراین در این مرحله اقدام به ایجاد کلاس StringUtils و متد findNumberOfOccurences در پروژه خود می کنیم: </p>

<ol>
<li>
<p>
با فرض اینکه بخواهیم کلاس هایمان درون دایرکتوری به نام classes  قرار گیرند این دایرکتوری را ایجاد و جهت شناسایی آن توسط composer با استفاده از روش PSR-4 فایل composer.json را همانند زیر تغییر داده سپس در ترمینال دستور composer dumpautoload را وارد می کنیم :
</p>

<pre><code class="language-json   line-numbers">{
	"autoload" : {
		"psr-4": {
			"Classes\\" : "classes/"
		}
	}
}
</code></pre>
</li>

<li>
<p>به دلیل اینکه کلاس StringUtils را در فضای نام Classes قرار داده ایم بنابراین جهت استفاده در تست خود خط 11 را مانند زیر تغییر دهید :</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testShouldBeAbleToCountNumberOfLettersInSimpleSentence()
    {
        $sentenceToScan = "TDD is awesome!";
        $characterToScanFor = 'e';
        $expectedResult = 2;

        $stringUtils = new \Classes\StringUtils();
        $result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);

        $this->assertEquals($expectedResult, $result);
    }
}

</code></pre>
</li>

<li>
<p>جهت پیاده سازی کلاس StringUtils فایلی به نام stringUtils.php ایجاد و کدهای زیر را درون آن قرار دهید :   </p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class StringUtils
{
    public function __construct()
    {
    }

    public function findNumberOfOccurences($sentenceToScan, $characterToScanFor)
    {
        return 2;
    }
}
</code></pre>

</li>

<li>
دستور phpunit را در ترمینال وارد کرده و نتیجه زیر را مشاهده می کنیم :

<div align="center">
<img src="/images/post/pass2.jpg" alt="{{page.title}}" />
</div>


</li>
</ol>

<p>
همانطور که در کد بالا مشاهده می کنید ما ساده ترین پیاده سازی را جهت pass شدن آزمون انجام دادیم. بگذارید دوباره موضوع تست را بررسی کنیم:
</p>

<p>
<b>
می خواهیم جمله "!TDD is awesome" بررسی و نتیجه شمارش تعداد کاراکتر "e" که برابر دو می باشد را برگرداند.
</b>
</p>

<p>
بنابراین ساده ترین راه برای pass  شدن آزمون این بوده که عدد 2 برگشت داده شود
</p>


<p>
برای مثال بالا می توانیم بگوئیم مورد آزمون ما جهت صحت درستی برنامه ناکافی باشد. برای بهبود کیفیت کد، تست دیگری اضافه خواهیم کرد.
</p>
<p>
می خواهیم صحت درستی شمارش تعداد کاراکتر n  در جمله  "Once is unique, twice is a coincidence, three times is a pattern." که برابر 5 می باشد را بررسی کنیم. بنابراین تست دیگری اضافه می کنیم و خواهیم داشت :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testShouldBeAbleToCountNumberOfLettersInSimpleSentence()
    {
        $sentenceToScan = "TDD is awesome!";
        $characterToScanFor = 'e';
        $expectedResult = 2;

        $stringUtils = new \Classes\StringUtils();
        $result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);

        $this->assertEquals($expectedResult, $result);
    }

    public function testShouldBeAbleToCountNumberOfLettersInAComplexSentence()
    {
        $sentenceToScan = "Once is unique, twice is a coincidence, three times is a pattern.";
        $characterToScanFor = 'n';
        $expectedResult = 5;

        $stringUtils = new \Classes\StringUtils();
        $result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);

        $this->assertEquals($expectedResult, $result);
    }
}
</code></pre>

<p>
بعد از اجرای تست نتیجه زیر مشاهده می گردد :

<div align="center">
<img src="/images/post/pass3.jpg" alt="{{page.title}}" />
</div>

</p>

<p>
همانطور که در توضیحات دلیل شکست مشاهده می کنید، نتیجه مورد انتظار عدد 5 بوده درحالیکه عدد 2 برگردانده شده است.
</p>

<p>
بنابراین ساده ترین کار برای pass  شدن آزمون این است که الگوریتمی برای شمارش تعداد کاراکتر یک جمله نوشته شود. بنابراین متد findNumberOfOccurences بصورت زیر تغییر می دهیم :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class StringUtils
{
    public function __construct()
    {
    }

    public function findNumberOfOccurences($sentenceToScan, $characterToScanFor)
    {
        $numberOfOccurenes = 0;
        for ($charIdx= 0; $charIdx < strlen($sentenceToScan); $charIdx++) {
            if ($sentenceToScan[$charIdx] == $characterToScanFor) {
                $numberOfOccurenes++;
            }
        }
        return $numberOfOccurenes;
    }
}
</code></pre>

<p>
این کد ممکن است بهینه ترین یا حتی بهترین راه برای حل این مشکل نباشد. اما در حال حاضر مهم نیست؛ این ساده ترین روش ممکنه است و ما به دنبال ساده ترین الگوریتم هایی هستیم که آزمون ها را انجام دهد. اگر دوباره تست های خود را اجرا کنیم می بینیم که این الگوریتم این نیاز را برآورده می کند:
</p>

<div align="center">
<img src="/images/post/pass4.jpg" alt="{{page.title}}" />
</div>

<p>
در ادامه مباحث پیشرفته تر TDD، با عملیات Refactoring این مثال را بازبینی خواهیم کرد.
</p>