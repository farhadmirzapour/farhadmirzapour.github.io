---
layout: documentation-laravel
title:   آشنایی با eloquent mutators
cattitle: اصول آموزش Laravel
date:   2018-01-19 19:57:42 +0330
jdate: جمعه 29 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/Eloquent-ORM
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/eloquent-mutators <br> www.tahlildadeh.com/ArticleDetails/آموزش-Mutator-ها-و-Accessor-ها-در-Eloquent
---
<p>
Accessor ها و Mutator ها به شما این امکان را می دهند تا attribute های Eloquent را به هنگام بازیابی از مدل یا مقدار دهی آن، فرمت دهی کنید. به عنوان مثال می توان به سناریویی اشاره کرد که در آن می خواهیم با استفاده از encrypter لارول مقداری را در حالی که در پایگاه داده ذخیره شده، رمزنگاری کنیم و سپس آن را در زمان دسترسی (به آن) در مدل Eloquent، بار دیگر رمزگشایی نماییم.
</p>

<p>
علاوه بر ارائه ی accessor ها و mutator های اختصاصی، Eloquent قادر است فیلدهای حاوی تاریخ و نوع داده ای date را به نمونه هایی از نوعCarbon کانورت کرده و حتی فیلدهای متنی را به فرمت JSON تبدیل کند.
</p>

<p>
<a name="defining-an-accessor"></a>
</p>

<br>
<h3>نحوه ی تعریف Accessor</h3>
<p>
برای تعریف Accessor، یک متد getFooAttribute در مدل خود ایجاد نمایید. واژه ی Foo در نام این متد در واقع نسخه ی CamelCase (بر اساس سیستم نوشتاری camelcase) اسم همان ستونی است که می خواهید به آن دسترسی داشته باشید. در این مثال، یک accessor برای اتریبیوت first_name تعریف می کنیم. به هنگام بازیابی و دسترسی به مقدار attribute نام برده، Eloquent به صورت خودکار accessor تعریف شده را صدا می زند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Get the user's first name.
     *
     * @param  string  $value
     * @return string
     */
    public function getFirstNameAttribute($value)
    {
        return ucfirst($value);
    }
}
</code></pre>

<p>
همان طور که می بینید، مقدار اولیه ی ستون به accessor ارسال می شود و به شما این امکان را می دهد تا به آسانی آن را ویرایش و بازیابی کنید. حال برای دسترسی به مقدار mutator، کافی است به اتریبیوت first_name دسترسی پیدا کنید:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$firstName = $user->first_name;
</code></pre>

<p>
همچنین می توانید با استفاده از accessors  جهت ایجاد مقادیر جدید استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the user's full name.
 *
 * @return string
 */
public function getFullNameAttribute()
{
    return "{$this->first_name} {$this->last_name}";
}
</code></pre>

<p>
<a name="defining-a-mutator"></a>
</p>

<br>
<h3>نحوه ی تعریف یک Mutator</h3>
<p>
به منظور تعریف mutator، یک متد به نام setFooAttribute در مدل ایجاد کنید. واژه ی Foo در اسم این متد در واقع نسخه ی camelcase نام همان ستونی است که می خواهید به مقدارش دسترسی داشته باشید. بنابراین ابتدا یک mutator برای اتریبیوت first_name تعریف می کنیم. به هنگام مقداردهی attribute ذکر شده در مدل مورد نظر، این mutator به صورت خودکار فراخوانده می شود:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Set the user's first name.
     *
     * @param  string  $value
     * @return void
     */
    public function setFirstNameAttribute($value)
    {
        $this->attributes['first_name'] = strtolower($value);
    }
}
</code></pre>

<p>
mutator مقداری که در attribute قرار داده (set) می شود را دریافت کرده و به شما اجازه می دهد تا مقدار آن را ویرایش کنید، سپس مقدار ویرایش شده را در متغیر (property) درون ساخته ی مدل Eloquent به نام $attributes ذخیره نمایید/قرار دهید. بنابراین، اگر بخواهیم مقدارfirst_name را با Sally تنظیم نماییم، می بایست به صورت زیر اقدام کنیم:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->first_name = 'Sally';

$user->save();
</code></pre>

<p>
در این مثال، تابع setFirstNameAttribute با مقدار Sally فراخوانده می شود. mutator سپس تابع strtolower را بر روی اسم اجرا کرده و مقدارش را در آرایه ی درون ساخته ی attributes$ قرار می دهد.
</p>
<p>
<a name="date-mutators"></a>
</p>

<br>
<h3><a href="#date-mutators">تاریخ در  Mutator</a></h3>
<p>
به صورت پیش فرض، Eloquent ستون های created_at و updated_at را به نمونه های (اشیای) از نوع Carbon تبدیل می کند که تعداد زیادی متد مفید در اختیار ما قرار داده و کلاس درون ساخته ی DateTime زبان PHP را ارث بری می کند.
</p>

<p>
می توانید مشخص کنید کدام فیلدها بایستی به صورت خودکار تبدیل (mutate) شوند یا این تبدیل را کاملا غیر فعال نمایید. برای این منظور کافی است property ای که dates$ نام دارد را در مدل خود بازنویسی کنید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The attributes that should be mutated to dates.
     *
     * @var array
     */
    protected $dates = [
        'created_at',
        'updated_at',
        'deleted_at'
    ];
}
</code></pre>

<p>
ستونی که از نوع date شناخته می شود را می توان با تایم استمپ های UNIX، تاریخ با فرمت رشته (Y-m-d)، تاریخ-زمان با فرمت رشته و البته یک نمونه از DateTime / Carbon مقداردهی نمود. مقدار date متعاقبا به صورت خودکار با فرمت مناسب در پایگاه داده ذخیره می شود:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->deleted_at = now();

$user->save();
</code></pre>

<p>
همان طور که در بالا ذکر شد، زمانی که attribute های لیست شده در متغیر(property) $dates را واکشی می کنید، این attribute ها به صورت خودکار به نمونه هایی از نوع Carbon تبدیل می شوند. این امر به شما اجازه می دهد تا متدهای Carbon را بر روی attribute های مورد نظر فراخوانی کنید:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

return $user->deleted_at->getTimestamp();
</code></pre>


<p>
به صورت پیش فرض، timestamp ها بدین صورت فرمت دهی می شوند: 'Y-m-d H:i:s'. در صورت تمایل می توانید این فرمت را اختصاصی تنظیم کنید. برای این منظور کافی است dateFormat property$ را در مدل خود مقداردهی نمایید. این property تعیین می کند attribute های حاوی مقدار تاریخ چگونه در پایگاه داده ذخیره شوند و همچنین فرمت آن ها را در زمان serialize (کد) شدن مدل به آرایه یا JSON مشخص می کند:
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
<a name="attribute-casting"></a>
</p>

<br>
<h3><a href="#attribute-casting">تبدیل Attribute</a></h3>
<p>
متغیر  casts$ (در مدل) روشی آسان برای تبدیل attribute ها به نوع داده های معمول فراهم می کند. property مزبور باید یک متغیر از نوع آرایه باشد که در آن کلید در واقع اسم attribute ای است که باید تبدیل شود و مقدار آن نوع داده ای است که می خواهید ستون (فیلد) به آن cast شود. نوع هایی که برای تبدیل پشتیبانی می شوند عبارتند از: integer، real، float، double، string، boolean، object، array،collection، date، datetime و timestamp.
</p>
<p>
در مثال زیر اتریبیوت is_admin که به صورت یک عدد صحیح 0 یا 1 در پایگاه داده ذخیره شده را به نوع داده ای Boolean تبدیل می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'is_admin' => 'boolean',
    ];
}
</code></pre>

<p>
اکنون دیگر مقدار attribute نام برده همیشه به هنگام دسترسی به آن، به یک مقدار از نوع بولی تبدیل می شود، هرچند مقدار اصلی به صورت یک عدد صحیح در پایگاه داده ذخیره شده:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

if ($user->is_admin) {
    //
}
</code></pre>

<p>
<a name="array-and-json-casting"></a>
</p>

<br>
<h3>تبدیل به نوع Array &amp; JSON </h3>
<p>
تبدیل نوع آرایه به خصوص برای کار با ستون هایی که به صورت JSON سریاله شده، ذخیره شده اند مفید واقع می شود. به عنوان مثال، اگر پایگاه داده ی شما دارای فیلدی از نوع TEXT باشد که با JSON سریاله شده مقداردهی شده است، در آن صورت افزودن نوع array به آن attributeباعث می شود، در زمان دسترسی به آن ATTRIBUTE در مدل Eloquent، نوع attribute خودکار به یک آرایه ی معمولی PHP تبدیل شود:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The attributes that should be cast to native types.
     *
     * @var array
     */
    protected $casts = [
        'options' => 'array',
    ];
}
</code></pre>

<p>
پس از تعریف عملیات تبدیل، می توانید به options attribute دسترسی پیدا کنید. خواهید دید که نوعش به صورت خودکار از JSON سریاله شده به یک آرایه ی متعارف PHP تبدیل می شود. زمانی که attribute نام برده را مقداردهی می کنید، آرایه ی ارائه شده مجددا برای ذخیره سازی به صورت خودکار به فرمت JSON (serialized) برگردانده می شود:
</p>

<pre><code class="language-php  line-numbers">
$user = App\User::find(1);

$options = $user->options;

$options['key'] = 'value';

$user->options = $options;

$user->save();
</code></pre>