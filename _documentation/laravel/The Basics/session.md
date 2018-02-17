---
layout: documentation
title:    استفاده از  Session
cattitle: اصول آموزش Laravel
date:   2018-02-09 14:29:42 +0330
jdate: جمعه 20 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/session <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/259/آموزش-http-session-در-لاراول
---
<p>
از آنجا که برنامه‌های کاربردی تحت وب HTTP به صورت stateless یا بی ثبات هستند و اطلاعات کاربر را حفظ نمی‌کنند، استفاده از سشن (session) این امکان را فراهم می‌کند که بتوان اطلاعات کاربر را در طول چند درخواست ذخیره کرد. لاراول در حالت پیش‌فرض دارای انواع مختلفی از سشن‌ها به صورت backend است که از طریق یک API ساده و یکپارچه می‌توان به آن‌ها دسترسی داشت. لاراول از backendهای پرطرفدار مثل Memcached و Redis پشتیبانی می‌کند.
</p>

<p>
<a name="configuration"></a>
</p>

<br>
<h3>پیکربندی تنظیمات session </h3>

<p>
فایل پیکربندی سشن در config/session.php قرار دارد. حتماً باید گزینه‌های در دسترس موجود در این فایل را بررسی کنید. به صورت پیش‌فرض پیکربندی تنظیمات لاراول از درایور سشن file استفاده می‌کند که برای بسیاری از برنامه‌ها کارایی لازم را ارائه می‌دهد. در برنامه‌های تولید شده (production)، برای دستیابی به کارایی بهتر و سریعتر می‌توانید از درایور memcached یا redis استفاده کنید.
</p>

<p>
گزینه پیکربندی driver سشن مشخص می‌کند که داده‌های سشن برای هر درخواست در کجا ذخیره می‌شوند. لاراول به صورت پیش‌فرض دارای چند درایور بزرگ و مهم است:
</p>

<div class="content-list">
<ul>
<li><code class=" language-php">file</code> - سشن‌ها در آدرس storage/framework/sessions ذخیره می‌شوند..</li>
<li><code class=" language-php">cookie</code> - سشن‌ها در کوکی‌های امن و رمزگذاری شده ذخیره می‌شوند..</li>
<li><code class=" language-php">database</code> - سشن‌ها در پایگاه داده‌ مربوط به برنامه کاربردی ذخیره می‌شوند..</li>
<li><code class=" language-php">memcached</code> / <code class=" language-php">redis</code> - سشن‌ها در یکی از storeهای سریع مبتنی بر کش موجود ذخیره می‌شوند.</li>
<li><code class=" language-php"><span class="token keyword">array</span></code> - سشن‌ها در آرایه PHP ذخیره می‌شوند و پایدار نخواهند بود.</li>
</ul>
</div>
<blockquote class="has-icon tip">
از آرایه driver در زمان انجام تست استفاده می‌شود و مانع از ذخیره داده‌ها به صورت دائمی در سشن می‌شود.
</blockquote>

<p>
<a name="driver-prerequisites"></a>
</p>

<br>
<h3>پیش نیازهای درایور session در لاراول</h3>

<br>
<h3>پایگاه داده</h3>

<p>
هر زمان که از درایور سشن database استفاده می‌کنید، باید یک جدول ایجاد کنید که حاوی آیتم‌های سشن باشد. در مثال زیر یک نمونه تعریف Schema برای این جدول را مشاهده می‌کنید:
</p>

<pre><code class="language-php  line-numbers">Schema::create('sessions', function ($table) {
    $table->string('id')->unique();
    $table->unsignedInteger('user_id')->nullable();
    $table->string('ip_address', 45)->nullable();
    $table->text('user_agent')->nullable();
    $table->text('payload');
    $table->integer('last_activity');
});
</code></pre>


<p>
می‌توان از دستور آرتیسان session:table برای ایجاد این migration استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">php artisan session:table

php artisan migrate
</code></pre>


<br>
<h3>Redis</h3>

<p>
قبل از استفاده از سشن‌های Redis در لاراول، باید پکیج predis/predis (~1.0) را با استفاده از Composer نصب کنید. می‌توانید کانکشن‌های Redis را در فایل پیکربندی database پیکربندی کنید. در درون فایل پیکربندی session ، از گزینه connection می‌توان برای مشخص کردن کانکشن Redis مورد استفاده توسط سشن، استفاده کرد.
</p>


<p>
<a name="using-the-session"></a>
</p>


<br>
<h3><a href="#using-the-session">بازیابی داده‌ها از session در لاراول</a></h3>

<p>
دو روش اصلی برای کار با داده‌های سشن در لاراول وجود دارد؛ روش اول استفاده از تابع کمکی session عمومی و روش دوم استفاده از یک نمونه Request است. در ابتدا، دسترسی به سشن از طریق یک نمونه Request را بررسی می‌کنیم که می‌تواند بر روی یک متد کنترلر اعلان نوع یا type-hint شود. توجه کنید که وابستگی‌های متد کنترلر به صورت خودکار از طریق service container لاراول تزریق می‌شوند:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class UserController extends Controller
{
    /**
     * Show the profile for the given user.
     *
     * @param  Request  $request
     * @param  int  $id
     * @return Response
     */
    public function show(Request $request, $id)
    {
        $value = $request->session()->get('key');

        //
    }
}
</code></pre>


<p>
در زمان بازیابی یک مقدار از سشن، می‌توان یک مقدار پیش‌فرض را نیز به عنوان آرگومان دوم به متد get انتقال داد. در صورتی که کلید مشخص شده در سشن موجود نباشد، این مقدار پیش‌فرض برگردانده می‌شود. اگر یک Closure را به عنوان مقدار پیش‌فرض به متد get انتقال دهید و کلید درخواست شده موجود نباشد، Closure اجرا شده و نتیجه آن بازگشت داده می‌شود:
</p>


<pre><code class="language-php  line-numbers">$value = $request->session()->get('key', 'default');

$value = $request->session()->get('key', function () {
    return 'default';
});
</code></pre>


<br>
<h3>استفاده از تابع کمکی session در لاراول</h3>

<p>
همچنین، می‌توان از تابع PHP عمومی session برای بازیابی و ذخیره داده‌ها در سشن استفاده کرد. زمانی که تابع کمکی session با یک آرگومان رشته‌ای واحد فراخوانی می‌شود، مقدار کلید آن سشن را باز می‌گرداند. زمانی که تابع کمکی با آرایه‌ای از جفت کلید و مقدار فراخوانی می‌شود، این مقادیر در سشن ذخیره می‌شوند:
</p>


<pre><code class="language-php  line-numbers">Route::get('home', function () {
    // Retrieve a piece of data from the session...
    $value = session('key');

    // Specifying a default value...
    $value = session('key', 'default');

    // Store a piece of data in the session...
    session(['key' => 'value']);
});
</code></pre>

<blockquote class="has-icon tip">
بین کار با سشن از طریق یک HTTP request و از طریق یک تابع کمکی session عمومی، تفاوت اندکی وجود دارد. هر دو روش از طریق متد assertSessionHas که در تمام گزینه‌های تست موجود است، قابل تست کردن هستند.
</blockquote>

<br>
<h3>بازیابی تمام داده‌‌های session</h3>

<p>
اگر بخواهید تمام داده‌ها را از سشن بازیابی کنید، باید از متد all به صورت زیراستفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$data = $request->session()->all();
</code></pre>


<br>
<h3>تعیین موجود بودن یک آیتم درون session</h3>

<p>
برای تعیین اینکه آیا یک مقدار در سشن وجود دارد یا خیر، می‌توان از متد has استفاده کرد. اگر مقدار موردنظر موجود باشد، این متد مقدار true را برمی‌گرداند، در غیر این صورت مقدار  null برگشت داده می‌شود:
</p>


<pre><code class="language-php  line-numbers">if ($request->session()->has('users')) {
    //
}
</code></pre>


<p>
برای تعیین وجود یک مقدار در سشن، حتی اگر مقدار آن null باشد، می‌توانید از متد exists استفاده کنید. اگر مقدار موجود باشد، این متد مقدار true را برمی‌گرداند:
</p>


<pre><code class="language-php  line-numbers">if ($request->session()->exists('users')) {
    //
}
</code></pre>


<p>
<a name="storing-data"></a>
</p>


<br>
<h3>ذخیره سازی داده‌ها درون session</h3>

<p>
به منظور ذخیره داده‌ها در سشن، معمولاً می‌توان از متد put یا تابع کمکی session به صورت زیر استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">// Via a request instance...
$request->session()->put('key', 'value');

// Via the global helper...
session(['key' => 'value']);
</code></pre>


<br>
<h3>push کردن مقادیر session آرایه </h3>

<p>
از متد push می‌توان برای push کردن یک مقدار جدید بر روی مقدار سشنی که در یک آرایه وجود دارد، استفاده کرد. برای مثال، اگر کلید user.teams حاوی آرایه‌ای از نام‌های تیم باشد، می‌توان یک مقدار جدید را بر روی آرایه به صورت مثال زیر push کرد:
</p>


<pre><code class="language-php  line-numbers">$request->session()->push('user.teams', 'developers');
</code></pre>


<br>
<h3>بازیابی و حذف یک مورد از session</h3>

<p>
با استفاده از متد pull می‌توان یک مورد را از سشن، توسط یک خط کد مانند مثال زیر بازیابی و حذف کرد:
</p>


<pre><code class="language-php  line-numbers">$value = $request->session()->pull('key', 'default');
</code></pre>


<p>
<a name="flash-data"></a>
</p>


<br>
<h3>Flash کردن داده‌ها برای درخواست بعدی در session</h3>

<p>
گاهی اوقات، ممکن است بخواهید آیتم‌های داده را فقط برای درخواست بعدی در سشن ذخیره کنید. این کار را می‌توان با استفاده از متد flash انجام داد. داده‌هایی که توسط این متد در سشن ذخیره می‌شود، در طول درخواست HTTP بعدی در دسترس خواهند بود و پس از آن حذف می‌شوند. استفاده از داده‌های flash شده، عمدتاً برای پیام‌های نشانگر وضعیت که عمر کوتاهی دارند، مفید است:
</p>


<pre><code class="language-php  line-numbers">$request->session()->flash('status', 'Task was successful!');
</code></pre>


<p>
اگر بخواهید، اطلاعات flash شده را برای چند درخواست بعدی حفظ کنید، می‌توانید از متد reflash استفاده کنید که تمام داده‌های flash شده را برای درخواست‌های اضافی بعدی حفظ می‌کند. برای حفظ داده‌های flash شده خاص، می‌توان از متد keep استفاده کرد:
</p>


<pre><code class="language-php  line-numbers">$request->session()->reflash();

$request->session()->keep(['username', 'email']);
</code></pre>


<p>
<a name="deleting-data"></a>
</p>


<br>
<h3>پاک کردن داده‌ها از session</h3>

<p>
متد forget یک تکه داده را از سشن حذف می‌کند. اگر بخواهید تمام داده‌ها را از سشن حذف کنید، می‌توانید از متد flush استفاده کنید:
</p>


<pre><code class="language-php  line-numbers">$request->session()->forget('key');

$request->session()->flush();
</code></pre>


<p>
<a name="regenerating-the-session-id"></a>
</p>


<br>
<h3>ایجاد دوباره شناسه session </h3>

<p>
ایجاد دوباره شناسه سشن، اغلب برای جلوگیری از حمله session fixation، توسط کاربران غیر مجاز صورت می‌گیرد. ایجاد دوباره شناسه سشن در لاراول به صورت خودکار در زمان احراز هویت کاربر انجام می‌شود، اگر از LoginController ساخته شده استفاده می‌کنید و نیاز به ایجاد دوباره شناسه سشن به صورت دستی دارید، می‌توانید از متد regenerate استفاده کنید.
</p>

<pre><code class="language-php  line-numbers">$request->session()->regenerate();
</code></pre>


<p>
<a name="adding-custom-session-drivers"></a>
</p>


<br>
<h3><a href="#adding-custom-session-drivers">اضافه کردن درایور‌های session به صورت سفارشی</a></h3>

<p>
<a name="implementing-the-driver"></a>
</p>


<br>
<h3>پیاده سازی درایور session در لاراول</h3>

<p>
درایور سشن سفارشی باید رابط SessionHandlerInterface را پیاده سازی کند. این رابط شامل چند متد ساده است که ما برای پیاده سازی به آن‌ها نیاز داریم. پیاده سازی MongoDB مانند مثال زیر است:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Extensions;

class MongoSessionHandler implements \SessionHandlerInterface
{
    public function open($savePath, $sessionName) {}
    public function close() {}
    public function read($sessionId) {}
    public function write($sessionId, $data) {}
    public function destroy($sessionId) {}
    public function gc($lifetime) {}
}
</code></pre>

<blockquote class="has-icon tip">
لاراول دایرکتوری پیش‌فرضی برای قرار دادن موارد اضافی ندارد. می‌توانید آن‌ها را هر جا که بخواهید قرار دهید. در این مثال، ما دایرکتوری Extensions را برای قرار دادن MongoSessionHandler ایجاد کردیم.
</blockquote>

<p>
از آنجا که اهداف از این متدها به راحتی قابل درک نیست، اجازه دهید به صورت خلاصه کارهایی که توسط هر متد انجام می‌شود را بررسی کنیم:
</p>

<div class="content-list">
<ul>
<li>The <code class=" language-php">open</code> معمولاً در سیستم‌های file based session store استفاده می‌شود. از آنجا که لاراول درایور سشن file را به صورت پیش‌فرض در خود دارد، نیاز به قرار دادن چیزی در این متد نخواهیم داشت و می‌توانید آن را به عنوان یک تکه کد خالی رها کنید. اما PHP از ما می‌خواهد این متد را پیاده سازی کنیم، می‌توان گفت این یک ضعف آشکار در طراحی رابط در زبان PHP است.</li>
<li>The <code class=" language-php">close</code>  مانند متد open ، معمولاً می‌تواند نادیده گرفته شود. برای اکثر درایورها، استفاده از آن لازم نیست.</li>
<li>The <code class=" language-php">read</code> باید رشته داده‌های سشن مربوط به پارامتر ورودی $sessionId را بر‌گرداند. هنگام بازیابی یا ذخیره داده‌های سشن در درایور نیازی به انجام serialization و یا سایر عملیات رمزگذاری نیست، زیرا لاراول عملیات serialization را برای شما اجرا می‌کند.</li>
<li>The <code class=" language-php">write</code> باید رشته $data ورودی مربوط به پارامتر $sessionId را به بعضی از سیستم‌های ذخیره سازی داده دائمی، مانند MongoDB، Dynamo و غیره بنویسد. مجدداً نباید عملیات serialization انجام دهید، لاراول قبلاً این کار را برای شما انجام داده است.</li>
<li>The <code class=" language-php">destroy</code> باید داده‌های مربوط به پارامتر $sessionId را از حافظه دائمی حذف کند.</li>
<li>The <code class=" language-php">gc</code> باید تمام داده‌های سشنی که قدیمی‌تر از پارامتر $lifetime ورودی هستند را از بین می‌برد که یک برچسب زمانی UNIX است. برای سیستم‌های مستقل مانند Memcached و Redis، این متد می‌تواند خالی باشد.</li>
</ul>
</div>

<p>
<a name="registering-the-driver"></a>
</p>


<br>
<h3>ثبت درایور session</h3>

<p>
زمانی که درایور پیاده سازی شد، می‌توانید آن را در فریم ورک ثبت کنید. برای اضافه کردن درایورهای اضافی به backend سشن در لاراول، می‌توانید از متد extend در fasade مربوط به Session استفاده کنید. باید متد extend را از متد boot در یک service provider فراخوانی کنید. می‌توانید این کار را از AppServiceProvider موجود انجام دهید یا یک provider جدید ایجاد کنید:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use App\Extensions\MongoSessionHandler;
use Illuminate\Support\Facades\Session;
use Illuminate\Support\ServiceProvider;

class SessionServiceProvider extends ServiceProvider
{
    /**
     * Perform post-registration booting of services.
     *
     * @return void
     */
    public function boot()
    {
        Session::extend('mongo', function ($app) {
            // Return implementation of SessionHandlerInterface...
            return new MongoSessionHandler;
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


<p>
زمانی که درایور سشن ثبت شد، می‌توانید از درایور mongo در فایل پیکربندی config/session.php خود استفاده کنید.
</p>
