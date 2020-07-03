---
layout: documentation-laravel
title:   تولید URL در لاراول
cattitle: اصول آموزش Laravel
date:   2018-02-11 09:38:42 +0330
jdate: یکشنبه 22 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/urls <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/255/تولید-url-لاراول-5-5
---
<p>
لاراول چند تابع کمکی (helper) برای تولید URLها در برنامه در اختیار ما قرار داده است. البته، استفاده از این توابع عمدتا در زمان ایجاد لینک‌ها‌ در قالب‌های HTML و پاسخ‌های API، یا در زمان ایجاد پاسخ‌های redirect شده به قسمت‌های دیگر برنامه، مفید خواهد بود.
</p>
<p>
<a name="the-basics"></a>
</p>

<br>
<h3><a href="#the-basics">ایجاد URLهای اصلی</a></h3>
<p>
<a name="generating-basic-urls"></a>
</p>

<br>
<h3>Generating Basic URLs</h3>
<p>
helper یا تابع کمکی url برای ایجاد URLهای دلخواه در برنامه استفاده می‌شود. URL ایجاد شده به صورت خودکار از طرح HTTP یا HTTPS استفاده می‌کند و از درخواست فعلی میزبانی می‌کند:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

echo url("/posts/{$post->id}");

// http://example.com/posts/1
</code></pre>

<p>
<a name="accessing-the-current-url"></a>
</p>

<br>
<h3>دسترسی به اطلاعات URL فعلی</h3>
<p>
اگر هیچ مسیری برای تابع کمکی url وجود نداشته باشد، یک نمونه کلاس Illuminate\Routing\UrlGenerator بازگردانده می‌شود، که امکان می‌دهد به اطلاعات مربوط به URL فعلی دسترسی داشته باشیم:
</p>

<pre><code class="language-php  line-numbers">// Get the current URL without the query string...
echo url()->current();

// Get the current URL including the query string...
echo url()->full();

// Get the full URL for the previous request...
echo url()->previous();
</code></pre>

<p>
همچنین، هر یک از این متدها می‌تواند از طریق fasade URL نیز قابل دسترسی باشد:
</p>

<pre><code class="language-php  line-numbers">echo URL::current();
</code></pre>

<p>
<a name="urls-for-named-routes"></a>
</p>

<br>
<h3><a href="#urls-for-named-routes">URLهای مسیرهای نامگذاری شده</a></h3>
<p>
برای تولید URL به مسیرهای نامگذاری شده، می‌توان از تابع کمکی route استفاده کرد. مسیرهای نامگذاری شده به شما امکان می‌دهد که URLها را بدون اتصال به URL واقعی تعیین شده در مسیر، ایجاد کنید. بنابراین، اگر URL مسیر تغییر کند، هیچ تغییری در فراخوانی‌ تابع route صورت نمی‌گیرد. برای مثال، فرض کنید برنامه دارای یک مسیر تعریف شده مانند مثال زیر است:
</p>

<pre><code class="language-php  line-numbers">Route::get('/post/{post}', function () {
    //
})->name('post.show');
</code></pre>

<p>
برای تولید یک URL برای این مسیر، می‌توانید از تابع کمکی route مانند مثال زیر استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">echo route('post.show', ['post' => 1]);

// http://example.com/post/1
</code></pre>

<p>
اغلب URLها را می‌توان با استفاده از کلید اولیه مدل‌های Eloquent ایجاد کرد. به همین دلیل، می‌توان مدل‌های Eloquent را به عنوان مقادیر پارامتر به تابع کمکی انتقال داد. تابع کمکی route به صورت خودکار کلید اصلی در مدل را استخراج می‌کند:
</p>

<pre><code class="language-php  line-numbers">echo route('post.show', ['post' => $post]);
</code></pre>

<p>
<a name="urls-for-controller-actions"></a>
</p>

<br>
<h3><a href="#urls-for-controller-actions">تولید URLها برای اکشن‌های کنترلر </a></h3>
<p>
از تابع action می‌توان جهت تولید URL برای یک اکشن کنترلر استفاده کرد. در این صورت، لازم نیست فضای نامی کامل کنترلر را انتقال داد. به جای آن، می توان نام کلاس کنترلر را نسبت به فضای نامی App\Http\Controllers انتقال داد:
</p>

<pre><code class="language-php  line-numbers">$url = action('HomeController@index');
</code></pre>

<p>
اگر متد کنترلر پارامترهای مسیر را می‌پذیرد، می‌توانید آن‌ها را به عنوان آرگومان دوم به این تابع انتقال دهید:
</p>

<pre><code class="language-php  line-numbers">$url = action('UserController@profile', ['id' => 1]);
</code></pre>

<p>
<a name="default-values"></a>
</p>

<br>
<h3><a href="#default-values">انتقال مقادیر  پیش‌فرض به URL </a></h3>
<p>
برای برخی از برنامه‌ها، ممکن است بخواهید مقادیر پیش‌فرض درخواستی را برای پارامترهای خاص URL تعیین کنید. برای مثال، فرض کنید بسیاری از مسیرها دارای یک پارامتر  {locale} باشند:
</p>

<pre><code class="language-php  line-numbers">Route::get('/{locale}/posts', function () {
    //
})->name('post.index');
</code></pre>

<p>
زمانی که تابع کمکی route را فراخوانی می‌کنید، انتقال locale ممکن است کمی کار را دشوار کند. بنابراین، می‌توانید از متد URL::defaults برای تعریف یک مقدار پیش‌فرض برای این پارامتر که همیشه در طول درخواست فعلی اعمال می‌شود، استفاده کنید. می‌توانید این متد را از middleware مسیر فراخوانی کنید تا بتوانید به درخواست فعلی دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Closure;
use Illuminate\Support\Facades\URL;

class SetDefaultLocaleForUrls
{
    public function handle($request, Closure $next)
    {
        URL::defaults(['locale' => $request->user()->locale]);

        return $next($request);
    }
}
</code></pre>

<p>
زمانی که مقدار پیش‌فرض برای پارامتر locale تنظیم شده باشد، دیگر نیازی به انتقال مقدار آن در زمان ایجاد URLها از طریق تابع کمکی route نیست.
</p>