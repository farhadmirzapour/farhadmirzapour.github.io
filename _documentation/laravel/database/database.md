---
layout: documentation
title:   پایگاه داده ها
cattitle: اصول آموزش Laravel
date:   2017-12-25 19:23:42 +0330
jdate: دوشنبه 4 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/collections
---
<p>
لاراول ارتباط با پایگاه داده ها را با استفاده از raw SQL, the fluent query builder و  the Eloquent ORM  بسیار ساده ساخته است. در حال حاضر، Laravel از چهار پایگاه داده پشتیبانی می کند:
</p>

<div class="content-list">
<ul>
<li>MySQL</li>
<li>PostgreSQL</li>
<li>SQLite</li>
<li>SQL Server</li>
</ul>
</div>

<br>
<h3>تنظیمات (Configuration)</h3>
<p>
پیکربندی پایگاه داده برای برنامه شما در <code class=" language-php">config/database.php</code> قرار دارد. در این فایل شما می توانید تمام اتصالات پایگاه داده خود را تعریف کنید.
</p>

<br>
<h3> تنظیمات Read &amp; Write Connections </h3>
<p>
گاهی اوقات شما ممکن است مایل به استفاده از یک کانکشن پایگاه داده برای دستورات SELECT، و کانکشن دیگر برای INSERT، UPDATE و DELETE statements. لاراول این امکان را ایجاد می کند تا کانکشن های مناسب مورد استفاده قرار می گیرد.
</p>


<pre><code class="language-php  line-numbers">'mysql' => [
    'read' => [
        'host' => '192.168.1.1',
    ],
    'write' => [
        'host' => '196.168.1.2'
    ],
    'sticky'    => true,
    'driver'    => 'mysql',
    'database'  => 'database',
    'username'  => 'root',
    'password'  => '',
    'charset' => 'utf8mb4',
    'collation' => 'utf8mb4_unicode_ci',
    'prefix'    => '',
],
</code></pre>

<p>
گزینه sticky  یک مقدار اختیاری است  تا عملیات خواندن فوری از دیتابیس  در زمانیکه عملیات نوشتن اطلاعات در دیتابیس در حال انجام است  صورت گیرد  و تضمین می کند که هر گونه اطلاعاتی که در طول چرخه درخواست نوشته شده است، بتواند بلافاصله پس از همان درخواست از پایگاه داده خوانده شود.
</p>



<br>
<h3>استفاده همزمان از چندین connection در پروژه</h3>
<p>
هنگامی که چندین connection (برای انجام عملیات مختلف) در پروژه خود تعریف شده دارید، می توانید از طریق متد connection در façade DB به هر یک از connection ها دسترسی داشته باشید. name ای که به متد connection ارسال می کنید باید با یکی از connection های لیست شده در فایل تنظیمات config/database.php منطبق باشد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::connection('foo')->select(...);
</code></pre>

<p>
همچنین می توانید به نمونه ی شی PDO خالص و زیرساختی دسترسی داشته باشید. برای این منظور کافی است متد getPdo را بر روی نمونه ای ازconnection صدا بزنید:
</p>

<pre><code class="language-php  line-numbers">$pdo = DB::connection()->getPdo();
</code></pre>


<br>
<h3><a href="#running-queries">اجرای  Raw SQL Queries</a></h3>
<p>
هنگامی که اتصال پایگاه داده خود را پیکربندی کرده اید، می توانید با استفاده از فاساد DB کوئری ها را اجرا کنید.
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Support\Facades\DB;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show a list of all of the application's users.
     *
     * @return Response
     */
    public function index()
    {
        $users = DB::select('select * from users where active = ?', [1]);

        return view('user.index', ['users' => $users]);
    }
}
</code></pre>

<p>
اولین آرگومان دستورات SQL و دومین آرگومان مقادیری است که بصورت binding جهت جلوگیری از حملات SQL injection مورد استفاده قرار می گیرد.
</p>

<p>
دستور SELECT آرایه ای از نتایج را بر می گرداند و هر نتیجه شی از کلاس PHP StdClass خواهد بود که به شما امکان می دهد به مقادیر نتایج دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">foreach ($users as $user) {
    echo $user->name;
}
</code></pre>

<br>
<h3>استفاده از Named Bindings</h3>

<p>
بجای استفاده از ؟ برای نشان دادن پارامترهای bindings،  می توانید  از نامگذاری پارامترها استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$results = DB::select('select * from users where id = :id', ['id' => 1]);
</code></pre>

<br>
<h3>اجرای دستور Insert</h3>
<p>
کد زیر نحوه اجرای دستور INSERT را نمایش می دهد :
</p>

<pre><code class="language-php  line-numbers">DB::insert('insert into users (id, name) values (?, ?)', [1, 'Dayle']);
</code></pre>

<br>
<h3>اجرای دستور Update</h3>
<p>
کد زیر نحوه اجرای دستور UPDATE را نمایش می دهد که تعداد رکوردهای تغییر داده شده را بر می گرداند :
</p>
<pre><code class="language-php  line-numbers">$affected = DB::update('update users set votes = 100 where name = ?', ['John']);
</code></pre>

<br>
<h3>اجرای دستور Delete</h3>
<p>
کد زیر نحوه اجرای دستور DELETE را نمایش می دهد که تعداد رکوردهای حذف شده را بر می گرداند :
</p>


<pre><code class="language-php  line-numbers">$deleted = DB::delete('delete from users');
</code></pre>

<br>
<h3>اجرای سایر دستورات SQL</h3>
<p>
تعدادی از دستورات SQL مقداری را بر نمی گردانند. برای اجرای اینگونه دستورات می توان از متد statement  فاساد DB  همانند زیر استفاده نمود :
</p>

<pre><code class="language-php  line-numbers">DB::statement('drop table users');
</code></pre>

<br>
<h3>گوش دادن به رخدادهای Query با متد Listen</h3>
<p>
می توانید با بهره گیری از متد listen در façade DB، برای تمامی کوئری ها listener تعریف کنید. متد ذکر شده به شما کمک می کند از کوئری هاlog گرفته و آن ها را عیب یابی کنید. می توانید query listener را در یک service provider ثبت کنید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\Facades\DB;
use Illuminate\Support\ServiceProvider;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        DB::listen(function ($query) {
            // $query->sql
            // $query->bindings
            // $query->time
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


<br>
<h3><a href="#database-transactions">بررسی Database Transactions</a></h3>
<p>
به منظور اجرای مجموعه ای از علملیات طی یک تراکنش بر روی پایگاه داده، Laravel متد transaction را ارائه می دهد. در صورتی که در طول اجرای تابع Closure متد transaction خطا رخ دهد، تراکنش به صورت خودکار به حالت قبلی برگردانده می شود (rollback). و اگر با موفقیت اجرا شد، تراکنش به صورت خودکار تایید ثبت (commit) می گردد.
</p>
<p>
به هنگام استفاده از متد transaction لزومی ندارد نگران بازگرداندن یا تایید ثبت تراکنش به صورت دستی باشید:
</p>

<pre><code class="language-php  line-numbers">DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
});
</code></pre>
<p>
نکته: هرنوع اثتثنائی که از درون Closure تعریف شده در متد  transaction رخ بدهد ، باعث RollBack شدن transaction بصورت خودکار میشود .
</p>

<br>
<h3>اداره کردن  deadlock </h3>
<p>
متد transaction  یک آرگومان اختیاری دوم را می پذیرد که تعداد دفعاتی را که یک transaction  باید در هنگام وقوع یک بن بست (deadlock) انجام شود، تعیین می کند. هنگامی که این تلاش ها به ثمر نرسید، یک استثنا رخ خواهد داد :
</p>

<pre><code class="language-php  line-numbers">DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);

    DB::table('posts')->delete();
}, 5);
</code></pre>

<br>
<h3>راه اندازی و اجرای Transactions  به صورت دستی </h3>
<p>
اگر می خواهید یک تراکنش را به صورت دستی اجرا کرده و کنترل کامل بر عملیات rollback (بازگشت تراکنش به حالت قبلی) و commit (تایید ثبت تراکنش)، می توانید متد begin Transaction را بر روی DB facade فراخوانی نمایید:
</p>


<pre><code class="language-php  line-numbers">DB::beginTransaction();
</code></pre>

<p>
می توانید تراکنش را به وسیله ی متد rollback به حالت قبلی برگردانید (به عقب برگردانید):
</p>

<pre><code class="language-php  line-numbers">DB::rollBack();
</code></pre>

<p>
در پایان می توانید تراکنش را به وسیله ی متد commit تایید ثبت نمایید:
</p>

<pre><code class="language-php  line-numbers">DB::commit();
</code></pre>

<blockquote>
متدهای مربوط به تراکنش façade DB همچنین در تراکنش های query builder و Eloquent ORM کاربرد دارد (می توان از آن ها برای کنترل تراکنش های query builder و Eloquent استفاده کرد).
</blockquote>
