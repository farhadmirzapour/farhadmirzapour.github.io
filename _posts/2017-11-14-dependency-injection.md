---
layout: post
title:   مفهوم Dependency Injection در PHP
date:   2017-11-14 22:00:00 +0330
jdate: سه شنبه 23 آبان 1396
categories: articles
#  permalink: /:categories/:title.html
thumbnail: what-is-dependency-injection.jpg
refrence: https://code.tutsplus.com/tutorials/dependency-injection-huh--net-26903 <br> http://roxo.ir/آموزش-تزریق-وابستگی/ <br> http://pikneek.com/programming/زند-فریم-ورک-۲-dependency-injection/
---
<p>
تزریق وابستگی یا Dependency Injection یک پترن و الگوی طراحی است که هدف اصلی آن حذف وابستگی‌ های موجود بین دو کلاس با استفاده از یک رابط (Interface) است. در تزریق وابستگی ها دو اصطلاح داریم:
</p>

<p>
<ol>
<li>
<strong>loosely coupled:</strong> بدین معنی‌ست که کمترین وابستگی بین دو کلاس وجود دارد.
</li>
<li>
<strong>tight coupled:</strong> بدین معنی‌ست که بیشترین وابستگی بین دو کلاس وجود دارد.
</li>
</ol>
</p>

<blockquote>
<p>
بنابراین باید برنامه و نرم‌افزار خود را طوری طراحی کنیم که تا حد ممکن استحکام ضعیف (loosely coupled) باشد. هنگامیکه دو کلاس tight coupled هستند (یعنی به همدیگر وابستگی بسیار دارند یا به اصطلاح دیگری در هم چفت شده‌اند) با استفاده از یک رابطه‌ی باینری به یکدیگر متصل هستند و این امر انعطاف‌پذیری نرم‌افزار را به شدت پایین می‌آورد.
</p>
</blockquote>

<strong>
چرا بایستی کد وابستگی پایینی داشته باشد؟
</strong>

<p>
<ul>
<li>
<strong>Extensibility (توسعه پذیری) :</strong> با افزودن یک کلاس به برنامه نیازمند تغییرات در بخش های دیگر نباشیم
</li>

<li>
<strong>Testability (قابلیت تست پذیری) :</strong> فرآیند تست واحد به سهولت و بدون نیاز به در نظرگرفتن بخش های دیگر در آزمون انجام گیرد
</li>
<li>
<strong>Late Binding (انقیاد پویا) :</strong> بتوان در زمان اجرا و کامپایل خدمات مورداستفاده برنامه را تغییر داد
</li>

<li>
<strong>Parallel Development (توسعه موازی) :</strong> توسعه یک بخش از برنامه منوط به توسعه بخش خاص دیگری نباشد
</li>

<li>
<strong>Maintainability (قابلیت نگه داری) :</strong> افزودن بخش های جدید به برنامه و تغییرات پرهزینه نشوند.
</li>
</ul>
</p>

<p>
استفاده از DI یک آینده نگری است. همه ما میدانیم که احتمال تغییرات در سورس همیشه وجود دارد. پس نحوه کد نویسی در بهترین حالت خود, با کمترین تغییرات در آینده بیشترین کارایی را فراهم می نماید. در حالت های کد نویسی آشفته و بی برنامه ممکن است کوچکترین تغییری نیازمند بررسی و تغییر چندین کلاس و چندین فایل و چندین ماژول باشد. اما با برنامه ریزی این تغییرات به حداقل خود خواهد رسید علاوه بر آن مدیریت تغییرات را برای شما آسوده تر خواهد کرد.
</p>



<p>
جهت مفهوم این مطلب به مثال زیر توجه کنید :
</p>


<pre><code class="language-php  line-numbers"><?php
class Photo {
    /**
     * @var PDO The connection to the database
     */
    protected $db;

    /**
     * Construct.
     */
    public function __construct()
    {
        $this->db = DB::getInstance();
    }
}
</code></pre>

<p>
در نگاه اول، این کد ممکن است به نظر بی عیب و بی نقص باشد. اما باید درنظر گرفت کلاس Photo وابستگی زیادی  به اتصال پایگاه داده دارد(ممکن است در آینده بخواهیم از انواع مختلف پایگاه داده ها همانند sqlserver,mysql,oracle ... استفاده نماییم که مجبور به تغییرات زیاد در این بخش شده و  حتی منجر به ایجاد باگ های زیاد در صورت فراموشی تغییرات مربوطه می گردد). آیا به این فکر می کنید چرا باید کلاس Photo  با منابع خارجی ارتباط برقرار کند؟
</p>

<blockquote>
<p>
ایده اصلی این است که هر کلاس مسئول یک وظیفه باشد و با توجه به این نکته مسئولیت اتصال به پایگاه داده طبیعی نیست. (<a href="/documentation/solid-object-oriented-design/single-responsibility-principle" target="_blank" title="مفهوم Single Responsibility Principle" >اصل Single Responsibility Principle</a>)
</p>
</blockquote>

<p>
بیایید نگاه دوباره ای به  کلاس Photo بیاندازیم و اتصال پایگاه داده را به خارج از این کلاس انتقال دهیم. دو راه برای انجام این کار وجود دارد:
</p>
<p>
<ul>
<li>
Constructor Injection (تزریق با استفاده از سازنده کلاس)
</li>
<li>
Setter Injection (تزریق با استفاده از متدهای setter  کلاس)
</li>
</ul>
</p>

<br>
<h3>تزریق با استفاده از سازنده کلاس (Constructor Injection)</h3>

<p>
در این روش، زمانی که متد سازنده کلاس اجرا می شود، همه وابستگی های مورد نیاز را تزریق می کنیم. همانند کد زیر :
</p>

<pre><code class="language-php  line-numbers"><?php
class Photo {
    /**
     * @var PDO The connection to the database
     */
    protected $db;

    /**
     * Construct.
     * @param PDO $db_conn The database connection
     */
    public function __construct($dbConn)
    {
        $this->db = $dbConn;
    }
}

$photo = new Photo($dbConn);
</code></pre>

<br>
<h3>تزریق با استفاده از متدهای setter  کلاس (Setter Injection)</h3>

<p>
در این روش می توان با استفاده از متدهایی که درون کلاس تعریف می کنیم اشیا کلاس های دیگر را به درون کلاس تزریق نماییم. همانند کد زیر :
</p>

<pre><code class="language-php  line-numbers"><?php
class Photo {
    /**
     * @var PDO The connection to the database
     */
    protected $db;

    public function __construct() {}

    /**
     * Sets the database connection
     * @param PDO $dbConn The connection to the database.
     */
    public function setDB($dbConn)
    {
        $this->db = $dbConn;
    }
}

$photo = new Photo;
$photo->setDB($dbConn);
</code></pre>

<p>
با این تغییر ساده، کلاس دیگر وابسته به هیچ ارتباط خاص نیست. سیستم بیرونی کنترل کامل را حفظ می کند. این تکنیک باعث می شود که کلاس به راحتی تست شود، زیرا ما می توانیم در هنگام عملیات mock کردن دیتابیس در فرآیند تست واحد،از متد setDB استفاده نمائیم.
</p>

<p>
مشکل اساسی جهت استفاده از تزریق کننده ها این است که کاربر باید از وابستگی های کلاس کاملا آگاه باشد و باید آن را به نحوی تنظیم کند. همانند زیر :
</p>

<pre><code class="language-php  line-numbers">$photo = new Photo;
$photo->setDB($dbConn);
$photo->setConfig($config);
$photo->setResponse($response);
</code></pre>

<p>
 قبل از این کاربر به سادگی می توانست نمونه جدیدی از کلاس Photo را ایجاد کند، اما اکنون باید تمام این وابستگی ها را به یاد داشته باشد که با همچنین در سردرگمی و پیچیدگی مواجه شدیم!!!
</p>


<p>
راه حل این مشکل استفاده از مفهوم  Inversion of Control) IoC) می باشد.
</p>

<br>
<h3>مفهوم Inversion of Control) IoC)</h3>

<p>
مفهوم Inversion of Control) IoC) بدین صورت بیان می گردد:
</p>

<blockquote>
<p>
 در این فریم ورک امکان تنظیم اولیه وابستگی‌های سیستم وجود دارد. برای مثال زمانیکه برنامه از یک IoC Container، نوع اینترفیس خاصی را درخواست می‌کند، این فریم ورک با توجه به تنظیمات اولیه‌اش، کلاسی مشخص را بازگشت خواهد داد.
</p>
</blockquote>

<p>
به کلاس IoC کد زیر دقت نمایید :
</p>

<pre><code class="language-php  line-numbers">// Also frequently called "Container"
class IoC {
    /**
     * @var PDO The connection to the database
     */
    protected $db;

    /**
     * Create a new instance of Photo and set dependencies.
     */
    public static newPhoto()
    {
        $photo = new Photo;
        $photo->setDB(static::$db);
        // $photo->setConfig();
        // $photo->setResponse();

        return $photo;
    }
}
</code></pre>

<p>
همانطور که در کد بالا مشاهده می کنید ایجاد یک نمونه از کلاس و نیز فراخوانی وابستگی های مرتبط با آن درون متد newPhoto که بصورت static تعریف شده انجام می گردد. بنابراین هر زمان که به کلاس Photo نیاز داشتیم نیازی به خاطر سپردن وابستگی های مرتبط با آن نیست و تنها کافی است بصورت زیر عمل می کنیم :
</p>

<pre><code class="language-php  line-numbers">$photo = IoC::newPhoto();
</code></pre>

<p>
بسیار خب فرض کنید علاوه بر کلاس Photo  کلاس های دیگری  نیز داشته باشیم، به جای ایجاد یک متد جدید در IoC  به ازای هر کلاس می توانیم از روش عمومی تر همانند زیر استفاده نمائیم :
</p>

<pre><code class="language-php  line-numbers">// Also frequently called "Container"
class IoC {
    /**
     * @var PDO The connection to the database
     */
    protected static $registry = array();

    /**
     * Add a new resolver to the registry array.
     * @param  string $name The id
     * @param  object $resolve Closure that creates instance
     * @return void
     */
    public static function register($name, Closure $resolve)
    {
        static::$registry[$name] = $resolve;
    }

    /**
     * Create the instance
     * @param  string $name The id
     * @return mixed
     */
    public static function resolve($name)
    {
        if ( static::registered($name) )
        {
            $name = static::$registry[$name];
            return $name();
        }

        throw new Exception('Nothing registered with that name, fool.');
    }

    /**
     * Determine whether the id is registered
     * @param  string $name The id
     * @return bool Whether to id exists or not
     */
    public static function registered($name)
    {
        return array_key_exists($name, static::$registry);
    }
}
</code></pre>

<p>
در کد بالا یک آرایه به نام  registry$ ایجاد کرده ایم که با استفاده از متد register مقادیر کلید (نام کلاس) و دیتای متناظر با آن که یک تابع از نوع Closure می باشد مقدار دهی می شود. همچنین از تابع resolve و registered برای بررسی اینکه آیا نام کلاس در آرایه قبلا ثبت شده است و یا  خیر استفاده می شود.
</p>

<p>
بنابراین تنها کافی است  برای ثبت یک کلاس و وابستگی های مرتبط با آن و سپس فراخوانی کلاس مربوط به آن بدین صورت عمل کنیم :
</p>

<pre><code class="language-php  line-numbers">// Add `photo` to the registry array, along with a resolver
IoC::register('photo', function() {
    $photo = new Photo;
    $photo->setDB('...');
    $photo->setConfig('...');

    return $photo;
});

// Fetch new photo instance with dependencies set
$photo = IoC::resolve('photo');
</code></pre>