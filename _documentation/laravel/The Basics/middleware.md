---
layout: documentation
title:   آموزش Middleware
cattitle: اصول آموزش Laravel
date:   2018-02-01 22:43:42 +0330
jdate: پنج شنبه 12 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/middleware <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/221/کاربرد-middleware-لاراول
---
<p>
middleware مکانیزم مناسبی ارائه می‌دهد که به وسیله آن می‌توان درخواست‌های HTTP را قبل از ورود به برنامه فیلتر کرد. برای مثال، لاراول شامل یک middleware است که بوسیله آن می‌توان مشخص کرد که کاربر برنامه به درستی احراز هویت شده است یا خیر. اگر کاربر تایید نشده باشد، middleware کاربر را دوباره به صفحه ورود به سایت (Login) هدایت می‌کند. ولی اگر کاربر تأیید هویت شده باشد، middleware به درخواست اجازه می‌دهد تا به برنامه وارد شده و عملیات دیگری را نیز انجام دهد. در ادامه بیشتر به middleware در لاراول می‌پردازیم.
</p>

<p>
البته، middlewareهای دیگری نیز برای انجام کارهای مختلف، علاوه بر احراز هویت نوشته شده است. برای مثال، یک CORS middleware مسئول اضافه کردن سرصفحه‌های مناسب به تمام پاسخ‌هایی است که از برنامه خارج می‌شوند. یک middleware ثبت وقایع، تمام درخواست‌های ورودی به برنامه را ثبت می‌کند.
</p>

<p>
در فریم ورک لاراول چند middleware به صورت پیش فرض وجود دارند که از جمله آن‌ها می‌توان به middleware احراز هویت و حفاظت CSRF اشاره کرد. همه این middlewareها در دایرکتوری app/Http/Middleware قرار دارند.
</p>

<p>
<a name="defining-middleware"></a>
</p>
<br>
<h3><a href="#defining-middleware">تعریف middleware </a></h3>

<p>
برای ایجاد یک middleware جدید، از دستور آرتیسان make:middleware استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">php artisan make:middleware CheckAge
</code></pre>

<p>
این دستور یک کلاس CheckAge جدید را در دایرکتوری app/Http/Middleware ایجاد می‌کند. در این middleware، فقط در صورتی که age بیشتر از 200 باشد، به مسیر دسترسی خواهیم داشت. در غیر این صورت، کاربران را به URI مربوط به home هدایت می‌شوند.
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Closure;

class CheckAge
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($request->age <= 200) {
            return redirect('home');
        }

        return $next($request);
    }
}
</code></pre>

<p>
همانطور که مشاهده می‌کنید، اگر age داده شده کمتر یا برابر 200 باشد، middleware یک HTTP redirect را به مشتری یا سرویس گیرنده بازمی‌گرداند؛ در غیر این صورت، درخواست به برنامه منتقل خواهد شد. برای ارسال محتاطانه یک درخواست به برنامه (middleware اجازه می‌دهد که درخواست یک pass به برنامه داشته باشد)، به سادگی تابع $next را با آرگومان $request فراخوانی کنید.
</p>

<p>
می‌توان middleware را به عنوان مجموعه‌ای از لایه‌ها در نظر گرفت که درخواست های HTTP باید از آن‌ها عبور کنند و وارد برنامه شوند. هر لایه باید درخواست‌ها را بررسی کند و حتی می‌تواند آن‌ها را به طور کامل رد کند.
</p>

<br>
<h3>انجام وظایف توسط Middleware قبل و بعد از اجرای درخواست در لاراول</h3>

<p>
اینکه آیا middleware قبل یا بعد از درخواست اجرا ‌شود، بستگی به خود middleware دارد. برای مثال، middleware زیر برخی از کارها را قبل از پردازش درخواست توسط برنامه انجام می‌دهد:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Closure;

class BeforeMiddleware
{
    public function handle($request, Closure $next)
    {
        // Perform action

        return $next($request);
    }
}
</code></pre>

<p>
ولی middleware مثال زیر وظیفه خود را پس از پردازش درخواست توسط برنامه انجام می‌دهد:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Closure;

class AfterMiddleware
{
    public function handle($request, Closure $next)
    {
        $response = $next($request);

        // Perform action

        return $response;
    }
}
</code></pre>

<p>
<a name="global-middleware"></a>
</p>
<br>
<h3>ثبت middleware بصورت Global</h3>

<p>
اگر می‌خواهید یک middleware بر روی هر درخواست HTTP در برنامه اجرا شود، می‌توانید به سادگی کلاس middleware را در خصوصت $middleware کلاس app/Http/Kernel.php لیست کنید.
</p>

<p>
<a name="assigning-middleware-to-routes"></a>
</p>
<br>
<h3>اختصاص middleware به مسیر در لاراول</h3>

<p>
اگر می‌خواهید middleware را به مسیرهای خاصی اختصاص دهید، ابتدا باید یک کلید به middleware در فایل app/Http/Kernel.php اختصاص دهید. در حالت پیش‌فرض، ویژگی $routeMiddleware  در این کلاس شامل ثبت middlewareهایی است که در درون لاراول قرار دارند. برای افزودن middleware خود، به سادگی آن را به این لیست اضافه کنید و یک کلید به آن اختصاص دهید. مثال زیر را در نظر بگیرید:
</p>

<pre><code class="language-php  line-numbers">// Within App\Http\Kernel Class...

protected $routeMiddleware = [
    'auth' => \Illuminate\Auth\Middleware\Authenticate::class,
    'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
    'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
    'can' => \Illuminate\Auth\Middleware\Authorize::class,
    'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
    'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
];
</code></pre>

<p>
 زمانی که middleware در هسته HTTP تعریف شده باشد، می‌توان از متد middleware برای اختصاص middleware به یک مسیر استفاده کرد:
</p>

<pre><code class="language-php  line-numbers">Route::get('admin/profile', function () {
    //
})->middleware('auth');
</code></pre>

<p>
همچنین می‌توان چند middleware را به مسیر مانند مثال زیر اختصاص داد:
</p>

<pre><code class="language-php  line-numbers">Route::get('/', function () {
    //
})->middleware('first', 'second');
</code></pre>

<p>
هنگام تعیین middleware، همچنین می‌توانید نام کامل کلاس موردنظر را منتقل کنید:
</p>

<pre><code class="language-php  line-numbers">use App\Http\Middleware\CheckAge;

Route::get('admin/profile', function () {
    //
})->middleware(CheckAge::class);
</code></pre>

<p>
<a name="middleware-groups"></a>
</p>
<br>
<h3>middleware گروه بندی شده در لاراول</h3>

<p>
گاهی اوقات ممکن است بخواهید چند middleware را با یک کلید واحد گروه بندی کنید تا به راحتی بتوانید آن‌ها را به مسیرها اختصاص دهید. این کار را با استفاده از خصوصیت $middlewareGroups در هسته HTTP می‌توان انجام داد.
</p>

<p>
علاوه بر middlewareهای پیش‌فرض لاراول،   web   و api دو گروه middleware هستند که شامل یک middleware مشترکند که ممکن است بخواهید آن‌ها را به web UI و API routes  اعمال کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * The application's route middleware groups.
 *
 * @var array
 */
protected $middlewareGroups = [
    'web' => [
        \App\Http\Middleware\EncryptCookies::class,
        \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
        \Illuminate\Session\Middleware\StartSession::class,
        \Illuminate\View\Middleware\ShareErrorsFromSession::class,
        \App\Http\Middleware\VerifyCsrfToken::class,
        \Illuminate\Routing\Middleware\SubstituteBindings::class,
    ],

    'api' => [
        'throttle:60,1',
        'auth:api',
    ],
];
</code></pre>

<p>
روش اختصاص middlewareهای گروه بندی شده به مسیرها و اکشن‌های کنترلر، مثل روش اختصاص middlewareهای تکی است. با استفاده از middlewareهای گروه بندی شده اختصاص چندین middleware به یک مسیر به راحتی انجام می‌گیرد.
</p>

<pre><code class="language-php  line-numbers">Route::get('/', function () {
    //
})->middleware('web');

Route::group(['middleware' => ['web']], function () {
    //
});
</code></pre>

<p>
گروه middleware web به صورت خودکار توسط RouteServiceProvider به فایل routes/web.php اعمال می‌شود.
</p>

<p>
<a name="middleware-parameters"></a>
</p>
<br>
<h3><a href="#middleware-parameters">middleware پارامترهای</a></h3>

<p>
همچنین middleware می‌تواند پارامترهای اضافی نیز دریافت کند. برای مثال، اگر برنامه قبل از انجام هر عملی خاصی از سوی کاربر احراز هویت شده، نیازمند مشخص کردن role مربوط به آن کاربر نیز باشد، بایستی یک middleware CheckRole  ایجاد کنید که نام role را به عنوان یک آرگومان دریافت می‌کند.
</p>

<p>
پارامترهای اضافی بعد از آرگومان $next به middleware منتقل می‌شوند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Closure;

class CheckRole
{
    /**
     * Handle the incoming request.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @param  string  $role
     * @return mixed
     */
    public function handle($request, Closure $next, $role)
    {
        if (! $request->user()->hasRole($role)) {
            // Redirect...
        }

        return $next($request);
    }

}
</code></pre>

<p>
برای مشخص کردن پارامتر middleware در هنگام تعیین مسیر، نام middleware و پارامترها با یک علامت : جدا می‌شوند. برای جدا کردن چند پارامتر باید از کاما استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">Route::put('post/{id}', function ($id) {
    //
})->middleware('role:editor');
</code></pre>

<p>
<a name="terminable-middleware"></a>
</p>
<br>
<h3><a href="#terminable-middleware">Middleware پایان‌ پذیر (Terminable Middleware)</a></h3>

<p>
گاهی اوقات بایستی بعضی از کارها توسط یک middleware پس از ارسال پاسخ HTTP به مرورگر، انجام شود. برای مثال، session middleware که به صورت ‌پیش‌فرض در لاراول قرار دارد، اطلاعات session را پس از ارسال پاسخ به مرورگر در حافظه می‌نویسد. اگر متد terminate را در middleware خود تعریف کنید، به صورت خودکار پس از ارسال پاسخ به مرورگر آن را فراخوانی می‌کند.
</p>

<pre><code class="language-php  line-numbers"><?php

namespace Illuminate\Session\Middleware;

use Closure;

class StartSession
{
    public function handle($request, Closure $next)
    {
        return $next($request);
    }

    public function terminate($request, $response)
    {
        // Store the session data...
    }
}
</code></pre>

<p>
متد terminate باید هم درخواست و هم پاسخ را دریافت کند. زمانی که یک middleware پایان ‌پذیر را تعریف می‌کنید، باید آن را به لیست مسیر یا middlewareهای عمومی در فایل app/Http/Kernel.php اضافه کنید.
</p>

<p>
هنگام فراخوانی متد terminate  در middleware، لاراول یک نمونه از middleware را در service container بایند می‌کند. زمانی که متدهای handle و terminate فراخوانی می‌شوند، اگر بخواهید از یک middleware مشترک استفاده کنید، باید middleware را در  container با استفاده از متد singleton ثبت کنید.
</p>