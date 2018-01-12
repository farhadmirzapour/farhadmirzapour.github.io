---
layout: documentation
title:   مفهوم Defects (نقص ها)
cattitle: اصول Test Driven Development
date:   2017-11-19 16:27:42 +0330
jdate: یکشنبه 28 آبان 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: http://blogs.telerik.com/james-bender/posts/13-09-25/30-days-of-tdd-day-eight-dealing-with-defects
---
<p>
 نقص ها (defects)نوع دیگری از نیازها (requerment) می باشند.
</p>

<blockquote>
<p>
یک نیاز توصیف می کند که نرم افزار شما چگونه باید کار کند، در حالی که یک نقص توصیف می کند که نرم افزار شما چگونه کار کرده است.
</p>
<p>A requirement describes how your software should work whereas a defect describes how your software should have worked</p>
</blockquote>



<p>
هنگامی که از TDD استفاده می کنید و نرم افزار خود را براساس  تست هایی که خود بر اساس نیازها ساخته شده اند ایجاد می کنید، باید به تمامی آن نیازها در کد خود مراجعه کنید. اگر آن نیازها درست طراحی شده باشند،بنابراین وجود نقص به معنای وجود چیزی بیشتر از یک نیاز جدید نیست که یا شما از وجود این نیازها آگاه نبودید و یا نیازی بابت اصلاح نیازهای موجود می باشند.
</p>

<p>
برای تشریح مفهوم نواقص به مثال بخش اولین آزمون خود برمی گردیم :
</p>

<p>دو آزمون ایجاد کرده بودیم که عبارت از :</p>
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

<p>همچنین برنامه ای بر پایه الگوریتمی نوشتیم که با گرفتن یک جمله و کاراکتر، تعداد کاراکتر موجود در آن جمله را بر می گرداند :</p>
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
بعد از اینکه این کد برای مدت کوتاهی اجرایی شد، نقصی از سوی یکی از کاربران گزارش شد:
</p>

<p>کاربر به جای ارسال یک کاراکتر، عدد 0 را ارسال کرده بود که نتیجه 65 بر گردانده شد</p>

<p>
صرف نظر از اینکه چه نوع نیاز جدیدی داریم، گردش کار یکسان است، ابتدا یک تست بنویسید. در این مورد، من می خواهم یک تست بنویسیم که نشان دهد که رفتار فعلی که توسط نقص توصیف شده است، در حقیقت یک خطا است :
</p>
<pre><code class="language-php   line-numbers">public function testShouldShowZeroWhenZeroSend()
{
    $sentenceToScan = "This test should return 0";
    $characterToScanFor = 0;
    $expectedResult = 0;

    $stringUtils = new \Classes\StringUtils();
    $result = $stringUtils->findNumberOfOccurences($sentenceToScan, $characterToScanFor);

    $this->assertEquals($expectedResult, $result);
}
</code></pre>

<p>همچنین جهت pass شدن این آزمون خط 12 را به کلاس StringUtils اضافه می کنیم :  </p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class StringUtils
{
    public function __construct()
    {
    }

    public function findNumberOfOccurences($sentenceToScan, $characterToScanFor)
    {
        return 0;
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
در حالی که ما این نقص را در تست درست کرده ایم، اما باعث شکسته شدن آزمون های قبل شدیم. اما باید توجه داشت بدون تست واحد فعلی من هیچ راهی آسان برای اثبات اینکه عملکرد فعلی  با تغییر من انجام شده ، وجود ندارد.
</p>