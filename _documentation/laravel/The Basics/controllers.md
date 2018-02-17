---
layout: documentation
title:   استفاده از Controller
cattitle: اصول آموزش Laravel
date:   2018-02-02 10:52:42 +0330
jdate: جمعه 13 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/controllers <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/230/controller-لاراول
---
<p>
به جای تعریف منطق مدیریت درخواست‌های برنامه به عنوان تابع Closure در فایل‌های مسیر لاراول، می‌توانید این کارها را با استفاده از کلاس‌های کنترلر انجام دهید. کنترلرها منطق پردازش درخواست‌های مرتبط به هم را در یک کلاس واحد دسته بندی می‌کنند. کنترلرها در دایرکتوری app/Http/Controllers قرار می‌گیرند. در ادامه چگونگی کار با کنترلرها و مسیردهی به آن‌ها را با هم بررسی می‌کنیم.
</p>


<p>
<a name="basic-controllers"></a>
</p>

<br>
<h3><a href="#basic-controllers"> پایه در لاراول</a></h3>

<p>
<a name="defining-controllers"></a>
</p>

<br>
<h3>تعریف Controllers</h3>

<p>
در مثال زیر نمونه‌ای از کلاس کنترلر پایه را مشاهده می‌کنید. توجه کنید که کنترلرهای برنامه از کلاس کنترلر پایه که به صورت پیش‌فرض در لاراول قرار دارد، ارث بری می‌کنند. کلاس پایه چندین متد ارائه می‌دهد، برای مثال متد middleware برای اتصال middleware به اکشن‌های کنترلر مورد استفاده قرار می‌گیرد:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use App\User;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function show($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
</code></pre>


<p>
می‌توان یک مسیر مانند مثال زیر برای این کنترلر تعریف کرد:
</p>


<pre><code class="language-php  line-numbers">Route::get('user/{id}', 'UserController@show');
</code></pre>


<p>
زمانی که یک درخواست ورودی با URI مربوط به مسیر مطابقت داشته باشد، متد show در کلاس UserController اجرا خواهد شد. همچنین پارامترهای موجود در مسیر نیز به متد ارسال می‌شوند.
</p>

<blockquote class="has-icon tip">
کنترلرها را می‌توان بدون نیاز به ارث بری از یک کلاس پایه تعریف کرد. اما در این صورت به امکاناتی مانند استفاده از متدهای middleware ، validate و dispatch دسترسی نخواهید داشت.
</blockquote>

<p>
<a name="controllers-and-namespaces"></a>
</p>

<br>
<h3>استفاده از controller و namespace در لاراول</h3>

<p>
نکته مهمی که باید به آن توجه داشته باشیم این است که در هنگام تعیین مسیر کنترلر، نیازی نیست که فضای نامی کنترلر را به صورت کامل تعریف کنیم. از آنجا که RouteServiceProvider فایل‌های مسیر را در یک گروه مسیر که فضای نامی ریشه را شامل می‌شود، بارگزاری می‌کند؛ فقط آن بخش از نام کلاس که بعد از بخش App\Http\Controllers در فضای نامی قرار دارد را مشخص می‌کنیم.
</p>


<p>
اگر بخواهید کنترلرها توسط فضای نامی در دایرکتوری App\Http\Controllers به صورت تودرتو گروه بندی کنید، می‌توانید به سادگی از نام کلاس خاصی نسبت به فضای نامی ریشه App\Http\Controllers استفاده کنید. بنابراین، اگر کلاس کامل کنترلر شما App\Http\Controllers\Photos\AdminController است، باید مانند مثال زیر مسیر را برای کنترلر ثبت کنید:
</p>


<pre><code class="language-php  line-numbers">Route::get('foo', 'Photos\AdminController@method');
</code></pre>


<p>
<a name="single-action-controllers"></a>
</p>

<br>
<h3>کنترلرهای تک اکشن (Single Action Controllers) در لاراول</h3>

<p>
اگر بخواهید کنترلری تعریف کنید که فقط یک اکشن داشته باشد، باید یک متد __invoke را در کنترلر قرار دهید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use App\User;
use App\Http\Controllers\Controller;

class ShowProfile extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  int  $id
     * @return Response
     */
    public function __invoke($id)
    {
        return view('user.profile', ['user' => User::findOrFail($id)]);
    }
}
</code></pre>


<p>
در هنگام ثبت مسیر، مشخص کردن متد تعریف شده برای کنترلرهای single action نیاز نیست. به مثال زیر توجه کنید:
</p>


<pre><code class="language-php  line-numbers">Route::get('user/{id}', 'ShowProfile');
</code></pre>


<p>
<a name="controller-middleware"></a>
</p>

<br>
<h3><a href="#controller-middleware">Controller Middleware در لاراول</a></h3>

<p>
می‌توان یک middleware را به مسیرهای کنترلر در فایل‌های مسیر به صورت زیر اختصاص داد:
</p>


<pre><code class="language-php  line-numbers">Route::get('profile', 'UserController@show')->middleware('auth');
</code></pre>


<p>
با این حال، برای راحتی کار می‌توان middleware را در سازنده کنترلر تعریف کرد. با استفاده از متد middleware در سازنده کنترلر، می‌توان به راحتی middleware را به اکشن کنترلر اختصاص داد. حتی می‌توان middleware را به متدهای خاصی در کلاس کنترلر محدود کرد:
</p>


<pre><code class="language-php  line-numbers">class UserController extends Controller
{
    /**
     * Instantiate a new controller instance.
     *
     * @return void
     */
    public function __construct()
    {
        $this->middleware('auth');

        $this->middleware('log')->only('index');

        $this->middleware('subscribed')->except('store');
    }
}
</code></pre>


<p>
همچنین کنترلر امکان ثبت middleware با استفاده از Closure را فراهم می‌کند. این موضوع روش مناسبی جهت تعریف middleware برای یک کنترلر بدون نیاز به تعریف کلاس کلی middleware است.
</p>


<pre><code class="language-php  line-numbers">$this->middleware(function ($request, $next) {
    // ...

    return $next($request);
});
</code></pre>

<blockquote class="has-icon tip">
 می‌توان middleware را به زیرمجموعه‌ای از اکشن‌های یک کنترلر اختصاص داد؛ با این حال، این موضوع نشان دهنده رشد بیش از حد کنترلر است. می‌توان به جای این کار، کنترلر را به کنترلرهای کوچکتر تقسیم کرد.
</blockquote>

<p>
<a name="resource-controllers"></a>
</p>

<br>
<h3><a href="#resource-controllers">کنترلر منابع یا resource controllers در لاراول</a></h3>

<p>
در مسیریابی منابع لاراول، مسیرهای معمول عملیات CRUD را می‌توان با یک خط کد به یک کنترلر اختصاص داد. برای مثال، اگر بخواهید کنترلری ایجاد کنید که تمام درخواست‌های HTTP برای عکس‌های ذخیره شده توسط برنامه را مدیریت کند، می‌توان به سرعت و با استفاده از دستور make:controller این کنترلر را ایجاد کرد:
</p>


<pre><code class="language-php  line-numbers">php artisan make:controller PhotoController --resource
</code></pre>


<p>
این دستور کنترلر را در app/Http/Controllers/PhotoController.php ایجاد می‌کند. این کنترلر برای هر یک از عملیات مربوط به منابع موجود، یک متد را شامل می‌شود.
</p>


<p>
سپس، می‌توان یک مسیر خوب برای کنترلر مانند مثال زیر ثبت کرد:
</p>


<pre><code class="language-php  line-numbers">Route::resource('photos', 'PhotoController');
</code></pre>


<p>
این اعلان ساده مسیر، مسیرهای متعددی را برای مدیریت انواع عملیات بر روی منابع (عکس‌ها) ایجاد می‌کند. اکنون کنترلر ایجاد شده متدهایی را برای هر کدام از این عملیات، ارائه کرده است. این متدها شامل یادداشت‌هایی هستند که مشخص می‌کند کدام درخواست‌های HTTP و URIها توسط آن‌ها مدیریت می‌شوند.
</p>


<p>
می‌توان چند کنترلر منابع را در یک زمان بوسیله انتقال یک آرایه به متد resources ثبت کرد:
</p>


<pre><code class="language-php  line-numbers">Route::resources([
    'photos' => 'PhotoController',
    'posts' => 'PostController'
]);
</code></pre>

<br>
<h3>عملیاتی که توسط کنترلر منابع مدیریت می‌شود:
</h3>
<table>
<thead>
<tr>
<th>Verb</th>
<th>URI</th>
<th>Action</th>
<th>Route Name</th>
</tr>
</thead>
<tbody>
<tr>
<td>GET</td>
<td><code class=" language-php"><span class="token operator">/</span>photos</code></td>
<td>index</td>
<td>photos.index</td>
</tr>
<tr>
<td>GET</td>
<td><code class=" language-php"><span class="token operator">/</span>photos<span class="token operator">/</span>create</code></td>
<td>create</td>
<td>photos.create</td>
</tr>
<tr>
<td>POST</td>
<td><code class=" language-php"><span class="token operator">/</span>photos</code></td>
<td>store</td>
<td>photos.store</td>
</tr>
<tr>
<td>GET</td>
<td><code class=" language-php"><span class="token operator">/</span>photos<span class="token operator">/</span><span class="token punctuation">{</span>photo<span class="token punctuation">}</span></code></td>
<td>show</td>
<td>photos.show</td>
</tr>
<tr>
<td>GET</td>
<td><code class=" language-php"><span class="token operator">/</span>photos<span class="token operator">/</span><span class="token punctuation">{</span>photo<span class="token punctuation">}</span><span class="token operator">/</span>edit</code></td>
<td>edit</td>
<td>photos.edit</td>
</tr>
<tr>
<td>PUT/PATCH</td>
<td><code class=" language-php"><span class="token operator">/</span>photos<span class="token operator">/</span><span class="token punctuation">{</span>photo<span class="token punctuation">}</span></code></td>
<td>update</td>
<td>photos.update</td>
</tr>
<tr>
<td>DELETE</td>
<td><code class=" language-php"><span class="token operator">/</span>photos<span class="token operator">/</span><span class="token punctuation">{</span>photo<span class="token punctuation">}</span></code></td>
<td>destroy</td>
<td>photos.destroy</td>
</tr>
</tbody>
</table>
<br>
<h3>تعیین مدل منابع (Resource Model) در لاراول</h3>

<p>
اگر از binding مدل مسیر استفاده می‌کنید و مایلید متدهای کنترلر منابع را به یک نمونه مدل type-hint کنید، می‌توانید از گزینه --model هنگام ایجاد کنترلر استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">php artisan make:controller PhotoController --resource --model=Photo
</code></pre>

<br>
<h3>متدهای فرم جعلی در لاراول</h3>

<p>
از آنجا که فرم‌های HTML نمی‌توانند درخواست‌های PUT ، PATCH یا DELETE را ایجاد کنند، باید یک فیلد پنهان _method برای ایجاد جعلی این درخواست‌های HTTP اضافه کنید. تابع کمکی method_field این فیلد را برای شما ایجاد می‌کند:
</p>


<pre><code class="language-php  line-numbers">{% raw %}{{ method_field('PUT') }}{% endraw %}
</code></pre>


<p>
<a name="restful-partial-resource-routes"></a>
</p>

<br>
<h3>اعلان مسیر‌های منابع به صورت سفارشی در لاراول</h3>

<p>
هنگام اعلان یک مسیر منابع، می‌توان زیرمجموعه‌ای از عملیاتی که کنترلر باید مدیریت کند را به جای مجموعه کامل عملیات پیش‌فرض تعریف کرد:
</p>


<pre><code class="language-php  line-numbers">Route::resource('photo', 'PhotoController', ['only' => [
    'index', 'show'
]]);

Route::resource('photo', 'PhotoController', ['except' => [
    'create', 'store', 'update', 'destroy'
]]);
</code></pre>

<br>
<h3>اعلان مسیرهای منابع API در لاراول</h3>

<p>
هنگام اعلان مسیرهای منابعی که توسط APIها استفاده می‌شوند، معمولا مسیرهای مربوط به عملیات create و edit را در قالب های HTML حذف می‌کنید. برای راحتی کار، می‌توانید متد apiResource را استفاده کنید که به صورت خودکار این دو مسیر را حذف می‌کند:
</p>


<pre><code class="language-php  line-numbers">Route::apiResource('photo', 'PhotoController');
</code></pre>


<p>
می‌توان چندین کنترلر API را در یک زمان با انتقال یک آرایه به متد apiResource ثبت کرد:
</p>


<pre><code class="language-php  line-numbers">Route::apiResources([
    'photos' => 'PhotoController',
    'posts' => 'PostController'
]);
</code></pre>


<p>
<a name="restful-naming-resource-routes"></a>
</p>

<br>
<h3>نام‌گذاری مسیرهای منابع در لاراول</h3>

<p>
در حالت پیش‌فرض تمام اکشن‌های کنترلر منابع دارای یک نام مسیر یا route name هستند؛ با این حال، می‌توانید این نام‌ها را با انتقال یک آرایه names با گزینه‌های موردنظر خود بازنویسی کنید:
</p>


<pre><code class="language-php  line-numbers">Route::resource('photo', 'PhotoController', ['names' => [
    'create' => 'photo.build'
]]);
</code></pre>


<p>
<a name="restful-naming-resource-route-parameters"></a>
</p>

<br>
<h3>پارامترهای مربوط به نام‌گذاری مسیرهای منابع در لاراول</h3>

<p>
در حالت پیش‌فرض، Route::resource پارامترهای مسیر را برای مسیرهای منابع بر اساس نام‌های یکتا سازی شده منابع ایجاد می‌کند. می‌توان این نام‌ها را براساس هر منبع، توسط انتقال آرایه گزینه‌های parameters بازنویسی کرد. آرایه parameters باید یک آرایه ترکیبی از نام‌های منابع و نام‌های پارامترها باشد:
</p>


<pre><code class="language-php  line-numbers">Route::resource('user', 'AdminUserController', ['parameters' => [
    'user' => 'admin_user'
]]);
</code></pre>


<p>
مثال فوق URIهای زیر را برای مسیر show منابع ایجاد می‌کند:
</p>


<pre><code class="language-php  line-numbers">/user/{admin_user}
</code></pre>


<p>
<a name="restful-localizing-resource-uris"></a>
</p>

<br>
<h3>ایجاد URLهای منابع به صورت محلی در لاراول</h3>

<p>
در حالت پیش‌فرض، URLهای منابع توسط Route::resource با استفاده از کلمات انگلیسی ایجاد می‌کند. برای مثال، اگر نیاز به محلی سازی کلمات اکشن‌های create و edit دارید، می‌توانید از متد Route::resourceVerbs استفاده کنید و در متد boot مربوط به AppServiceProvider این عمل را انجام دهید:
</p>


<pre><code class="language-php  line-numbers">use Illuminate\Support\Facades\Route;

/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Route::resourceVerbs([
        'create' => 'crear',
        'edit' => 'editar',
    ]);
}
</code></pre>


<p>
زمانی که کلمات محلی سازی شدند، URIهای زیر توسط ثبت منابع مسیر Route::resource('fotos', 'PhotoController') ،   تولید می‌‌شود:
</p>


<pre><code class="language-php  line-numbers">/fotos/crear

/fotos/{foto}/editar
</code></pre>


<p>
<a name="restful-supplementing-resource-controllers"></a>
</p>

<br>
<h3>اضافه کردن مسیرهای اضافی به کنترلر منابع در لاراول</h3>

<p>
اگر نیاز به اضافه کردن مسیرهای اضافی، علاوه بر مجموعه پیش‌فرض مسیرهای منابع به یک کنترلر منابع دارید، قبل از فراخوانی Route::resource شما باید این مسیرها را تعریف کنید، در غیر این صورت، مسیرهای تعریف شده توسط متد resource بر روی مسیرهای اضافی که قصد اضافه کردن آن‌ها را داریم، اولویت خواهند داشت:
</p>


<pre><code class="language-php  line-numbers">Route::get('photos/popular', 'PhotoController@method');

Route::resource('photos', 'PhotoController');
</code></pre>

<blockquote class="has-icon tip">

<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> Remember to keep your controllers focused. If you find yourself routinely needing methods outside of the typical set of resource actions, consider splitting your controller into two, smaller controllers.
</p>

</blockquote>

<p>
<a name="dependency-injection-and-controllers"></a>
</p>

<br>
<h3><a href="#dependency-injection-and-controllers">تزریق وابستگی و Controller در لاراول</a></h3>
<p>
از  service container لاراول برای resolve کردن تمام کنترلرهای لاراول استفاده می‌شود. در نتیجه، قادر خواهیم بود، هر وابستگی که ممکن است کنترلر در سازنده به آن نیاز داشته باشد را اعلان نوع یا type-hint کنیم. وابستگی‌های اعلان شده به صورت خودکار resolve می‌شوند و تزریق وابستگی به نمونه‌ی کنترلر انجام می‌شود:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use App\Repositories\UserRepository;

class UserController extends Controller
{
    /**
     * The user repository instance.
     */
    protected $users;

    /**
     * Create a new controller instance.
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        $this->users = $users;
    }
}
</code></pre>


<p>
همچنین می‌توانید هر قرارداد لاراول را نیز اعلان نوع یا type-hint کنید. اگر container می‌تواند آن را resolve کند، می‌توانید آن را اعلان نوع کنید. بسته به برنامه، تزریق وابستگی‌ها به کنترلر، ممکن است تست پذیری بهتری را فراهم کند.
</p>

<br>
<h3>تزریق وابستگی به متد در لاراول</h3>

<p>
علاوه بر تزریق وابستگی در سازنده، می‌توانید وابستگی‌ها را در متدهای کنترلر نیز type-hint کنید. یک مورد استفاده معمول برای روش تزریق در متد، تزریق نمونه Illuminate\Http\Request به متد کنترلر مانند مثال زیر است:
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
        $name = $request->name;

        //
    }
}
</code></pre>


<p>
اگر متد کنترلر انتظار دریافت ورودی از یک پارامتر مسیر را دارد، می‌توانید به سادگی آرگومان‌های مسیر را پس از وابستگی‌ها لیست کنید. برای مثال، اگر مسیر شما به صورت مثال زیر تعریف شده باشد:
</p>


<pre><code class="language-php  line-numbers">Route::put('user/{id}', 'UserController@update');
</code></pre>


<p>
می‌توانید Illuminate\Http\Request را اعلان نوع کنید و با تعریف متد کنترلر خود به صورت زیر به پارامتر id دسترسی داشته باشید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Update the given user.
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


<p>
<a name="route-caching"></a>
</p>

<br>
<h3><a href="#route-caching">ذخیره سازی مسیر در حافظه نهان (route caching) در لاراول</a></h3>
<blockquote class="has-icon note">
 مسیرهای مبتنی بر Closure با مسیرهای ذخیره شده در کش سازگاری ندارند. برای استفاده از route caching، بایستی هر مسیر مبتنی بر Closure را به کلاس‌های کنترلر تبدیل کنید.
</blockquote>

<p>
اگر برنامه شما به صورت انحصاری از مسیرهای مبتنی بر کنترلر استفاده می‌کند، می‌توانید به راحتی از route caching در لاراول استفاده کنید. استفاده از route cache به میزان قابل توجهی زمان لازم برای ثبت تمام مسیرهای برنامه در لاراول را کاهش می‌دهد. در برخی موارد، با استفاده از این روش ثبت مسیر می‌تواند حتی تا 100 برابر سریع‌تر انجام شود. برای ساخت یک route cache، باید دستور آرتیسان route:cache را اجرا کنید:
</p>


<pre><code class="language-php  line-numbers">php artisan route:cache
</code></pre>


<p>
پس از اجرای این دستور، فایل مسیرهای کش شده بر روی هر درخواست بارگزاری می‌شود. توجه کنید، اگر بخواهید هر مسیر جدیدی را اضافه کنید، باید یک route cache جدید ایجاد کنید. به همین دلیل، توصیه می‌شود، دستور route:cache را در هنگام استقرار و نصب پروژه اجرا نمایید.
</p>


<p>
می‌توانید از دستور route:clear مانند مثال زیر برای پاک کردن route cache استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">php artisan route:clear
</code></pre>

<p>
در این مقاله از سری مقالات آموزشی لاراول در لیداوب قصد ما آشنا کردن شما با مبحث مهم و کلیدی کنترلرهای لاراول و چگونگی کار با آن‌ها و همچنین مسیردهی آن‌ها بود. امیدواریم این مقاله به شما کرده باشد، تا هر چه سریعتر بتوانید با اصول اولیه و مفاهیم لاراول آشنا شده و راه یادگیری خود را هموار کنید.
</p>