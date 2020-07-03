---
layout: documentation-laravel
title:   validation یا اعتبارسنجی در لاراول
cattitle: اصول آموزش Laravel
date:   2018-02-11 10:30:42 +0330
jdate: یکشنبه 22 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/The-Basics
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/validation <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/262/اعتبارسنجی-در-لاراول-5-5
---
<p>
جهت اعتبارسنجی داده‌های ورودی در برنامه، روش‌های مختلفی توسط لاراول ارائه شده است. در حالت پیش‌فرض، کلاس کنترلر پایه لاراول از یک خصوصیت ValidatesRequests استفاده می‌کند که یک متد مناسب ارائه می‌دهد که با قوانین اعتبارسنجی قدرتمند برای اعتبارسنجی درخواست‌های HTTP ورودی استفاده می‌شود.
</p>
<p>
<a name="validation-quickstart"></a>
</p>

<br>
<h3><a href="#validation-quickstart">شروع اعتبارسنجی در لاراول</a></h3>
<p>
برای اطلاع از ویژگی‌های قدرتمند اعتبارسنجی در لاراول، توجه شما را به یک نمونه کامل از اعتبارسنجی فرم و پیام‌های خطایی که به کاربر نمایش داده می‌شود، جلب می‌کنیم:
</p>
<p>
<a name="quick-defining-the-routes"></a>
</p>

<br>
<h3>تعریف مسیرها</h3>
<p>
ابتدا فرض می‌کنیم، مسیرهای زیر در فایل routes/web.php تعریف شده‌اند:
</p>

<pre><code class="language-php  line-numbers">Route::get('post/create', 'PostController@create');

Route::post('post', 'PostController@store');
</code></pre>

<p>
مسیر GET یک فرم برای کاربر نمایش می‌دهد که بتواند یک پست جدید ایجاد کند، در حالی که، مسیر POST پست جدید را در پایگاه داده ذخیره می‌کند.
</p>
<p>
<a name="quick-creating-the-controller"></a>
</p>

<br>
<h3>ایجاد کنترلر</h3>
<p>
پس، یک کنترلر ساده که این مسیرها را مدیریت می‌کند را در نظر می‌گیریم. فعلاً، متد store را به صورت خالی نگه می‌‌داریم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class PostController extends Controller
{
    /**
     * Show the form to create a new blog post.
     *
     * @return Response
     */
    public function create()
    {
        return view('post.create');
    }

    /**
     * Store a new blog post.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        // Validate and store the blog post...
    }
}
</code></pre>

<p>
<a name="quick-writing-the-validation-logic"></a>
</p>

<br>
<h3>پیاده سازی منطق اعتبارسنجی</h3>
<p>
اکنون می‌توانیم، متد store را با کدهای اعتبارسنجی جهت تأیید اعتبار پست جدید کاربر پر کنیم. برای انجام این کار، از متد validate که توسط شئ Illuminate\Http\Request ارائه شده، استفاده می‌کنیم.
</p>
<p>
اگر قوانین اعتبارسنجی تصویب شوند، کد در حالت نرمال به اجرا ادامه می‌دهد؛ با این حال، اگر اعتبارسنجی با خطا روبرو شود، یک استثنا یا exception پرتاب شده و به صورت خودکار پاسخ خطای مناسب به کاربر ارسال می‌شود. در مورد درخواست‌های HTTP سنتی، یک پاسخ redirect تولید می‌شود و کاربر را به صفحه قبل بازگشت می‌دهد، در حالی که در مورد درخواست‌های AJAX یک پاسخ JSON به کاربر ارسال می‌شود.
</p>
<p>
برای درک بهتر متد validate ، اجازه دهید نگاهی دوباره به متد store بیاندازیم:
</p>
<pre><code class="language-php  line-numbers">/**
 * Store a new blog post.
 *
 * @param  Request  $request
 * @return Response
 */
public function store(Request $request)
{
    $validatedData = $request->validate([
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ]);

    // The blog post is valid...
}
</code></pre>

<p>
همانطور که مشاهده می‌کنید، می‌توانیم به راحتی قوانین اعتبارسنجی دلخواه را به متد validate انتقال دهیم. در این صورت، اگر عملیات اعتبارسنجی ناموفق باشد، پاسخ مناسب به صورت خودکار تولید می‌شود. اگر اعتبارسنجی با موفقیت انجام بگیرد، کنترلر به صورت نرمال به اجرای خود ادامه خواهد داد.
</p>

<br>
<h3>توقف اجرا در اولین شکست اعتبارسنجی</h3>
<p>
گاهی اوقات ممکن است بخواهید، پس از اولین شکست در عملیات اعتبارسنجی یک صفت، اجرای قوانین اعتبارسنجی بعدی را بر روی آن صفت متوقف کنید. برای انجام این کار، باید قانون bail را به آن صفت اختصاص دهید:
</p>

<pre><code class="language-php  line-numbers">$request->validate([
    'title' => 'bail|required|unique:posts|max:255',
    'body' => 'required',
]);
</code></pre>

<p>
در این مثال، اگر قانون unique در صفت title با شکست مواجه شود، قانون max بررسی نخواهد شد. قوانین به ترتیبی که مشخص شده‌اند، اعتبارسنجی می‌شوند.
</p>

<br>
<h3>صفت‌های تودرتو (nested attributes) در پارامترهای درخواست</h3>
<p>
اگر درخواست HTTP، شامل پارامترهای تودوتو (nested) باشد، در قوانین اعتبارسنجی می‌توان آن‌ها را با استفاده از علامت «نقطه» مشخص کرد:
</p>

<pre><code class="language-php  line-numbers">$request->validate([
    'title' => 'required|unique:posts|max:255',
    'author.name' => 'required',
    'author.description' => 'required',
]);
</code></pre>

<p>
<a name="quick-displaying-the-validation-errors"></a>
</p>

<br>
<h3>نمایش خطاهای اعتبارسنجی </h3>
<p>
اگر پارامترهای درخواست ورودی با قوانین اعتبارسنجی داده شده مطابقت نداشته باشند، چکار باید کرد؟ همانطور که قبلاً ذکر شد، لاراول به صورت خودکار کاربر را به مکان قبلی هدایت خواهد کرد. علاوه بر این، تمام خطاهای اعتبارسنجی به صورت خودکار در سشن نوشته می‌شوند.
</p>
<p>
نباید به صورت صریح پیام‌های خطا را در مسیر GET به view بایند کنیم. به این دلیل که لاراول خطاها را در داده‌های سشن بررسی می‌کند و اگر آن‌ها در دسترس باشند، به صورت خودکار آن‌ها را به view بایند می‌کند. متغیر $errors یک نمونه از کلاس Illuminate\Support\MessageBag است.
</p>
<blockquote class="has-icon tip">
 متغیر $errors توسط میدلور Illuminate\View\Middleware\ShareErrorsFromSession به view بایند می‌شود که به وسیله گروه middleware web  ارائه می‌شود. زمانی که این middleware اعمال می‌شود متغیر $errors همواره در view در دسترس خواهد بود و در هر زمانی می‌توان از آن استفاده کرد.
</blockquote>
<p>
بنابراین، در این مثال، زمانی که اعتبارسنجی ناموفق باشد، کاربر به متد create کنترلر برگردانده می‌شود و می‌توانیم پیام‌های خطا را در view به کاربر نمایش دهیم:
</p>

```html
<!-- /resources/views/post/create.blade.php -->

<h1>Create Post</h1>

@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{% raw %}{{ $error }}{% endraw %}</li>
            @endforeach
        </ul>
    </div>
@endif

<!-- Create Post Form -->
```

<p>
<a name="a-note-on-optional-fields"></a>
</p>

<br>
<h3>فیلدهای اختیاری در اعتبارسنجی</h3>
<p>
لاراول به صورت پیش‌فرض شامل میدلورهای TrimStrings و ConvertEmptyStringsToNull در پشته middleware عمومی برنامه است. این middlewarelها توسط کلاس App\Http\Kernel در پشته لیست شده‌اند. به همین دلیل، اگر بخواهید مقادیر null توسط عملیات اعتبارسنجی یک مقدار نامعتبر در نظر گرفته نشوند، باید فیلد‌های درخواست اختیاری را به عنوان nullable علامتگذاری کنید. به مثال زیر توجه کنید:
</p>

<pre><code class="language-php  line-numbers">$request->validate([
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
    'publish_at' => 'nullable|date',
]);
</code></pre>

<p>
در این مثال، فیلد publish_at مشخص شده است که می‌تواند شامل یک مقدار null یا یک نمایش تاریخ معتبر باشد. اگر اصلاح کننده nullable به تعریف قانون اعتبارسنجی اضافه نشود، validator مقدار null را یک تاریخ نامعتبر در نظر می‌گیرد.
</p>
<p>
<a name="quick-ajax-requests-and-validation"></a>
</p>

<br>
<h3>درخواست‌های AJAX در اعتبارسنجی</h3>
<p>
در این مثال، ما از یک فرم سنتی استفاده کردیم. با این حال، بسیاری از برنامه‌های کاربردی از درخواست‌های AJAX برای ارسال داده‌ها به برنامه استفاده می‌کنند. هنگام استفاده از متد validate در طول یک درخواست AJAX، لاراول یک پاسخ redirect به صفحه قبل ایجاد نمی‌کند. به جای این کار، لاراول یک پاسخ JSON شامل تمام خطاهای اعتبارسنجی ایجاد می‌کند. این پاسخ JSON با یک کد وضعیت 422 HTTP به کاربر ارسال می‌شود.
</p>
<p>
<a name="form-request-validation"></a>
</p>

<br>
<h3><a href="#form-request-validation">اعتبارسنجی توسط form request</a></h3>
<p>
<a name="creating-form-requests"></a>
</p>

<p>
برای نوشتن سناریوهای پیچیده جهت اعتبارسنجی داده‌ها، می‌توانید یک درخواست فرم یا form request ایجاد کنید. درخواست‌های فرم، کلاس‌های درخواست سفارشی هستند که منطق اعتبارسنجی را در درون خود جای می‌دهند. برای ایجاد یک کلاس form request، می‌توان از دستور آرتیسان make:request به صورت زیر استفاده کرد:
</p>

<pre><code class="language-php  line-numbers">php artisan make:request StoreBlogPost
</code></pre>

<p>
کلاس ایجاد شده در دایرکتوری app/Http/Requests قرار می‌گیرد. اگر این دایرکتوری موجود نباشد، در زمان اجرای دستور make:request ایجاد خواهد شد. اجازه دهید، چند قانون اعتبارسنجی را به متد rules اضافه کنیم:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the validation rules that apply to the request.
 *
 * @return array
 */
public function rules()
{
    return [
        'title' => 'required|unique:posts|max:255',
        'body' => 'required',
    ];
}
</code></pre>

<p>
چگونه می‌توان قوانین اعتبارسنجی را ارزیابی کرد؟ تمام آن چیزی که باید انجام داد، این است که form request را در متد کنترلر اعلان نوع یا type-hint کنید. درخواست فرم ورودی قبل از فراخوانی متد کنترلر اعتبارسنجی می‌شود. به ایم معنی که دیگر نیازی به وارد کردن منطق اعتبارسنجی خود در درون کلاس کنترلر نخواهید داشت:
</p>

<pre><code class="language-php  line-numbers">/**
 * Store the incoming blog post.
 *
 * @param  StoreBlogPost  $request
 * @return Response
 */
public function store(StoreBlogPost $request)
{
    // The incoming request is valid...
}
</code></pre>

<p>
اگر عملیات اعتبارسنجی ناموفق باشد، یک پاسخ redirect ایجاد شده و کاربر را به صفحه قبل برمی‌‌گرداند. خطاها نیز در سشن flash می‌شوند (نوشته می‌شوند) تا برای نمایش دادن به کاربر در دسترس قرا گیرند. اگر درخواست ورودی یک درخواست AJAX باشد، یک پاسخ HTTP با کد وضعیت 422، شامل یک نمایش JSON از خطاهای اعتبارسنجی به کاربر نمایش داده خواهد شد.
</p>

<br>
<h3>اضافه کردن after hook به form request</h3>
<p>
اگر بخواهید یک hook after را به form request اضافه کنید، می‌توانید از متد withValidator استفاده کنید. این متد اعتبارسنجی ایجاد شده را به صورت کامل دریافت می‌کند و این امکان را می‌دهد که قبل از ارزیابی قوانین اعتبارسنجی به صورت واقعی، بتوانید متدهایش را فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Configure the validator instance.
 *
 * @param  \Illuminate\Validation\Validator  $validator
 * @return void
 */
public function withValidator($validator)
{
    $validator->after(function ($validator) {
        if ($this->somethingElseIsInvalid()) {
            $validator->errors()->add('field', 'Something is wrong with this field!');
        }
    });
}
</code></pre>

<p>
<a name="authorizing-form-requests"></a>
</p>

<br>
<h3>احراز هویت Form Request</h3>
<p>
کلاس form request نیز شامل یک متد authorize است. می‌توانید در این متد، مجوز کاربر احراز هویت شده به ویرایش یک منبع داده شده را بررسی کنید. برای مثال، می‌توانید تعیین کنید که آیا کاربر کامنتی بر روی یک پست دارد که می‌خواهد آن را ویرایش کند؟
</p>

<pre><code class="language-php  line-numbers">/**
 * Determine if the user is authorized to make this request.
 *
 * @return bool
 */
public function authorize()
{
    $comment = Comment::find($this->route('comment'));

    return $comment && $this->user()->can('update', $comment);
}
</code></pre>

<p>
از آنجایی که تمام form requestها، از کلاس request لاراول ارث‌ بری می‌کنند، می‌توان از متد user برای دسترسی به کاربری که در حال حاضر احراز هویت شده است، استفاده کرد. به فراخوانی متد route در مثال بالا توجه کنید، این متد امکان می‌دهد که به پارامترهای URI تعریف شده در فراخوانی مسیر مانند پارامتر {comment} در مثال زیر دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">Route::post('comment/{comment}');
</code></pre>

<p>
اگر متد authorize مقدار false را برگرداند، یک پاسخ HTTP با کد وضعیت 403 به صورت خودکار بازگردانده می‌شود و متد کنترلر اجرا نمی‌شود.
</p>
<p>
اگر قصد دارید منطق احراز هویت را در قسمت دیگری از برنامه خود بگنجانید، می‌‌توانید به راحتی مقدار true را از متد authorize برگردانید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Determine if the user is authorized to make this request.
 *
 * @return bool
 */
public function authorize()
{
    return true;
}
</code></pre>

<p>
<a name="customizing-the-error-messages"></a>
</p>

<br>
<h3>سفارشی سازی پیام‌های خطای اعتبارسنجی </h3>
<p>
می‌توان پیام‌های خطای مورد استفاده در form request را با بازنویسی متد messages سفارشی کرد. این متد باید آرایه‌ای از صفت و قانون اعتبارسنجی آن و همچنین پیام‌های خطای مربوط به آن‌ها را بازگرداند:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the error messages for the defined validation rules.
 *
 * @return array
 */
public function messages()
{
    return [
        'title.required' => 'A title is required',
        'body.required'  => 'A message is required',
    ];
}
</code></pre>

<p>
<a name="manually-creating-validators"></a>
</p>

<br>
<h3><a href="#manually-creating-validators">ایجاد اعتبارسنجی به صورت دستی در لاراول</a></h3>
<p>
اگر نمی‌خواهید از متد validate در درخواست استفاده کنید، می‌توانید یک نمونه اعتبارسنجی را به صورت دستی با استفاده از facade Validator  ایجاد کنید. متد make در facade یک نمونه validator جدید ایجاد می‌کند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Validator;
use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class PostController extends Controller
{
    /**
     * Store a new blog post.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $validator = Validator::make($request->all(), [
            'title' => 'required|unique:posts|max:255',
            'body' => 'required',
        ]);

        if ($validator->fails()) {
            return redirect('post/create')
                        ->withErrors($validator)
                        ->withInput();
        }

        // Store the blog post...
    }
}
</code></pre>

<p>
اولین آرگومانی که به متد make انتقال داده می شود، داده‌ای است که باید اعتبارسنجی شود. آرگومان دوم قوانین اعتبارسنجی است که باید بر روی داده‌ها اعمال شوند.
</p>
<p>
پس از بررسی داده، اگر اعتبارسنجی ناموفق باشد؛ می‌توان از متد withErrors برای flash کردن پیام‌های خطا در session استفاده کرد. هنگام استفاده از این متد متغیر $errors به صورت خودکار با viewهای برنامه پس از redirect شدن، به اشتراک گذاشته می‌شود که امکان می‌دهد به سادگی بتوانید، آن‌ها را به کاربر نمایش دهید. متد withErrors یک validator، MessageBag یا یک array می‌پذیرد.
</p>
<p>
<a name="automatic-redirection"></a>
</p>

<br>
<h3>تغییر مسیر اتوماتیک (automatic redirection) در اعتبارسنجی لاراول</h3>
<p>
اگر می‌خواهید، به صورت دستی یک نمونه validator ایجاد کنید، اما هنوز هم از تغییرمسیر (redirect) خودکار ارائه شده توسط متد validate درخواست استفاده می‌کنید، می‌توانید متد validate را بر روی نمونه validator موجود فراخوانی کنید. اگر اعتبارسنجی ناموفق باشد، کاربر به صورت خودکار redirect می‌شود (به صفحه قبلی برمی‌گردد) و یا در صورت استفاده از درخواست AJAX یک پاسخ JSON به شما ارائه خواهد داد:
</p>

<pre><code class="language-php  line-numbers">Validator::make($request->all(), [
    'title' => 'required|unique:posts|max:255',
    'body' => 'required',
])->validate();
</code></pre>

<p>
<a name="named-error-bags"></a>
</p>

<br>
<h3>بسته‌های خطای نامگذاری شده (Named Error Bags) </h3>
<p>
اگر چند فرم در یک صفحه داشته باشید، می‌توانید MessageBag خطاها را نامگذاری کنید که امکان اینکه پیام‌های خطا را برای یک فرم خاص بازیابی کنید را فراهم می‌کند. جای نگرانی نیست، می‌توانید نام مشخص شده را به عنوان آرگومان دوم به withErrors انتقال دهید:
</p>

<pre><code class="language-php  line-numbers">return redirect('register')
            ->withErrors($validator, 'login');
</code></pre>

<p>
پس از آن، می‌توانید به نمونه MessageBag از متغیر $errors دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">{% raw %}{{ $errors->login->first('email') }}{% endraw %}
</code></pre>

<p>
<a name="after-validation-hook"></a>
</p>

<br>
<h3>After Validation Hook</h3>
<p>
همچنین validator اجازه می‌دهد تا پس از کامل شدن عملیات اعتبارسنجی، attach callbacks را اجرا کنید. این موضوع این امکان را می‌دهد که بتوانید به راحتی اعتبارسنجی بیشتری انجام دهید و حتی پیام‌های خطای بیشتری را به مجموعه پیام‌‌های خطا اضافه کنید. برای شروع کار، می‌توانید از متد after بر روی یک نمونه validator استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$validator = Validator::make(...);

$validator->after(function ($validator) {
    if ($this->somethingElseIsInvalid()) {
        $validator->errors()->add('field', 'Something is wrong with this field!');
    }
});

if ($validator->fails()) {
    //
}
</code></pre>

<p>
<a name="working-with-error-messages"></a>
</p>

<br>
<h3><a href="#working-with-error-messages">کار با پیام‌های خطا در اعتبارسنجی لاراول
</a></h3>
<p>
پس از فراخوانی متد errors بر روی یک نمونه Validator ، یک نمونه کلاس Illuminate\Support\MessageBag دریافت خواهید کرد که متدهای مختلفی را برای کار با پیام‌های خطا ارائه می‌دهد. متغیر $errors که به صورت خودکار برای همه viewها در دسترس است نیز نمونه‌ای از کلاس MessageBag است.
</p>

<br>
<h3>بازیابی اولین پیام خطا برای یک فیلد</h3>
<p>
برای بازیابی اولین پیام خطا برای یک فیلد، می‌توانید از متد first استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$errors = $validator->errors();

echo $errors->first('email');
</code></pre>


<br>
<h3>بازیابی تمام پیام‌های خطا برای یک فیلد</h3>
<p>
اگر نیاز به بازیابی یک آرایه از تمام پیام‌های خطا برای یک فیلد دارید، می‌توانید از متد get استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">foreach ($errors->get('email') as $message) {
    //
}
</code></pre>

<p>
اگر یک فیلد فرم آرایه را اعتبارسنجی می‌کنید، می‌توانید تمام پیام‌های خطا را برای هر عنصر آرایه با استفاده از کاراکتر * بازیابی کنید.
</p>

<pre><code class="language-php  line-numbers">foreach ($errors->get('attachments.*') as $message) {
    //
}
</code></pre>


<br>
<h3>بازیابی تمام پیام‌های خطا برای تمام فیلدها</h3>
<p>
برای بازیابی آرایه‌ای از تمام پیام‌های خطا برای تمام فیلدها، می‌توانید از متد all استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">foreach ($errors->all() as $message) {
    //
}
</code></pre>


<br>
<h3>تعیین وجود پیام خطا برای یک فیلد</h3>
<p>
متد has برای تعیین اینکه آیا پیام‌ خطا برای یک فیلد مشخص وجود دارد یا خیر، استفاده می‌شود:
</p>

<pre><code class="language-php  line-numbers">if ($errors->has('email')) {
    //
}
</code></pre>

<p>
<a name="custom-error-messages"></a>
</p>

<br>
<h3>پیام‌های خطای سفارشی در اعتبارسنجی لاراول</h3>
<p>
در صورت نیاز، می‌توانید به جای استفاده از حالت پیش‌فرض، پیام‌های خطای اعتبارسنجی را به صورت سفارشی ایجاد کنید. چندین راه برای ایجاد پیام‌های خطا به صورت سفارشی وجود دارد. در روش اول، می‌توانید پیام‌های خطای سفارشی را به عنوان آرگومان سوم به Validator::make انتقال دهید:
</p>

<pre><code class="language-php  line-numbers">$messages = [
    'required' => 'The :attribute field is required.',
];

$validator = Validator::make($input, $rules, $messages);
</code></pre>

<p>
در این مثال، بخش :attribute با نام واقعی فیلدی که اعتبارسنجی ‌می‌شود جایگزین می‌شود. همچنین می‌توانید از نام‌‌های دیگر در پیام‌های اعتبارسنجی استفاده کنید.
</p>

<pre><code class="language-php  line-numbers">$messages = [
    'same'    => 'The :attribute and :other must match.',
    'size'    => 'The :attribute must be exactly :size.',
    'between' => 'The :attribute value :input is not between :min - :max.',
    'in'      => 'The :attribute must be one of the following types: :values',
];
</code></pre>


<br>
<h3>تعیین پیام خطای سفارشی برای یک صفت مشخص</h3>
<p>
گاهی اوقات ممکن است بخواهید، فقط برای یک فیلد خاص پیام‌های خطای سفارشی ایجاد کنید. این کار را می‌توانید با استفاده از علامت «نقطه» انجام دهید. ابتدا، اسم صفت را مشخص کنید و به دنبال آن از قانون اعتبارسنجی استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">$messages = [
    'email.required' => 'We need to know your e-mail address!',
];
</code></pre>

<p>
<a name="localization"></a>
</p>

<br>
<h3>تعیین پیام‌های خطای سفارشی در فایل‌های language</h3>
<p>
در اغلب موارد، به جای انتقال مستقیم پیام‌های خطای سفارشی خود به validator می‌توانید آن‌ها را در یک فایل language قرار دهید. برای انجام این کار، پیام‌های خود را به آرایه custom در فایل resources/lang/xx/validation.php اضافه کنید.
</p>

<pre><code class="language-php  line-numbers">'custom' => [
    'email' => [
        'required' => 'We need to know your e-mail address!',
    ],
],
</code></pre>


<br>
<h3>مشخص کردن صفت سفارشی در فایل‌های language</h3>
<p>
اگر بخواهید بخش :attribute از پیام اعتبارسنجی با یک نام attribute سفارشی جایگزین شود، می‌توانید نام سفارشی خود را در آرایه attributes از فایل resources/lang/xx/validation.php مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">'attributes' => [
    'email' => 'email address',
],
</code></pre>

<p>
<a name="available-validation-rules"></a>
</p>

<br>
<h3><a href="#available-validation-rules">Available Validation Rules</a></h3>
<p>
 در این فهرست، لیستی از قوانین اعتبارسنجی موجود در لاراول و توابع آن‌ها را مشاهده می‌کنید:
</p>
<style>
    .collection-method-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
    }

    .collection-method-list a {
        display: block;
    }
</style>
<div class="collection-method-list">
<p>
<a href="#rule-accepted">Accepted</a>
<a href="#rule-active-url">Active URL</a>
<a href="#rule-after">After (Date)</a>
<a href="#rule-after-or-equal">After Or Equal (Date)</a>
<a href="#rule-alpha">Alpha</a>
<a href="#rule-alpha-dash">Alpha Dash</a>
<a href="#rule-alpha-num">Alpha Numeric</a>
<a href="#rule-array">Array</a>
<a href="#rule-before">Before (Date)</a>
<a href="#rule-before-or-equal">Before Or Equal (Date)</a>
<a href="#rule-between">Between</a>
<a href="#rule-boolean">Boolean</a>
<a href="#rule-confirmed">Confirmed</a>
<a href="#rule-date">Date</a>
<a href="#rule-date-equals">Date Equals</a>
<a href="#rule-date-format">Date Format</a>
<a href="#rule-different">Different</a>
<a href="#rule-digits">Digits</a>
<a href="#rule-digits-between">Digits Between</a>
<a href="#rule-dimensions">Dimensions (Image Files)</a>
<a href="#rule-distinct">Distinct</a>
<a href="#rule-email">E-Mail</a>
<a href="#rule-exists">Exists (Database)</a>
<a href="#rule-file">File</a>
<a href="#rule-filled">Filled</a>
<a href="#rule-image">Image (File)</a>
<a href="#rule-in">In</a>
<a href="#rule-in-array">In Array</a>
<a href="#rule-integer">Integer</a>
<a href="#rule-ip">IP Address</a>
<a href="#rule-json">JSON</a>
<a href="#rule-max">Max</a>
<a href="#rule-mimetypes">MIME Types</a>
<a href="#rule-mimes">MIME Type By File Extension</a>
<a href="#rule-min">Min</a>
<a href="#rule-nullable">Nullable</a>
<a href="#rule-not-in">Not In</a>
<a href="#rule-numeric">Numeric</a>
<a href="#rule-present">Present</a>
<a href="#rule-regex">Regular Expression</a>
<a href="#rule-required">Required</a>
<a href="#rule-required-if">Required If</a>
<a href="#rule-required-unless">Required Unless</a>
<a href="#rule-required-with">Required With</a>
<a href="#rule-required-with-all">Required With All</a>
<a href="#rule-required-without">Required Without</a>
<a href="#rule-required-without-all">Required Without All</a>
<a href="#rule-same">Same</a>
<a href="#rule-size">Size</a>
<a href="#rule-string">String</a>
<a href="#rule-timezone">Timezone</a>
<a href="#rule-unique">Unique (Database)</a>
<a href="#rule-url">URL</a>
</p>
</div>
<p>
<a name="rule-accepted"></a>
</p>

<br>
<h3>accepted</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید دارای مقادیر yes ، on ، 1 ، یا true باشد. این قانون برای اعتبارسنجی پذیرش «شرایط استفاده از خدمات» مفید است.
</p>
<p>
<a name="rule-active-url"></a>
</p>

<br>
<h3>active_url</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید دارای یک رکورد معتبر A یا AAAA براساس تابع پی اچ پی dns_get_record باشد.
</p>
<p>
<a name="rule-after"></a>
</p>

<br>
<h3>after:<em>date</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید دارای یک مقدار پس از یک تاریخ معین باشد. تاریخ‌ها به تابع پی اچ پی strtotime  منتقل می‌شوند:
</p>

<pre><code class="language-php  line-numbers">'start_date' => 'required|date|after:tomorrow'
</code></pre>

<p>
به جای انتقال یک رشته شامل تاریخ که توسط تابع strtotime ارزیابی می‌شود، می‌توانید یک فیلد دیگر برای مقایسه با تاریخ معین، مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">'finish_date' => 'required|date|after:start_date'
</code></pre>

<p>
<a name="rule-after-or-equal"></a>
</p>

<br>
<h3>after_or_equal:<em>date</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک مقدار برابر یا پس از یک تاریخ معین باشد. برای اطلاعات بیشتر، قانون after را مطالعه کنید.
</p>
<p>
<a name="rule-alpha"></a>
</p>

<br>
<h3>alpha</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید به صورت کامل شامل حروف الفبا باشد.
</p>
<p>
<a name="rule-alpha-dash"></a>
</p>

<br>
<h3>alpha_dash</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، می‌تواند دارای کاراکترهای عددی و حروف باشد، همچنین خط تیره و زیر خط را نیز می‌تواند شامل شود.
</p>
<p>
<a name="rule-alpha-num"></a>
</p>

<br>
<h3>alpha_num</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید به صورت کامل دارای کاراکترهای حروف و عدد باشد.
</p>
<p>
<a name="rule-array"></a>
</p>

<br>
<h3>array</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک آرایه PHP باشد.
</p>
<p>
<a name="rule-before"></a>
</p>

<br>
<h3>before:<em>date</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک مقدار قبل از تاریخ معین باشد. تاریخ‌ها به تابع پی اچ پی strtotime منتقل می‌شوند.
</p>
<p>
<a name="rule-before-or-equal"></a>
</p>

<br>
<h3>before_or_equal:<em>date</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک مقدار قبل یا برابر یک تاریخ معین باشد. تاریخ‌ها به تابع پی اچ پی strtotime منتقل می‌شوند.
</p>
<p>
<a name="rule-between"></a>
</p>

<br>
<h3>between:<em>min</em>,<em>max</em></h3>
<p>
اندازه فیلدی که اعتبارسنجی می‌شود، باید بین مقادیر min و max باشد. رشته‌ها، اعداد، آرایه‌ها، و فایل‌ها با همان روش قانون size ارزیابی می‌شوند.
</p>
<p>
<a name="rule-boolean"></a>
</p>

<br>
<h3>boolean</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید قابل تبدیل به یک مقدار boolean باشد. ورودی قابل قبول می‌تواند مقادیر true ، false ، 1 ، 0 ، “ 1 ” و “ 0 ” باشد.
</p>
<p>
<a name="rule-confirmed"></a>
</p>

<br>
<h3>confirmed</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید با فیلد foo_confirmation مطابقت داشته باشد. برای مثال، اگر فیلدی که اعتبارسنجی می‌شود، فیلد password باشد، باید یک فیلد password_confirmation  مربوط به آن در ورودی باشد.
</p>
<p>
<a name="rule-date"></a>
</p>

<br>
<h3>date</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک تاریخ معتبر براساس تابع پی اچ پی strtotime باشد.
</p>
<p>
<a name="rule-date-equals"></a>
</p>

<br>
<h3>date_equals:<em>date</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک مقدار برابر با تاریخ معین باشد. تاریخ‌ها به تابع پی اچ پی strtotime منتقل می‌شوند.
</p>
<p>
<a name="rule-date-format"></a>
</p>

<br>
<h3>date_format:<em>format</em></h3>
<p>
فرمت فیلد تاریخ مورد اعتبارسنجی، باید با فرمت داده شده مطابقت داشته باشد. هنگام اعتبارسنجی یک فیلد، باید از date یا date_format استفاده کنید، نمی‌توان از هر دو آن‌ها به صورت همزمان استفاده کرد.
</p>
<p>
<a name="rule-different"></a>
</p>

<br>
<h3>different:<em>field</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید دارای مقدار متفاوتی از فیلد معین باشد.
</p>
<p>
<a name="rule-digits"></a>
</p>

<br>
<h3>digits:<em>value</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید عددی باشد و باید دارای طول دقیق براساس value باشد.
</p>
<p>
<a name="rule-digits-between"></a>
</p>

<br>
<h3>digits_between:<em>min</em>,<em>max</em></h3>
<p>
طول فیلدی که اعتبارسنجی می‌شود، باید بین min و max باشد.
</p>
<p>
<a name="rule-dimensions"></a>
</p>

<br>
<h3>dimensions</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک تصویر باشد و محدودیت‌های ابعادی اعمال شده توسط پارامترهای قانون اعتبارسنجی را رعایت کند.
</p>

<pre><code class="language-php  line-numbers">'avatar' => 'dimensions:min_width=100,min_height=200'
</code></pre>

<p>
محدودیت‌های موجود عبارتند از: min_width، max_width، min_height، max_height، width، height، ratio.
</p>
<p>
محدودیت ratio باید به صورت عرض تقسیم بر ارتفاع باشد. این را می‌توان با یک عبارت مانند 3/2 یا یک مقدار اعشاری مانند 1.5 مشخص کرد:
</p>

<pre><code class="language-php  line-numbers">'avatar' => 'dimensions:ratio=3/2'
</code></pre>

<p>
از آنجا که این قانون مستلزم دریافت چند آرگومان است، می‌توانید از متد Rule::dimensions استفاده کنید تا به راحتی بتوانید قانون اعتبارسنجی خود را بسازید:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Validation\Rule;

Validator::make($data, [
    'avatar' => [
        'required',
        Rule::dimensions()->maxWidth(1000)->maxHeight(500)->ratio(3 / 2),
    ],
]);
</code></pre>

<p>
<a name="rule-distinct"></a>
</p>

<br>
<h3>distinct</h3>
<p>
در هنگام کار با آرایه‌ها، فیلدی که اعتبارسنجی می‌شود، نباید دارای مقادیر تکراری باشد.
</p>

<pre><code class="language-php  line-numbers">'foo.*.id' => 'distinct'
</code></pre>

<p>
<a name="rule-email"></a>
</p>

<br>
<h3>email</h3>
<p>
فرمت فیلدی که اعتبارسنجی می‌شود، باید براساس فرمت یک آدرس ایمیل باشد.
</p>
<p>
<a name="rule-exists"></a>
</p>

<br>
<h3>exists:<em>table</em>,<em>column</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در جدول پایگاه داده معین موجود باشد.
</p>

<br>
<h3>مثالی ساده از قانون اعتبارسنجی exists</h3>

<pre><code class="language-php  line-numbers">'state' => 'exists:states'
</code></pre>


<br>
<h3>مشخص کردن نام سفارشی برای یک ستون</h3>

<pre><code class="language-php  line-numbers">'state' => 'exists:states,abbreviation'
</code></pre>

<p>
می‌توان یک database connection خاص مشخص کرد که می‌توان از آن در کوئری exists استفاده کرد. این کار را می‌توان با اضافه کردن نام connection توسط علامت «نقطه» به نام جدول انجام داد:
</p>

<pre><code class="language-php  line-numbers">'email' => 'exists:connection.staff,email'
</code></pre>

<p>
اگر بخواهید، اجرای کوئری را توسط قانون اعتبارسنجی سفارشی کنید، می‌توانید از کلاس Rule استفاده کنید تا به راحتی قانون مورد نظر را تعریف کنید. در این مثال، به جای آنکه از کاراکتر | برای ایجاد محدودیت در قوانین اعتبارسنجی استفاده کنیم، آن‌ها را به صورت یک آرایه مشخص کردیم:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Validation\Rule;

Validator::make($data, [
    'email' => [
        'required',
        Rule::exists('staff')->where(function ($query) {
            $query->where('account_id', 1);
        }),
    ],
]);
</code></pre>

<p>
<a name="rule-file"></a>
</p>

<br>
<h3>file</h3>
<p>
فیلد فایلی که اعتبارسنجی می‌شود، باید با موفقیت آپلود شود.
</p>
<p>
<a name="rule-filled"></a>
</p>

<br>
<h3>filled</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، در صورت وجود نباید خالی باشد.
</p>
<p>
<a name="rule-image"></a>
</p>

<br>
<h3>image</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک تصویر با فرمت‌های (jpeg، png، bmp، gif، svg) باشد.
</p>
<p>
<a name="rule-in"></a>
</p>

<br>
<h3>in:<em>foo</em>,<em>bar</em>,...</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در لیست داده‌های موجود قرار گیرد. از آنجا که این قاعده نیاز دارد که یک آرایه را implode کنید، متد Rule::in کمک می‌کند تا به سادگی این قانون را بسازید:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Validation\Rule;

Validator::make($data, [
    'zones' => [
        'required',
        Rule::in(['first-zone', 'second-zone']),
    ],
]);
</code></pre>

<p>
<a name="rule-in-array"></a>
</p>

<br>
<h3>in_array:<em>anotherfield</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در مقادیر anotherfield نیز وجود داشته باشد.
</p>
<p>
<a name="rule-integer"></a>
</p>

<br>
<h3>integer</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک عدد صحیح یا integer باشد.
</p>
<p>
<a name="rule-ip"></a>
</p>

<br>
<h3>ip</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک آدرس IP باشد.
</p>

<br>
<h3>ipv4</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک آدرس IPv4 باشد.
</p>

<br>
<h3>ipv6</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک آدرس IPv6 باشد.
</p>
<p>
<a name="rule-json"></a>
</p>

<br>
<h3>json</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک رشته معتبر JSON باشد.
</p>
<p>
<a name="rule-max"></a>
</p>

<br>
<h3>max:<em>value</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید کمتر یا برابر با بیشترین مقدار باشد. رشته‌ها، اعداد، آرایه‌ها، و فایل‌ها با همان روش قانون size ارزیابی می‌شوند.
</p>
<p>
<a name="rule-mimetypes"></a>
</p>

<br>
<h3>mimetypes:<em>text/plain</em>,...</h3>
<p>
فایلی که اعتبارسنجی می‌شود، باید با یکی از انواع MIME مشخص شده مطابقت داشته باشد:
</p>

<pre><code class="language-php  line-numbers">'video' => 'mimetypes:video/avi,video/mpeg,video/quicktime'
</code></pre>

<p>
برای تعیین نوع MIME فایل آپلود شده، محتویات فایل خوانده می‌شود و فریم ورک تلاش می‌کند تا نوع MIME را حدس بزند که ممکن است با نوع MIME ارائه شده توسط کلاینت متفاوت باشد.
</p>
<p>
<a name="rule-mimes"></a>
</p>

<br>
<h3>mimes:<em>foo</em>,<em>bar</em>,...</h3>
<p>
فایلی که اعتبارسنجی می‌شود، باید دارای یک نوع MIME مربوط به یکی از پسوند‌های لیست شده باشد.
</p>

<br>
<h3>Basic Usage Of MIME Rule</h3>

<pre><code class="language-php  line-numbers">'photo' => 'mimes:jpeg,bmp,png'
</code></pre>

<p>
با اینکه فقط پسوند فایل را برای اعتبارسنجی مشخص کردیم، ولی این قانون در واقع با خواندن محتویات فایل و حدس زدن نوع MIME آن، اعتبارسنجی را انجام می‌دهد.
</p>
<p>
لیست کامل انواع MIME و پسوند مربوط به آن‌ها را می‌توانید در لینک زیر مشاهده کنید:
<br>
 https://svn.apache.org/repos/asf/httpd/httpd/trunk/docs/conf/mime.types
</p>
<p>
<a name="rule-min"></a>
</p>

<br>
<h3>min:<em>value</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید حداقل مقدار را داشته باشد. رشته‌ها، اعداد، آرایه‌ها، و فایل‌ها با همان روش قانون size ارزیابی می‌شوند.
</p>
<p>
<a name="rule-nullable"></a>
</p>

<br>
<h3>nullable</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، می‌تواند مقدار null داشته باشد. این قانون در هنگام اعتبارسنجی اولیه مانند رشته‌ها و اعداد صحیح که می‌توانند مقادیر null را داشته باشند، مفید باشد.
</p>
<p>
<a name="rule-not-in"></a>
</p>

<br>
<h3>not_in:<em>foo</em>,<em>bar</em>,...</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، نباید در لیست داده‌های موجود قرار گیرد. از متد Rule::notIn می‌توان استفاده کرد و به سادگی این قانون را ایجاد کرد:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Validation\Rule;

Validator::make($data, [
    'toppings' => [
        'required',
        Rule::notIn(['sprinkles', 'cherries']),
    ],
]);
</code></pre>

<p>
<a name="rule-numeric"></a>
</p>

<br>
<h3>numeric</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید عددی باشد.
</p>
<p>
<a name="rule-present"></a>
</p>

<br>
<h3>present</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در داده‌ ورودی موجود باشد، اما می‌تواند خالی نیز باشد.
</p>
<p>
<a name="rule-regex"></a>
</p>

<br>
<h3>regex:<em>pattern</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید با عبارات منظم (regular expression) داده شده مطابقت داشته باشد.
</p>
<p>
نکته: در هنگام استفاده از الگوریتم regex ، می‌توانید به جای استفاده از pipe delimiter، قوانین را در آرایه مشخص کنید، به خصوص اگر عبارات منظم شامل یک کاراکتر pipe باشند.
</p>
<p>
<a name="rule-required"></a>
</p>

<br>
<h3>required</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در داده ورودی موجود باشد و خالی نیز نباشد. در صورتی یک فیلد empty یا خالی محسوب می‌شود که یکی از شرایط زیر را دارا باشد:
</p>
<div class="content-list">
<ul>
<li>مقدار null است.</li>
<li>مقدار یک رشته خالی است.</li>
<li>مقدار یک آرایه خالی یا شئ Countable خالی است.</li>
<li>مقدار یک فایل آپلود شده بدون مسیر است.</li>
</ul>
</div>
<p>
<a name="rule-required-if"></a>
</p>

<br>
<h3>required_if:<em>anotherfield</em>,<em>value</em>,...</h3>
<p>
اگر فیلد anotherfield برابر با هر مقداری باشد، فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد.
</p>
<p>
<a name="rule-required-unless"></a>
</p>

<br>
<h3>required_unless:<em>anotherfield</em>,<em>value</em>,...</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد، مگر اینکه فیلد anotherfield برابر با هر مقداری باشد.
</p>
<p>
<a name="rule-required-with"></a>
</p>

<br>
<h3>required_with:<em>foo</em>,<em>bar</em>,...</h3>
<p>
تنها اگر هر یک از فیلدهای مشخص شده دیگر موجود باشند، فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد.
</p>
<p>
<a name="rule-required-with-all"></a>
</p>

<br>
<h3>required_with_all:<em>foo</em>,<em>bar</em>,...</h3>
<p>
تنها اگر تمام فیلدهای مشخص شده دیگر موجود باشند، فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد.
</p>
<p>
<a name="rule-required-without"></a>
</p>

<br>
<h3>required_without:<em>foo</em>,<em>bar</em>,...</h3>
<p>
تنها زمانی که هر یک از فیلدهای مشخص شده دیگر موجود نباشند، فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد.
</p>
<p>
<a name="rule-required-without-all"></a>
</p>

<br>
<h3>required_without_all:<em>foo</em>,<em>bar</em>,...</h3>
<p>
تنها زمانی که تمام فیلدهای مشخص شده دیگر موجود نباشند، فیلدی که اعتبارسنجی می‌شود، باید موجود باشد و خالی نیز نباشد.
</p>
<p>
<a name="rule-same"></a>
</p>

<br>
<h3>same:<em>field</em></h3>
<p>
فیلد داده شده باید با فیلدی که اعتبارسنجی می‌شود، مطابق باشد.
</p>
<p>
<a name="rule-size"></a>
</p>

<br>
<h3>size:<em>value</em></h3>
<p>
اندازه فیلدی که اعتبارسنجی می‌شود، باید مطابق با مقدار مشخص شده باشد. برای داده‌های رشته‌ای، این مقدار تعداد کاراکترها را تعیین می‌کند. برای داده‌های عددی، این مقدار یک عدد صحیح را تعیین می‌کند. برای آرایه‌ها، این اندازه به count آرایه اشاره می‌‌کند. برای فایل‌ها، این اندازه به اندازه فایل در کیلوبایت اشاره می‌کند.
</p>
<p>
<a name="rule-string"></a>
</p>

<br>
<h3>string</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک رشته باشد. اگر بخواهید این فیلد بتواند مقدار null را نیز داشته باشد، باید قانون اعتبارسنجی nullable را نیز به این فیلد اختصاص دهید.
</p>
<p>
<a name="rule-timezone"></a>
</p>

<br>
<h3>timezone</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک شناسه منطقه زمانی معتبر براساس تابع پی اچ پی ‍ timezone_identifiers_list باشد.
</p>
<p>
<a name="rule-unique"></a>
</p>

<br>
<h3>unique:<em>table</em>,<em>column</em>,<em>except</em>,<em>idColumn</em></h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید در جدول پایگاه داده مشخص، یکتا باشد. اگر گزینه column مشخص نشده باشد، نام فیلد مورد استفاده قرار می‌گیرد.
</p>
<p>
<strong>مشخص کردن یک نام ستون سفارشی</strong>
</p>

<pre><code class="language-php  line-numbers">'email' => 'unique:users,email_address'
</code></pre>

<p>
<strong>مشخص کردن اتصال پایگاه داده (database connection) سفارشی</strong>
</p>
<p>
گاهی اوقات ممکن است نیاز باشد که یک connection سفارشی برای کوئری‌های پایگاه داده که توسط Validator ایجاد شده‌اند، تنظیم کنید. همانطور که در مثال بالا مشاهده کردید، تنظیم unique:users به عنوان یک قانون اعتبارسنجی از کانکشن پیش‌فرض پایگاه داده برای ایجاد کوئری از پایگاه داده استفاده می‌کند. به جای این کار می‌توان، کانکشن سفارشی و نام جدول را با استفاده از علامت «نقطه» مشخص کرد:
</p>

<pre><code class="language-php  line-numbers">'email' => 'unique:connection.users,email_address'
</code></pre>

<p>
<strong>وادار نمودن قانون Unique برای نادیده گرفتن یک ID مشخص</strong>
</p>
<p>
 گاهی اوقات ممکن است بخواهید، یک ID مشخص را در طول بررسی قانون اعتبارسنجی unique نادیده بگیرید. برای مثال، یک صفحه «ویرایش مشخصات کاربر» را که شامل نام کاربر، آدرس ایمیل و مکان است را در نظر بگیرید. می‌خواهید مشخص کنید که فیلد آدرس ایمیل یکتا است. با این حال، اگر کاربر تنها فیلد نام را تغییر دهد و فیلد آدرس ایمیل تغییری نکند؛ در این صورت، به دلیل اینکه کاربر از قبل صاحب این آدرس ایمیل بوده است، ممکن است نخواهید یک خطای اعتبارسنجی صادر کنید.
</p>
<p>
برای نادیده گرفتن شناسه کاربر توسط این قانون اعتبارسنجی، می‌توانیم از کلاس Rule استفاده کنیم تا بتوانیم به سادگی این قانون را بسازیم. در این مثال، همچنین به جای مشخص کردن قوانین اعتبارسنجی با کاراکتر | می‌توانیم از یک آرایه برای ایجاد محدودیت در قوانین اعتبارسنجی استفاده ‌کنیم:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Validation\Rule;

Validator::make($data, [
    'email' => [
        'required',
        Rule::unique('users')->ignore($user->id),
    ],
]);
</code></pre>

<p>
اگر جدول از یک نام ستون به غیر از id برای کلید اصلی استفاده می‌کند، می‌توانید نام ستون را در هنگام فراخوانی متد ignore مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">'email' => Rule::unique('users')->ignore($user->id, 'user_id')
</code></pre>

<p>
<strong>اضافه کردن بندهای اضافی با where</strong>
</p>
<p>
می‌توان با استفاده از متد where ، محدودیت‌های پرس و جوی اضافی را برای سفارشی سازی یک query ایجاد کرد. برای مثال، اجازه دهید یک محدودیت اضافه کنیم که account_id با مقدار 1 را مشخص می‌کند:
</p>

<pre><code class="language-php  line-numbers">'email' => Rule::unique('users')->where(function ($query) {
    return $query->where('account_id', 1);
})
</code></pre>

<p>
<a name="rule-url"></a>
</p>

<br>
<h3>url</h3>
<p>
فیلدی که اعتبارسنجی می‌شود، باید یک URL معتبر باشد.
</p>
<p>
<a name="conditionally-adding-rules"></a>
</p>

<br>
<h3><a href="#conditionally-adding-rules">افزودن قوانین اعتبارسنجی به صورت شرطی</a></h3>

<br>
<h3>اعتبارسنجی در صورت وجود</h3>
<p>
در برخی موارد ممکن است بخواهید، تنها اگر فیلد در آرایه ورودی موجود باشد، بررسی‌های اعتبارسنجی را بر روی آن فیلد اجرا کنید. برای انجام سریع این کار، می‌توانید قانون sometimes را به لیست قوانین خود اضافه کنید:
</p>

<pre><code class="language-php  line-numbers">$v = Validator::make($data, [
    'email' => 'sometimes|required|email',
]);
</code></pre>

<p>
در مثال بالا، فیلد email تنها در صورتی که در آرایه $data موجود باشد، اعتبارسنجی می‌شود.
</p>
<blockquote class="has-icon tip">
اگر بخواهید فیلدی را که همیشه باید وجود داشته باشد، همچنین ممکن است خالی نیز باشد را اعتبارسنجی کنید، این یادداشت در مورد فیلدهای اختیاری را بررسی کنید.
</blockquote>

<br>
<h3>اعتبارسنجی شرطی پیچیده</h3>
<p>
گاهی اوقات ممکن است بخواهید، منطق شرطی پیچیده‌تری را به قوانین اعتبارسنجی اضافه کنید. برای مثال، ممکن است به یک فیلد مشخص فقط در صورتی که فیلد دیگری دارای مقدار بیشتر از 100 باشد، نیاز داشته باشید. یا ممکن است تنها زمانی که فیلد دیگری موجود باشد، به دو فیلد با یک مقدار معین نیاز داشته باشید. افزودن این قوانین اعتبارسنجی در لاراول کار سختی نیست. ابتدا یک نمونه Validator با قوانین استاتیک که هرگز تغییر نمی‌کند، ایجاد کنید:
</p>

<pre><code class="language-php  line-numbers">
$v = Validator::make($data, [
    'email' => 'required|email',
    'games' => 'required|numeric',
]);
</code></pre>

<p>
برای مثال فرض می‌کنیم، برنامه ما برای جمع آوری مجموعه بازی ساخته شده است. اگر یک جمع آوری کننده بازی در برنامه ثبت نام کرده و بیش از 100 بازی داشته باشد، از او می‌خواهیم توضیح دهد که چرا تعداد بازی‌های زیادی دارد. برای مثال، شاید فروشگاه مجازی بازی اجرا می‌کند یا شاید فقط از جمع آوری بازی لذت می‌برد. برای اضافه کردن این موارد به صورت شرطی، می‌توان از متد sometimes در نمونه Validator استفاده کرد.
</p>

<pre><code class="language-php  line-numbers">$v->sometimes('reason', 'required|max:500', function ($input) {
    return $input->games >= 100;
});
</code></pre>

<p>
آرگومان اولی که به متد sometimes منتقل می‌شود، نام فیلدی است که می‌خواهیم به صورت شرطی اعتبارسنجی کنیم. آرگومان دوم قوانین اعتبارسنجی است که قصد اضافه کردن آن‌ها را داریم. در صورتی که Closure به عنوان آرگومان سوم منتقل شده، مقدار true را برگرداند، این قوانین اضافه خواهند شد. این متد این امکان را می‌دهد که به سادگی بتوانیم اعتبارسنجی شرطی پیچیده را اعمال کنیم. حتی می‌توانید اعتبار‌سنجی شرطی را برای چند فیلد به صورت همزمان اضافه کنید:
</p>

<pre><code class="language-php  line-numbers">$v->sometimes(['reason', 'cost'], 'required', function ($input) {
    return $input->games >= 100;
});
</code></pre>

<blockquote class="has-icon tip">
پارامتر $input ارسال شده به Closure نمونه‌ای از Illuminate\Support\Fluent است و می‌توان از آن برای دسترسی به ورودی و فایل‌ها نیز استفاده کرد.
</blockquote>
<p>
<a name="validating-arrays"></a>
</p>

<br>
<h3><a href="#validating-arrays">اعتبارسنجی آرایه‌ها</a></h3>
<p>
اعتبارسنجی آرایه‌ها براساس فیلدهای ورودی نباید کار سختی باشد. می‌توان از علامت «نقطه» برای اعتبارسنجی صفات درون یک آرایه استفاده کرد. برای مثال، اگر درخواست HTTP ورودی دارای فیلد photos[profile] باشد، می‌توان آن را به صورت زیر اعتبارسنجی کرد:
</p>

<pre><code class="language-php  line-numbers">$validator = Validator::make($request->all(), [
    'photos.profile' => 'required|image',
]);
</code></pre>

<p>
همچنین می‌توان هر عنصری در درون آرایه را اعتبارسنجی کرد. برای مثال، برای اعتبارسنجی اینکه هر ایمیل در یک فیلد ورودی آرایه، منحصر به فرد است، می‌توان به صورت زیر عمل کرد:
</p>

<pre><code class="language-php  line-numbers">$validator = Validator::make($request->all(), [
    'person.*.email' => 'email|unique:users',
    'person.*.first_name' => 'required_with:person.*.last_name',
]);
</code></pre>

<p>
به همین ترتیب، هنگام مشخص کردن پیام‌های اعتبار‌سنجی خود در فایل‌های language، می‌توانید از کاراکتر * استفاده کنید، که در این صورت به راحتی می‌توان از یک پیام اعتبارسنجی واحد را برای فیلدهای آرایه استفاده کرد.
</p>

<pre><code class="language-php  line-numbers">'custom' => [
    'person.*.email' => [
        'unique' => 'Each person must have a unique e-mail address',
    ]
],
</code></pre>

<p>
<a name="custom-validation-rules"></a>
</p>

<br>
<h3><a href="#custom-validation-rules">ایجاد قوانین اعتبارسنجی سفارشی</a></h3>
<p>
<a name="using-rule-objects"></a>
</p>

<br>
<h3>استفاده از Rule Objects</h3>
<p>
لاراول انواع مختلفی از قوانین اعتبارسنجی را ارائه می‌دهد؛ با این حال ممکن است بخواهید برخی از قوانین را به صورت سفارشی ایجاد کنید. یک متد ثبت قوانین اعتبارسنجی سفارشی از اشیاء rule استفاده می‌کند. برای ایجاد یک شئ جدید rule، می‌توانید از دستور آرتیسان make:rule استفاده کنید. بیایید از این دستور برای تولید یک قانون یا rule استفاده کنیم، این قانون باید مشخص کند که یک رشته دارای حروف بزرگ است. لاراول rule جدید را در دایرکتوری app/Rules قرار می‌دهد:
</p>

<pre><code class="language-php  line-numbers">php artisan make:rule Uppercase
</code></pre>

<p>
هنگامی که قانون جدید ایجاد شد، می‌توانیم رفتار آن را نیز تعریف کنیم. یک شئ rule دارای دو متد است: passes و message . متد passes مقدار صفت و نام را دریافت می‌کند و بسته به اینکه آیا مقدار صفت معتبر است یا خیر، باید مقدار true یا false را بازگرداند. متد message پیام خطای اعتبارسنجی را که باید در هنگام ناموفق بودن اعتبارسنجی صادر شود، بازمی‌گرداند.
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Rules;

use Illuminate\Contracts\Validation\Rule;

class Uppercase implements Rule
{
    /**
     * Determine if the validation rule passes.
     *
     * @param  string  $attribute
     * @param  mixed  $value
     * @return bool
     */
    public function passes($attribute, $value)
    {
        return strtoupper($value) === $value;
    }

    /**
     * Get the validation error message.
     *
     * @return string
     */
    public function message()
    {
        return 'The :attribute must be uppercase.';
    }
}
</code></pre>

<p>
اگر می‌خواهید یک پیام خطا را از فایل‌های translation خود بازگردانید، می‌توانید تابع کمکی trans را از متد message فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the validation error message.
 *
 * @return string
 */
public function message()
{
    return trans('validation.uppercase');
}
</code></pre>

<p>
هنگامی که این قانون تعریف شد، می‌توانید با انتقال یک نمونه از شئ rule با سایر قوانین اعتبارسنجی خود، آن‌ها را به validator پیوست کنید.
</p>

<pre><code class="language-php  line-numbers">use App\Rules\Uppercase;

$request->validate([
    'name' => ['required', new Uppercase],
]);
</code></pre>

<p>
<a name="using-extensions"></a>
</p>

<br>
<h3>استفاده از extension برای ثبت قوانین اعتبار‌سنجی سفارشی</h3>
<p>
روش دیگر برای ثبت قوانین اعتبار‌سنجی سفارشی، استفاده از متد extend در facade مربوط به Validator است. می‌توان از این روش در یک service provider استفاده کرد تا یک قانون اعتبارسنجی سفارشی را ثبت کرد:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App\Providers;

use Illuminate\Support\ServiceProvider;
use Illuminate\Support\Facades\Validator;

class AppServiceProvider extends ServiceProvider
{
    /**
     * Bootstrap any application services.
     *
     * @return void
     */
    public function boot()
    {
        Validator::extend('foo', function ($attribute, $value, $parameters, $validator) {
            return $value == 'foo';
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
<p>
validator Closure سفارشی چهار آرگومان دریافت می‌کند:
</p>

<p>
نام $attribute  که اعتبارسنجی می‌شود، $value مربوط به صفت، آرایه‌ای از $parameters ارسال شده به قانون اعتبارسنجی و یک نمونه Validator .
</p>
<p>
همچنین، می‌توان به جای انتقال یک Closure، یک کلاس و متد را به متد extend انتقال داد:
</p>

<pre><code class="language-php  line-numbers">Validator::extend('foo', 'FooValidator@validate');
</code></pre>


<br>
<h3>تعریف پیام خطا برای قوانین اعتبارسنجی سفارشی</h3>
<p>
همچنین، باید یک پیام خطا را برای قانون سفارشی خود تعریف کنید. این کار را می‌توان با استفاده از یک آرایه inline پیام سفارشی یا با اضافه کردن یک ورودی در فایل اعتبارسنجی language انجام داد. این پیام باید در سطح اول آرایه قرار گیرد، نباید این پیام‌ها را در آرایه custom که مختص پیام‌های خطای attribute است، قرار داد:
</p>

<pre><code class="language-php  line-numbers">"foo" => "Your input was invalid!",

"accepted" => "The :attribute must be accepted.",

// The rest of the validation error messages...
</code></pre>

<p>
هنگام ایجاد یک قانون اعتبارسنجی سفارشی، ممکن است نیاز به جایگزینی یک place-holder سفارشی برای پیام‌های خطا داشته باشید. می‌توانید با ایجاد یک Validator سفارشی همانطور که در مثال بالا توضیح داده شد، یک فراخوانی به متد replacer در facade مربوط به Validato r انجام دهید. می‌توان این کار را در متد boot  یک service provider انجام داد:
</p>

<pre><code class="language-php  line-numbers">/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Validator::extend(...);

    Validator::replacer('foo', function ($message, $attribute, $rule, $parameters) {
        return str_replace(...);
    });
}
</code></pre>


<br>
<h3>Implicit Extensions</h3>
<p>
در حالت پیش‌فرض، در هنگام اعتبارسنجی یک attribute که موجود نیست یا مقدار آن خالی است، همانطور که در قانون required تعریف شده است، قوانین اعتبارسنجی نرمال، مانند extension سفارشی، اجرا نمی‌شود. برای مثال، قانون unique در برابر یک مقدار null اجرا نمی‌شود:
</p>

<pre><code class="language-php  line-numbers">$rules = ['name' => 'unique'];

$input = ['name' => null];

Validator::make($input, $rules)->passes(); // true
</code></pre>

<p>
برای اجرای یک قانون، حتی زمانی که یک attribute خالی باشد، قانون یا rule باید اشاره کند که این attribute موردنیاز است. برای ایجاد این extension implicit می‌توانید از متد Validator::extendImplicit() استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">Validator::extendImplicit('foo', function ($attribute, $value, $parameters, $validator) {
    return $value == 'foo';
});
</code></pre>

<blockquote class="has-icon note">
extension implicit تنها مشخص می‌کند که attribute موردنیاز است. اینکه آیا یک attribute ناموجود یا خالی نامعتبر است یا خیر، به خودتان بستگی دارد.
</blockquote>