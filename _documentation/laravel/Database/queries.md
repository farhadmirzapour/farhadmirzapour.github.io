---
layout: documentation-laravel
title:   آشنایی با Query Builder
cattitle: اصول آموزش Laravel
date:   2017-12-26 20:37:42 +0330
jdate: سه شنبه 5 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/Database
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/collections <br> www.tahlildadeh.com/ArticleDetails/آموزش-ابزار-Query-Builder-در-Laravel
---
<p>
ابزار کوئری ساز یا به انگلیسی Query Builder یک interface بهینه و کارآمد برای ایجاد و اجرای کوئری جهت پرس و جو از پایگاه داده فراهم می کند. این ابزار قابلیت اجرای غالب عملیات مورد نظر در پایگاه داده ی اپلیکشن را دارا بوده و نیز برای تمامی سیستم های بانک اطلاعاتی که مورد پشتیبانی لاراول است قابل استفاده می باشد.
</p>
<p>
query builder چارچوب نرم افزاری لاراول (جهت گنجاندن مقادیر در query ها) از PDO parameter binding برای محافظت از اپلیکیشن تحت وب شما در برابر حملات SQL injection بهره می گیرد. از اینرو لزومی ندارد رشته های ارسالی به عنوان پارامتر (binding ها) را برای کسب اطمینان از عدم وجود کدهای مخرب چک کنید.
</p>

<br>
<h3>واکشی تمامی سطرهای یک جدول</h3>
<p>
برای نوشتن کوئری، ابتدا متد table را در façade DB درج می کنیم. متد table یک نمونه ی query builder از جدول مورد پرس و جو برگردانده و بدین وسیله به شما امکان می دهد قیود (constraint های) بیشتری را به صورت زنجیره ای به کوئری الحاق کنید و در نهایت نتایج مد نظر را در خروجی دریافت نمایید. در مثال زیر با فراخوانی متد get کلیه ی رکوردها را از جدول مورد نظر استخراج می کنیم:
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
        $users = DB::table('users')->get();

        return view('user.index', ['users' => $users]);
    }
}
</code></pre>

<p>
متد get  یک Illuminate \ Support \ Collection را که شامل نتایجی است که در آن هر نتیجه یک نمونه شی از کلاس PHP StdClass می باشد. می توانید به مقدار هر ستون به صورت یک property (از شی) دسترسی داشته باشید (می توان با دسترسی به ستون به صورت یکproperty از شی مورد نظر، به مقدار آن ستون دست یافت):
</p>

<pre><code class="language-php  line-numbers">foreach ($users as $user) {
    echo $user->name;
}
</code></pre>

    
<br>
<h3>بازیابی تنها یک سطر / ستون از جدول مورد نظر</h3>

<p>
برای دسترسی به تنها یک سطر از جدول مورد نظر، می توان متد first را بکار برد. این متد تنها یک شی StdClass را در خروجی برمی گرداند:
</p>

<pre><code class="language-php  line-numbers">$user = DB::table('users')->where('name', 'John')->first();

echo $user->name;
</code></pre>

<p>
اگر به کل یک سطر نیازی نیست، می توانید یک تک مقدار را با فراخوانی متد value از رکورد واکشی کنید. این متد مقدار ستون را به صورت مستقیم در خروجی برمی گرداند:
</p>

<pre><code class="language-php  line-numbers">$email = DB::table('users')->where('name', 'John')->value('email');
</code></pre>

    
<br>
<h3>بازیابی  لیستی از مقادیر ستون</h3>
<p>
اگر می خواهید در خروجی یک آرایه داشته باشید که تمامی مقادیر یک ستون را دربرگیرد، می توانید متد pluck را بکار ببرید. در این مثال آرایه ای ازtitle ها را در خروجی دریافت می کنیم:
</p>

<pre><code class="language-php  line-numbers">$titles = DB::table('roles')->pluck('title');

foreach ($titles as $title) {
    echo $title;
}
</code></pre>

<p>
همچنین می توانید یک ستون کلید (key column) اختصاصی برای آرایه ی خروجی (نتیجه) مشخص نمایید:
</p>
    
<pre><code class="language-php  line-numbers">$roles = DB::table('roles')->pluck('title', 'name');

foreach ($roles as $name => $title) {
    echo $title;
}
</code></pre>


<br>
<h3>واکشی سطرها از جدول به صورت تکه تکه (متد chunk)</h3>

<p>
در شرایطی که با هزاران رکورد سروکار دارید (مانند سناریوی واکشی هزاران رکورد)، توصیه می کنیم متد chunk را بکار ببرید. این متد در هر برهه ی زمانی تنها تکه ی کوچکی از داده ها را بازگردانده و سپس هر تکه را برای پردازش به تابع Closure می دهد. این متد برای نوشتن دستوراتArtisan که هزاران رکورد را یکجا پردازش می کنند، بسیار مفید واقع می شود. در نمونه ی زیر کل جدول users را در قالب رکورد صدتایی به صورت جدا پردازش می کنیم:
</p>
    
<pre><code class="language-php  line-numbers">DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});
</code></pre>

<p>
جهت توقف پردازش رکوردها به این شکل (پردازش رکوردها در گروه های صد تایی) کافی است مقدار false را از تابع Closure بازگردانید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    // Process the records...

    return false;
});
</code></pre>

    
<br>
<h3>توابع تجمعی (Aggregate)</h3>
<p>
query builder توابع متعددی برای اجرای عملیات مختلف بر روی یک کل را فراهم می آورد. این توابع یک مقدار را بر اساس داده های یک ستون محاسبه و بر می گرداند. توابع تجمعی عبارتند از: count، max، min، avg و sum. می توانید هر یک از این متدها را پس از ساخت کوئری خود فراخوانی نمایید:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->count();

$price = DB::table('orders')->max('price');
</code></pre>

<p>
می توانید این متدها را با دیگر عبارات مانند دستورات شرطی where ترکیب نموده و کوئری های پیچیده تری بسازید:
</p>
    
<pre><code class="language-php  line-numbers">$price = DB::table('orders')
                ->where('finalized', 1)
                ->avg('price');
</code></pre>
    
<br>
<h3>دستور Select</h3>
<p>
گاهی لزومی ندارد تمامی ستون های یک جدول را از پایگاه داده واکشی کنیم. متد select به شما این امکان را می دهد تا یک دستور Select سفارشی برای کوئری نوشته و تنها ستون های دلخواه را در خروجی دریافت نمایید:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->select('name', 'email as user_email')->get();
</code></pre>

<p>
متد distinct به شما این امکان را می دهد تا فقط ستون های غیر تکراری را در خروجی لحاظ نمایید:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->distinct()->get();
</code></pre>

<p>
چنانچه از قبل یک نمونه از query builder دارید و اکنون می خواهید یک ستون جدید به دستور select آن اضافه کنید، در آن صورت می توانید از متد addSelect استفاده نمایید:
</p>

<pre><code class="language-php  line-numbers">$query = DB::table('users')->select('name');

$users = $query->addSelect('age')->get();
</code></pre>

<br>
<h3>عبارت های خالص (raw expression)</h3>
<p>
گاهی لازم می شود یک عبارت خالص در کوئری مورد نظرتان بکار ببرید. عبارت خالص به صورت رشته در کوئری تزریق می شوند. از این رو بایستی حین تزریق عبارت های خالص از ایجاد حفره ی امنیتی و فراهم آوردن زمینه برای حملات SQL injection خودداری نمایید.
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                     ->select(DB::raw('count(*) as user_count, status'))
                     ->where('status', '<>', 1)
                     ->groupBy('status')
                     ->get();
</code></pre>

<blockquote>
<p>
به منظور ایجاد یک عبارت خالص، می توان متد DB::raw را بکار برد (مورد استفاده ی این متد گنجاندن یک کوئری در دل کوئری دیگر است. لازم به ذکر است که این متد کوئری را از نظر کدهای مخرب چک نمی کند):
</p>
</blockquote>

    
<br>
<h3> متدهای Raw</h3>
<p>
به جای استفاده از DB :: raw، شما همچنین ممکن است از روش های زیر برای قرار دادن یک عبارت خام به بخش های مختلف درخواست خود استفاده کنید.
</p>

<br>
<h3>متد selectRaw</h3>
<p>
متد selectRaw را می توان به جای (DB :: raw (...) استفاده کرد). این روش در آرگومان دوم خود یک آرایه اختیاری از مقادیر bindings  را می پذیرد:
</p>

<pre><code class="language-php  line-numbers">$orders = DB::table('orders')
                ->selectRaw('price * ? as price_with_tax', [1.0825])
                ->get();
</code></pre>

    
<br>
<h3>متد whereRaw <span class="token operator">/</span> orWhereRaw</h3>
<p>
روشهای whereRaw و orWhereRaw را می توان برای تزریق یک جمله Raw به کار برد. این روش در آرگومان دوم خود یک آرایه اختیاری از مقادیر bindings  را می پذیرد:
</p>

<pre><code class="language-php  line-numbers">$orders = DB::table('orders')
                ->whereRaw('price > IF(state = "TX", ?, 100)', [200])
                ->get();
</code></pre>

    
<br>
<h3>متد havingRaw <span class="token operator">/</span> orHavingRaw</h3>

 <p>
متدهای havingRaw و orHavingRaw جهت نشاندن یک رشته Raw به عنوان مقدار having استفاده می شود :
 </p>


<pre><code class="language-php  line-numbers">$orders = DB::table('orders')
                ->select('department', DB::raw('SUM(price) as total_sales'))
                ->groupBy('department')
                ->havingRaw('SUM(price) > 2500')
                ->get();
</code></pre>

    
<br>
<h3> متد orderByRaw</h3>
 <p>
متد orderByRaw  جهت نشاندن یک رشته Raw به عنوان مقدار order by استفاده می شود :
 </p>

<pre><code class="language-php  line-numbers">$orders = DB::table('orders')
                ->orderByRaw('updated_at - created_at DESC')
                ->get();
</code></pre>


<br>
<h3>دستور Inner Join</h3>
<p>
از query builder می توان جهت اجرای دستورات مختلف join بهره گرفت. برای اجرای یک عملیات ساده ی inner join اس کیو ال، کافی است متدjoin را بر روی نمونه ی ایجاد شده از جدول (نمونه ی query builder) فراخوانی کنید. اولین آرگومان ارسالی به این متد اسم جدولی است که می خواهید به آن عملیات پیوند را انجام دهید. دیگر آرگومان های پاس داده شده قیود و constraint های اعمال شده بر روی ستون ها در عملیات join را تعیین می کنند. همان طور که در این مثال مشاهده می کنید، در قالب تنها یک کوئری می توان به چندین جدول پیوند انجام داد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
            ->join('contacts', 'users.id', '=', 'contacts.user_id')
            ->join('orders', 'users.id', '=', 'orders.user_id')
            ->select('users.*', 'contacts.phone', 'orders.price')
            ->get();
</code></pre>

    
<br>
<h3>دستور Left Join Clause</h3>

<p>
اگر می خواهید بجای عملیات inner join، عملیات left join را بر روی جدولی اجرا کنید، کافی است متد leftJoin را به صورت زیر فراخوانی نمایید. لازم به ذکر است که متد نام برده از نظر نوع و تعدادی ورودی (signature) با متد join یکسان است:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
            ->leftJoin('posts', 'users.id', '=', 'posts.user_id')
            ->get();
</code></pre>

    
<br>
<h3>دستور Cross Join Clause</h3>
<p>
جهت اجرای عملیات cross join، کافی است متد crossJoin را فراخوانی کرده و اسم جدولی که می خواهید عملیات cross join بر روی آن اجرا شود را به عنوان آرگومان به آن پاس دهید. نتیجه ی عملیات cross join ترکیبی است که از قرار گرفتن هر سطر یا رکورد از جدول اول در کنار تمامی سطرهای جدول دوم بدست می آید، به عبارتی دیگر نتیجه cross join حاصلضرب دکارتی دو جدول در هم است.
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('sizes')
            ->crossJoin('colours')
            ->get();
</code></pre>

    
<br>
<h3>دستورات پیچیده ی join</h3>
<p>
می توانید دستورات join پیچیده تری بنویسید. برای شروع یک تابع Closure به عنوان آرگومان دوم به متد join ارسال کنید. این تابع یک شیJoinClause به عنوان پارامتر ورودی می گیرد که به شما اجازه می دهد constraint ها و محدودیت هایی را در دستور join تعریف نمایید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
        ->join('contacts', function ($join) {
            $join->on('users.id', '=', 'contacts.user_id')->orOn(...);
        })
        ->get();
</code></pre>

<p>
می توانید دستورات join پیچیده تری بنویسید. برای شروع یک تابع Closure به عنوان آرگومان دوم به متد join ارسال کنید. این تابع یک شیJoinClause به عنوان پارامتر ورودی می گیرد که به شما اجازه می دهد constraint ها و محدودیت هایی را در دستور join تعریف نمایید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
        ->join('contacts', function ($join) {
            $join->on('users.id', '=', 'contacts.user_id')
                 ->where('contacts.user_id', '>', 5);
        })
        ->get();
</code></pre>


<br>
<h3>دستور Unions</h3>
<p>
query builder به شما امکان می دهد تا نتیجه ی حاصل از اجرای دو کوئری را به راحتی با یکدیگر ادغام یا متحد نمایید. برای مثال یک کوئری اولیه ایجاد می کنیم و سپس با فراخوانی متد union، آن را با یک کوئری دیگر ترکیب می کنیم:
</p>

<pre><code class="language-php  line-numbers">$first = DB::table('users')
            ->whereNull('first_name');

$users = DB::table('users')
            ->whereNull('last_name')
            ->union($first)
            ->get();
</code></pre>

<blockquote>
می توان متد unionAll را برای لحاظ کردن مقادیر تکراری صدا زد. این متد از نظر signature با متد union تفاوتی ندارد.
 </blockquote>

    
<br>
<h3>دستور Where</h3>
<p>
به منظور افزودن دستورات where به کوئری، می توانید متد where را بر روی نمونه ی query builder صدا بزنید. در ساده ترین نوع فراخوانی متد where، ارسال سه آرگومان الزامی می باشد. اولین آرگومان اسم ستون می باشد. دومین آرگومان یکی از عملگرهای مورد پشتیبانی پایگاه داده است. سومین آرگومان مقداری است که با ستون مورد نظر از جدول مقایسه می شود.
</p>
<p>
در مثال زیر بررسی می کنیم آیا مقدار ستون "votes" برابر 100 هست یا خیر:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->where('votes', '=', 100)->get();
</code></pre>

<p>
به منظور سادگی بیشتر، اگر فقط می خواهید بررسی کنید مقدار یک ستون برابر با یک مقدار مشخص هست یا خیر، می توانید مقدار دلخواه را به طور مسقیم به عنوان آرگومان دوم به متد where ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->where('votes', 100)->get();
</code></pre>

<p>
مسلما می توان از دیگر عملگرهای مورد پشتیبانی پایگاه داده در عبارت where استفاده کرد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->where('votes', '>=', 100)
                ->get();

$users = DB::table('users')
                ->where('votes', '<>', 100)
                ->get();

$users = DB::table('users')
                ->where('name', 'like', 'T%')
                ->get();
</code></pre>

<p>
می توان آرایه ای از شرط ها را به تابع where ارسال کرد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->where([
    ['status', '=', '1'],
    ['subscribed', '<>', '1'],
])->get();
</code></pre>

    
<br>
<h3>دستورات Or</h3>
<p>
می توان علاوه بر افزودن constraint های عبارت where (قیودی که در قالب عبارت where به کوئری اعمال می شوند) به صورت زنجیره ای، عبارتor را نیز به کوئری اضافه کرد. query builder این کار با ارائه ی متد orWhere امکان پذیر ساخته است. متد مزبور از نظر آرگومان های ورودی با متد where تفاوتی ندارد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->where('votes', '>', 100)
                    ->orWhere('name', 'John')
                    ->get();
</code></pre>

    
<br>
<h3>دیگر دستورات where</h3>

<p><strong>دستور whereBetween</strong></p>
<p>
متد whereBetween بررسی می کند آیا مقدار ستون مورد نظر بین دو رینج مشخص هست یا خیر:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereBetween('votes', [1, 100])->get();
</code></pre>

<br>
<p><strong>دستور whereNotBetween</strong></p>
<p>
این متد بررسی می کند آیا مقدار ستون مورد نظر خارج از رینج یا دامنه ی دو مقدار مشخص شده هست یا خیر (ستون هایی که مقادیر آن ها داخل رینج عددی مشخص شده نباشد را جستجو می کند):
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereNotBetween('votes', [1, 100])
                    ->get();
</code></pre>

<br>
<p><strong>دستور whereIn / whereNotIn</strong></p>
<p>
متد wherein ستون هایی را که مقدار آن ها داخل آرایه ی ورودی باشد را جستجو می کند:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereIn('id', [1, 2, 3])
                    ->get();
</code></pre>

<p>
متد whereNotIn رکوردهایی را جستجو می کند که مقادیر آن ها محدود به آرایه ی ورودی نباشد (مقدار آن ها در آرایه ی ارسالی به عنوان آرگومان نباشد):
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereNotIn('id', [1, 2, 3])
                    ->get();
</code></pre>

<br>
<p><strong>دستور whereNull / whereNotNull</strong></p>
<p>
متد whereNull ستون هایی را برمی گرداند که مقدار آن ها NULL باشد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereNull('updated_at')
                    ->get();
</code></pre>

<p>
متد whereNotNull رکوردهایی را جستجو و برمی گرداند که مقدار آن ها NULL نباشد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                    ->whereNotNull('updated_at')
                    ->get();
</code></pre>

<br>
<p><strong>دستورات whereDate / whereMonth / whereDay / whereYear / whereTime</strong></p>
<p>
برای مقایسه مقادیر ستون با یک تاریخ، از متد whereDate استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereDate('created_at', '2016-12-31')
                ->get();
</code></pre>

<p>
برای مقایسه مقادیر ستون با ماه یک سال، از متد whereMonth استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereMonth('created_at', '12')
                ->get();
</code></pre>

<p>
برای مقایسه مقادیر ستون با روز یک سال، از متد whereDay استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereDay('created_at', '31')
                ->get();
</code></pre>

<p>
برای مقایسه مقادیر ستون با  یک سال، از متد whereYear استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereYear('created_at', '2016')
                ->get();
</code></pre>

<p>
برای مقایسه مقادیر ستون با  یک زمان، از متد whereTime استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereTime('created_at', '=', '11:20')
                ->get();
</code></pre>

<br>
<p><strong>whereColumn</strong></p>
<p>
متد whereColumn را می توان جهت مقایسه و کسب اطمینان از برابر بودن دو ستون مورد استفاده قرار داد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereColumn('first_name', 'last_name')
                ->get();
</code></pre>

<p>
همچنین می توان یک عملگر مقایسه ای به متد پاس داد:
</p>
    
<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereColumn('updated_at', '>', 'created_at')
                ->get();
</code></pre>

<p>
متد whereColumn نیز می تواند آرایه ای از چندین شرط را به عنوان آرگومان بپذیرد. این شرط ها توسط عملگر and با یکدیگر پیوند می خورند:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->whereColumn([
                    ['first_name', '=', 'last_name'],
                    ['updated_at', '>', 'created_at']
                ])->get();
</code></pre>

<br>
<h3>کوئری های تودرتو</h3>

<p>
گاهی اوقات ملزوم به ساخت دستورات شرطی (where) پیچیده تری می شوید. از جمله می توان به عبارت های "where exists" (برای بررسی وجود ستون) یا کوئری های تودرتو اشاره کرد. query builder فریم ورک Laravel انجام این کار را هم برای شما آسان می سازد. برای شروع، به مثال ساده ای که در آن where های تودرتو نوشته و constraint ها داخل پرانتز گروه بندی شده اند می پردازیم:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
            ->where('name', '=', 'John')
            ->orWhere(function ($query) {
                $query->where('votes', '>', 100)
                      ->where('title', '<>', 'Admin');
            })
            ->get();
</code></pre>

<p>
همان طور که مشاهده می کنید، ارسال تابع Closure به متد orWhere به عنوان آرگومان به query builder اعلان کرده که باید محدودیت ها را گروه بندی کرده و داخل پرانتز قرار دهد. Closure یک نمونه query builder به عنوان ورودی گرفته که شما با استفاده از آن می توانید محدودیت هایی که باید به صورت گروه بندی شده در پرانتز قرار گیرد را اعمال کنید. مثال فوق کوئری زیر را تولید می کند:
</p>

<pre><code class="language-php  line-numbers">select * from users where name = 'John' or (votes > 100 and title <> 'Admin')
</code></pre>


<br>
<h3>متد whereExists</h3>
<p>
متد whereExists به شما اجازه می دهد عبارت های where exists بنویسید. این متد یک تابع Closure به عنوان آرگومان می گیرد. خود این تابع یک نمونه ی query builder به عنوان ورودی دریافت کرده که با استفاده از آن کوئری را تعریف می کنید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
            ->whereExists(function ($query) {
                $query->select(DB::raw(1))
                      ->from('orders')
                      ->whereRaw('orders.user_id = users.id');
            })
            ->get();
</code></pre>

<p>
کوئری فوق دستور ساده ی SQL زیر را تولید می کند:
</p>

<pre><code class="language-php  line-numbers">select * from users
where exists (
    select 1 from orders where orders.user_id = users.id
)
</code></pre>

<p><a name="json-where-clauses"></a></p>
    
<br>
<h3>کوئری گرفتن از ستون های از نوع JSON</h3>

<p>
لاراول به شما امکان می دهد از ستون های از نوع JSON کوئری بگیرید. در حال حاضر این قابلیت تنها در پایگاه داده ی MySQL ویرایش 5.7 وPostgres پشتیبانی می شود. برای گرفتن کوئری از ستون از نوع JSON، می بایست عملگر -> را مورد استفاده قرار دهید:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->where('options->language', 'en')
                ->get();

$users = DB::table('users')
                ->where('preferences->dining->meal', 'salad')
                ->get();
</code></pre>


<br>
<h3>متد orderBy</h3>
<p>
متد orderBy به شما امکان می دهد تا نتایج حاصل از اجرای یک کوئری را مرتب سازی کنید. اولین آرگومان ارسالی به این متد باید اسم ستونی باشد که می خواهید نتایج بر اساس آن مرتب شوند، در حالی که آرگومان دوم ترتیب مرتب سازی اعم از نزولی (asc) یا صعودی (desc) را کنترل می کند:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->orderBy('name', 'desc')
                ->get();
</code></pre>

    
<br>
<h3>متد های latest / oldest</h3>
<p>
متدهای  latest and oldest  به شما اجازه می دهند نتایج را براساس تاریخ مرتب کنید. به طور پیش فرض نتایج براساس ستون created_at مرتب می شوند. همچنین می توانید نام ستونی که می خواهید نتایج براساس آن مرتب شوند را مشخص نمایید :
</p>

<pre><code class="language-php  line-numbers">$user = DB::table('users')
                ->latest()
                ->first();
</code></pre>

    
<br>
<h3>متد inRandomOrder</h3>
<p>
متد inRandomOrder نتایج یک کوئری را به صورت تصادفی مرتب سازی می کند. در مثال زیر با فراخوانی این متد یک کاربر را به طور تصادفی از جدول واکشی می کنیم:
</p>

<pre><code class="language-php  line-numbers">$randomUser = DB::table('users')
                ->inRandomOrder()
                ->first();
</code></pre>

    
<br>
<h3>متدهای groupBy / having</h3>
<p>
توابع groupByو having را می توان جهت گروه بندی نتایج کوئری مورد استفاده قرار داد. متد having از نظر نوع و تعداد پارامتر ورودی (signature) با متد where یکسان می باشد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->groupBy('account_id')
                ->having('account_id', '>', 100)
                ->get();
</code></pre>

<p>
همچنین می توانید چندیدن ستون را جهت گروه بندی انتخاب نمایید :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->groupBy('first_name', 'status')
                ->having('account_id', '>', 100)
                ->get();
</code></pre>

<br>
<h3>skip / take</h3>

<p>
به منظور محدود سازی نتایج حاصل از اجرای کوئری یا لحاظ نکردن تعداد خاصی از نتایج در کوئری (OFFSET)، می توان متدهای skip و take را مورد استفاده قرار داد:
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')->skip(10)->take(5)->get();

</code></pre>

<p>
همچنین می توانید از متدهای limit و offset نیز استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$users = DB::table('users')
                ->offset(10)
                ->limit(5)
                ->get();
</code></pre>

<br>
<h3>دستورات شرطی</h3>
<p>
گاهی می خواهید یک شرط تنها زمانی به یک کوئری اعمال شود که شرط دیگری صادق باشد. به عنوان مثال ممکن است بخواهید یک شرط where تنها در صورتی اعمال شود که درخواست ورودی دارای مقدار خاصی باشد. این کار را می توانید با فراخوانی متد when انجام دهید:
</p>

<pre><code class="language-php  line-numbers">$role = $request->input('role');

$users = DB::table('users')
                ->when($role, function ($query) use ($role) {
                    return $query->where('role_id', $role);
                })
                ->get();
</code></pre>

<p>
متد when تنها زمانی تابع Closure را اجرا می کند که اولین پارامتر true باشد. در صورتی که اولین پارامتر false باشد، تابع Closure اجرا نخواهد شد.
</p>

<p>
همچنین می توان تابع Closure دیگری به عنوان آرگومان سوم مشخص کرد که در صورتیکه اولین پارامتر false  باشد آن دستورات اجرا شوند :
</p>

<pre><code class="language-php  line-numbers">$sortBy = null;

$users = DB::table('users')
                ->when($sortBy, function ($query) use ($sortBy) {
                    return $query->orderBy($sortBy);
                }, function ($query) {
                    return $query->orderBy('name');
                })
                ->get();
</code></pre>

<br>
<h3>متد Inserts</h3>
<p>
query builder همچنین یک متد به نام insert ارائه می کند که توسط آن می توانید رکوردهایی را داخل جدول درج نمایید. متد insert آرایه ای از اسم و مقادیر ستون ها را به عنوان ورودی گرفته و در جدول درج می کند:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->insert(
    ['email' => 'john@example.com', 'votes' => 0]
);
</code></pre>

<p>
می توان با یکبار فراخوانی تابع insert تعداد زیادی رکورد را در جدول وارد کرد. برای این منظور کافی است آرایه ای از آرایه ها را به عنوان آرگومان به تابع ذکر شده ارسال نمایید. هر آرایه نشانگر یک سطر است که در جدول درج می شود:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->insert([
    ['email' => 'taylor@example.com', 'votes' => 0],
    ['email' => 'dayle@example.com', 'votes' => 0]
]);
</code></pre>

    
<br>
<h3> ستون Auto-Incrementing IDs</h3>

<p>
اگر جدول مورد نظر دارای یک ستون خود افزاینده id است، در آن صورت می توان با فراخوانی متد insertGetId یک رکورد را وارد جدول کرده و سپس ID آن را بازیابی کنید:
</p>

<pre><code class="language-php  line-numbers">$id = DB::table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0]
);
</code></pre>

<blockquote>
اگر از پایگاه داده ی PostgreSQL استفاده می کنید، در آن صورت بایستی اسم ستون خود افزاینده (auto-incrementing یا کلید اصلی) را id تنظیم کنید (در واقع این پایگاه داده انتظار دارد اسم ستون نام برده id باشد). اگر می خواهید id را از sequence دیگری بازیابی کنید، آنگاه بایستی اسم sequence را به عنوان آرگومان دوم به متد insertGetId ارسال نمایید.
</blockquote>


<br>
<h3>دستور Updates</h3>
<p>
query builder با ارائه ی متد update به شما اجازه می دهد رکوردهای جاری را بروز رسانی کنید. این متد مانند insert، آرایه ای از جفت اسم و مقادیر ستون (column / value pair) را برای بروز رسانی به عنوان ورودی می پذیرد. با استفاده از دستورات شرطی where می توانید بر روی کوئری update قید اعمال کرده و آن را محدود نمایید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);
</code></pre>


<br>
<h3>بروز رسانی ستون هایی از نوع JSON</h3>
<p>
هنگام به روز رسانی ستون JSON، شما باید از ->  برای دسترسی به کلید مناسب در شی JSON استفاده کنید. این عملیات تنها در پایگاه های داده پشتیبانی می شود که ستون های JSON را پشتیبانی می کنند:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')
            ->where('id', 1)
            ->update(['options->enabled' => true]);
</code></pre>


<br>
<h3>توابع Increment و Decrement</h3>

<p>
query builder چارچوب نرم افزاری لاراول متدهایی را هم برای افزایش یا کاهش مقدار ستون مورد نظر فراهم می کند. این دو متد صرفا یک نوع میان برای دستور update محسوب می شوند.
</p>

<p>
دو متد نام برده حداقل یک آرگومان می پذیرند: 1. اسم ستونی که بروز آوری می شود. در صورت تمایل می توان یک آرگومان دوم به تابع ارسال کرد که واحد افزایش یا کاهش مقدار ستون را مشخص می کند:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->increment('votes');

DB::table('users')->increment('votes', 5);

DB::table('users')->decrement('votes');

DB::table('users')->decrement('votes', 5);
</code></pre>

<p>
می توان ستون های بیشتری را برای بروز رسانی مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->increment('votes', 1, ['name' => 'John']);
</code></pre>


<br>
<h3>متد Deletes</h3>
<p>
query builder همچنین یک متد به نام delete فراهم می کند که توسط آن می توان رکوردهای دلخواه را از پایگاه داده حذف کرد:
</p>

<p>
می توانید با اضافه کردن عبارت های شرطی where به کوئری، دستورات delete را محدود نمایید (قیودی را بر کوئری اعمال کنید):
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->delete();

DB::table('users')->where('votes', '>', 100)->delete();
</code></pre>

<p>
اگر می خواهید جدول را کاملا خالی کنید (تمامی سطرهای آن را حذف و ستون شمارشی ID را به صفر بازگردانید)، کافی است متد truncate را صدا بزنید:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->truncate();
</code></pre>

<br>
<h3>قفل گذاری بدبینانه (Pessimistic Locking)</h3>
<p>
query builder با ارائه ی تعدادی تابع به شما امکان می دهد به راحتی بر روی دستورات select خود قفل های pessimistic اعمال کنید. برای اجرای دستور با یک قفل مشترک (shared lock)، می توان متد sharedLock را بر روی کوئری صدا زد. قفل مشترک مانع از این می شود که سطرهای انتخابی پیش از اتمام تایید ثبت و commit شدن کامل تراکنش، ویرایش شوند (قفل اشتراکی یا shared lock به زمان اطلاق می شود که دو تراکنش مجوز در سطح خواندن / read access دریافت کنند). در واقع با استفاده از متد sharedLock یک قفل pessimistic برای دستورSelect ایجاد می کنیم:
</p>


<pre><code class="language-php  line-numbers">DB::table('users')->where('votes', '>', 100)->sharedLock()->get();
</code></pre>

<p>
می توانید بجای متد نام برده از lockForUpdate استفاده کنید. این متد حین اجرای دستور select مانع از بروز رسانی و ویرایش سطرها می شود:
</p>

<pre><code class="language-php  line-numbers">DB::table('users')->where('votes', '>', 100)->lockForUpdate()->get();
</code></pre>