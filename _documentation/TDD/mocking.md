---
layout: documentation
title:    Mock Objects و Stub Methods
cattitle: اصول Test Driven Development
date:   2017-11-24 12:00:00 +0330
jdate: جمعه 3 آذر 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: https://jtreminio.com/2013/03/unit-testing-tutorial-part-4-mock-objects-stub-methods-dependency-injection/ <br> https://www.telerik.com/blogs/30-days-of-tdd-day-11-what-s-the-deal-with-mocking <br> http://elmah.ir/post/94-۳۰-روز-با-tdd--روز-یازدهم---درباره-mocking/ <br>  http://zerotohero.ir/article/android/مقدمه-ای-بر-unit-test-در-جاوا-و-اندروید
---
<p>
اغلب نرم‌افزارهایی که شما توسعه می‌دهید، از کلاس‌ها و اجزا (component) مختلفی تشکیل می‌شوند. در حالت ایده‌آل، هر کلاس یا جزء برای اجرای وظایف خاصی طراحی می‌شوند که این همان Single Responsibility Principle است. این کلاس‌ها و اجزا در کنار هم یک برنامه (Application) را تشکیل می‌دهند. طبیعت خاص کلاس‌های و اجزای واحد، وابستگی را غیرقابل اجتناب می‌کند. رابط کاربر (User Interface) شما به کلاس‌های business domain وابسته است و خود کلاس‌های business domain به چیزهایی مثل انبارهای خارجی داده (دیتابیس، فایل سیستم)، وب سرویس یا منابع و سیستم‌های خارجی دیگر وابسته هستند.
</p>


<p>
وقتی ما تستهای واحد را می نویسیم، لازم است به یاد داشته باشیم که این آزمون باید بر روی کد خاصی که ما می خواهیم آن را تست کنیم متمرکز شده باشد. به کد زیر توجه کنید:
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class Payment
{
    const API_ID = 123456;
    const TRANS_KEY = 'TRANSACTION KEY';

    public function processPayment(array $paymentDetails)
    {
        $transaction = new \AuthorizeNetAIM(self::API_ID, self::TRANS_KEY);
        $transaction->amount = $paymentDetails['amount'];
        $transaction->card_num = $paymentDetails['card_num'];
        $transaction->exp_date = $paymentDetails['exp_date'];

        $response = $transaction->authorizeAndCapture();

        if ($response->approved) {
            return $this->savePayment($response->transaction_id);
        } else {
            throw new \Exception($response->error_message);
        }
    }

    public function savePayment($transactionId)
    {
        // Logic for saving transaction ID to database or anywhere else would go in here
        return true;
    }
}
</code></pre>



<p>
باتوجه به کد بالا متد processPayment  حاوی منطقی  برای پرداخت اینترنتی با اتصال به یک وب سرویس می باشد. همچنین متد savePayment (خط 25) وظیفه ذخیره سازی برخی از داده ها در  پایگاه داده را برعهده دارد.
</p>

<p>
دلایل زیادی وجود دارد که نمی خواهیم از  کلاس واقعی و اجزای وابسته به آن در نوشتن تست استفاده کنیم :
</p>

<ul>
<li>
همانطور که ذکر شد، می خواهیم تست خود را بر روی قطعه کدی خاص (متدها و کلاس) متمرکز کنیم. این امر سبب می شود پیدا کردن نقص بسیار آسان تر و سریع تر گردد
</li>

<li>
دلیل دیگری که نمی خواهیم از کلاس های واقعی برای آزمون ها استفاده کنیم این است که آنها می توانند آزمون های غیر قابل پیش بینی را انجام دهند. اگر تست واحد من یک پایگاه داده را هر بار که اجرا می کنیم بخواند، انتظار می رود مقدار خاصی در پایگاه داده وجود داشته باشد که در رنج آن پایگاه داده باشیم. همین امر سبب می شود آزمون ما غیر قابل پیش بینی گردد.
</li>

<li>
سرعت مسئله دیگر است. می خواهیم تست هایم سریع باشند. کدهایی که با یک منبع خارجی مانند یک پایگاه داده یا یک سرویس وب ارتباط برقرار می کنند با توجه به تاخیر در برقراری ارتباط با این منابع خارجی، سرعت اجرای آن  کاهش می یابد و زمانی که صدها آزمایش قرار است انجام شود سرعت اجرای تست نیز بسیار آهسته تر اجرا خواهند شد.
</li>

</ul>

<p>
 Dependency Injection گام نخست برای راه حل این مشکل است. بنابراین شی transaction$ که از کلاس AuthorizeNetAIM  ایجاد شده در خارج از متد processPayment ایجاد و به عنوان آرگومان تزریق می گردد :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class Payment
{
    const API_ID = 123456;
    const TRANS_KEY = 'TRANSACTION KEY';

    public function processPayment(\AuthorizeNetAIM $transaction , array $paymentDetails)
    {
        $transaction->amount = $paymentDetails['amount'];
        $transaction->card_num = $paymentDetails['card_num'];
        $transaction->exp_date = $paymentDetails['exp_date'];

        $response = $transaction->authorizeAndCapture();

        if ($response->approved) {
            return $this->savePayment($response->transaction_id);
        } else {
            throw new \Exception($response->error_message);
        }
    }

    public function savePayment($transactionId)
    {
        // Logic for saving transaction ID to database or anywhere else would go in here
        return true;
    }
}
</code></pre>


<h3>مفهوم Mocks و Stubs و Fakes</h3>

<p>
اکنون به مبحث Mocking  می‌پردازیم. اگر واژه Mock را در فرهنگ لغت انگلیسی جستجو کنید, مفهوم “something made as an imitation” را مشاهده خواهید کرد. در واقع می‌توان اینطور تعریف کرد که Mocking به عملی تلقی می‌شود که یک چیز را به عنوان بدل ایجاد کنیم. Mocking در Unit Testing استفاده می‌شود. در هنگام  Test یک Object, ممکن است این Object به Object های دیگری وابسته و برای پردازش به مقادیری که این Object ها بر می‌گردانند, نیاز داشته باشد. در واقع هدف از Mocking این است که می‌خواهیم کد ها را بدون دخالت Dependency های آنها اجرا کنیم. هدف این است که Mocked Object ها, نقش Object های واقعی را ایفا کنند. متد و یا متد هایی در این Object با متغیر های معینی فراخوانی می‌شود و نتیجه مورد نظر را بر می‌گرداند.
</p>

<p>
برای مثال فرض کنید به یک متد که برای احراز هویت کاربر است, مقادیر نام کاربری و رمز عبور را ارسال می‌کنیم و سپس مشخص می‌کنیم که اگر مقادیر وارد شده, با مقادیر مورد نظر برابر بودند,  سطح دسترسی خاصی را برگرداند. واضح است که این پردازش نیاز به ارتباط با پایگاه داده دارد. اما در اینجا تنها ورودی و خروجی متد را مشخص کرده‌ایم و پردازش متد مد نظر ما نیست. همین امر باعث می‌شود که حتی اگر متد دارای Dependency های خاصی است, در Testing مشکلی ایجاد نکند.
</p>

<p>
روش دیگری نیز به نام Stubbing وجود دارد که در کنار Mocking قرار می‌گیرد. استفاده از Stubbing بسیار آسان است و هیچ Extra Dependency را در زمان Testing, دخالت نمی‌دهد. در واقع بخشی از کلاسی که مورد نیاز است را پیاده سازی می‌کنیم تا در زمان Testing بتوان از آن استفاده کرد.
</p>

<p>
 چه زمانی mock یک mock نیست؟ چه زمانی stub یا fake است؟ تفاوت این‌ها چیست؟ تفسیر Martin Fowler:
</p>

<p>
<ul>
<li>
<b>Fakes:</b> یک Fake شی‌ای است که یک مکانیزم داخلی دارد که نتایج قابل پیش‌بینی برمی‌گرداند، اما منطق کاری واقعی را پیاده‌سازی نکرده است.
</li>

<li>
<b>Stubs:</b> یک Stub شی‌ای است که یک نتیجه مشخص را بر اساس یک سری ورودی مشخص برمی‌گرداند. اگر من به stub بگویم که هر وقت شخصی با شناسه 42 را خواستم عبارت John Doe را برگردان، stub همین کار را خواهد کرد. با این حال اگر من از stub بخواهم که شخصی با شناسه 41 را برگرداند، نمی‌داند چه کار باید بکند. بر حسب اینکه از کدام mocking framework استفاده کنم، stub یا exception ایجاد می‌کند یا یک شی null برمی‌گرداند. stub می‌تواند بعضی اطلاعات مربوط به نحوه فراخوانی مثل تعداد فراخوانی یا اینکه با چه داده‌هایی فراخوانی شده است را به یاد داشته باشد.
</li>

<li>
<b>Mocks:</b> یک Mock یک نسخه پیچیده‌تر از stub است. همچنان مانند stub مقادیر را برمی‌گرداند، اما همچنین می‌تواند طوری برنامه‌ریزی شود که باید چند بار فراخوانی شود، به چه ترتیب یا به چه داده‌هایی.
</li>

<li>
<b>Spy:</b> یک Spy نوعی mock است که یک شی را می‌گیرد و به جای ایجاد یک شی mock متدهایی که tester می‌خواهد mock کند را جایگزین می‌کند. Spy ها برای کدهای غیر TDD عالی هستند، اما باید خیلی مراقب باشید چرا که فراموش کردن چیزی که می‌بایست mock شود ممکن است نتایج فاجعه‌باری داشته باشد.
</li>

<li>
<b>Dummy:</b> یک Dummy شی‌ای است که می‌تواند به عنوان جایگزین یک شی دیگر پاس داده شود اما استفاده نمی‌شود. Dummy ها در واقع placeholder محسوب می‌شوند.
</li>

</ul>
</p>

<p>
بر اساس تعاریف بالا، بعضی انواع mock ها هستند که خودمان می‌توانیم آن‌ها را ایجاد کنیم مثل Fake ها و Dummy ها. همچنین می‌توان یک stub ساده نوشت، اما زمان انجام این کار بر روی بهره‌وری من تاثیر خواهد داشت. خوشبختانه راه بهتری وجود دارد: استفاده از فریمورک‌های mocking. در ادامه از ابزار mock موجود در PHPunit استفاده خواهیم کرد.
</p>

<p>
برای مثال بالا می خواهیم یک تست واحد نوشته شود. با توجه به کد بالا متد authorizeAndCapture در خط 15  عملیات اجرای تراکنش را برعهده داده دارد و نتیجه را بصورت یک شی در متغیر response$ ذخیره می نماید. چنانچه مقدار response->approved$ برابر true  باشد به معنای موفقیت آمیز بودن تراکنش بوده و مقدار transaction_id را در پایگاه داده ذخیره می نماید. بنابراین از متد assertTrue جهت بررسی نتایج استفاده می شود و کدی مانند زیر خواهیم داشت :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class PaymentTest  extends \PHPUnit_Framework_TestCase
{

    public function testProcessPaymentReturnsTrueOnSuccessfulPayment()
    {
        //Arrange
        $paymentDetails = array(
            'amount'   => 123.99,
            'card_num' => '4111-1111-1111-1111',
            'exp_date' => '03/2013'
        );

        $payment = new \Classes\Payment();
        $authorizeNet = new \AuthorizeNetAIM($payment::API_ID, $payment::TRANS_KEY);

        //Act
        $result = $payment->processPayment($authorizeNet, $paymentDetails);

        //Assert
        $this->assertTrue($result);
    }
}
</code></pre>

<p>
فهمیدن کد این تست باید خیلی ساده باشد. در قسمت Arrange  مبلغ و یک کارت پرداخت اینترنتی فرضی ایجاد کردیم و در قسمت Act , Assert  مراحل پرداخت مورد آزمون قرار می گیرد.
</p>

<p>
 زمانیکه آزمون را اجرا می کنیم به دلیل اینکه یا کلاس AuthorizeNetAIM پیاده سازی نشده است و یا نمی توان به دلایلی نظیر قطعی اینترنت و ... با این وب سرویس ارتباط برقرار کرد بنابراین مشاهده می کنیم که آزمون fail  می شود.
</p>

<p>
بنابراین در اینجا از mock  جهت ایجاد یک شی غیر واقعی از کلاس AuthorizeNetAIM استفاده می نمائیم :
</p>

<pre><code class="language-php   line-numbers"><?php
namespace Test;

class PaymentTest  extends \PHPUnit_Framework_TestCase
{

    public function testProcessPaymentReturnsTrueOnSuccessfulPayment()
    {
        //Arrange
        $paymentDetails = array(
            'amount'   => 123.99,
            'card_num' => '4111-1111-1111-1111',
            'exp_date' => '03/2013'
        );

        $payment = new \Classes\Payment();
        $authorizeNet = $this->getMock('\AuthorizeNetAIM', array('__construct','authorizeAndCapture'), array($payment::API_ID, $payment::TRANS_KEY));

        //Act
        $result = $payment->processPayment($authorizeNet, $paymentDetails);

        //Assert
        $this->assertTrue($result);
    }
}
</code></pre>

<p>
همانطور که مشاهده می کنید در خط 17  با استفاده از متد getMock کلاسی به نام AuthorizeNetAIM ایجاد کردیم که دارای دو متد  construct__ و authorizeAndCapture می باشد. سپس نمونه ای از این کلاس را ایجاد کرده است. ساختار این متد بدین صورت است :
</p>

<blockquote>
<p style="direction:ltr;text-align:left">
public function getMock($originalClassName, $methods = array(), array $arguments = array(), $mockClassName = '', $callOriginalConstructor = TRUE, $callOriginalClone = TRUE, $callAutoload = TRUE, $cloneArguments = TRUE)
</p>
</blockquote>

<p>
همچنین می توان از متد getMockBuilder نیز استفاده کرد که بدین صورت پیاده سازی می شود :
</p>

<pre><code class="language-php   line-numbers">$authorizeNet = $this->getMockBuilder('\AuthorizeNetAIM')
                     ->setMethods(array('__construct','authorizeAndCapture'))
                     ->setConstructorArgs(array($payment::API_ID, $payment::TRANS_KEY))
                     ->getMock();
</code></pre>

<p>
هم اکنون تست را بار دیگر اجرا می کنیم و نتیجه زیر مشاهده می گردد :
</p>

<div align="center">
<img src="/images/post/mocktest2.jpg" alt="{{page.title}}" />
</div>

<p>
این پیغام به این دلیل است که متدهایی که در کلاس های mock ایجاد می گردند همگی مقدار NULL  را برگشت می دهند. برای مثال چنانچه دستور زیر را اجرا کنید مشاهده می کنید که مقدار NULL  نمایش داده می شود :
</p>

<pre><code class="language-php   line-numbers">var_dump($authorizeNet->authorizeAndCapture());
</code></pre>
<p>
 بنابراین در اینجا از stub  جهت override کردن متد مورد نظر و تعیین  مقدار بازگشتی  خود استفاده می کنیم . بنابراین کدی مانند زیر خواهیم داشت :
</p>


<pre><code class="language-php   line-numbers"><?php
namespace Test;

class PaymentTest  extends \PHPUnit_Framework_TestCase
{

    public function testProcessPaymentReturnsTrueOnSuccessfulPayment()
    {
        //Arrange
        $paymentDetails = array(
            'amount'   => 123.99,
            'card_num' => '4111-1111-1111-1111',
            'exp_date' => '03/2013'
        );

        $payment = new \Classes\Payment();
        $authorizeNet = $this->getMock('\AuthorizeNetAIM', array('__construct','authorizeAndCapture'), array($payment::API_ID, $payment::TRANS_KEY));

        $response = new \stdClass();
        $response->approved = true;
        $response->transaction_id = 123;

        $authorizeNet->expects($this->once())
            ->method('authorizeAndCapture')
            ->will($this->returnValue($response));

        //Act
        $result = $payment->processPayment($authorizeNet, $paymentDetails);

        //Assert
        $this->assertTrue($result);
    }
}
</code></pre>

<p>
می دانیم نوع بازگشتی authorizeAndCapture یک شی می باشد که دارای دو عنصر approved که حاوی مقدار boolean  و transaction_id که حاوی نوع داده int  می باشد. بنابراین ابتدا مقدار بازگشتی را در خطوط 19 الی 21 ایجاد کردیم.
</p>

<p>
سپس در خطوط 23 الی 25 مقدار بازگشتی مورد نظر خود به متد authorizeAndCapture نسبت داده ایم.
</p>

<p>
با استفاده از متد expects تعداد دفعاتی که انتظار می رود متد مورد نظر ما در کد اجرا شود را تعیین می کنیم که می تواند شامل once, any, never  باشد.
</p>
<p>
با استفاده از متد method متد مورد نظر خود را تعیین می کنیم.
</p>

<p>
و در آخر با استفاده از متدهای will , returnValue مقدار بازگشتی متد را مشخص می کنیم.
</p>

<p>
هم اکنون با اجرای دوباره آزمون مشاهده خواهیم کرد که آزمون pass شده است.
</p>