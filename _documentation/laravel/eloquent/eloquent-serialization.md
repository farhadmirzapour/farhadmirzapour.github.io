---
layout: documentation
title:   آشنایی با eloquent serialization
cattitle: اصول آموزش Laravel
date:   2018-01-19 22:37:42 +0330
jdate: جمعه 29 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/eloquent-serialization <br> www.tahlildadeh.com/ArticleDetails/آموزش-serialization-در-Eloquent
---
<p>
به هنگام ساخت API برای JSON، اغلب لازم می شود مدل ها و رابطه های خود را به فرمت آرایه یا JSON تبدیل نمایید. Eloquent برای اجرای این نوع عملیات تبدیل متدهای کارآمدی را ارائه نموده و همچنین به شما اجازه می دهد attribute هایی که در فرایند serialization لحاظ می شوند را مدیریت کنید (سریالی شدن / serialization به پروسه ای اشاره دارد که در آن یک object یا مجموعه ای از object ها که به یکدیگر اشاره می کنند به صورت گروهی از بایت ها یا فرمت داده ای XML درآمده که متعاقبا می توان آن را ذخیره یا منتقل کرد. در مقابل Deserialization به فرایندی گفته می شود که در آن گروهی از بایت ها دریافت شده و به دنبال آن به object یا مجموعه ای از object ها تبدیل می شوند).
</p>
<p>
<a name="serializing-models-and-collections"></a>
</p>

<br>
<h3>تبدیل یک مدل به آرایه Serializing To Arrays</h3>
<p>
می توان با استفاده از متد toArray یک مدل و رابطه های بارگذاری شده آن را به نوع آرایه تبدیل نمایید. متد مذکور یک تابع recursive یا به اصطلاح بازگشتی است، بنابراین تمامی attribute ها و relation ها (از جمله رابطه ی رابطه ها) به آرایه تبدیل می شوند:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::with('roles')->first();

return $user->toArray();
</code></pre>

<p>
همچنین می توانید collection ها را به آرایه تبدیل نمایید:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::all();

return $users->toArray();
</code></pre>

<p>
<a name="serializing-to-json"></a>
</p>

<br>
<h3>تبدیل یک مدل به فرمت JSON (Serializing To JSON)</h3>
<p>
برای تبدیل یک مدل به JSON، می توانید متد toJson را فراخوانی نمایید. این متد نیز مانند toArray، یک تابع بازگشتی است. از اینرو تمامیattribute ها و relation ها به نوع JSON تبدیل می شوند:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

return $user->toJson();
</code></pre>

<p>
یا می توانید یک مدل یا collection را به نوع رشته تبدیل نمایید. این فرایند خود سبب فراخوانی خودکار متد toJson می شود و رشته ی تولید شده را به JSON تبدیل می کند:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

return (string) $user;
</code></pre>

<p>
از آنجایی که مدل ها و collection ها پس از تبدیل نوع به رشته، به فرمت JSON تبدیل می شوند، شما می توانید شی Eloquent را مستقیما ازroute ها یا controller های اپلیکیشن بازگردانی نمایید (return):
</p>

<pre><code class="language-php  line-numbers">Route::get('users', function () {
    return App\User::all();
});
</code></pre>

<p>
<a name="hiding-attributes-from-json"></a>
</p>

<br>
<h3><a href="#hiding-attributes-from-json">مخفی سازی attribute ها به هنگام تبدیل به JSON یا آرایه</a></h3>
<p>
گاهی لازم می شود attribute هایی نظیر گذرواژه ها که در نسخه ی تبدیل شده به آرایه یا JSON آن مدل لحاظ می شود را محدود نمایید. برای این منظور، کافی است یک property به نام $hidden به مدل خود اضافه نمایید و سپس attribute دلخواه را در آن قرار دهید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The attributes that should be hidden for arrays.
     *
     * @var array
     */
    protected $hidden = ['password'];
}
</code></pre>

<blockquote class="has-icon note">
در زمان پنهان سازی رابطه ها لازم است از اسم متد استفاده کنید، نه از اسم dynamic property آن.
</blockquote>

<p>
یا (به روشی دیگر) می توانید با استفاده از property به نام visible یک لیست سفید تعریف نموده و در آن attribute هایی که می خواهید در نسخه ی تبدیل شده به JSON یا آرایه از مدل موجود باشند را مشخص نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The attributes that should be visible in arrays.
     *
     * @var array
     */
    protected $visible = ['first_name', 'last_name'];
}
</code></pre>


<br>
<h3>ویرایش قابلیت رویت و دسترسی یک property مخفی به صورت موقت</h3>
<p>
اگر می خواهید برخی attribute هایی که غالبا مخفی هستند را در نمونه ی مدل خروجی قابل دسترس و visible نمایید (لیستی از attribute ها تعریف کنید که در آرایه یا JSON تبدیل وجود داشته باشند)، در آن صورت بایستی متد makeVisible را بکار ببرید.
</p>

<pre><code class="language-php  line-numbers">return $user->makeVisible('attribute')->toArray();
</code></pre>

<p>
به همین ترتیب، می توان برخی از attribute هایی که اغلب visible و قابل دسترس هستند را به صورت موقت از دسترس خارج نمایید (hiddenکنید). برای نیل به این هدف، Eloquent متد makeHidden را در اختیار برنامه نویس قرار می دهد. این متد در خروجی نمونه ای از مدل مورد نظر را برگردانده و زمینه را برای فراخوانی زنجیره ای متدها فراهم می آورد:
</p>

<pre><code class="language-php  line-numbers">return $user->makeHidden('attribute')->toArray();
</code></pre>

<p>
<a name="appending-values-to-json"></a>
</p>

<br>
<h3><a href="#appending-values-to-json">افزودن attribute های جدید در نسخه ی آرایه و JSON از مدل</a></h3>
<p>
گاهی لازم می شود attribute هایی را در آرایه اضافه کنید که ستون متناظری در پایگاه داده ندارند. برای این منظور ابتدا بایستی یک accessorبرای مقدار مورد نظر تعریف نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Get the administrator flag for the user.
     *
     * @return bool
     */
    public function getIsAdminAttribute()
    {
        return $this->attributes['admin'] == 'yes';
    }
}
</code></pre>

<p>
پس از ایجاد accessor مربوطه، کافی است اسم attribute را به متغیر appends در مدل اضافه نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The accessors to append to the model's array form.
     *
     * @var array
     */
    protected $appends = ['is_admin'];
}
</code></pre>

<p>
پس از اضافه شدن attribute به لیست مقادیر appends، این attribute هم در نسخه ی تبدیل شده به آرایه و هم در نسخه ی JSON از مدل لحاظ خواهد شد. attribute های موجود در آرایه appends از تنظیمات visible و hidden پیکربندی شده در مدل کاملا پیروی می کنند.
</p>
<p>
<a name="date-serialization"></a>
</p>

<br>
<h3><a href="#date-serialization">Date Serialization</a></h3>
<p>
لاراول کتابخانه تاریخ کربن را به منظور ارائه سفارشی مناسب از فرمت سریال JSON کربن گسترش می دهد. برای سفارشی کردن اینکه چگونه تمامی کربن ها در طول برنامه شما سریال می شوند، از روش Carbon :: serializeUsing استفاده کنید. روش serializeUsing یک Closure  را پذیرفته که رشته ای از تاریخ برای سریال سازی JSON را برمی گرداند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\Carbon;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Perform post-registration booting of services.
     *
     * @return void
     */
    public function boot()
    {
        Carbon::serializeUsing(function ($carbon) {
            return $carbon->format('U');
        });
    }

    /**
     * Register bindings in the container.
     *
     * @return void
     */
    public function register()
    {
        //
    }
}
</code></pre>