---
layout: documentation
title:   حفاظت CSRF
cattitle: اصول آموزش Laravel
date:   2018-02-02 10:13:42 +0330
jdate: جمعه 13 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/csrf <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/225/حفاظت-csrf-لاراول
---
<p>
به حملاتی که از طریق درخواست‌های متقابل جعلی به سایت صورت می‌گیرد، CSRF گفته می‌شود. ان عبارت مخفف عبارت cross-site request forgery است. لاراول یک روش آسان برای حفاظت در برابر این حملات ارائه می‌دهد. درخواست‌های متقابل جعلی نوعی سوءاستفاده مخرب است که به موجب آن هکرها دستورات غیرمجاز را از طریق کاربر لاگین شده اجرا می‌کنند. در این در از آموزش لاراوال 5.5 با هم این قابلیت لاراول را بررسی می‌کنیم.
</p>

<p>
هر کاربر فعال که توسط برنامه مدیریت می‌شود، یک سشن دارد. لاراول به صورت خودکار یک نشانه CSRF برای هر سشن تولید می‌کند. از این نشانه برای شناسایی کاربر لاگین شده‌ که درخواست‌ها را به صورت واقعی به برنامه می‌فرستد، استفاده می‌کنیم.
</p>

<p>
هر زمان که یک فرم HTML را در برنامه خود تعریف می‌کنید، باید فیلد پنهان CSRF را در درون فرم قرار دهید تا middleware مربوط به حفاظت CSRF بتواند درخواست کاربر را تایید کند. برای تولید فیلد نشانه می‌توان از تابع کمکی csrf_field به صورت زیر استفاده کرد:
</p>

```html
<form method="POST" action="/profile">
    {% raw %}{{ csrf_field() }}{% endraw %}
    ...
</form>
```

<p>
middlewareVerifyCsrfToken، که در گروه middleware web قرار دارد، به صورت خودکار نشانه‌ی موجود در درخواست ورودی برنامه را با نشانه ذخیره شده در سشن (session) کاربر مقایسه می‌کند تا مشخص کند که با هم مطابقت دارند یا خیر.
</p>

<br>
<h3>توکن CSRF و جاوا اسکریپت</h3>

<p>
در هنگام ایجاد برنامه‌های کاربردی تحت جاوا اسکریپت، برای راحتی کار، به صورت خودکار کتابخانه HTTP جاوا اسکریپت نشانه CSRF را برای هر درخواست خروجی پیوست می‌کند. در حالت پیش‌فرض، فایل resources/assets/js/bootstrap.js مقدار تگ متا csrf-token را با کتابخانه Axisse HTTP ثبت می‌کند. اگر از این کتابخانه در برنامه خود استفاده نمی‌کنید، باید این کار را به صورت دستی برای برنامه خود پیکربندی کنید.
</p>


<p>
<a name="csrf-excluding-uris"></a>
</p>

<br>
<h3><a href="#csrf-excluding-uris">حذف برخی از URIها از حفاظت CSRF در لاراول</a></h3>

<p>
گاهی اوقات ممکن است بخواهید مجموعه‌ای از URIها را از حفاظت CSRF حذف کنید. برای مثال، اگر از روش Stripe برای پردازش پرداخت‌ها استفاده می‌کنید و همچنین از سیستم webhook نیز بهره می‌برید، باید مسیر Stripe webhook handler خود را از حفاظت CSRF حذف کنید، زیرا Stripe نمی‌داند که کدام نشانه CSRF برای مسیرهای شما ارسال می‌شود.
</p>

<p>
این نوع مسیرها را باید خارج از گروه middlewareweb  قرار دهید که RouteServiceProvider در تمام مسیرها در فایل routes/web.php اعمال می‌کند. می‌توانید مسیرها را با اضافه کردن URIهای خود به خصوصیت $except به middleware VerifyCsrfToken حذف کنید:
</p>

<pre><code class="language-php  line-numbers">
<?php

namespace App\Http\Middleware;

use Illuminate\Foundation\Http\Middleware\VerifyCsrfToken as Middleware;

class VerifyCsrfToken extends Middleware
{
    /**
     * The URIs that should be excluded from CSRF verification.
     *
     * @var array
     */
    protected $except = [
        'stripe/*',
        'http://example.com/foo/bar',
        'http://example.com/foo/*',
    ];
}
</code></pre>

<p>
<a name="csrf-x-csrf-token"></a>
</p>

<br>
<h3><a href="#csrf-x-csrf-token">در لاراول X-CSRF-TOKEN</a></h3>

<p>
middlewareVerifyCsrfToken  علاوه بر چک کردن نشانه CSRF به عنوان پارامتر POST، همچنین هدر درخواست X-CSRF-TOKEN  را بررسی می‌کند. می‌توانید نشانه را در یک تگ meta مربوط به HTML ذخیره کنید:
</p>

```html
<meta name="csrf-token" content="{% raw %}{{ csrf_token() }}{% endraw %}">
```

<p>
زمانی که تگ meta را ایجاد کردید، می‌توانید به یک کتابخانه مانند jQuery بگویید که به صورت خودکار نشانه‌ها را به تمام هدرهای درخواست اضافه کند. این روش، یک محافظت ساده و روان CSRF برای برنامه‌های مبتنی بر AJAX فراهم می‌‌کند:
</p>


<pre><code class="language-php  line-numbers">$.ajaxSetup({
    headers: {
        'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content')
    }
});
</code></pre>
<blockquote class="has-icon tip">

<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> By default, the <code class=" language-php">resources<span class="token operator">/</span>assets<span class="token operator">/</span>js<span class="token operator">/</span>bootstrap<span class="token punctuation">.</span>js</code> file registers the value of the <code class=" language-php">csrf<span class="token operator">-</span>token</code> meta tag with the Axios HTTP library. If you are not using this library, you will need to manually configure this behavior for your application.
</p>

</blockquote>

<p>
<a name="csrf-x-xsrf-token"></a>
</p>

<br>
<h3><a href="#csrf-x-xsrf-token">در لاراول X-XSRF-TOKEN</a></h3>

<p>
لاراول نشانه CSRF فعلی را در کوکی XSRF-TOKEN ذخیره می‌کند که هر پاسخی که توسط فریم ورک تولید شده است را شامل می‌شود. می‌توانید از مقدار کوکی برای تنظیم هدر درخواست X-XSRF-TOKEN استفاده کنید.
</p>


<p>
برخی از کتابخانه‌ها و فریم ورک‌های جاوا اسکریپت، مانند Angular و Axios، این مقدار را به صورت خودکار در هدر X-XSRF-TOKEN قرار می‌دهند.
</p>
