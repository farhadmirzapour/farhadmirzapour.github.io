---
layout: documentation-laravel
title:   ایجاد view
cattitle: اصول آموزش Laravel
date:   2018-02-09 08:28:42 +0330
jdate: جمعه 20 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/views <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/248/آموزش-لاراول-استفاده-از-view-در-لاراول
---
<p>
در یک برنامه لاراول خدمات HTML، توسط صفحات view ارائه می‌شوند؛ بنابراین، به واسطه آن منطق یا کد برنامه که معمولاً در کنترلر برنامه قرار دارد را از لایه نمایشی جدا می‌شود. صفحات view در دایرکتوری resources/views ذخیره می‌شوند. در مثال زیر نمونه‌ای از یک view ساده را مشاهده می‌کنید:
</p>

```html
<!-- View stored in resources/views/greeting.blade.php -->

<html>
    <body>
        <h1>Hello, {% raw %}{{ $name }}{% endraw %}</h1>
    </body>
</html>
```


<p>
از آنجا که این view در resources/views/greeting.blade.php ذخیره می‌شود، می‌توانیم آن را با استفاده از تابع کمکی view عمومی به صورت زیر بازگردانیم:
</p>


<pre><code class="language-php  line-numbers">Route::get('/', function () {
    return view('greeting', ['name' => 'James']);
});
</code></pre>


<p>
همانطور که مشاهده می‌کنید، آرگومان اول در تابع کمکی view با نام فایل موجود در دایرکتوری resources/views برابر است. آرگومان دوم آرایه‌ای از داده‌ها است که باید به view انتقال داده شود. در این مثال، متغیر name انتقال داده شده است که می‌توان با استفاده از سینتکس Blade آن را نمایش داد.
</p>


<p>
البته، می‌توان viewها را به صورت تودرتو زیردایرکتوری‌های موجود در دایرکتوری اصلی یا همان resources/views قرار داد. در این صورت، می‌توان از علامت «نقطه» برای مشاهده view‌های تودرتو استفاده کرد. برای مثال، اگر view در مسیر resources/views/admin/profile.blade.php ذخیره شده باشد، برای دسترسی به آن باید مانند مثال زیر عمل کرد:
</p>


<pre><code class="language-php  line-numbers">return view('admin.profile', $data);
</code></pre>


<br>
<h3>تعیین موجود بودن یک view در برنامه</h3>

<p>
اگر بخواهید تعیین کنید که آیا یک view موجود است یا خیر، می‌توانید از facade مربوط به View استفاده کنید. متد exists در صورت موجود بودن یک view مقدار true  را بازمی‌گرداند.
</p>


<pre><code class="language-php  line-numbers">use Illuminate\Support\Facades\View;

if (View::exists('emails.customer')) {
    //
}
</code></pre>


<br>
<h3>ایجاد اولین view موجود</h3>

<p>
برای ایجاد اولین view موجود در آرایه‌ای از viewها، می‌توان از متد first استفاده کرد. این امر زمانی مفید است که برنامه یا پکیج امکان اینکه viewها سفارشی یا بازنویسی شوند را می‌دهد:
</p>


<pre><code class="language-php  line-numbers">return view()->first(['custom.admin', 'admin'], $data);
</code></pre>


<p>
فراخوانی این متد توسط facade View  هم می‌تواند انجام شود:
</p>


<pre><code class="language-php  line-numbers">use Illuminate\Support\Facades\View;

return View::first(['custom.admin', 'admin'], $data);
</code></pre>


<p>
<a name="passing-data-to-views"></a>
</p>


<br>
<h3><a href="#passing-data-to-views">انتقال اطلاعات به view</a></h3>

<p>
همان طور که در مثال‌های قبلی مشاهده کردید، می توان آرایه‌ای از داده‌ها را به view انتقال داد:
</p>


<pre><code class="language-php  line-numbers">return view('greetings', ['name' => 'Victoria']);
</code></pre>


<p>
هنگام انتقال اطلاعات به این روش، داده‌ها باید به همراه کلید و مقدار در یک آرایه‌ قرار گیرند. در داخل view، می‌توان به هر مقدار با استفاده از کلید مربوط به آن مانند <?php echo $key; ?> دسترسی داشت. به جای انتقال آرایه‌ای کامل از داده‌ها به تابع کمکی view ، مانند مثال زیر می‌توان از متد with جهت اضافه کردن تکه داده‌ها به صورت جداگانه به view، استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">return view('greeting')->with('name', 'Victoria');
</code></pre>


<p>
<a name="sharing-data-with-all-views"></a>
</p>


<br>
<h3>اشتراک گذاری داده‌ در تمام viewهای برنامه </h3>

<p>
گاهی اوقات، لازم است بخشی از داده‌ها را با تمام viewهایی که توسط برنامه ارائه شده‌اند، به اشتراک بگذارید. این کار را می‌توان با استفاده از متد share  در facade view  انجام داد. معمولاً، فراخوانی‌های متد share را بایستی در متد boot موجود در service provider قرار داد. می‌توانید آن‌ها را به AppServiceProvider   اضافه کنید یا یک service provider جداگانه برای آن‌ها بسازید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\Facades\View;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        View::share('key', 'value');
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
</code></pre>


<p>
<a name="view-composers"></a>
</p>


<br>
<h3><a href="#view-composers">استفاده از کامپوزرهای view</a></h3>

<p>
کامپوزرهای view ، توابع callback یا متدهای کلاسی هستند که در هنگام رندر شدن یا نمایش یک view فراخوانی می‌شوند. اگر بخواهید هر زمان که  view نمایش داده می‌شود، داده‌ها را به آن انتقال دهید، استفاده از  view composer می‌تواند این منطق را در یک مکان واحد برای شما سازماندهی کند.
</p>


<p>
برای درک بهتر این موضوع، اجازه دهید view composer را در service provider ثبت کنیم. از facade مربوط به View برای دسترسی به زیرمجموعه‌های پیاده سازی قرارداد Illuminate\Contracts\View\Factory استفاده می‌کنیم. باید توجه داشت که لاراول دایرکتوری پیش‌فرض برای view composer ندارد. با این وجود، می‌توانید آن‌ها را سازماندهی کنید. برای مثال، می‌توانید یک دایرکتوری app/Http/ViewComposers برای آن‌ها ایجاد کنید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\Facades\View;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function boot()
    {
        // Using class based composers...
        View::composer(
            'profile', 'App\Http\ViewComposers\ProfileComposer'
        );

        // Using Closure based composers...
        View::composer('dashboard', function ($view) {
            //
        });
    }

    /**
     * Register the service provider.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
</code></pre>

<blockquote class="has-icon note">
توجه داشته باشید که اگر بخواهید یک service provider جدید برای ثبت view composer خود ایجاد کنید، باید حتما این service provider را به آرایه providers در پیکربندی config/app.php اضافه کنید.
</blockquote>

<p>
اکنون که composer ثبت شده است، هر زمان که ویو profile نمایش داده می‌شود، متد ProfileComposer@compose اجرا می‌شود. حال اجازه دهید در مثال زیر، کلاس composer را تعریف کنیم:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\ViewComposers;

use Illuminate\View\View;
use App\Repositories\UserRepository;

class ProfileComposer
{
    /**
     * The user repository implementation.
     *
     * @var UserRepository
     */
    protected $users;

    /**
     * Create a new profile composer.
     *
     * @param  UserRepository  $users
     * @return void
     */
    public function __construct(UserRepository $users)
    {
        // Dependencies automatically resolved by service container...
        $this->users = $users;
    }

    /**
     * Bind data to the view.
     *
     * @param  View  $view
     * @return void
     */
    public function compose(View $view)
    {
        $view->with('count', $this->users->count());
    }
}
</code></pre>


<p>
درست قبل از آنکه view نمایش داده شود، متد  composer مربوط به composer با نمونه کلاس Illuminate\View\View فراخوانی می‌شود. می‌توانید با استفاده از متد with داده‌ها را به ویو bind کنید.
</p>

<blockquote class="has-icon tip">
تمام view composerها از طریق service container تشخیص داده شده و resolve می‌شوند، بنابراین، می‌توانید هر وابستگی‌ که به آن نیاز دارید را در سازنده composer خود اعلان نوع یا type-hint کنید.
</blockquote>

<br>
<h3>پیوست composer به چندین view </h3>

<p>
می‌توان با انتقال آرایه‌ای از viewها به عنوان اولین آرگومان به متد composer  یک view composer را به چندین view به صورت همزمان پیوست کرد:
</p>


<pre><code class="language-php  line-numbers">View::composer(
    ['profile', 'dashboard'],
    'App\Http\ViewComposers\MyViewComposer'
);
</code></pre>


<p>
همچنین، متد composer کاراکتر * را به عنوان یک پارامتر wildcard می‌پذیرد که امکان می‌دهد که یک composer را به تمام viewها پیوست کنید:
</p>


<pre><code class="language-php  line-numbers">View::composer('*', function ($view) {
    //
});
</code></pre>


<br>
<h3>view creator استفاده از</h3>

<p>
استفاده از view creatorها بسیار شبیه به view composerها هستند. با این حال، آن‌ها بلافاصله پس از ایجاد view اجرا می‌شوند و اجازه اماده شدن view برای نمایش را نمی‌دهند. برای ثبت یک view creator، می‌توان از متد creator به صورت مثال زیر استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">View::creator('profile', 'App\Http\ViewCreators\ProfileCreator');
</code></pre>