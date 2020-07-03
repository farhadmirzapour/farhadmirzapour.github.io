---
layout: documentation-laravel
title:   Routing یا مسیریابی در لاراول
cattitle: اصول آموزش Laravel
date:   2018-01-28 20:38:42 +0330
jdate: یکشنبه 08 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/routing <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/206/مسیریابی-لاراول
---
<p>
مسیرهای پایه در لاراول یک URI و یک تابع Closure می‌پذیرند که یک متد بسیار ساده برای تعریف مسیرها ارائه می‌دهد:
</p>
<pre><code class="language-php  line-numbers">Route::get('foo', function () {
    return 'Hello World';
});
</code></pre>

<br>
<h3>فایل‌های route پیش‌فرض در لاراول</h3>
<p>
تمام مسیرهای لاراول در فایل‌های route که در دایرکتوری routes قرار می‌گیرند، تعریف شده‌ است. این فایل‌ها به صورت خودکار توسط فریم ورک بارگزاری می‌شوند. فایل routes/web.php مسیرهایی را برای رابط وب تعریف می‌کند. این مسیرها به گروه middlewareweb اختصاص داده می‌شوند که قابلیت‌هایی مانند حالت جلسه و حفاظت CSRF را ارائه می‌دهند. مسیرهای موجود در routes/api.php در گروه middleware api  قرار می‌گیرند.
</p>

<p>
دراکثر برنامه‌های کاربردی لاراول، بایستی کار خود را با تعریف مسیرها در فایل routes/web.php شروع کنید. می‌توان به مسیرهای تعریف شده در فایل routes/web.php با وارد کردن URL مسیر تعریف شده در مرورگر، دسترسی داشت. به عنوان مثال، اگر در مرورگر خود آدرس http://your-app.dev/user را وارد کنید، می‌توانید به مسیر زیر دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">Route::get('/user', 'UserController@index');
</code></pre>

<p>
مسیرهای تعریف شده در فایل routes/api.php توسط RouteServiceProvider در یک گروه مسیر قرار می‌گیرند. در این گروه، پیشوند /api به صورت خودکار به URL اعمال می‌شود، در این صورت، دیگر لازم نیست به صورت دستی آن را به مسیرهای درون فایل اعمال کنید. می‌توان پیشوند و گزینه‌های دیگر در گروه مسیر با ایجاد تغییر در کلاس RouteServiceProvider تنظیم کرد.
</p>

<br>
<h3>متدهای router قابل دسترس در مسیریابی لاراول</h3>
<p>
توسط روتر می‌توان مسیرهایی را که به هر درخواست از HTTP پاسخ می‌دهند را ثبت و تعریف کرد:
</p>

<pre><code class="language-php  line-numbers">Route::get($uri, $callback);
Route::post($uri, $callback);
Route::put($uri, $callback);
Route::patch($uri, $callback);
Route::delete($uri, $callback);
Route::options($uri, $callback);
</code></pre>

<p>
گاهی ممکن است لازم باشد مسیری را که به چندین درخواست HTTP پاسخ می‌دهد، ثبت کنید. می‌توان این کار را با استفاده از متد match انجام داد. حتی می‌توان با استفاده از متد any مسیری را ثبت کرد که به تمام درخواست‌های HTTP پاسخ می‌دهد:
</p>

<pre><code class="language-php  line-numbers">Route::match(['get', 'post'], '/', function () {
    //
});
Route::any('foo', function () {
    //
});
</code></pre>

<br>
<h3>در مسیریابی لاراول CSRF محافظت</h3>
<p>
هر فرم‌ HTML که به مسیرهای POST ، PUT یا DELETE که در فایل مسیرهای web تعریف شده‌اند، اشاره دارد، باید شامل یک فیلد نشانه CSRF باشند. در غیر این صورت، درخواست رد خواهد شد.
</p>

```html
<form method="POST" action="/profile">
    {% raw %}{{ csrf_field() }}{% endraw %}
    ...
</form>
```

<p>
<a name="redirect-routes"></a>
</p>

<br>
<h3>تغییر مسیر یا redirect route در لاراول</h3>
<p>
اگر مسیری را تعریف می‌کنید که باید به URI دیگری هدایت می‌شود، باید از متد Route::redirect استفاده کنید. این متد یک میانبر مناسب ارائه می‌دهد، به طوری که برای انجام یک تغییر مسیر (redirect) ساده دیگر نیازی به تعریف یک مسیر کامل یا یک کنترلر نخواهید داشت:
</p>

<pre><code class="language-php  line-numbers">Route::redirect('/here', '/there', 301);
</code></pre>

<p>
<a name="view-routes"></a>
</p>

<br>
<h3>مسیریابی در لاراول : مسیرهای view</h3>
<p>
اگر مسیر یا route نیاز به برگرداندن یک view داشته باشد، می‌توانید از متد Route::view استفاده کنید. درست مثل متد redirect ، این متد نیز یک میانبر ساده ارائه می‌دهد، بنابراین نباید یک مسیر کامل یا کنترلر تعریف شود. متد view یک URI به عنوان آرگومان اول و نام یک view را به عنوان آرگومان دوم دریافت می‌کند. علاوه بر این می‌توان آرایه‌ای از داده‌ها را برای انتقال به view به عنوان آرگومان سوم منتقل کنید که استفاده از این آرگومان اختیاری است:
</p>

<pre><code class="language-php  line-numbers">Route::view('/welcome', 'welcome');
Route::view('/welcome', 'welcome', ['name' => 'Taylor']);
</code></pre>

<p>
<a name="route-parameters"></a>
</p>

<br>
<h3><a href="#route-parameters">پارامترهای مسیر (route parameters) در مسیریابی لاراول</a></h3>
<p>
گاهی ممکن است نیاز به گرفتن بخش‌هایی از URI در مسیر خود داشته باشید. برای مثال، ممکن است نیاز به دریافت شناسه کاربر (id) از URL داشته باشید. این کار را می‌توان با تعیین پارامترهای مسیر انجام داد. به مثال زیر توجه کنید:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/{id}', function ($id) {
    return 'User '.$id;
});
</code></pre>

<p>
همچنین می‌توان چند پارامتر موردنیاز را به صورت زیر تعریف کرد:
</p>

<pre><code class="language-php  line-numbers">Route::get('posts/{post}/comments/{comment}', function ($postId, $commentId) {
    //
});
</code></pre>

<p>
پارامترهای مسیر در داخل براکت {} قرار می‌گیرند و باید از حروف الفبا تشکیل شده باشند و نباید حاوی کاراکتر ( - ) باشد. به جای استفاده از کاراکتر ( - ) می‌توان از کاراکتر زیرخط ( _ ) استفاده کرد. پارامترهای مسیر به callback / controllers مسیر براساس سفارش تزریق می‌شوند. نام آرگومان‌های callback / controller زیاد مهم نیست.
</p>

<p>
<a name="parameters-optional-parameters"></a>
</p>

<br>
<h3>پارامترهای اختیاری یا Optional در مسیریابی لاراول</h3>
<p>
گاهی ممکن است نیاز به تعیین پارامتر مسیر داشته باشید، اما استفاده از پارامتر مسیر را اختیاری کنید. این کار را می‌توان با قرار دادن یک علامت سوال ( ؟ ) بعد از نام پارامتر انجام داد:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/{name?}', function ($name = null) {
    return $name;
});
Route::get('user/{name?}', function ($name = 'John') {
    return $name;
});
</code></pre>

<p>
<a name="parameters-regular-expression-constraints"></a>
</p>

<br>
<h3>عبارات منظم (Regular Expression) در مسیریابی لاراول</h3>
<p>
می‌توان فرمت پارامترهای مسیر را با استفاده از متد where در یک نمونه مسیر تنظیم کرد. متد where نام پارامتر را دریافت می کند و عبارت منظمی که پارامتر بوسیله آن باید محدود شود را تعریف می‌کند :
</p>

<pre><code class="language-php  line-numbers">Route::get('user/{name}', function ($name) {
    //
})->where('name', '[A-Za-z]+');
Route::get('user/{id}', function ($id) {
    //
})->where('id', '[0-9]+');
Route::get('user/{id}/{name}', function ($id, $name) {
    //
})->where(['id' => '[0-9]+', 'name' => '[a-z]+']);
</code></pre>

<p>
<a name="parameters-global-constraints"></a>
</p>

<br>
<h3>استفاده از عبارات منظم به صورت عمومی در مسیریابی لاراول</h3>
<p>
اگر بخواهید یک پارامتر مسیر همواره با یک «عبارات منظم» خاص محدود شود، می‌توانید از متد pattern استفاده کنید. این الگوها را در متد boot مربوط به RouteServiceProvider خود تعریف کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Define your route model bindings, pattern filters, etc.
 *
 * @return void
 */
public function boot()
{
    Route::pattern('id', '[0-9]+');
    parent::boot();
}
</code></pre>

<p>
زمانی که الگوی خاصی تعریف شده باشد، با استفاده از نام پارامتر به صورت خودکار به تمام مسیرها اعمال می‌شود:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/{id}', function ($id) {
    // Only executed if {id} is numeric...
});
</code></pre>

<p>
<a name="named-routes"></a>
</p>

<br>
<h3><a href="#named-routes">مسیرهای نام‌گذاری شده در Routing </a></h3>
<p>
مسیرهای نامگذاری شده (Named routes) این امکان را می‌دهد تا به راحتی برای آن‌ها URL تولید کنید یا از آن‌ها برای redirect کردن در برنامه استفاده کنید. برای اختصاص یک نام به یک مسیر از متد name در تعریف مسیر به صورت زیر استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/profile', function () {
    //
})->name('profile');
</code></pre>

<p>
همچنین می‌توان نام مسیر را به اکشن‌های یک کنترلر به صورت زیر اختصاص داد:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/profile', 'UserController@showProfile')->name('profile');
</code></pre>

<br>
<h3>تولید URLها برای مسیرهای نامگذاری شده در لاراول</h3>
<p>
زمانی که یک نام برای یک مسیر مشخص اختصاص دادید، از طریق تابع route عمومی، می‌توانید از نام مسیر در زمان تولید URLها یا تغییرمسیرها (redirect) به صورت زیر استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">// Generating URLs...
$url = route('profile');
// Generating Redirects...
return redirect()->route('profile');
</code></pre>

<p>
اگر مسیر نامگذاری شده شامل پارامتر باشد، می‌توانید پارامترها را به عنوان آرگومان دوم به تابع route منتقل کنید. پارامترهای داده شده به صورت خودکار به URL تولید شده وارد می‌شوند:
</p>

<pre><code class="language-php  line-numbers">Route::get('user/{id}/profile', function ($id) {
    //
})->name('profile');
$url = route('profile', ['id' => 1]);
</code></pre>

<br>
<h3>بررسی مسیر فعلی در مسیر یابی لاراول</h3>
<p>
برای مشخص کردن اینکه آیا درخواست فعلی HTTP به یک مسیر نامگذاری شده هدایت شده است، می‌توانید از متد named در نمونه Route استفاده کنید. برای مثال، می‌توانید نام مسیر فعلی را در middleware مسیر چک کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Handle an incoming request.
 *
 * @param  \Illuminate\Http\Request  $request
 * @param  \Closure  $next
 * @return mixed
 */
public function handle($request, Closure $next)
{
    if ($request->route()->named('profile')) {
        //
    }
    return $next($request);
}
</code></pre>

<p>
<a name="route-groups"></a>
</p>

<br>
<h3><a href="#route-groups">گروه‌های مسیر در لاراول Route Groups</a></h3>
<p>
ممکن است برخی از ویژگی‌های مسیر، مانند middleware یا فضای نامی (namespaces)، در تعداد زیادی از مسیرها مشترک باشد. می‌توان بدون نیاز به تعریف جداگانه این ویژگی‌ها در هر یک از مسیرها، از امکان گروه‌ بندی مسیرها استفاده کرد و آن ویژگی‌ها را به تمام مسیرهای موجود در گروه اختصاص داد. ویژگی‌های به اشتراک گذاشته شده در قالب آرایه به عنوان اولین پارامتر در متد Route::group تعیین می‌شوند.
</p>

<p>
<a name="route-group-middleware"></a>
</p>

<br>
<h3>مسیریابی در Middleware</h3>
<p>
برای اختصاص یک Middleware به تمامی مسیرهای موجود در یک گروه، قبل از تعریف گروه می‌توانید از متد middleware استفاده کنید. Middlewareها به ترتیبی که در آرایه لیست شده‌اند بر روی مسیرها اعمال می‌شوند:
</p>

<pre><code class="language-php  line-numbers">Route::middleware(['first', 'second'])->group(function () {
    Route::get('/', function () {
        // Uses first & second Middleware
    });
    Route::get('user/profile', function () {
        // Uses first & second Middleware
    });
});
</code></pre>

<p>
<a name="route-group-namespaces"></a>
</p>

<br>
<h3>فضای نام Namespaces</h3>
<p>
می‌توان یک فضای نامی PHP را به یک گروه از کنترلرها با استفاده از متد namespace اختصاص داد:
</p>

<pre><code class="language-php  line-numbers">Route::namespace('Admin')->group(function () {
    // Controllers Within The "App\Http\Controllers\Admin" Namespace
});
</code></pre>

<p>
در حالت پیش‌فرض، RouteServiceProvider فایل‌های مسیر را در یک گروه namespace قرار می‌دهد، که امکان می‌دهد تا مسیرهای کنترلر را بدون مشخص کردن پیشوند کامل App\Http\Controllers ثبت کنید. پس از آن فقط نیاز به مشخص کردن بخشی از فضای نامی دارید که بعد از App\Http\Controllers قرار دارد.
</p>

<p>
<a name="route-group-sub-domain-routing"></a>
</p>

<br>
<h3>در لاراول Sub-Domain مسیریابی</h3>
<p>
از گروه‌های مسیر همچنین برای مدیریت مسیریابی زیر دامنه (Sub-Domain) استفاده می‌شود. زیر دامنه‌ها پارامترهای مسیر مثل URIهای مسیر را اختصاص داده و همچنین امکان می‌دهند تا بخشی از زیر دامنه را برای استفاده در مسیر یا کنترلر خود دریافت کنید. می‌توان زیر دامنه را قبل از تعریف گروه با فراخوانی متد domain مشخص کرد:
</p>

<pre><code class="language-php  line-numbers">Route::domain('{account}.myapp.com')->group(function () {
    Route::get('user/{id}', function ($account, $id) {
        //
    });
});
</code></pre>

<p>
<a name="route-group-prefixes"></a>
</p>

<br>
<h3>در مسیریابی لاراول Route پیشوندهای</h3>
<p>
برای اضافه کردن پیشوند به هر مسیر موجود در گروه از متد prefix استفاده می‌شود . برای مثال، در صورتیکه بخواهید تمام مسیرهای URIهای موجود در گروه، پیشوند admin را داشته باشد، به صورت زیر عمل کنید:
</p>

<pre><code class="language-php  line-numbers">Route::prefix('admin')->group(function () {
    Route::get('users', function () {
        // Matches The "/admin/users" URL
    });
});
</code></pre>

<p>
<a name="route-model-binding"></a>
</p>

<br>
<h3><a href="#route-model-binding">  Route Model Binding  در مسیریابی لاراول</a></h3>
<p>
هنگام تزریق یک model ID به یک مسیر یا یک اکشن مربوط به کنترلر، اغلب یک پرس و جو برای بازیابی مدلی که مربوط به آن شناسه است، بکار می‌برید. binding مدل مسیر در لاراول راه مناسبی است که از طریق آن می‌توان به طور خودکار یک نمونه مدل را به طور مستقیم در مسیرها تزریق کرد. برای مثال، به جای تزریق یک شناسه کاربر (ID)، می‌توان کل نمونه مدل User که با شناسه داده شده مطابقت دارد را تزریق کرد.
</p>

<p>
<a name="implicit-binding"></a>
</p>

<br>
<h3> Implicit Binding : مسیریابی در لاراول</h3>
<p>
لاراول به طور خودکار مدل‌های Eloquent تعریف شده در مسیرها یا اکشن‌های کنترلر که نام‌های متغیر type-hint شده آن‌ها با نام یک بخش از مسیر منطبق است را resolve می‌کند. مثال زیر را در نظر بگیرید:
</p>

<pre><code class="language-php  line-numbers">Route::get('api/users/{user}', function (App\User $user) {
    return $user->email;
});
</code></pre>

<p>
از آنجایی که متغیر $user به عنوان Eloquent modelApp\User  اعلان نوع یا type-hint شده است و نام متغیر با بخش {user} در URI مطابق است، لاراول به طور خودکار نمونه مدلی را که شناسه یا ID آن با مقدار موجود در URI منطبق است را تزریق می‌کند. اگر نمونه مطابق در پایگاه داده یافت نشود، یک پاسخ 404 HTTP به طور اتوماتیک تولید می‌شود.
</p>

<br>
<h3> مسیریابی در لاراول : سفارشی سازی نام کلید</h3>
<p>
اگر بخواهید binding مدل در پایگاه داده از ستونی غیر از id در هنگام بازیابی یک کلاس مدل استفاده کند، می‌توانید متد getRouteKeyName را در مدل Eloquent بازنویسی کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the route key for the model.
 *
 * @return string
 */
public function getRouteKeyName()
{
    return 'slug';
}
</code></pre>

<p>
<a name="explicit-binding"></a>
</p>

<br>
<h3> Explicit Binding : مسیریابی در لاراول</h3>
<p>
برای ثبت یک پیوند صریح (Explicit Binding)، از متد model روتر برای مشخص کردن کلاس برای یک پارامتر داده استفاده کنید. بایستی binding مدل صریح خود را در متد boot در کلاس RouteServiceProvider تعریف کنید:
</p>

<pre><code class="language-php  line-numbers">public function boot()
{
    parent::boot();
    Route::model('user', App\User::class);
}
</code></pre>

<p>
سپس، مسیری را که شامل یک پارامتر {user} است را تعریف کنید:
</p>

<pre><code class="language-php  line-numbers">Route::get('profile/{user}', function (App\User $user) {
    //
});
</code></pre>

<p>
از آنجایی که ما تمام پارامترهای {user} را به مدل App\User محدود کرده‌ایم، نمونه‌ای از کلاس User به مسیر تزریق می‌شود. برای مثال، هنگام ایجاد یک درخواست برای profile/1 یک نمونه از کلاس User که دارای شناسه 1 است از پایگاه داده تزریق می‌شود.
</p>

<p>
اگر نمونه مطابق با آن در پایگاه داده یافت نشود، یک پاسخ 404 HTTP به طور اتوماتیک تولید می‌شود.
</p>

<br>
<h3>سفارشی سازی Resolution Logic در مسیریابی لاراول</h3>
<p>
اگر بخواهید از Resolution Logic استفاده کنید، باید از متد Route::bind استفاده کنید. Closure به متد bind ، مقدار یک بخش از URI را دریافت می‌کند و نمونه کلاسی را که باید به مسیر تزریق شود را بازمی‌گرداند:
</p>

<pre><code class="language-php  line-numbers">public function boot()
{
    parent::boot();
    Route::bind('user', function ($value) {
        return App\User::where('name', $value)->first() ?? abort(404);
    });
}
</code></pre>

<p>
<a name="form-method-spoofing"></a>
</p>

<br>
<h3><a href="#form-method-spoofing">جعل متد فرم در مسیریابی در لاراول</a></h3>
<p>
فرم‌های HTML از عملیات PUT ، PATCH یا DELETE پشتیبانی نمی‌کنند. بنابراین، هنگام تعریف مسیرهای PUT ، PATCH یا DELETE که از یک فرم HTML فراخوانی می‌شوند، نیاز دارید یک فیلد _method مخفی را به فرم اضافه کنید. مقدار فرستاده شده با فیلد _method به عنوان متد درخواست HTTP مورد استفاده قرار می‌گیرد:
</p>

```html
<form action="/foo/bar" method="POST">
    <input type="hidden" name="_method" value="PUT">
    <input type="hidden" name="_token" value="{% raw %}{{ csrf_token() }}{% endraw %}">
</form>
```

<p>
می‌توانید از تابع کمکی method_field برای تولید فیلد _method استفاده کنید:
</p>

<pre><code class="language-php  line-numbers"> method_field('PUT')
</code></pre>

<p>
<a name="accessing-the-current-route"></a>
</p>

<br>
<h3><a href="#accessing-the-current-route">دسترسی به مسیر فعلی در لاراول</a></h3>
<p>
می‌توانید از متدهای current ، currentRouteName و currentRouteAction در  facade Route  برای دسترسی به اطلاعات مربوط به مسیر رسیدگی به درخواست ورودی استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$route = Route::current();
$name = Route::currentRouteName();
$action = Route::currentRouteAction();
</code></pre>

<p>
برای کسب اطلاعات بیشتر درباره کلاس پایه Route facade و Route instance، به مستندات API مراجعه کنید.
</p>
