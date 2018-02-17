---
layout: documentation
title:   پاسخ Response
cattitle: اصول آموزش Laravel
date:   2018-02-10 21:26:42 +0330
jdate: شنبه 20 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/responses <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/244/ایجاد-پاسخ-http-در-لاراول
---
<h3>پاسخ نوع Strings &amp; Arrays</h3>

<p>
تمام مسیرها و کنترلرهای لاراول باید یک پاسخ HTTP ایجاد کنند که در مرحله آخر، این پاسخ به مرورگر کاربر ارسال می‌‌‌شود. لاراول روش‌های مختلفی را برای برگرداندن یک پاسخ فراهم کرده است. ساده‌ترین نوع یک پاسخ HTTP در لاراول، برگرداندن یک رشته از مسیر یا کنترلر موجود در برنامه است. فریم ورک به صورت خودکار رشته را به یک پاسخ HTTP کامل تبدیل می‌کند:
</p>


<pre><code class="language-php  line-numbers">Route::get('/', function () {
    return 'Hello World';
});
</code></pre>


<p>
علاوه بر برگرداندن رشته‌ها از مسیرها و کنترلرهای برنامه، آرایه‌ها هم می‌توانند به عنوان یک پاسخ به مرورگر کاربر ارسال شوند. فریم ورک به صورت خودکار آرایه‌ را به یک پاسخ JSON تبدیل می‌کند:
</p>


<pre><code class="language-php  line-numbers">Route::get('/', function () {
    return [1, 2, 3];
});
</code></pre>

<blockquote class="has-icon tip">
Eloquent collections نیز می‌توانند از مسیرها و كنترلرهای برنامه به عنوان یک پاسخ برگردانده شوند که به صورت خودکار توسط فریم ورک به JSON تبدیل می‌شوند.
</blockquote>

<br>
<h3>پاسخ نوع  Response Objects</h3>

<p>
در اکثر موارد، این تنها رشته‌ها و آرایه‌های ساده نیستند که از اکشن‌های مسیر در برنامه برگردانده می‌شوند، بلکه نمونه‌ کلاس Illuminate\Http\Response یا viewها نیز می‌توانند به عنوان یک پاسخ برگردانده شوند.
</p>


<p>
برگرداندن یک نمونه کلاس Response به صورت کامل، این امکان را ایجاد می‌کند که هدرها و کد وضعیت HTTP مربوط به پاسخ را به صورت سفارشی ایجاد کنید. نمونه کلاس Response از کلاس Symfony\Component\HttpFoundation\Response ارث می‌برند که متدهای مختلفی را برای ایجاد یک پاسخ HTTP در اختیار ما قرار می‌دهد:
</p>


<pre><code class="language-php  line-numbers">Route::get('home', function () {
    return response('Hello World', 200)
                  ->header('Content-Type', 'text/plain');
});
</code></pre>


<p>
<a name="attaching-headers-to-responses"></a>
</p>


<br>
<h3>الحاق هدرها به پاسخ‌های HTTP در لاراول</h3>

<p>
توجه کنید که اکثر متدهای پاسخ، به یکدیگر مرتبط هستند که امکان ایجاد ساختار واضحی از پاسخ‌ها را فراهم می‌کند. برای مثال، می‌توان از متد header برای اضافه کردن مجموعه‌ای از هدرها به یک پاسخ قبل از ارسال آن به مرورگر کاربر استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">return response($content)
            ->header('Content-Type', $type)
            ->header('X-Header-One', 'Header Value')
            ->header('X-Header-Two', 'Header Value');
</code></pre>


<p>
همچنین، می‌توان از متد withHeaders برای تعیین آرایه‌ای از هدرها، برای اضافه کردن به پاسخ استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">return response($content)
            ->withHeaders([
                'Content-Type' => $type,
                'X-Header-One' => 'Header Value',
                'X-Header-Two' => 'Header Value',
            ]);
</code></pre>


<p>
<a name="attaching-cookies-to-responses"></a>
</p>


<br>
<h3>الحاق کوکی‌ها به HTTP response در لاراول</h3>

<p>
متد cookie در نمونه کلاس response، الحاق کوکی‌ها به پاسخ‌ها را به سادگی امکان‌پذیر می‌سازد. برای مثال، می‌توان از متد cookie برای ایجاد یک کوکی استفاده کرد و به سادگی آن را به یک نمونه کلاس response به صورت مثال زیر الحاق کرد:
</p>


<pre><code class="language-php  line-numbers">return response($content)
                ->header('Content-Type', $type)
                ->cookie('name', 'value', $minutes);
</code></pre>


<p>
متد cookie چند آرگومان دیگر را نیز می‌پذیرد که به نسبت کمتری استفاده می‌شوند.
</p>


<pre><code class="language-php  line-numbers">->cookie($name, $value, $minutes, $path, $domain, $secure, $httpOnly)
</code></pre>


<p>
همچنین، از facade مربوط به Cookie می‌توان برای صف بندی کوکی‌ها جهت الحاق به یک پاسخ خروجی از برنامه استفاده کرد. متد queue ، یک نمونه Cookie یا آرگومان‌های موردنیاز جهت ایجاد یک نمونه Cookie را می‌پذیرد. این کوکی‌ها به پاسخ خروجی، قبل از ارسال به مرورگر کاربر متصل می‌شوند:
</p>


<pre><code class="language-php  line-numbers">Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
</code></pre>


<p>
<a name="cookies-and-encryption"></a>
</p>


<br>
<h3>کوکی‌ها و رمزگذاری در لاراول Cookies &amp; Encryption</h3>

<p>
در حالت پیش‌فرض، تمام کوکی‌هایی که در لاراول ایجاد می‌شوند، رمزگذاری و امضا می‌شوند، بنابراین توسط کلاینت قابل تغییر یا خواندن نیستند. اگر می‌خواهید عملیات رمزگذاری را برای زیرمجموعه‌ای از کوکی‌هایی که توسط برنامه ایجاد شده‌اند غیرفعال کنید، می‌توانید از خصوصیت $except در middleware مربوط به رمزگذاری کوکی‌ها یا App\Http\Middleware\EncryptCookis که در دایرکتوری app/Http/Middleware قرار دارد، استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">/**
 * The names of the cookies that should not be encrypted.
 *
 * @var array
 */
protected $except = [
    'cookie_name',
];
</code></pre>


<p>
<a name="redirects"></a>
</p>


<br>
<h3><a href="#redirects">تغییر مسیرها (redirects) در لاراول</a></h3>

<p>
پاسخ‌های redirect، نمونه‌هایی از کلاس Illuminate\Http\RedirectResponse هستند که هدرهای موردنیاز مناسب جهت هدایت کاربر به یک URL دیگر را ارائه می‌دهند. چند متد برای ایجاد نمونه کلاس RedirectResponse وجود دارد. ساده‌ترین متد استفاده از تابع کمکی redirect عمومی به صورت زیر است:
</p>


<pre><code class="language-php  line-numbers">Route::get('dashboard', function () {
    return redirect('home/dashboard');
});
</code></pre>


<p>
گاهی اوقات، ممکن است بخواهید کاربر را به موقعیت قبلی‌ هدایت کنید، مثلاً زمانی که فرم ارسال شده به سرور نامعتبر است. این کار را می‌توان با استفاده از تابع کمکی back عمومی انجام داد. از آنجا که این ویژگی از session استفاده می‌کند، اطمینان حاصل کنید که مسیر فراخوانی تابع کمکی back از گروه middleware web  استفاده می‌کند یا اینکه تمام middlewareهای مربوط به session اعمال شده باشند:
</p>


<pre><code class="language-php  line-numbers">Route::post('user/profile', function () {
    // Validate the request...

    return back()->withInput();
});
</code></pre>


<p>
<a name="redirecting-named-routes"></a>
</p>


<br>
<h3>تغییر مسیر (redirect) به مسیرهای نامگذاری شده در لاراول</h3>

<p>
زمانی که تابع کمکی redirect بدون هیچ پارامتری فراخوانی می‌شود، نمونه ای از Illuminate\Routing\Redirector برگردانده می‌شود که این امکان را می‌دهد که بتوانید، هر متدی را در نمونه کلاس Redirector فراخوانی کنید. برای مثال، برای ایجاد RedirectResponse به یک مسیر نام‌گذاری شده در لاراول، می‌توانید از متد route استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">return redirect()->route('login');
</code></pre>


<p>
اگر مسیر موردنظر دارای پارامتر باشد، می‌توانید آن‌ها را به عنوان آرگومان دوم به متد route انتقال دهید:
</p>


<pre><code class="language-php  line-numbers">// For a route with the following URI: profile/{id}

return redirect()->route('profile', ['id' => 1]);
</code></pre>


<br>
<h3>پرکردن پارامترها از مدل‌های Eloquent در لاراول</h3>

<p>
برای هدایت به مسیری با پارامتر ID که داده‌‌های درون آن از طریق یک مدل Eloquent در آن قرار داده می‌شوند، به سادگی می‌توانید خود مدل را انتقال دهید. داده مربوط به فیلد ID به صورت خودکار از مدل استخراج می‌شود:
</p>


<pre><code class="language-php  line-numbers">// For a route with the following URI: profile/{id}

return redirect()->route('profile', [$user]);
</code></pre>


<p>
اگر می‌خواهید مقدار را در پارامتر مسیر قرار دهید، باید متد getRouteKey را در مدل Eloquent خود بازنویسی کنید:
</p>


<pre><code class="language-php  line-numbers">/**
 * Get the value of the model's route key.
 *
 * @return mixed
 */
public function getRouteKey()
{
    return $this->slug;
}
</code></pre>


<p>
<a name="redirecting-controller-actions"></a>
</p>


<br>
<h3>تغییر مسیر (redirect) به اکشن‌های کنترلر ل Redirecting To Controller Actions</h3>

<p>
می‌توانید تغییر مسیر به اکشن‌های کنترلر ایجاد کنید. برای انجام این کار، باید نام کنترلر و اکشن را به متد action انتقال دهید. توجه کنید که لازم نیست، فضای نامی کامل را برای کنترلر مشخص کنید؛ زیرا RouteServiceProvider در لاراول به صورت خودکار فضای نامی پایه کنترلر را مشخص می‌کند:
</p>


<pre><code class="language-php  line-numbers">return redirect()->action('HomeController@index');
</code></pre>


<p>
اگر مسیر کنترلر نیاز به انتقال پارامتر داشته باشد، می‌توانید آن‌ها را به عنوان آرگومان دوم به متد action انتقال دهید:
</p>


<pre><code class="language-php  line-numbers">return redirect()->action(
    'UserController@profile', ['id' => 1]
);
</code></pre>


<p>
<a name="redirecting-external-domains"></a>
</p>


<br>
<h3>تغییر مسیر (redirect) به دامنه‌های خارجی (Redirecting To External Domains)</h3>

<p>
گاهی ممکن است نیاز به تغییر مسیر به یک دامنه خارج از برنامه خود داشته باشید. این کار را می‌توان با فراخوانی متد away انجام داد که یک RedirectResponse را بدون رمزگذاری، اعتبار سنجی یا تأیید اضافی URL ایجاد می‌کند:
</p>


<pre><code class="language-php  line-numbers">return redirect()->away('https://www.google.com');
</code></pre>


<p>
<a name="redirecting-with-flashed-session-data"></a>
</p>


<br>
<h3>تغییر مسیر (redirect) به داده‌های قرار داده شده در session</h3>

<p>
تغییر مسیر به یک URL جدید و انتقال اطلاعات به session معمولاً به صورت همزمان انجام می‌گیرد. معمولاً این کار بعد از انجام موفقیت‌آمیز یک اکشن انجام می‌شود و یک پیغام موفقیت‌آمیز در session قرار داده می‌شود. برای راحتی کار، می‌توانید یک نمونه RedirectResponse ایجاد کنید و با اتصال متدها به یکدیگر، اطلاعات را در session قرار دهید:
</p>


<pre><code class="language-php  line-numbers">Route::post('user/profile', function () {
    // Update the user's profile...

    return redirect('dashboard')->with('status', 'Profile updated!');
});
</code></pre>


<p>
بعد از اینکه کاربر بهURL جدید هدایت شد، می‌توانید پیغام‌های قرار داده شده در session را نمایش دهید. این کار را می‌توان با استفاده از سینتکس Blade مانند مثال زیر انجام داد:
</p>

```html
@if (session('status'))
    <div class="alert alert-success">
        {% raw %}{{ session('status') }}{% endraw %}
    </div>
@endif
```


<p>
<a name="other-response-types"></a>
</p>


<br>
<h3>
<a href="#other-response-types">انواع پاسخ‌‌های دیگر در لاراول</a></h3>

<p>
از تابع کمکی response می‌توان برای ایجاد انواع دیگری از نمونه response استفاده کرد. زمانی که تابع کمکی response را بدون آرگومان فراخوانی می‌کنید، پیاده سازی قرارداد Illuminate\Contracts\Routing\ResponseFactory برگردانده می‌شود. این قرارداد چند متد مفید برای ایجاد پاسخ ارائه می‌دهد.
</p>


<p>
<a name="view-responses"></a>
</p>


<br>
<h3>پاسخ‌های View Responses</h3>

<p>
اگر نیاز به کنترل وضعیت و هدر یک پاسخ دارید و همچنین باید یک view را به عنوان محتوای پاسخ برگردانید، باید از متد view استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">return response()
            ->view('hello', $data, 200)
            ->header('Content-Type', $type);
</code></pre>


<p>
البته، اگر نیازی به انتقال کد وضعیت HTTP یا هدرهای سفارشی ندارید، باید از تابع کمکی view عمومی استفاده کنید.
</p>


<p>
<a name="json-responses"></a>
</p>


<br>
<h3>پاسخ‌های JSON Responses</h3>

<p>
متد json به صورت خودکار هدر Content-Type را به application/json تنظیم می‌کند و همچنین، با استفاده از تابع json_encode در PHP آرایه داده شده را به JSON تبدیل می‌کند:
</p>


<pre><code class="language-php  line-numbers">return response()->json([
    'name' => 'Abigail',
    'state' => 'CA'
]);
</code></pre>


<p>
اگر می‌خواهید یک پاسخ JSONP ایجاد کنید، می‌توانید از متد json با ترکیب متد withCallback استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">return response()
            ->json(['name' => 'Abigail', 'state' => 'CA'])
            ->withCallback($request->input('callback'));
</code></pre>


<p>
<a name="file-downloads"></a>
</p>


<br>
<h3>دانلود فایل File Downloads</h3>

<p>
متد download پاسخی ایجاد می‌کند که مرورگر کاربر را وادار به دانلود فایل در مسیر داده شده می‌کند. متد download نام فایل را به عنوان آرگومان دوم در متد می‌پذیرد که نام فایل مشاهده شده توسط کاربری که فایل را دانلود کرده است را مشخص می‌کند. در آخر، می‌توانید آرایه ای از هدرهای HTTP را به عنوان آرگومان سوم به متد انتقال دهید:
</p>


<pre><code class="language-php  line-numbers">return response()->download($pathToFile);

return response()->download($pathToFile, $name, $headers);

return response()->download($pathToFile)->deleteFileAfterSend(true);
</code></pre>

<blockquote class="has-icon note">
Symfony HttpFoundation، که دانلود فایل‌ها را مدیریت می‌کند، نیاز دارد که فایل دانلود شده دارای یک نام فایل به صورت ASCII باشد.
</blockquote>

<br>
<h3>پاسخ‌های Streamed Downloads</h3>

<p>
گاهی اوقات  ممکن است بخواهید پاسخ رشته یک عملیات داده شده را به یک پاسخ قابل بارگیری بدون نیاز به نوشتن محتویات عملیات روی دیسک تبدیل کنید. شما می توانید از روش streamDownload در این سناریو استفاده کنید. این روش یک callback، نام فایل و یک آرایه اختیاری از هدر ها را به عنوان استدلال های آن می پذیرد:
</p>


<pre><code class="language-php  line-numbers">return response()->streamDownload(function () {
    echo GitHub::api('repo')
                ->contents()
                ->readme('laravel', 'laravel')['contents']
}, 'laravel-readme.md');
</code></pre>


<p>
<a name="file-responses"></a>
</p>


<br>
<h3>پاسخ‌های فایل File Responses</h3>

<p>
برای نمایش یک فایل، مانند یک تصویر یا PDF به طور مستقیم در مرورگر کاربر به جای دانلود آن فایل، می‌توانید از متد file استفاده کنید. این متد مسیر فایل را به عنوان آرگومان اول و یک آرایه هدر را به عنوان آرگومان دوم می‌پذیرد:
</p>


<pre><code class="language-php  line-numbers">return response()->file($pathToFile);

return response()->file($pathToFile, $headers);
</code></pre>


<p>
<a name="response-macros"></a>
</p>


<br>
<h3>
<a href="#response-macros">ماکروهای پاسخ یا response در لاراول</a></h3>

<p>
اگر بخواهید یک پاسخ سفارشی ایجاد کنید که بتوانید استفاده مجدد از آن در بسیاری از مسیرها و کنترلرهای خود داشته باشید، می‌توانید از متد macro در facade مربوط به Response استفاده کنید. مثال زیر، تعریف ماکرو در متد boot از service provider را نشان می‌دهد:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Response;

class ResponseMacroServiceProvider extends ServiceProvider
{
    /**
     * Register the application's response macros.
     *
     * @return void
     */
    public function boot()
    {
        Response::macro('caps', function ($value) {
            return Response::make(strtoupper($value));
        });
    }
}
</code></pre>


<p>
تابع macro یک اسم به عنوان آرگومان اول و یک Closure به عنوان آرگومان دوم می‌پذیرد. در زمان فراخوانی نام ماکرو از پیاده‌سازی ResponseFactory یا تابع کمکی response این Closure مربوط به ماکرو اجرا می‌شود:
</p>


<pre><code class="language-php  line-numbers">return response()->caps('foo');
</code></pre>