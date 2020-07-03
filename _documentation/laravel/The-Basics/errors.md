---
layout: documentation-laravel
title:   مدیریت Errors & Logging
cattitle: اصول آموزش Laravel
date:   2018-02-11 19:21:42 +0330
jdate: یکشنبه 22 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/errors <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/283/مدیریت-خطاها-در-لاراول-5-5
---
<p>
زمانی که یک پروژه جدید در لاراول ایجاد می‌کنید، مدیریت خطاها و استثناها از قبل برای شما پیکربندی می‌شود. کلاس App\Exceptions\Handler مکانی است که تمام استثناهای ایجاد شده توسط برنامه در آن ثبت (log) می‌شوند و پس از آن، دوباره به کاربر نمایش داده می‌شوند. در ادامه مقاله، نحوه مدیریت خطاها و استثناها در لاراول را بیشتر مورد بررسی قرار خواهیم داد.
</p>

<p>
لاراول برای عملیات ثبت وقایع یا logging، از کتابخانه Monolog استفاده می‌کند، که از چند نوع log handler کارآمد پشتیبانی می‌کند. لاراول تعدادی از این handlerها را تنظیم کرده است که این امکان را می‌دهد که بتوانید از بین یک فایل log واحد، فایل‌های log چرخشی یا نوشتن اطلاعات خطا در سیستم log، یکی را انتخاب کنید.
</p>

<p>
<a name="configuration"></a>
</p>
<h3><a href="#configuration">پیکربندی جزئیات خطا در لاراول</a></h3>
<p>
<a name="error-detail"></a>
</p>
<h3>Error Detail</h3>
<p>
گزینه debug در فایل پیکربندی config/app.php مشخص می‌کند که چه اطلاعاتی درباره یک خطا به کاربر نمایش داده شود. در حالت پیش‌فرض، این گزینه به این صورت تنظیم شده است که بتواند مقدارش را از متغیر محیطی APP_DEBUG که در فایل .env ذخیره می‌شود، دربافت کند.
</p>

<p>
در هنگام توسعه محلی برنامه، باید متغیر محیطی APP_DEBUG را با مقدار true تنظیم کنید. در محیط تولید برنامه (در زمان استفاده از برنامه در محیط تجاری)، همواره باید این متغیر را با مقدار false تنظیم کنید. در صورتی که در این مرحله، این متغیر با مقدار true تنظیم شده باشد، ممکن است مقادیر مهم پیکربندی برنامه در معرض دید کاربران نهایی قرار گیرد.
</p>

<p>
<a name="log-storage"></a>
</p>
<br>
<h3>ذخیره سازی Log در مدیریت خطاها</h3>
<p>
در حالت پیش‌فرض، لاراول از نوشتن داده‌های log به فایل‌های daily ، single ، syslog و errorlog پشتیبانی می‌کند. جهت تنظیم اینکه لاراول از کدام مکانیزم ذخیره سازی برای log استفاده کند، می‌توانید گزینه log را در فایل پیکربندی config/app.php خود تغییر دهید. برای مثال، اگر مایل به استفاده از فایل‌های log روزانه به جای یک فایل واحد هستید، باید در فایل پیکربندی app ، مقدار log  را به daily تنظیم کنید:
</p>

<pre><code class="language-php  line-numbers">'log' => 'daily'
</code></pre>

<br>
<h3>تعیین ماکزیمم نگهداری فایل‌های log روزانه</h3>
<p>
در هنگام استفاده از حالت ثبت daily ، لاراول در حالت پیش‌فرض، تنها پنج روز از فایل‌های log نگهداری می‌کند. اگر بخواهید، تعداد روزهای نگهداری فایل‌های log را تنظیم کنید، می‌توانید این تعداد را به عنوان یک مقدار پیکربندی log_max_files در فایل پیکربندی app اضافه کنید:
</p>

<pre><code class="language-php  line-numbers">'log_max_files' => 30
</code></pre>

<p>
<a name="log-severity-levels"></a>
</p>
<br>
<h3>سطوح log در مدیریت خطاهای لاراول</h3>
<p>
در هنگام استفاده از کتابخانه Monolog، پیغام‌های log می‌توانند دارای سطوح مختلفی باشند. در حالت پیش‌فرض، لاراول همه لایه‌های log را در حافظه می‌نویسد. با این حال، ممکن است بخواهید در زمان استفاده از برنامه در محیط تولید، حداقل سطوح را برای آن تنظیم کنید؛ با افزودن گزینه log_level در فایل پیکربندی app.php خود، می‌توانید این کار را انجام دهید.
</p>

<p>
زمانی که این گزینه پیکربندی شد، لاراول همه سطوحی را که بالاتر یا برابر آن باشند، ثبت (log) می‌کند. برای مثال، یک log_level پیش‌فرض از error ، پیام‌های error، critical، alert و پیام‌های emergency را ثبت می‌کند:
</p>

<pre><code class="language-php  line-numbers">'log_level' => env('APP_LOG_LEVEL', 'error'),
</code></pre>
<blockquote class="has-icon tip">
Monolog سطوح log زیر را به رسمیت می‌شناسد که از کمترین سطح تا بیشترین سطح عبارتند از:
<br>
debug ،info ،notice ،warning ،error ،critical ،alert ،emergency
</blockquote>
<p>
<a name="custom-monolog-configuration"></a>
</p>
<br>
<h3>پیکربندی سفارشی کتابخانه Monolog</h3>
<p>
اگر بخواهید کنترل کاملی بر چگونگی پیکربندی کتابخانه Monolog برای برنامه خود داشته باشید، می‌توانید از متد configureMonologUsing برنامه استفاده کنید. درست قبل از اینکه متغیر $app توسط فایل بازگردانده شود، بایستی فراخوانی این متد را در فایل bootstrap/app.php خود قرار دهید:
</p>

<pre><code class="language-php  line-numbers">$app->configureMonologUsing(function ($monolog) {
    $monolog->pushHandler(...);
});

return $app;
</code></pre>
<h3>سفارشی کردن نام channel در مدیریت خطا</h3>
<p>
در حالت پیش‌فرض، Monolog با نامی مطابق با محیط فعلی مانند محیط توسعه محلی ( local ) یا محیط استفاده از برنامه ( production ) تنظیم شده است. برای تغییر این مقدار، می‌توانید گزینه log_channel را در فایل پیکربندی app.php اضافه کنید:
</p>

<pre><code class="language-php  line-numbers">'log_channel' => env('APP_LOG_CHANNEL', 'my-app-name'),
</code></pre>

<p>
<a name="the-exception-handler"></a>
</p>
<br>
<h3><a href="#the-exception-handler">مدیریت استثنا و متد report</a></h3>
<p>
<a name="report-method"></a>
</p>
<h3>The Report Method</h3>
<p>
تمام استثناها (exception) توسط کلاس App\Exceptions\Handler مدیریت می‌شوند. این کلاس شامل دو متد report و render است. در ادامه، هر یک از این متدها را به طور دقیق مورد بررسی قرار خواهیم داد؛ متد report جهت ثبت استثناها یا ارسال آن‌ها به یک سرویس خارجی مانند Bugsnag یا Sentry استفاده می‌شود. در حالت پیش‌فرض، متد report استثناها را به کلاس پایه‌ای که استثنا در آن ثبت می‌شود، ارسال می‌کند. با این حال، می‌توانید استثناها را به هر روشی که می‌خواهید ثبت کنید.
</p>

<p>
برای مثال، اگر بخواهید انواع استثناها را به روش‌های مختلف گزارش کنید، می‌توانید از عملگر مقایسه‌ای instanceof زبان PHP به صورت زیر استفاده کنید.
</p>

<pre><code class="language-php  line-numbers">/**
 * Report or log an exception.
 *
 * This is a great spot to send exceptions to Sentry, Bugsnag, etc.
 *
 * @param  \Exception  $exception
 * @return void
 */
public function report(Exception $exception)
{
    if ($exception instanceof CustomException) {
        //
    }

    return parent::report($exception);
}
</code></pre>
<br>
<h3>The <code class=" language-php">report</code> Helper</h3>
<p>
گاهی اوقات، ممکن است بخواهید یک استثنا را گزارش کنید، اما مدیریت درخواست فعلی همچنان ادامه داشته باشد. تابع کمکی report  امکانی را فراهم می‌کند که بتوانید بدون اینکه یک صفحه خطا نمایش داده شود، سریعاً یک استثنا را با استفاده از متد report مربوط به exception handler خود گزارش کنید:
</p>

<pre><code class="language-php  line-numbers">public function isValid($value)
{
    try {
        // Validate the value...
    } catch (Exception $e) {
        report($e);

        return false;
    }
}
</code></pre>

<br>
<h3>نادیده گرفتن استثناها بر‌اساس نوع در مدیریت خطای</h3>
<p>
خصوصیت $dontReport از exception handler آرایه‌ای از انواع استثناها که ثبت نشده‌اند را شامل می‌شود. برای مثال، استثناهایی که از خطاهای 404 نتیجه می‌شوند و چند نوع دیگر از خطاها که در فایل‌های log نوشته نشده‌اند. می‌توان انواع استثناهای دیگر را به این آرایه اضافه کرد:
</p>

<pre><code class="language-php  line-numbers">/**
 * A list of the exception types that should not be reported.
 *
 * @var array
 */
protected $dontReport = [
    \Illuminate\Auth\AuthenticationException::class,
    \Illuminate\Auth\Access\AuthorizationException::class,
    \Symfony\Component\HttpKernel\Exception\HttpException::class,
    \Illuminate\Database\Eloquent\ModelNotFoundException::class,
    \Illuminate\Validation\ValidationException::class,
];
</code></pre>

<p>
<a name="render-method"></a>
</p>
<br>
<h3>متد render در مدیریت خطا</h3>
<p>
متد render مسئول تبدیل یک استثنا به یک پاسخ HTTP است که باید به مرورگر کاربر ارسال شود. در حالت پیش‌فرض، استثنا به کلاس پایه‌ای که پاسخ HTTP را ایجاد می‌کند، منتقل می‌شود. با این وجود، می‌توانید نوع استثنا خود را بررسی کرده و پاسخ سفارشی خود را به مرورگر برگردانید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Render an exception into an HTTP response.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Exception  $exception
 * @return \Illuminate\Http\Response
 */
public function render($request, Exception $exception)
{
    if ($exception instanceof CustomException) {
        return response()->view('errors.custom', [], 500);
    }

    return parent::render($request, $exception);
}
</code></pre>

<p>
<a name="renderable-exceptions"></a>
</p>
<br>
<h3>استثنا قابل گزارش‌گیری و قابل نمایش</h3>
<p>
به جای بررسی نوع استثنا در متدهای report و render در exception handler، می‌توانید متدهای report و render را مستقیماً در استثنا سفارشی خود تعریف کنید. وقتی این متدها موجود باشند، به صورت خودکار توسط فریم ورک فراخوانی می‌شوند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Exceptions;

use Exception;

class RenderException extends Exception
{
    /**
     * Report the exception.
     *
     * @return void
     */
    public function report()
    {
        //
    }

    /**
     * Render the exception into an HTTP response.
     *
     * @param  \Illuminate\Http\Request
     * @return \Illuminate\Http\Response
     */
    public function render($request)
    {
        return response(...);
    }
}
</code></pre>

<p>
<a name="http-exceptions"></a>
</p>
<br>
<h3><a href="#http-exceptions">HTTP Exceptions</a></h3>
<p>
برخی از استثناها کد خطاهای HTTP از سرور را توصیف می‌کنند؛ به عنوان مثال، می‌تواند یک پیغام «صفحه موردنظر یافت نشد» با خطای (404) یا پیغام «خطای غیر مجاز بودن» با خطای (401) یا حتی یک خطای 500 تولید شده توسط توسعه دهنده برنامه باشد. برای تولید این پاسخ از هر جایی در برنامه، می‌توانید از تابع کمکی abort استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">abort(404);
</code></pre>

<p>
تابع کمکی abort بلافاصله یک استثنا ایجاد می‌کند که پس از آن توسط exception handler به مرورگر رندر می‌شود. در صورت تمایل، می‌توانید متن پاسخ را به صورت زیر مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">abort(403, 'Unauthorized action.');
</code></pre>

<p>
<a name="custom-http-error-pages"></a>
</p>
<br>
<h3>سفارشی سازی صفحات خطای HTTP</h3>
<p>
لاراول این امکان را می‌دهد که برای هر یک از کدهای وضعیت HTTP یک صفحه خطای سفارشی ایجاد کرده و نمایش دهید. برای مثال، اگر می‌خواهید یک صفحه خطای سفارشی برای کد وضعیت 404 HTTP ایجاد کنید، بایستی ابتدا یک فایل resources/views/errors/404.blade.php  ایجاد کنید. این فایل با هر خطای 404 ایجاد شده در برنامه نمایش داده می‌شود. نام viewهای موجود در این دایرکتوری باید با نام کد مربوط به HTTP مطابقت داشته باشند. نمونه HttpException ایجاد شده توسط تابع abort به عنوان یک متغیر $exception به view انتقال داده می‌شود.
</p>

```html
<h2>{% raw %}{{{% endraw %} $exception->getMessage() {% raw %}}}{% endraw %}</h2>
</code></pre>
```

<p>
<a name="logging"></a>
</p>
<br>
<h3><a href="#logging">logging در مدیریت خطا</a></h3>
<p>
لاراول یک لایه انتزاعی ساده را از کتابخانه قدرتمند Monolog ارائه می‌دهد. در حالت پیش‌فرض، لاراول یک فایل log در دایرکتوری storage/logs برای برنامه ایجاد می‌کند. می‌توان اطلاعات را در logs توسط facade Logوشت:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use App\User;
use Illuminate\Support\Facades\Log;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function showProfile($id)
    {
        Log::info('Showing user profile for user: '.$id);

        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
</code></pre>

<p>
ثبت کننده وقایع (Logger) هشت سطح logging را که در RFC 5424 تعریف شده است، ارائه می‌دهد:
</p>
<p>
debug ،info ،notice ،warning ،error ،critical ،alert ،emergency
</p>

<pre><code class="language-php  line-numbers">Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);
</code></pre>

<br>
<h3>اطلاعات contextual در مدیریت خطای لاراول</h3>
<p>
می‌توان آرایه‌ای از داده‌های contextual را به متدهای log انتقال داد. این داده‌های contextual با پیغام log قالب بندی شده و نمایش داده می‌شوند:
</p>

<pre><code class="language-php  line-numbers">Log::info('User failed to login.', ['id' => $user->id]);
</code></pre>

<br>
<h3>دسترسی به نمونه پایه Monolog</h3>
<p>
Monolog دارای تعدادی از handlerهای اضافی است که ممکن است بخواهید از آن‌ها برای عملیات logging استفاده کنید. در صورت لزوم، می‌توانید به صورت زیر به نمونه پایه Monolog که توسط لاراول استفاده می‌شود، دسترسی داشته باشید.
</p>

<pre><code class="language-php  line-numbers">
$monolog = Log::getMonolog();
</code></pre>

