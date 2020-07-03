---
layout: documentation-laravel
title:   آشنایی با eloquent
cattitle: اصول آموزش Laravel
date:   2017-12-29 16:07:42 +0330
jdate: جمعه 8 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/Eloquent-ORM
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/collections <br> www.tahlildadeh.com/ArticleDetails/آموزش-ابزار-Eloquent-در-لاوارل
---
<p>
لاراول همراه با یک ORM پیش فرض به نام Eloquent ارائه می شود. این ORM برای کار با پایگاه داده الگوی ActiveRecord را پیاده سازی می کند. هر یک از جداول در پایگاه داده یک مدل متناظر و معادل خود دارند که برای تعامل با جدول مورد نظر از آن ها استفاده می شود. مدل ها به شما این امکان را می دهند تا به راحتی از داده های جدول کوئری گرفته و نیز رکوردهای جدید در جدول وارد کنید.
</p>
<p>
پیش از هر چیز باید یک connection برای اتصال به پایگاه داده در فایل config/database.php تعریف کنید.
</p>

<br>
<h3>تعریف Models</h3>
<p>
برای شروع یک مدل Eloquent ایجاد کنید. مدل ها به طور معمول در پوشه ی app قرار می گیرند، با این حال شما می توانید آن ها را در هر مکانی که امکان بارگذاری خودکار (auto-load) آن ها با توجه به تنظیمات فایل composer.json وجود دارد قرار دهید. تمامی مدل های Eloquent از کلاسIlluminate\Database\Eloquent\Model ارث بری می کنند.
</p>

<p>
آسان ترین روش برای ایجاد یک نمونه ی مدل اجرای دستور آرتیزان make:model است.
</p>

<pre><code class="language-php  line-numbers">php artisan make:model User
</code></pre>

<p>
اگر می خواهید همزمان با ایجاد مدل یک migration نیز ایجاد کنید، در آن صورت کافی است گزینه ی --migration یا -m را به انتهای دستور آرتیزان اضافه نمایید:
</p>

<pre><code class="language-php  line-numbers">php artisan make:model User --migration

php artisan make:model User -m
</code></pre>


<br>
<h3>قراردادها و قواعد مربوط به مدل های Eloquent</h3>
<p>
اکنون به یک کلاس نمونه ی مدل به نام Flight می پردازیم. از این کلاس برای واکشی و ذخیره ی اطلاعات از جدول flights بهره می گیریم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    //
}
</code></pre>


<br>
<h3>اسم جداول</h3>
<p>
همان طور که می بینید به Eloquent اعلان نکردیم که باید از کدام جدول برای مدل Flight استفاده کند. اگر یک اسم به طور صریح برای جدول مشخص نکنید، فرم snake case و جمع همان کلاس به عنوان اسم جدول لحاظ می شود. مشخص است که در چنین شرایطی Eloquent فرض می گیرد مدل Flight رکوردها را در جدول flights ذخیره کرده است. در صورت متفاوت بودن نام مدل از نام جدول، می توانید با تعریف متغیر (property) table در مدل مورد نظر، آن را مشخص کنید (با تعریف خاصیت table یک نام سفارشی برای جدول تعریف نمایید):
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The table associated with the model.
     *
     * @var string
     */
    protected $table = 'my_flights';
}
</code></pre>


<br>
<h3>کلیدهای اصلی (primary key)</h3>
<p>
Eloquent فرض می گیرد هر جدول یک ستون کلید اصلی به نام id دارد. می توانید با تعریف یک متغیر (property) به نام$primaryKey اسم جدیدی برای این ستون تنظیم کنید و به اصطلاح این قرارداد را بازنویسی نمایید.
</p>
<p>
علاوه بر آن Eloquent فرض می گیرد که ستون کلید اصلی دارای مقداری از نوع عدد صحیح و افزایش پذیر (incrementing integer) است، از این رو مقدار این ستون به صورت خودکار به int تبدیل می شود. اگر می خواهید از یک کلید اصلی غیر عددی و غیر افزایشی استفاده کنید، در آن صورت می بایست مقدار متغیر (property) $incrementing را که به صورت public تعریف شده، در مدل بر روی false تنظیم کنید.
</p>


<br>
<h3>قواعد مربوط به timestamp ها</h3>
<p>
Eloquent انتظار دارد دو ستون به نام های created_at و updated_at در جداول شما وجود داشته باشد. اگر نمی خواهید مدیریت این ستون ها به صورت خودکار به Eloquent واگذار شود، بایستی متغیر (property) $timestamps را با false مقداردهی کنید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * Indicates if the model should be timestamped.
     *
     * @var bool
     */
    public $timestamps = false;
}
</code></pre>

<p>
اگر می خواهید فرمت timestamp های خود را مطابق نیاز تنظیم کنید، بایستی متغیر (property) $dateFormat را در مدل تعریف نموده و مقداردهی کنید. این متغیر علاوه بر تعیین چگونگی ذخیره ی attribute های مربوط به تاریخ در پایگاه داده، فرمت آن ها را در زمان کد و سریاله شدن مدل به array / JSON نیز تعریف می کند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The storage format of the model's date columns.
     *
     * @var string
     */
    protected $dateFormat = 'U';
}
</code></pre>

<p>
اگر شما بخواهید نام ستون های استفاده شده برای ذخیره timestamps را سفارشی نمایید، کافی است ثابت CREATED_AT و UPDATED_AT را در مدل خود تنظیم کنید:
</p>

<pre><code class="language-php  line-numbers"><?php

class Flight extends Model
{
    const CREATED_AT = 'creation_date';
    const UPDATED_AT = 'last_update';
}
</code></pre>


<br>
<h3>تعریف connection</h3>
<p>
در حالت پیش فرض، تمامی مدل های Eloquent از connection از پیش تنظیم شده اپلیکیشن برای اتصال به پایگاه داده استفاده می کنند. در صورت نیاز به تعریف یک connection دیگر، کافی است متغیر $connection را در مدل بکار ببرید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The connection name for the model.
     *
     * @var string
     */
    protected $connection = 'connection-name';
}
</code></pre>


<br>
<h3>بازیابی چندین مدل</h3>
<p>
پس از ایجاد مدل و جدول متناظر آن در پایگاه داده، می توانید نسبت به واکشی اطلاعات از پایگاه داده اقدام نمایید. هر مدل Eloquent به مثابه ی یک query builder قدرتمند ایفای نقش می کند و به شما اجازه می دهد به راحتی از جدول مربوط به مدل کوئری بگیرید. مثال:
</p>

<pre><code class="language-php  line-numbers"><?php

use App\Flight;

$flights = App\Flight::all();

foreach ($flights as $flight) {
    echo $flight->name;
}
</code></pre>


<br>
<h3>اعمال محدودیت های بیشتر (constraint)</h3>
<p>
متد all از توابع Eloquent تمامی رکوردهای درون جدول متناظر مدل را برمی گرداند. از آنجایی که هر مدل Eloquent به عنوان یک query builder نیز عمل می کند، می توانید به راحتی constraint هایی را به کوئری اعمال کرده و سپس با استفاده از متد get تمامی نتایج را بازیابی کنید:
</p>

<pre><code class="language-php  line-numbers">$flights = App\Flight::where('active', 1)
               ->orderBy('name', 'desc')
               ->take(10)
               ->get();
</code></pre>

<blockquote>
کلیه ی متدهای query builder در کوئری های Eloquent نیز در دسترس هستند.
</blockquote>


<br>
<h3>collection ها</h3>
<p>
خروجی توابع Eloquent نظیر all و get که مجموعه ای از نتایج را برمی گردانند، در واقع نمونه ای از کلاسIlluminate\Database\Eloquent\Collection می باشد. کلاس Collection تعداد زیادی متد مفید برای انجام عملیات مختلف بر روی خروجی ها و نتایج Eloquent ارائه می دهد.
</p>

<pre><code class="language-php  line-numbers">$flights = $flights->reject(function ($flight) {
    return $flight->cancelled;
});
</code></pre>

<p>
می توان به راحتی داخل collection مانند آرایه حلقه زد (چرخید):
</p>

<pre><code class="language-php  line-numbers">foreach ($flights as $flight) {
    echo $flight->name;
}
</code></pre>


<br>
<h3>اداره ی حجم سنگین نتایج با متد chunk (دریافت خروجی به صورت خورد خورد)</h3>
<p>
برای پردازش یکجای هزاران رکورد، بد نیست دستور chunk را فراخوانی کنید. این متد رکوردهای Eloquent را در قالب تکه یا chunk هایی (مثلا 100 رکورد) واکشی کرده و آن ها را برای پردازش به تابع Closure می دهد.
</p>

<p>
استفاده از chunk به هنگام کار با حجم بالای نتایج سبب صرفه جویی در مصرف حافظه می شود:
</p>

<pre><code class="language-php  line-numbers">Flight::chunk(200, function ($flights) {
    foreach ($flights as $flight) {
        //
    }
});
</code></pre>

<p>
اولین آرگومان ارسالی به این متد تعداد رکوردهایی است که می خواهید در هر بار (در قالب بسته هایی) واکشی شود. تابع Closure که به عنوان آرگومان دوم پاس داده شده نیز به ازای هر بسته ی واکشی شده از پایگاه داده فراخوانی می گردد.
</p>


<br>
<h3>Using Cursors</h3>
<p>
متد Cursors به شما این امکان را می دهد تا از طریق یک نشانگر پایگاه داده،  تنها یک پرس و جو را اجرا کنیم. هنگام پردازش مقادیر زیاد داده ها، متد Cursors می تواند به میزان قابل توجهی استفاده از حافظه را کاهش داد:
</p>

<pre><code class="language-php  line-numbers">foreach (Flight::where('foo', 'bar')->cursor() as $flight) {
    //
}
</code></pre>

<br>
<h3>بازیابی یک نمونه از مدل / تک مقدار عددی (aggregate)</h3>
<p>
با فراخوانی توابع first و find می توان بجای واکشی تمامی رکوردهای یک جدول، تنها یک رکورد (تک رکوردهایی) را از جدول استخراج کرد. بجای واکشی مجموعه ای از مدل ها، این متدها یک تک نمونه از مدل را برمی گرداند:
</p>

<pre><code class="language-php  line-numbers">// Retrieve a model by its primary key...
$flight = App\Flight::find(1);

// Retrieve the first model matching the query constraints...
$flight = App\Flight::where('active', 1)->first();
</code></pre>

<p>
می توانید متد find را با آرایه ای از کلیدهای اصلی صدا بزنید که مجموعه ای از رکوردهای منطبق و متناظر را در خروجی برمی گرداند:
</p>

<pre><code class="language-php  line-numbers">$flights = App\Flight::find([1, 2, 3]);
</code></pre>


<br>
<h3>بازیابی یک مدل یا صدور exception در صورت یافت نشدن مدل</h3>
<p>
گاهی ممکن است لازم شود در صورت عدم وجود یا یافت نشدن مدل مورد نظر، یک exception صادر کنید. این روش به خصوص در route ها یاcontroller ها مفید واقع می شود. متد findOrFail و firstOrFail اولین نتیجه ی حاصل از اجرای کوئری را در خروجی برمی گرداند. و اگر نتیجه ای بازگردانده نشد، یک خطای Illuminate\Database\Eloquent\ModelNotFoundException صادر می شود:
</p>

<pre><code class="language-php  line-numbers">$model = App\Flight::findOrFail(1);

$model = App\Flight::where('legs', '>', 100)->firstOrFail();
</code></pre>

<p>
در صورتی که exception مدیریت (catch) نشود، یک پاسخ HTTP با کد وضعیت 404 به صورت خودکار به مرورگر برگردانده می شود، از اینرو در زمان استفاده از دو متد مزبور ضرورتی ندارد چک هایی را به صورت صریح برای بازگرداندن پاسخ های 404 به کاربر بنویسید:
</p>

<pre><code class="language-php  line-numbers">Route::get('/api/flights/{id}', function ($id) {
    return App\Flight::findOrFail($id);
});
</code></pre>


<br>
<h3>بازیابی تک مقادیر عددی (aggregates)</h3>
<p>
می توانید از توابع تجمعی نظیر count، sum، max نیز استفاده کنید. این توابع بجای بازیابی یک نمونه ی کامل از مدل در خروجی، مقدار عددی مربوطه را برمی گردانند:
</p>

<pre><code class="language-php  line-numbers">$count = App\Flight::where('active', 1)->count();

$max = App\Flight::where('active', 1)->max('price');
</code></pre>


<br>
<h3>عملیات درج ساده Inserts</h3>
<p>
به منظور ایجاد یک رکورد جدید در پایگاه داده، کافی است یک نمونه مدل ایجاد کرده، attribute های مربوطه را در مدل تنظیم کنید و سپس متدsave را صدا بزنید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use App\Flight;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class FlightController extends Controller
{
    /**
     * Create a new flight instance.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate the request...

        $flight = new Flight;

        $flight->name = $request->name;

        $flight->save();
    }
}
</code></pre>

<p>
در این مثال فقط پارامتر name را از درخواست HTTP ورودی می گیریم و به اتریبیوت name از نمونه مدل App\Flight تخصیص می دهیم. هنگامی که متد save را فرا می خوانیم، یک رکورد داخل پایگاه داده درج می گردد. ستون های created_at و updated_at نیز به صورت خودکار مقداردهی می شود، بنابراین نیازی نیست آن ها را به صورت دستی تنظیم کنید.
</p>

<br>
<h3>عملیات بروز رسانی ساده Updates</h3>
<p>
متد save را می توان جهت بروز رسانی مدل های از پیش موجود در پایگاه داده نیز مورد استفاده قرار داد. به منظور بروز رسانی یک مدل، ابتدا آن را بازیابی نموده، attribute های مد نظر برای بروز رسانی را مقداردهی (set) و در نهایت متد save را فراخوانی نمایید. بار دیگر یادآور می شویم که ستون updated_at به صورت اتوماتیک بروز آوری می شود و نیازی نیست مقدار آن را به صورت دستی تنظیم کنید:
</p>

<pre><code class="language-php  line-numbers">$flight = App\Flight::find(1);

$flight->name = 'New Flight Name';

$flight->save();
</code></pre>

<p>
عملیات update را می توان بر روی هر تعداد مدلی که با کوئری منطبق است اجرا نمود. در این مثال تمامی پروازهای جاری (active) که مقصد (destination) آن ها شهر San Diego هست با تاخیر علامت گذاری می شوند:
</p>

<pre><code class="language-php  line-numbers">App\Flight::where('active', 1)
          ->where('destination', 'San Diego')
          ->update(['delayed' => 1]);
</code></pre>

<p>
متد update آرایه ای از جفت های (اسم) ستون و مقدار را به عنوان آرگومان دریافت می کند که نشانگر ستون های متناظری است که باید بروز آوری شوند.
</p>

<br>
<h3>تخصیص جمعی (mass assignment)</h3>
<p>
می توانید با بکار گیری متد create یک نمونه ی جدید از مدل را در تنها یک خط کد در پایگاه داده درج و ذخیره کنید. نمونه مدل درج شده توسط متد به شما برگردانده می شود. اما پیش از این کار، بایستی اتریبیوت fillable یا guarded را در مدل مورد نظر تعریف کنید چرا که تمامی مدل هایEloquent به طور پیش فرض در مقابل تخصیص جمعی محافظت شده هستند.
</p>

<p>
خطر امنیتی یا آسیب پذیری در اثر mass-assignment به زمانی گفته می شود که کاربری یک پارامتر HTTP غیر منتظره از طریق request ارسال کند و در پی آن پارامتر مزبور ستونی را که انتظارش را ندارید در پایگاه داده ی شما ویرایش کند. برای مثال کاربر مخرب ممکن است از طریق درخواست HTTP یک پارامترis_admin ارسال کند. این پارامتر بلافاصله به متد create مدل نگاشت شده و به هکر اجازه می دهد سطح دسترسی خود را به admin ارتقا دهد.
</p>

<p>
برای شروع باید مشخص کنید می خواهید به کدام یک از attribute های مدل قابلیت تخصیص جمعی (در تخصیص جمعی به مدل انتساب داده شوند) را اعطا کنید. برای این منظور کافی است متغیر (property) $fillable را در مدل بکار ببرید (این property مشخص می کند کدام یک از attribute ها را در تخصیص جمعی می توان مقداردهی کرد). در مثال زیر با مقداردهی property مذکور، به اتریبیوت name از مدل Flight امکان تخصیص جمعی را اعطا می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['name'];
}
</code></pre>

<p>
پس از اعطای قابلیت تخصیص جمعی به attribute های مورد نظر، می توان متد create را صدا زده و یک رکورد جدید را در پایگاه داده وارد کرد. متد نام برده نمونه مدل ذخیره شده در پایگاه داده را در خروجی برمی گرداند:
</p>

<pre><code class="language-php  line-numbers">$flight = App\Flight::create(['name' => 'Flight 10']);
</code></pre>

<p>
اگر در حال حاضر نمونه ای از مدل دارید، می توانید از متد fill  برای پر کردن آن با یک آرایه از ویژگی ها استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$flight->fill(['name' => 'Flight 22']);
</code></pre>


<br>
<h3> ویژگی های نگهداری Guarding Attributes</h3>
<p>
متغیر $fillable مانند یک لیست سفید است که attribute های با قابلیت mass assign در آن درج شده. نقطه ی مقابل آن متغیر $guardedهست که attribute های فاقد این قابلیت را به صورت آرایه در خود ذخیره می کند (در این پراپرتی attribute هایی را ذخیره می کنیم که در مقابل تخصیص جمعی محافظت شده هستند). از اینرو تمامی attribute هایی که در آرایه حاضر نیستند، در تخصیص جمعی قابل انتساب به مدل هستند.
</p>

<p>
می توان گفت که$guarded یک نوع لیست سیاه است. ناگفته پیدا است که این دو property را نمی توان همزمان با هم بکار برد:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The attributes that aren't mass assignable.
     *
     * @var array
     */
    protected $guarded = ['price'];
}
</code></pre>

<p>
همان طور که می بینید فقط اتریبیوت price در مقابل تخصیص جمعی محافظت شده می باشد.
</p>

<p>
اگر می خواهید تمام ویژگی های توزیع را تعیین کنید، می توانید guarded$ را به عنوان یک آرایه خالی تعریف کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * The attributes that aren't mass assignable.
 *
 * @var array
 */
protected $guarded = [];
</code></pre>

<br>
<h3>متدهای firstOrCreate/ firstOrNew</h3>
<p>
دو متد دیگر وجود دارد که می توان از آن ها برای ایجاد مدل بهره گرفت: firstOrCreate و firstOrCreate. متد firstOrCreate ابتدا سعی می کند رکورد را بر اساس جفت های اسم ستون / مقدار ارائه شده پیدا کند. اگر مدل در پایگاه داده یافت نشد، یک رکورد با attribute های داده شده درج می کند.
</p>

<p>
متد firstOrNew مشابه متد firstOrCreate ابتدا تلاش می کند رکورد منطبق با attribute های ارائه شده را پیدا کند. در صورت یافت نشدن رکورد مورد نظر، یک نمونه مدل جدید در خروجی برمی گرداند (ایجاد می کند). دقت داشته باشید که مدلی که توسط متد firstOrNew برگردانده می شود، هنوز به طور دائمی در پایگاه داده ذخیره نشده است. جهت تثبیت و ذخیره ی دائمی آن باید متد save را صدا بزنید:
</p>


<pre><code class="language-php  line-numbers">// Retrieve flight by name, or create it if it doesn't exist...
$flight = App\Flight::firstOrCreate(['name' => 'Flight 10']);

// Retrieve flight by name, or create it with the name and delayed attributes...
$flight = App\Flight::firstOrCreate(
    ['name' => 'Flight 10'], ['delayed' => 1]
);

// Retrieve by name, or instantiate...
$flight = App\Flight::firstOrNew(['name' => 'Flight 10']);

// Retrieve by name, or instantiate with the name and delayed attributes...
$flight = App\Flight::firstOrNew(
    ['name' => 'Flight 10'], ['delayed' => 1]
);
</code></pre>


<br>
<h3>متد updateOrCreate</h3>
<p>
شما همچنین ممکن است در موقعیت هایی قرار بگیرید که می خواهید یک مدل موجود را به روز کنید یا یک مدل جدید ایجاد کنید چنانچه وجود نداشته باشد. Laravel یک روش updateOrCreate را برای انجام این کار در یک مرحله فراهم می کند. بنابراین نیازی به متد  ()save نخواهد بود:
</p>

<pre><code class="language-php  line-numbers">// If there's a flight from Oakland to San Diego, set the price to $99.
// If no matching model exists, create one.
$flight = App\Flight::updateOrCreate(
    ['departure' => 'Oakland', 'destination' => 'San Diego'],
    ['price' => 99]
);
</code></pre>


<br>
<h3>حذف مدل</h3>
<p>
به منظور حذف مدل از پایگاه داده، متد delete را در نمونه ی ایجاد شده از مدل فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">$flight = App\Flight::find(1);

$flight->delete();
</code></pre>


<br>
<h3>حذف مدل از پایگاه داده با استفاده از کلید اصلی</h3>
<p>
در مثال فوق ابتدا مدل را واکشی می کنیم، سپس متد delete را صدا می زنیم. اما اگر کلید اصلی مدل را می دانید، دیگری نیازی به واکشی آن نیست. برای این منظور متد destroy را صدا زده و کلید اصلی مدل را به عنوان پارامتر ورودی به آن پاس می دهیم:
</p>

<pre><code class="language-php  line-numbers">App\Flight::destroy(1);

App\Flight::destroy([1, 2, 3]);

App\Flight::destroy(1, 2, 3);
</code></pre>


<br>
<h3>حذف مدل با استفاده از کوئری ساده</h3>
<p>
می توانید یک کوئری delete نوشته و آن را بر روی مدل های مورد نظر اجرا کنید. در مثال حاضر تمامی پروازهایی (flight) که غیرفعال (مقدار ستونactive آن ها 0 هست) هستند را حذف می کنیم:
</p>

<pre><code class="language-php  line-numbers">$deletedRows = App\Flight::where('active', 0)->delete();
</code></pre>

<br>
<h3>حذف موقت (soft delete)</h3>
<p>
علاوه بر امکان حذف دائمی رکوردها، Eloquent به شما اجازه می دهد مدل ها را به طور موقت حذف کنید. به حذف موقت رکوردها soft delete گفته می شود. soft delete کردن مدل در واقع سبب می شود یک اتریبیوت deleted_at در مدل (تعریف) مقداردهی و در پایگاه داده ذخیره گردد. اگر این attribute در مدل مقدار دهی شده باشد (مقدارش non-null باشد)، آن مدل soft delete شده است. برای فعال سازی قابلیت soft deleteبرای مدل مورد نظر، کافی است Illuminate\Database\Eloquent\SoftDeletes trait را در مدل بکار برده و ستون deleted_at را به متغیر (property) $dates اضافه نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Flight extends Model
{
    use SoftDeletes;

    /**
     * The attributes that should be mutated to dates.
     *
     * @var array
     */
    protected $dates = ['deleted_at'];
}
</code></pre>

<p>
همچنین لازم است ستون deleted_at را به جدول اضافه نمایید. schema builder لاراول یک متد کمکی برای ایجاد این ستون در اختیار شما قرار می دهد:
</p>

<pre><code class="language-php  line-numbers">Schema::table('flights', function ($table) {
    $table->softDeletes();
});
</code></pre>

<p>
حال زمانی که متد delete را در مدل صدا می زنید، ستون deleted_at با تاریخ و زمانی جاری مقداردهی می شود. همچنین به هنگام گرفتن کوئری از مدلی که قابلیت soft delete برای آن فعال سازی شده، خواهید دید که مدل های soft delete شده در هیچ یک از مجموعه نتایج حاصل از اجرای کوئری لحاظ نمی شوند.
</p>

<p>
برای پی بردن به اینکه آیا رکوردی به صورت موقت حذف شده یا خیر، متد trashed را صدا بزنید:
</p>

<pre><code class="language-php  line-numbers">if ($flight->trashed()) {
    //
}
</code></pre>

<br>
<h3>آوردن مدل های soft delete شده در مجموعه نتایج</h3>
<p>
همان طور که در بالا گفته شد، مدل هایی که به صورت موقتی حذف شده اند در مجموعه نتایج لحاظ نخواهند شد. متد withTrashed این امکان را فراهم می کند تا مدل های soft delete شده را نیز در خروجی لحاظ کنید:
</p>

<pre><code class="language-php  line-numbers">$flights = App\Flight::withTrashed()
                ->where('account_id', 1)
                ->get();
</code></pre>

<p>
متد withTrashed همچنین می تواند بر روی یک پرس و جو رابطه ای استفاده شود:
</p>

<pre><code class="language-php  line-numbers">$flight->history()->withTrashed()->get();
</code></pre>


<br>
<h3>بازیابی فقط مدل های soft delete شده</h3>
<p>
متد onlyTrashed تنها مدل هایی که به صورت موقت حذف شده اند را در خروجی لحاظ می کند:
</p>

<pre><code class="language-php  line-numbers">$flights = App\Flight::onlyTrashed()
                ->where('airline_id', 1)
                ->get();
</code></pre>


<br>
<h3>بازگردانی مدل های soft delete شده به وضعیت فعال</h3>
<p>
گاهی لازم می شود یک مدلی که به صورت موقت حذف شده را به حالت فعال برگردانید (un-delete نمایید). برای این منظور کافی است متدrestore را در نمونه ی مدل فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">$flight->restore();
</code></pre>

<p>
می توانید با فراخوانی متد restore در کوئری به سرعت چندین مدل soft delete شده را به حالت فعال برگردانید:
</p>

<pre><code class="language-php  line-numbers">App\Flight::withTrashed()
        ->where('airline_id', 1)
        ->restore();
</code></pre>

<p>
متد restore نیز مانند متد withTrashed بر روی رابطه ها قابل استفاده می باشد:
</p>

<pre><code class="language-php  line-numbers">$flight->history()->restore();
</code></pre>


<br>
<h3>حذف مدل ها به طور دائمی</h3>
<p>
گاهی لازم می شود یک مدل را به معنای واقعی از پایگاه داده حذف کنید. Eloquent متد forceDelete را برای این منظور ارائه می کند. جهت حذف دائمی یک مدل soft delete شده از بانک اطلاعاتی، کافی است متد forceDelete را صدا بزنید:
</p>

<pre><code class="language-php  line-numbers">// Force deleting a single model instance...
$flight->forceDelete();

// Force deleting all related models...
$flight->history()->forceDelete();
</code></pre>

<br>
<h3>حوزه ی سراسری (global scope)</h3>
<p>
حوزه یا scope های سراسری به شما این امکان را می دهند تا به تمامی کوئری هایی که بر روی مدل معین اجرا می شوند، محدودیت (constraint) اعمال کنید. قابلیت درون ساخته ی soft deleting چارچوب نرم افزاری لاراول خود از scope سراسری استفاده می کند و به وسیله ی آن تنها مدل های حذف نشده (non-deleted) را از پایگاه داده بیرون می کشد.
</p>
<p>
می توانید scope های سراسری اختصاصی خود را بنویسید و بدین وسیله مطمئن شوید که به هر کوئری که بر روی مدل خاص اجرا می شود، محدودیت های دلخواه اعمال شود.
</p>

<br>
<h3>تعریف scope های سراسری</h3>
<p>
نوشتن scope های سراسری آسان است. درگام نخست یک کلاس تعریف کنید که اینترفیس Illuminate\Database\Eloquent\Scope را پیاده سازی کند. لازمه ی interface ذکر شده، پیاده سازی متد apply می باشد. این متد می تواند با توجه به نیاز محدودیت هایی (constraint) را در قالب عبارات where به کوئری مورد نظر اعمال کند:
</p>

<pre><code class="language-php  line-numbers">
<?php

namespace App\Scopes;

use Illuminate\Database\Eloquent\Scope;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Builder;

class AgeScope implements Scope
{
    /**
     * Apply the scope to a given Eloquent query builder.
     *
     * @param  \Illuminate\Database\Eloquent\Builder  $builder
     * @param  \Illuminate\Database\Eloquent\Model  $model
     * @return void
     */
    public function apply(Builder $builder, Model $model)
    {
        $builder->where('age', '>', 200);
    }
}
</code></pre>

<p>
هیچ پوشه ی پیش فرضی برای ذخیره ی scope ها در اپلیکیشن لاراول در نظر گرفته نشده است. بنابراین می بایست یک پوشه ی اختصاصی به نامScopes داخل دایرکتوری app اپلیکیشن لاراول خود جهت ذخیره ی scope ها ایجاد نمایید.
</p>


<br>
<h3>تخصیص scope های سراسری به مدل</h3>
<p>
به منظور تخصیص scope سراسری به مدل، باید متد boot مدل مورد نظر را بازنویسی نموده و سپس متد addGlobalScope را بکار ببرید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use App\Scopes\AgeScope;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The "booting" method of the model.
     *
     * @return void
     */
    protected static function boot()
    {
        parent::boot();

        static::addGlobalScope(new AgeScope);
    }
}
</code></pre>

<p>
پس از اضافه کردن scope به مدل، کوئری مورد نظر برای واکشی تمامی سطرها " User::all() " معادل کوئری خالص SQL زیر خواهد شد:
</p>

<pre><code class="language-php  line-numbers">select * from `users` where `age` > 200
</code></pre>


<br>
<h3>ایجاد Scope های سراسری فاقد شناسه (Anonymous Global Scopes)</h3>
<p>
Eloquent همچنین به شما اجازه می دهد scope های سراسری موردنیازتان را به وسیله ی توابع Closure تعریف نمایید. این روش به خصوص برای تعریف scope های ساده که به کلاس مجزا نیاز ندارند مفید واقع می شود:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Builder;

class User extends Model
{
    /**
     * The "booting" method of the model.
     *
     * @return void
     */
    protected static function boot()
    {
        parent::boot();

        static::addGlobalScope('age', function (Builder $builder) {
            $builder->where('age', '>', 200);
        });
    }
}
</code></pre>


<br>
<h3>حذف scope های سراسری</h3>
<p>
در صورت نیاز به حذف scope سراسری از یک کوئری (خارج کردن آن کوئری از پوشش حوزه ی سراسری)، کافی است متد withoutGlobalScopeرا فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">User::withoutGlobalScope(AgeScope::class)->get();
</code></pre>

<p>
برای حذف تمامی scope های سراسری تعریف شده، لازم است متد withoutGlobalScopes را صدا بزنید:
</p>

<pre><code class="language-php  line-numbers">// Remove all of the global scopes...
User::withoutGlobalScopes()->get();

// Remove some of the global scopes...
User::withoutGlobalScopes([
    FirstScope::class, SecondScope::class
])->get();
</code></pre>

<br>
<h3>تعریف scope های محلی</h3>
<p>
scope های محلی این امکان را فراهم می کنند تا یک مجموعه constraint مشترک و پرکاربرد تعریف و آن ها را بارها در سرتاسر اپلیکیشن خود بکار ببرید. برای مثال ممکن است لازم باشد مکررا تمامی کاربرانی که "محبوب و popular" تلقی می شوند را واکشی کنید. جهت تعریف یک scope برای این منظور، کافی است واژه ی scope را به ابتدای اسم متد (مدل Eloquent) الصاق نمایید. scope ها همیشه بایستی در خروجی یک نمونه یquery builder برگردانند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Scope a query to only include popular users.
     *
     * @param \Illuminate\Database\Eloquent\Builder $query
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function scopePopular($query)
    {
        return $query->where('votes', '>', 100);
    }

    /**
     * Scope a query to only include active users.
     *
     * @param \Illuminate\Database\Eloquent\Builder $query
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function scopeActive($query)
    {
        return $query->where('active', 1);
    }
}
</code></pre>


<br>
<h3>استفاده از Scope محلی</h3>
<p>
پس از اینکه scope و حوزه ی پرس و جو را تعریف کردید، می توانید متدهای مربوط به scope را برای کوئری گرفتن از مدل فراخوانی نمایید. گفتنی است که برای فراخوانی متد نیازی به ذکر پیشوند scope نیست.
</p>
<p>
می توانید متدهای scope را به صورت زنجیره ای و دنبال هم فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::popular()->active()->orderBy('created_at')->get();
</code></pre>


<br>
<h3>تعریف scope هایی با قابلیت دریافت پارامتر (dynamic scope)</h3>
<p>
گاهی نیاز می شود scope های تعریف کنید که امکان دریافت پارامتر را دارند. برای این منظور کافی است پارامترهای دلخواه را به scope اضافه کنید. دقت داشته باشید که این پارامترها می بایست پس از آرگومان $query تعریف شوند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Scope a query to only include users of a given type.
     *
     * @param \Illuminate\Database\Eloquent\Builder $query
     * @param mixed $type
     * @return \Illuminate\Database\Eloquent\Builder
     */
    public function scopeOfType($query, $type)
    {
        return $query->where('type', $type);
    }
}
</code></pre>

<p>
حال می توانید به هنگام فراخوانی scope پارامترهای مورد نظر را به آن پاس دهید:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::ofType('admin')->get();
</code></pre>

<br>
<h3>رخدادها (Events)</h3>
<p>
مدل های Eloquent رخدادهای فراوانی را اعلان و ایجاد (fire) می کنند. با بهره گیری از این اتفاق می توان در مراحل مختلف چرخه ی حیات مدل (lifecycle) عملیات مختلفی را انجام داد. برای انجام این دست عملیات از متدهای زیر استفاده می کنیم: creating، created، updating،updated، saving، saved، deleting،deleted، restoring،restored.
</p>
<p>
event ها به شما اجازه می دهند هر زمان که یک کلاس مدل معین در پایگاه داده ذخیره یا بروز رسانی شد، کد خاصی را اجرا کنید.
</p>

<p>
هرگاه یک مدل جدید برای اولین بار ذخیره می شود، به دنبالش رخدادهای creating و created اعلان می شوند. چنانچه مدل از پیش در پایگاه داده وجود داشته و متعاقبا متد save برای آن صدا زده شود، آنگاه رخدادهای updating / updated اعلان می شوند. در هر دو سناریو رخدادهایsaving / saved (چه مدل از قبل موجود باشد و چه برای اولین بار است که در پایگاه داده ذخیره می شود) اجرا می شوند.
</p>

<p>
برای شروع، تعریف ویژگی $ dispatchesEvents در مدل Eloquent خود را که نقاط مختلف چرخه عمر مدل Eloquent را به کلاس رویداد خود نشان می دهد، مشخص کنید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use App\Events\UserSaved;
use App\Events\UserDeleted;
use Illuminate\Notifications\Notifiable;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;

    /**
     * The event map for the model.
     *
     * @var array
     */
    protected $dispatchesEvents = [
        'saved' => UserSaved::class,
        'deleted' => UserDeleted::class,
    ];
}
</code></pre>


<br>
<h3>ایجاد ناظر Observers</h3>

<p>
اگر برای بسیاری از رویدادهای مربوط به یک مدل خاص گوش می دهید، می توانید از ناظران برای جمع آوری تمام شنوندگان خود به یک کلاس واحد استفاده کنید. کلاس های ناظران نامهای متفاوتی دارند که منجر به رویدادهای صلح آمیز می شود که می خواهید گوش دهید. هر یک از این روشها مدل را به عنوان تنها استدلال خود دریافت می کند. Laravel یک دایرکتوری پیش فرض برای ناظران را شامل نمی شود، بنابراین شما می توانید هر دایرکتوری ای را که دوست دارید کلاس های ناظر خود را در آن قرار دهید ایجاد کنید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Observers;

use App\User;

class UserObserver
{
    /**
     * Listen to the User created event.
     *
     * @param  \App\User  $user
     * @return void
     */
    public function created(User $user)
    {
        //
    }

    /**
     * Listen to the User deleting event.
     *
     * @param  \App\User  $user
     * @return void
     */
    public function deleting(User $user)
    {
        //
    }
}
</code></pre>

<p>
برای ثبت نام یک ناظر، از روش مشاهده در مدل مورد نظر خود استفاده کنید. شما می توانید ناظران را در روش بوت یکی از ارائه دهندگان خدمات ثبت نام کنید. در این مثال، ناظر در AppServiceProvider را ثبت می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use App\User;
use App\Observers\UserObserver;
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
        User::observe(UserObserver::class);
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
