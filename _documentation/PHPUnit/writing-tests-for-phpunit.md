---
layout: documentation
title:    تست نویسی در PHPUnit
cattitle: آموزش PHPUnit
date:   2017-12-03 10:57:42 +0330
jdate: یکشنبه 12 آذر 1396
caturl: 2017/11/27/php-unit.html
# permalink: /:categories/:title.html
thumbnail: phpunit-compressor.jpg
refrence: https://phpunit.de/manual/current/en/writing-tests-for-phpunit.html#writing-tests-for-phpunit.exceptions
---
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
کلاس آزمون باید گسترش یافته از PHPUnit\Framework\TestCase و یا هرکلاس دیگری که این وظیفه را برعهده دارد باشد.
</li>
</ul>

<p>
در کد زیر نمونه ای از یک آزمون را مشاهده می کنید :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class StackTest extends TestCase
{
    public function testVerifyAccountMatchesPasswordGiven()
    {

    }
}
</code></pre>

<p>
در کد بالا  آزمونی ایجاد کرده ایم که با توجه به نام آن به راحتی می توان فهمید که قرار است آزمونی درخصوص بررسی صحت درستی عملکرد متدی که تعداد حروف خاصی را در یک رشته شمارش می نماید را انجام دهد.
</p>

<br>
<strong>
مثال 1: تست عملیات آرایه با PHPUnit
 </strong>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class StackTest extends TestCase
{
    public function testPushAndPop()
    {
        $stack = [];
        $this->assertEquals(0, count($stack));

        array_push($stack, 'foo');
        $this->assertEquals('foo', $stack[count($stack)-1]);
        $this->assertEquals(1, count($stack));

        $this->assertEquals('foo', array_pop($stack));
        $this->assertEquals(0, count($stack));
    }
}
</code></pre>

<br>
<h3>نوشتن Test Dependencies (آزمون های وابسته به یکدیگر)</h3>

<p>
آزمایشات واحد عمدتا به عنوان یک تمرین خوب برای کمک به توسعه دهندگان جهت شناسایی و رفع اشکالات، اصلاح کد و به عنوان مستند سازی برای یک واحد نرم افزار تحت آزمون نوشته می شود. برای دستیابی به این مزایا، تست واحد به طور ایده آل باید تمام مسیرهای ممکن در یک برنامه را پوشش دهد. اغلب وابستگی های ضمنی بین روش های تست وجود دارد، که در سناریوی اجرای آزمون مخفی شده است.
</p>

<p>
مثال 2.2 نشان می دهد که چگونه از استفاده از متن @depends تعریف می کند تا وابستگی بین روش های تست را بیان کند.
</p>

<br>
<strong>
مثال 2 : استفاده از <span class="redbold" >depends@</span> در حاشیه نویسی جهت بیان وابستگی ها
</strong>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    public function testEmpty()
    {
        $stack = [];
        $this->assertEmpty($stack);

        return $stack;
    }

    /**
     * @depends testEmpty
     */
    public function testPush(array $stack)
    {
        array_push($stack, 'foo');
        $this->assertEquals('foo', $stack[count($stack)-1]);
        $this->assertNotEmpty($stack);

        return $stack;
    }

    /**
     * @depends testPush
     */
    public function testPop(array $stack)
    {
        $this->assertEquals('foo', array_pop($stack));
        $this->assertEmpty($stack);
    }
}
</code></pre>

<p>
در مثال بالا، اولین تست، ()testEmpty یک آرایه جدید ایجاد می کند و با استفاده از متد assertEmpty اظهار می کند که آن خالی است و سپس به عنوان نتیجه آن را برگرداند. تست دوم، ()testPush به ()testEmpty بستگی دارد و نتیجه آن آزمون وابسته را به عنوان آرگومان دریافت می کند. همچنین در نهایت ()testPop به ()testPush وابستگی دارد.
</p>

<br>
<strong>
مثال 3: بهره گیری از وابستگی بین آزمایش ها
 </strong>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class DependencyFailureTest extends TestCase
{
    public function testOne()
    {
        $this->assertTrue(false);
    }

    /**
     * @depends testOne
     */
    public function testTwo()
    {
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit --verbose DependencyFailureTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

Time: 0 seconds, Memory: 5.00Mb

There was 1 failure:

1) DependencyFailureTest::testOne
Failed asserting that false is true.

/home/sb/DependencyFailureTest.php:6
There was 1 skipped test:

1) DependencyFailureTest::testTwo
This test depends on "DependencyFailureTest::testOne" to pass.


FAILURES!
Tests: 1, Assertions: 1, Failures: 1, Skipped: 1.
</code></pre>


<br>
<strong>
مثال 4: تست با چندین وابستگی
 </strong>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    public function testProducerFirst()
    {
        $this->assertTrue(true);
        return 'first';
    }

    public function testProducerSecond()
    {
        $this->assertTrue(true);
        return 'second';
    }

    /**
     * @depends testProducerFirst
     * @depends testProducerSecond
     */
    public function testConsumer()
    {
        $this->assertEquals(
            ['first', 'second'],
            func_get_args()
        );
    }
}
</code></pre>
<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit --verbose MultipleDependenciesTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

...

Time: 0 seconds, Memory: 3.25Mb

OK (3 tests, 3 assertions)
</code></pre>

<br>
<h3>
مفهوم Data Providers
</h3>
<p>
یک روش آزمون می تواند آرگومان های دلخواه را قبول کند. این آرگومان ها توسط dataProvider با استفاده از حاشیه نویسی <span class="redbold">dataProvider@</span> مشخص می شود.
</p>

<br>
<strong>
مثال 5: با استفاده از یک dataProvider که آرایه ای از آرایه ها را باز می گرداند
</strong>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    /**
     * @dataProvider additionProvider
     */
    public function testAdd($a, $b, $expected)
    {
        $this->assertEquals($expected, $a + $b);
    }

    public function additionProvider()
    {
        return [
            [0, 0, 0],
            [0, 1, 1],
            [1, 0, 1],
            [1, 1, 2]
        ];
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit DataTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

...F

Time: 0 seconds, Memory: 5.75Mb

There was 1 failure:

1) DataTest::testAdd with data set #3 (1, 1, 3)
Failed asserting that 2 matches expected 3.

/home/sb/DataTest.php:9

FAILURES!
Tests: 4, Assertions: 4, Failures: 1.
</code></pre>


<p>
هنگام استفاده از تعداد زیادی از مجموعه داده ها مفید است که هر یک را با کلید رشته به جای پیش فرض عددی نامگذاری کنید.
</p>

<br>
<strong>
مثال 6: استفاده از dataProvider با مجموعه داده های نامگذاری شده
</strong>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    /**
     * @dataProvider additionProvider
     */
    public function testAdd($a, $b, $expected)
    {
        $this->assertEquals($expected, $a + $b);
    }

    public function additionProvider()
    {
        return [
            'adding zeros'  => [0, 0, 0],
            'zero plus one' => [0, 1, 1],
            'one plus zero' => [1, 0, 1],
            'one plus one'  => [1, 1, 3]
        ];
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit DataTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

...F

Time: 0 seconds, Memory: 5.75Mb

There was 1 failure:

1) DataTest::testAdd with data set "one plus one" (1, 1, 3)
Failed asserting that 2 matches expected 3.

/home/sb/DataTest.php:9

FAILURES!
Tests: 4, Assertions: 4, Failures: 1.
</code></pre>

<br>
<strong>
مثال 7: ترکیب depends وdataProvider در تست
</strong>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    public function provider()
    {
        return [['provider1'], ['provider2']];
    }

    public function testProducerFirst()
    {
        $this->assertTrue(true);
        return 'first';
    }

    public function testProducerSecond()
    {
        $this->assertTrue(true);
        return 'second';
    }

    /**
     * @depends testProducerFirst
     * @depends testProducerSecond
     * @dataProvider provider
     */
    public function testConsumer()
    {
        $this->assertEquals(
            ['provider1', 'first', 'second'],
            func_get_args()
        );
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit --verbose DependencyAndDataProviderComboTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

...F

Time: 0 seconds, Memory: 3.50Mb

There was 1 failure:

1) DependencyAndDataProviderComboTest::testConsumer with data set #1 ('provider2')
Failed asserting that two arrays are equal.
--- Expected
+++ Actual
@@ @@
Array (
-    0 => 'provider1'
+    0 => 'provider2'
1 => 'first'
2 => 'second'
)

/home/sb/DependencyAndDataProviderComboTest.php:31

FAILURES!
Tests: 4, Assertions: 4, Failures: 1.
</code></pre>


<br>
<h3>تست Testing Exceptions</h3>

<p>
مثال زیر نشان می دهد که چگونه از <span class="redbold">()expectException</span> در تست برای اینکه کدام استثنا رخ داده است استفاده می شود.
</p>

<br>
<strong>
مثال 8: نحوه استفاده از متد expectException
</strong>

<pre><code class="language-php   line-numbers"><?php
use PHPUnit\Framework\TestCase;

class ExceptionTest extends TestCase
{
    public function testException()
    {
        $this->expectException(InvalidArgumentException::class);
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit ExceptionTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

F

Time: 0 seconds, Memory: 4.75Mb

There was 1 failure:

1) ExceptionTest::testException
Expected exception InvalidArgumentException

FAILURES!
Tests: 1, Assertions: 1, Failures: 1.
</code></pre>

<p>
در ادامه متد <span class="redbold">()expectException</span>، متدهای <span class="redbold">()expectExceptionCode</span> و <span class="redbold">()expectExceptionMessage</span> و  <span class="redbold">()expectExceptionMessageRegExp</span> نیز جهت بررسی استثنائات مطرح شده توسط کد تحت آزمون مورد استفاده قرار می گیرند.
</p>

<br>
<strong>
مثال 9: نحوه استفاده از حاشیه نویسی  <span class="redbold">expectException@</span>
</strong>

<pre><code class="language-php   line-numbers"><?php
    /**
     * @expectedException InvalidArgumentException
     */
    public function testException()
    {
    }
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit ExceptionTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

F

Time: 0 seconds, Memory: 4.75Mb

There was 1 failure:

1) ExceptionTest::testException
Expected exception InvalidArgumentException

FAILURES!
Tests: 1, Assertions: 1, Failures: 1.
</code></pre>

<br>
<h3>
 تست PHP Errors
</h3>
<p>
به طور پیش فرض، PHPUnit اعلان های  errors, warnings,  notices که در هنگام اجرای یک آزمون ایجاد می شوند را به به یک استثنا تبدیل می کند.
</p>

<br>
<strong>
مثال 10:انتظار برای نمایش PHP error با استفاده از <span class="redbold">expectedException@</span>
</strong>

<pre><code class="language-php   line-numbers"><?php
use PHPUnit\Framework\TestCase;

class ExpectedErrorTest extends TestCase
{
    /**
     * @expectedException PHPUnit\Framework\Error
     */
    public function testFailingInclude()
    {
        include 'not_existing_file.php';
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit -d error_reporting=2 ExpectedErrorTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

.

Time: 0 seconds, Memory: 5.25Mb

OK (1 test, 1 assertion)
</code></pre>

<br>
<strong>
مثال 11:تست مقدار بازگشتی کد که از خطاهای PHP استفاده می کند
</strong>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    public function testFileWriting() {
        $writer = new FileWriter;
        $this->assertFalse(@$writer->write('/is-not-writeable/file', 'stuff'));
    }
}

class FileWriter
{
    public function write($file, $content) {
        $file = fopen($file, 'w');
        if($file == false) {
            return false;
        }
        // ...
    }
}
</code></pre>


<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit ErrorSuppressionTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

.

Time: 1 seconds, Memory: 5.25Mb

OK (1 test, 1 assertion)
</code></pre>


<br>
<h3>تست Output
</h3>
<p>
گاهی اوقات شما می خواهید صحت اجرای یک روش، به عنوان مثال، یک خروجی مورد انتظار را بررسی کنید (به عنوان مثال از طریق echo یا print)
</p>

<br>
<strong>
مثال 12:تست خروجی یک تابع یا متد
</strong>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

use PHPUnit\Framework\TestCase;

class Examples extends TestCase
{
    public function testExpectFooActualFoo()
    {
        $this->expectOutputString('foo');
        print 'foo';
    }

    public function testExpectBarActualBaz()
    {
        $this->expectOutputString('bar');
        print 'baz';
    }
}
</code></pre>

<p>
نتیجه خروجی :
</p>

<pre><code class="language-bash">
phpunit OutputTest
PHPUnit 6.5.0 by Sebastian Bergmann and contributors.

.F

Time: 0 seconds, Memory: 5.75Mb

There was 1 failure:

1) OutputTest::testExpectBarActualBaz
Failed asserting that two strings are equal.
--- Expected
+++ Actual
@@ @@
-'bar'
+'baz'


FAILURES!
Tests: 2, Assertions: 2, Failures: 1.
</code></pre>


<br>
<strong>
جدول زیر لیست متدهای برای تست خروجی را نمایش می دهد:
</strong>

<table style="direction:ltr" summary="Methods for testing output" border="1">
<colgroup><col><col></colgroup>
<thead><tr><th align="left">Method</th>
<th align="left">Meaning</th></tr></thead>
<tbody>
<tr><td align="left"><code class="literal">void expectOutputRegex(string $regularExpression)</code></td><td align="left">Set up the expectation that the output matches a <code class="literal">$regularExpression</code>.</td></tr><tr><td align="left"><code class="literal">void expectOutputString(string $expectedString)</code></td><td align="left">Set up the expectation that the output is equal to an <code class="literal">$expectedString</code>.</td></tr><tr><td align="left"><code class="literal">bool setOutputCallback(callable $callback)</code></td><td align="left">Sets up a callback that is used to, for instance, normalize the actual output.</td></tr>
<tr><td align="left"><code class="literal">string getActualOutput()</code></td><td align="left">Get the actual output.</td></tr>
</tbody></table>
