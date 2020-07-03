---
layout: documentation-laravel
title:   اصول requests
cattitle: اصول آموزش Laravel
date:   2018-02-03 10:40:42 +0330
jdate: شنبه 14 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/requests <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/239/درخواست-http-لاراول
---
<p>
برای دسترسی به نمونه‌ای از درخواست HTTP فعلی در لاراول از طریق روش تزریق وابستگی (dependency injection)، باید نمونه کلاس Illuminate\Http\Request را در متد کنترلر خود اعلان نوع یا type-hint کنید. درخواست ورودی به صورت خودکار توسط service container تزریق می‌شود:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Store a new user.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $name = $request->input('name');

        //
    }
}
</code></pre>


<br>
<h3>تزریق وابستگی و استفاده از پارامترهای مسیر</h3>

<p>
در صورتی که، متد کنترلر در انتظار دریافت ورودی از یک پارامتر مسیر باشد، می‌توانید پارامترهای مسیر را پس از وابستگی‌های دیگر لیست کنید. برای مثال، اگر مسیر شما مانند مثال زیر تعریف شده باشد:
</p>


<pre><code class="language-php  line-numbers">Route::put('user/{id}', 'UserController@update');
</code></pre>


<p>
می‌توانید نمونه کلاس Illuminate\Http\Request را اعلان نوع کنید و با تعریف متد کنترلر به صورت مثال زیر به پارامتر مسیر یا همان id ، دسترسی داشته باشید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Update the specified user.
     *
     * @param  Request  $request
     * @param  string  $id
     * @return Response
     */
    public function update(Request $request, $id)
    {
        //
    }
}
</code></pre>


<br>
<h3>دسترسی به درخواست HTTP  از طریق route closure</h3>

<p>
همچنین، می‌توانید نمونه کلاس Illuminate\Http\Request را در route closure اعلان نوع کنید. service container به صورت خودکار درخواست ورودی را به Closure در هنگام اجرای آن تزریق می‌کند:
</p>


<pre><code class="language-php  line-numbers">use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
</code></pre>


<p>
<a name="request-path-and-method"></a>
</p>


<br>
<h3>به کارگیری متد‌ها و مسیر در درخواست HTTP </h3>

<p>
جهت بررسی درخواست HTTP در برنامه، نمونه کلاس Illuminate\Http\Request چندین متد ارائه می‌دهد و از کلاس Symfony\Component\HttpFoundation\Request ارث بری می‌کند. در ادامه، برخی از مهمترین متدهای این کلاس را با هم بررسی می‌کنیم.
</p>


<br>
<h3>بازیابی مسیر درخواست‌ها HTTP </h3>

<p>
متد path اطلاعات مربوط به مسیر یک درخواست را بازمی‌گرداند. بنابراین، اگر درخواست ورودی به آدرس http://domain.com/foo/bar ارسال شود، این متد قسمت foo/bar را بازمی‌گرداند:
</p>


<pre><code class="language-php  line-numbers">$uri = $request->path();
</code></pre>


<p>
توسط متد is می‌توانید مطمئن شوید که مسیر درخواست ورودی مطابق با یک الگوی خاص تعریف شده است یا خیر. در هنگام استفاده از این متد می‌توان کاراکتر * را نیز به عنوان یک wildcard بکار برد:
</p>


<pre><code class="language-php  line-numbers">if ($request->is('admin/*')) {
    //
}
</code></pre>


<br>
<h3>دریافت URL درخواست‌ها </h3>

<p>
برای بازیابی URL کامل یک درخواست ورودی، می‌توانید متد url یا fullUrl را بکار ببرید. متد url برای بازیابی URL بدون رشته پرس و جو بکار می‌رود، در حالی که متد fullUrl رشته پرس و جو را نیز شامل می‌شود.
</p>


<pre><code class="language-php  line-numbers">// Without Query String...
$url = $request->url();

// With Query String...
$url = $request->fullUrl();
</code></pre>


<br>
<h3>بازیابی متد درخواست‌ها </h3>

<p>
متد method فعل یا متد درخواست HTTP را برای یک درخواست بازگشت می‌دهد. می‌توانید از متد isMethod برای بررسی اینکه متد درخواست HTTP با یک رشته مشخص مطابق است یا خیر، استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$method = $request->method();

if ($request->isMethod('post')) {
    //
}
</code></pre>


<p>
<a name="psr7-requests"></a>
</p>


<br>
<h3>استفاده از درخواست‌های PSR-7</h3>

<p>
استاندارد PSR-7 رابط‌هایی را برای پیغام‌های HTTP مانند درخواست‌ها و پاسخ‌ها ارائه می‌دهد. اگر می‌خواهید به نمونه‌ای از درخواست PSR-7 به جای یک درخواست لاراول دسترسی داشته باشید، در ابتدا بایستی چند کتابخانه مهم را نصب کنید. لاراول از کامپوننت Symfony HTTP Message Bridge برای تبدیل درخواست‌ها و پاسخ‌های لاراول به پیاده سازی‌های سازگار با PSR-7 استفاده می‌کند:
</p>


<pre><code class="language-php  line-numbers">composer require symfony/psr-http-message-bridge
composer require zendframework/zend-diactoros
</code></pre>


<p>
زمانی که این کتابخانه‌ها را نصب کردید، می‌توانید با type-hint کردن رابط درخواست درroute Closure یا متد کنترلر خود به یک درخواست PSR-7 دسترسی داشته باشید:
</p>


<pre><code class="language-php  line-numbers">use Psr\Http\Message\ServerRequestInterface;

Route::get('/', function (ServerRequestInterface $request) {
    //
});
</code></pre>

<blockquote class="has-icon tip">
 اگر نمونه‌ای از پاسخ PSR-7 از یک مسیر یا کنترلر برگردانده شود، این پاسخ به صورت خودکار به یک پاسخ لاراول تبدیل می‌شود و توسط فریم ورک نمایش داده می‌شود.
</blockquote>

<p>
<a name="input-trimming-and-normalization"></a>
</p>


<br>
<h3><a href="#input-trimming-and-normalization">مرتب سازی ورودی درخواست HTTP</a></h3>

<p>
به صورت پیش‌فرض middlewareهای TrimStrings و ConvertEmptyStringsToNull در پشته middleware عمومی برنامه لاراول قرار دارند. این middlewareها توسط کلاس App\Http\Kernel در پشته لیست شده‌اند. این middlewareها به صورت خودکار تمام فیلدهای رشته ورودی بر روی درخواست را مرتب می‌کنند، همچنین هر فیلد رشته خالی را به null تبدیل می‌کنند. این موضوع باعث می‌شود تا هیچ نگرانی در مورد کارهای کوچک و عادی از این قبیل، در مسیرها و کنترلرهای خود نداشته نباشیم.
</p>


<p>
اگر می‌خواهید این ویژگی را در برنامه خود غیرفعال کنید، می‌توانید این دو middleware را از پشته middleware برنامه خود حذف کنید، می‌توانید این کار را با حذف آن‌ها از خصوصیت $middleware کلاس App\Http\Kernel انجام دهید.
</p>


<p>
<a name="retrieving-input"></a>
</p>


<br>
<h3><a href="#retrieving-input">دريافت ورودی درخواست در لاراول</a></h3>

<h3>بازیابی تمام داده‌های ورودی درخواست</h3>

<p>
می‌توانید تمام داده‌های ورودی را به عنوان یک array با استفاده از متد all بازیابی کنید:
</p>


<pre><code class="language-php  line-numbers">$input = $request->all();
</code></pre>


<br>
<h3>بازیابی مقدار ورودی درخواست </h3>

<p>
با استفاده از چند متد ساده، می‌توانید به تمام ورودی‌های کاربر از نمونه کلاس Illuminate\Http\Request دسترسی داشته باشید و نگران اینکه کدام متد HTTP برای درخواست استفاده شود، نباشید. صرف نظر از متد درخواست HTTP، از متد input نیز می‌توانید برای بازیابی ورودی کاربر استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('name');
</code></pre>


<p>
می‌توانید یک مقدار پیش‌فرض را به عنوان آرگومان دوم به متد input منتقل کنید. اگر مقدار ورودی در درخواست کاربر موجود نباشد، این مقدار بازگردانده خواهد شد:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('name', 'Sally');
</code></pre>


<p>
در هنگام کار با فرم‌هایی که ورودی‌های آن‌ها به صورت آرایه‌ای هستند، می‌توانید از علامت «نقطه» برای دسترسی به آرایه‌ها استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('products.0.name');

$names = $request->input('products.*.name');
</code></pre>


<br>
<h3>گرفتن ورودی از رشته پرس و جو درخواست</h3>

<p>
در حالی که متد input مقادیر را از کل درخواست کاربر (شامل رشته پرس و جو) بازیابی می‌کند، متد query ، مقادیر را فقط از رشته پرس و جو بازیابی می‌کند:
</p>


<pre><code class="language-php  line-numbers">$name = $request->query('name');
</code></pre>


<p>
اگر مقدار داده رشته پرس و جو درخواست شده موجود نباشد، آرگومان دوم این متد بازگشت داده می‌شود:
</p>


<pre><code class="language-php  line-numbers">$name = $request->query('name', 'Helen');
</code></pre>


<p>
می‌توانید متد query را بدون هیچ آرگومانی به منظور بازیابی تمام مقادیر رشته پرس و جو به عنوان یک آرایه انجمنی فراخوانی کنید:
</p>


<pre><code class="language-php  line-numbers">$query = $request->query();
</code></pre>


<br>
<h3>دریافت ورودی درخواست از طریق خصوصیات پویا </h3>

<p>
همچنین، می‌توانید به ورودی کاربر با استفاده از خواص پویا بر روی نمونه کلاس Illuminate\Http\Request دسترسی داشته باشید. برای مثال، اگر یکی از فرم‌های درخواست، شامل یک فیلد name باشد، می‌توانید به مقدار فیلد مانند مثال زیر دسترسی داشته باشید:
</p>


<pre><code class="language-php  line-numbers">$name = $request->name;
</code></pre>


<p>
هنگام استفاده از خواص پویا، لاراول ابتدا مقدار پارامتر را در کل درخواست جستجو می‌کند. اگر مقدار پارامتر موجود نباشد، لاراول مقدار پارامتر را در پارامترهای مسیر جستجو می‌کند.
</p>


<br>
<h3>بازیابی مقادیر ورودی JSON در درخواست لاراول</h3>

<p>
هنگام ارسال درخواست‌های JSON به برنامه، تا زمانی که هدر Content-Type درخواست به درستی بر روی application/json تنظیم شده باشد، می‌توانید از طریق متد input به داده‌های JSON دسترسی داشته باشید. حتی می‌توانید از علامت «نقطه» برای کاوش در آرایه‌های JSON استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('user.name');
</code></pre>


<br>
<h3>بازیابی بخش‌هایی از داده‌های ورودی </h3>

<p>
اگر نیاز به بازیابی زیرمجموعه‌ای از داده‌های ورودی دارید، می‌توانید از متدهای only و except استفاده کنید. هر دو متد، یک array یا یک لیست پویا از آرگومان‌ها را می‌پذیرند.
</p>


<pre><code class="language-php  line-numbers">$input = $request->only(['username', 'password']);

$input = $request->only('username', 'password');

$input = $request->except(['credit_card']);

$input = $request->except('credit_card');
</code></pre>

<blockquote class="has-icon tip">
 متد only جفت كليد و مقداری که درخواست مي‌كنيد را برمی‌گرداند. با این وجود، اگر کلید و مقداری در درخواست موجود نباشد، برگردانده نمی‌شوند.
</blockquote>

<br>
<h3>تعیین مقدار ورودی درخواست در صورت وجود</h3>

<p>
از متد has برای تعیین اینکه آیا یک مقدار در درخواست موجود است یا خیر، استفاده می‌شود. اگر مقدار در یک درخواست موجود باشد، این متد مقدار true را برمی‌گرداند:
</p>


<pre><code class="language-php  line-numbers">if ($request->has('name')) {
    //
}
</code></pre>


<p>
زمانی که از یک آرایه استفاده می‌شود، متد has برای تعیین اینکه تمام مقادیر مشخص شده در آرایه موجود هستند یا خیر، استفاده می‌شود:
</p>


<pre><code class="language-php  line-numbers">if ($request->has(['name', 'email'])) {
    //
}
</code></pre>


<p>
می‌توانید از متد filled برای تعیین اینکه یک مقدار در درخواست موجود است و خالی نیست، استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">if ($request->filled('name')) {
    //
}
</code></pre>


<p>
<a name="old-input"></a>
</p>


<br>
<h3>حفظ ورودی‌های قبلی درخواست در لاراول</h3>

<p>
لاراول این امکان را فراهم می‌کند که ورودی یک درخواست را در طول درخواست بعدی حفظ کنید. این ویژگی برای بارگزاری مجدد فرم‌ها با مقادیر قبلی، پس از تشخیص خطاهای اعتبار سنجی بسیار مفید است. با این حال، اگر از ویژگی‌های اعتبار سنجی موجود در لاراول استفاده می‌کنید، لزومی ندارد که از این متدها به صورت دستی استفاده کنید، زیرا برخی از امکانات اعتبار سنجی از پیش ساخته شده در لاراول این متدها را به صورت خودکار فراخوانی می‌کنند.
</p>


<br>
<h3>اضافه کردن ورودی درخواست به session در لاراول</h3>

<p>
متد flash در نمونه کلاس Illuminate\Http\Request ورودی فعلی را به صورت فوری در session قرار می‌دهد، به طوری که این ورودی در طول درخواست بعدی کاربر به برنامه نیز در دسترس باشد:
</p>


<pre><code class="language-php  line-numbers">$request->flash();
</code></pre>


<p>
همچنین، برای قرار دادن زیرمجموعه‌ای از داده‌های درخواست در session می‌توانید از متدهای flashOnly و flashExcept استفاده کنید. این متدها برای حفظ اطلاعات حساسی مانند کلمات عبور در session مفید هستند.
</p>


<pre><code class="language-php  line-numbers">$request->flashOnly(['username', 'email']);

$request->flashExcept('password');
</code></pre>


<br>
<h3>قرار دادن ورودی درخواست در session و تغییر مسیر (redirect) </h3>

<p>
از آنجا که اغلب ورودی در session قرار داده می‌شود و سپس تغییر مسیر (redirect) به صفحه قبل انجام می‌گیرد، می‌توانید با استفاده از متد withInput قرار دادن ورودی در sessionرا به تغییر مسیر یا redirect متصل کنید:
</p>


<pre><code class="language-php  line-numbers">return redirect('form')->withInput();

return redirect('form')->withInput(
    $request->except('password')
);
</code></pre>


<br>
<h3>بازیابی ورودی قبلی قرار داده شده در session</h3>

<p>
برای بازیابی داده‌های قرار داده شده در session از درخواست قبلی، از متد old بر روی نمونه کلاس Request استفاده می‌شود. متد old داده‌های ورودی که قبلا در session قرار گرفته‌اند را از session بازیابی می‌کند:
</p>


<pre><code class="language-php  line-numbers">$username = $request->old('username');
</code></pre>


<p>
همچنین، لاراول یک تابع کمکی old عمومی ارائه می‌دهد. اگر ورودی‌های قبلی را در یک قالب Blade نمایش می‌دهید، به راحتی می‌توانید از تابع کمکی old استفاده کنید. اگر هیچ ورودی قبلی برای فیلد داده موجود نباشد، مقدار null بازگشت داده خواهد شد:
</p>


```html
<input type="text" name="username" value="{% raw %}{{ old('username') }}{% endraw %}">
```


<p>
<a name="cookies"></a>
</p>


<br>
<h3>کوکی‌ها و درخواست‌ها در لاراول</h3>

<br>
<h3>بازیابی کوکی‌ها از درخواست‌ها </h3>

<p>
تمام کوکی‌هایی که توسط فریم ورک لاراول ایجاد می‌شوند، رمزگذاری شده و توسط یک کد تأیید اعتبار امضا می‌شوند، بنابراین، اگر کوکی‌ها توسط کلاینت تغییر کنند، نامعتبر تلقی خواهند شد. برای بازیابی مقدار کوکی از یک درخواست، می‌توان از متد cookie بر روی نمونه کلاس Illuminate\Http\Request استفاده کرد.
</p>


<pre><code class="language-php  line-numbers">$value = $request->cookie('name');
</code></pre>


<p>
همچنین، می‌توانید از facade Cookie  نیز برای دسترسی به مقادیر کوکی استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$value = Cookie::get('name');
</code></pre>


<br>
<h3>پیوست کوکی‌ها به پاسخ‌های HTTP در لاراول</h3>

<p>
می‌توانید یک کوکی را به یک پاسخ و نمونه خروجی کلاس Illuminate\Http\Response با استفاده از متد cookie پیوست کنید. در این صورت، باید اسم، مقدار و تعداد دقیقه‌هایی را که کوکی باید معتبر باشد، را به این متد انتقال دهید:
</p>


<pre><code class="language-php  line-numbers">return response('Hello World')->cookie(
    'name', 'value', $minutes
);
</code></pre>


<p>
متد cookie چند آرگومان دیگر را نیز می‌پذیرد که به نسبت کمتری استفاده می‌شوند.
</p>


<pre><code class="language-php  line-numbers">return response('Hello World')->cookie(
    'name', 'value', $minutes, $path, $domain, $secure, $httpOnly
);
</code></pre>


<p>
همچنین، می‌توانید از facade Cookie  برای صف بندی کوکی جهت پیوست به پاسخی که از برنامه خارج می‌شود، استفاده کنید. متد queue یک نمونه کلاس Cookie یا آرگومان‌های موردنیاز برای ایجاد یک نمونه کوکی را می‌پذیرد. این کوکی‌ها قبل از ارسال پاسخ خروجی به مرورگر به آن وصل می‌شوند:
</p>


<pre><code class="language-php  line-numbers">Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
</code></pre>


<br>
<h3>ایجاد نمونه کوکی برای پاسخ‌ها در لاراول</h3>

<p>
اگر می‌خواهید یک نمونه کلاس مانند Symfony\Component\HttpFoundation\Cookie ایجاد کنید که بتواند به یک مورد پاسخ در یک زمان دیگر وصل شود، می‌توانید از تابع کمکی cookie عمومی استفاده کنید. این کوکی دوباره به کلاینت ارسال نمی‌شود مگر آنکه به یک نمونه پاسخ وصل شود:
</p>


<pre><code class="language-php  line-numbers">$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
</code></pre>


<p>
<a name="files"></a>
</p>


<br>
<h3><a href="#files">فایل‌ها و درخواست‌ها در لاراول</a></h3>

<p>
<a name="retrieving-uploaded-files"></a>
</p>


<br>
<h3>بازیابی فایل‌های آپلود شده</h3>

<p>
می‌توانید به فایل‌های آپلود شده از نمونه کلاس Illuminate\Http\Request ، با استفاده از متد file یا با استفاده از خواص پویا دسترسی داشته باشید. متد file یک نمونه از کلاس Illuminate\Http\UploadedFile را برمی‌گرداند که از کلاس SplFileInfo مربوط به PHP ارث بری می‌کند و متدهای متعددی برای تعامل با فایل‌ها در اختیار ما قرار می‌دهد:
</p>


<pre><code class="language-php  line-numbers">$file = $request->file('photo');

$file = $request->photo;
</code></pre>


<p>
می‌توان با استفاده از متد hasFile وجود یک فایل در یک درخواست را مشخص کرد:
</p>


<pre><code class="language-php  line-numbers">if ($request->hasFile('photo')) {
    //
}
</code></pre>


<br>
<h3>تایید بارگزاری‌های موفق فایل‌ها</h3>

<p>
از طریق متد isValid علاوه بر چک کردن وجود فایل، می‌توان مشخص کرد که هیچ‌گونه مشکلی در آپلود فایل وجود ندارد:
</p>


<pre><code class="language-php  line-numbers">if ($request->file('photo')->isValid()) {
    //
}
</code></pre>


<br>
<h3>مسیرهای فایل‌ها و پسوندها</h3>

<p>
کلاس UploadedFile شامل متدهایی برای دسترسی به مسیر کامل و مناسب فایل و پسوند آن است. متد extension تلاش می‌کند، پسوند فایل را بر اساس محتویات داخل آن حدس بزند. این پسوند ممکن است از پسوندی که توسط کلاینت ارائه شده است، متفاوت باشد:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->path();

$extension = $request->photo->extension();
</code></pre>


<br>
<h3>استفاده از سایر متدهای فایل</h3>

<p>
انواع متدهای دیگری نیز در نمونه کلاس UploadedFile وجود دارد. می‌توانید مستندات API را برای کسب اطلاعات بیشتر در مورد این متدها بررسی کنید.
</p>


<p>
<a name="storing-uploaded-files"></a>
</p>


<br>
<h3>ذخیره فایل‌های آپلود شده</h3>

<p>
برای ذخیره یک فایل آپلود شده، معمولاً از یکی از فایل‌ سیستم‌های پیکربندی شده استفاده می‌شود. نمونه کلاس UploadedFile دارای یک متد store است که یک فایل آپلود شده را بر روی یکی از دیسک‌هایی که ممکن است مکانی در فایل سیستم محلی شما یا حتی یک مکان ذخیره سازی ابری مانند Amazon S3 باشد، ذخیره می‌کند.
</p>


<p>
متد store مسیری را که فایل باید در دایرکتوری ریشه فایل سیستم پیکربندی شده ذخیره ‌شود، می‌پذیرد. این مسیر نباید شامل یک نام فایل باشد، زیرا یک شناسه یا ID منحصر به فرد به صورت خودکار به عنوان نام فایل ایجاد می‌شود.
</p>


<p>
متد store همچنین یک آرگومان دوم که استفاده از آن اختیاری است را برای نام دیسکی که باید برای ذخیره فایل مورد استفاده قرار گیرد، می‌پذیرد. این متد مسیر فایل را نسبت به ریشه دیسک بازمی‌گرداند:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->store('images');

$path = $request->photo->store('images', 's3');
</code></pre>


<p>
اگر نمی‌خواهید نام فایل به صورت خودکار تولید شود، می‌توانید از متد storeAs که مسیر، نام فایل و نام دیسک را به عنوان آرگومان می‌پذیرد، استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->storeAs('images', 'filename.jpg');

$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
</code></pre>


<p>
<a name="configuring-trusted-proxies"></a>
</p>


<br>
<h3><a href="#configuring-trusted-proxies">پیکربندی پروکسی‌های معتبر (Trusted Proxies)</a></h3>

<p>
زمانی که برنامه‌های خود را پشت یک توازن بار اجرا می‌کنید که گواهینامه‌های TLS / SSL را متوقف می‌کنند، متوجه خواهید شد که برنامه شما گاهی لینک‌های HTTPS را ایجاد نمی‌کند. به طور معمول، این امر به دلیل این است که برنامه در حال بارگزاری ترافیک از یک متعادل کننده بار در پورت 80 است و ایجاد لینک‌های امن را فراموش می‌کند.
</p>


<p>
برای حل این مشکل، می‌توانید از middleware App\Http\Middleware\TrustProxies که به صورت پیش‌فرض در یک برنامه لاراول قرار دارد، استفاده کنید. این middleware به شما امکان می‌دهد تا بتوانید به سرعت متعادل کننده بار یا پروکسی‌هایی که باید توسط برنامه مورد اعتماد قرار گیرند را سفارشی کنید. پروکسی‌های مورد اعتماد باید به عنوان یک آرایه در خصوصیت $proxies این middleware لیست شوند. علاوه بر پیکربندی پروکسی‌های قابل اعتماد، می‌توانید هدرهایی را که توسط پروکسی ارسال می‌شوند با اطلاعات مربوط به درخواست اصلی را نیز پیکربندی کنید.
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Fideloper\Proxy\TrustProxies as Middleware;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array
     */
    protected $proxies = [
        '192.168.1.1',
        '192.168.1.2',
    ];

    /**
     * The current proxy header mappings.
     *
     * @var array
     */
    protected $headers = [
        Request::HEADER_FORWARDED => 'FORWARDED',
        Request::HEADER_X_FORWARDED_FOR => 'X_FORWARDED_FOR',
        Request::HEADER_X_FORWARDED_HOST => 'X_FORWARDED_HOST',
        Request::HEADER_X_FORWARDED_PORT => 'X_FORWARDED_PORT',
        Request::HEADER_X_FORWARDED_PROTO => 'X_FORWARDED_PROTO',
    ];
}
</code></pre>


<br>
<h3>ایجاد اعتماد به تمام پروکسی‌ها</h3>

<p>
اگر از Amazon AWS یا یکی دیگر از ارائه دهندگان تعادل بار ابری استفاده می کنید، ممکن است آدرس‌های IP متعادل کننده‌های واقعی را ندانید. در این صورت، می‌توانید از ** برای اعتماد به تمام پروکسی‌ها استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">/**
 * The trusted proxies for this application.
 *
 * @var array
 */
protected $proxies = '**';
</code></pre>