---
layout: documentation
title:   ابزار PHP Unit
cattitle: اصول Test Driven Development
date:   2017-11-17 10:08:42 +0330
jdate: جمعه 26 آبان 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: https://jtreminio.com/2013/03/unit-testing-tutorial-introduction-to-phpunit/
---
<p>
 با استغاده از برنامه PHPUnit امکانات لازم جهت انجام مراحل تست واحد برای برنامه های PHP فراهم می گردد.
</p>

<h3>نصب PHPUnit</h3>

<p>
جهت نصب PHP unit  می توان از برنامه composer استفاده نمائیم . بنابراین دستور زیر را در ترمینال وارد نمائید تا مراحل نصب تکمیل گردد :
</p>

<pre><code class="language-bash   line-numbers">$ composer global require phpunit/phpunit
</code></pre>

<p>
برنامه بصورت global  نصب گرددید به این معنا که در تمامی پروژه ها و دایرکتوری ها می توان از این برنامه استفاده کرد. جهت اطمینان از صحت نصب، دستور زیر را در ترمینال وارد تا ورژن نصب شده نمایش داده شود :
</p>

<pre><code class="language-bash   line-numbers">$ phpunit --v
</code></pre>

<p>
بسیار خب... می خواهیم از این قسمت به بعد مراحل تنظیمات و نوشتن تست را بصورت یک پروژه بیان کنیم. بنابراین دایرکتوری به نام myproject ایجاد و درون آن دایرکتوری دیگری به نام test  ایجادمی کنیم.
</p>
<p>
همچنین چون ما از روش PSR-4  جهت لودینگ کلاس ها استفاده می کنیم بنابراین فایلی به نام composer.json  را در مسیر اصلی پروژه ایجاد و کد زیر را درون آن قرار دهید :
</p>

<pre><code class="language-bash   line-numbers">{

}
</code></pre>
سپس درون ترمینال دستور زیر را وارد تا پوشه vendor  ایجاد گردد :

<pre><code class="language-bash   line-numbers">composer dump
</code></pre>

<br>
<h3>تنظیمات PHP Unit  در پروژه</h3>
<p>
جهت تنظیمات مورد نظرمان  از فایل phpunit.xml در شاخه اصلی پروژه استفاده می نمائیم. همانند کد زیر :
</p>

<pre ><code class="language-xml">&lt;?xml version="1.0" encoding="UTF-8"?<script type="prism-html-markup">>
<phpunit colors="true" bootstrap="vendor/autoload.php">
    <testsuites>
        <testsuite name="Application Test Suite">
            <directory>test/</directory>
        </testsuite>
    </testsuites>
</phpunit>
</script></code></pre>

<p>
در کد بالا دو تنظیم انجام داده ایم که عبارتند از :
<ul>
<li>
با استفاده از ویژگی colors="true" تعیین کردیم که نتایج آزمایش بصورت رنگی در ترمینال نمایش داده شود
</li>

<li>
با استفاده از تگ directory تعیین کردیم که مسیر فایل های تست ما درون پوشه test  در شاخه اصلی قرار دارند.
</li>
</ul>
</p>


<h3>ساختار و نام فایل تست در PHP Unit</h3>

<ul>
<li>
<p>
تمامی فایل های تست حتما باید با کلمه <b>Test</b>  به پایان برسد. همچنین جهت آشکار سازی فایل تست که متعلق به کدام قسمت از برنامه است بهتر است فایل ها و مسیرهای همنام در دایرکتوری تست ایجاد گردند. برای مثال چنانچه در پروژه داشته باشیم :
</p>


<blockquote>
<p align="left">
foo.php
<br>
bar.php
<br>
Controller/baz.php
</p>
</blockquote>

<p>
نام و مسیر فایل های تست ما بدینگونه خواهد بود :
</p>

<blockquote>
<p align="left">
fooTest.php
<br>
barTest.php
<br>
Controller/bazTest.php
</p>
</blockquote>
</li>

<li>
نام کلاس درون فایل تست بهتر است همنام با نام فایل باشد.
</li>

<li>
نام آزمون حتما باید با کلمه test بصورت حروف کوچک آغاز گردد.
</li>

<li>
<p>
اسامی آزمون ها باید توصیفی از عملی باشد که مورد آزمایش قرار می گیرد و بهتر است بصورت اختصار و کوتاه نباشد تا بیانگر آزمایشی باشد که انجام می دهد.
</p>
<p>
 برای مثال چنانچه تابعی به نام ()verifyAccount داشته باشیم که عمل مطابقت رمز عبور را برای یک حساب کاربری انجام می دهد آنگاه می توانیم نام آزمون را اینطور انتخاب کنیم :
</p>

<blockquote>
<p align="left" style="direction:left">
()testVerifyAccountMatchesPasswordGiven
</p>
</blockquote>

</li>

<li>
سطح دسترسی متدهای آزمون باید از نوع public  باشند.
</li>

<li>
کلاس آزمون باید گسترش یافته از PHPUnit_Framework_TestCase و یا هرکلاس دیگری که این وظیفه را برعهده دارد باشد.
</li>
</ul>

<p>
در کد زیر نمونه ای از یک آزمون را مشاهده می کنید :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class StringUtils extends \PHPUnit_Framework_TestCase
{
    public function testVerifyAccountMatchesPasswordGiven()
    {

    }
}
</code></pre>

<p>
در کد بالا  آزمونی ایجاد کرده ایم که با توجه به نام آن به راحتی می توان فهمید که قرار است آزمونی درخصوص بررسی صحت درستی عملکرد متدی که تعداد حروف خاصی را در یک رشته شمارش می نماید را انجام دهد.
</p>
