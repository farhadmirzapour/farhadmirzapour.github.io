---
layout: post
title:  معرفی Laravel Collective
date:   2017-12-13 21:06:42 +0330
jdate: چهارشنبه 22 آذر 1396
categories: articles
# permalink: /:categories/:title.html
thumbnail: laravelcollective.jpg
refrence: https://laravelcollective.com/docs/5.4/html
---
<p>
Laravel Collective جهت ایجاد فرم های ایمن و سریع در فریم ورک laravel کاربرد دارد به عبارتی پس از آنکه ما این امکان جانبی را نصب نمودیم دیگر نیاز به کدنویسی های پیچیده html برای فرم های خود نداریم و به سادگی می توانیم از طریق داکیومنتی که خود لاراول کالکتیو در اختیار ما قرار می دهد هر عملیاتی را انجام دهیم.
</p>

<h3>نصب</h3>
<p>
جهت نصب این پکیج با استفاده از composer ، فایل composer.json پروژه خود را همانند زیر ویرایش نمائید :
</p>

<pre><code class="language-json  line-numbers">composer require "laravelcollective/html":"^5.4.0"
</code></pre>

<p>
سپس، provider جدید خود را به آرایه providers واقع در config / app.php اضافه کنید:
</p>

<pre><code class="language-php  line-numbers">'providers' => [
    // ...
    Collective\Html\HtmlServiceProvider::class,
    // ...
  ],
</code></pre>

<p>
نهایتا دو نام مستعار کلاس  را به آرایه aliases واقع در config / app.php اضافه نمایید :
</p>

<pre><code class="language-php  line-numbers">  'aliases' => [
    // ...
      'Form' => Collective\Html\FormFacade::class,
      'Html' => Collective\Html\HtmlFacade::class,
    // ...
  ],

</code></pre>

<br>
<h3>ایجاد Form</h3>

<pre><code class="language-php  line-numbers">{% raw %}{{{% endraw %} Form::open(['url' => 'foo/bar']) {% raw %}}}{% endraw %}
    //
{% raw %}{{ Form::close() }}{% endraw %}
</code></pre>


<p>
به طور پیش فرض، از روش POST جهت ارسال اطلاعات در فرم استفاده می شود، با این حال، شما می توانید روش دیگری را مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">echo Form::open(['url' => 'foo/bar', 'method' => 'put'])
</code></pre>

<blockquote>
توجه داشته باشید: از آنجا که فرمهای HTML فقط از POST و GET پشتیبانی می کنند، جهت استفاده از روشهای  PUT و DELETE ، بطور اتوماتیک  یک فیلد پنهان method_ به فرم شما اضافه می گردد.
</blockquote>

<p>جهت ارسال اطلاعات فرم به route و یا action  کنترلر همانند زیر عمل می کنیم :</p>

<pre><code class="language-php  line-numbers">echo Form::open(['route' => 'route.name'])
echo Form::open(['action' => 'Controller@method'])
</code></pre>



<p>همچنین جهت ارسال پارامتر به route و یا action کنترلر  همانند زیر عمل می کنیم:</p>

<pre><code class="language-php  line-numbers">echo Form::open(['route' => ['route.name', $user->id]])

echo Form::open(['action' => ['Controller@method', $user->id]])
</code></pre>

<p>
همچنین جهت آپلود و ارسال فایل مانند زیر عمل کنید :
</p>


<pre><code class="language-php  line-numbers">echo Form::open(['url' => 'foo/bar', 'files' => true])
</code></pre>

<br>
<h3>
استفاده از توکن CSRF در فرم
</h3>
<p>
Laravel  روش آسانی برای حفاظت از اپلیکیشن شما در برابر حملات cross-site request forgeries  فراهم کرده است. اگر شما از روش Form::open با استفاده از POST، PUT یا DELETE استفاده کنید، نشانه CSRF به صورت خودکار به عنوان فیلد پنهان به فرم شما اضافه می شود. همچنین، اگر شما مایل به ایجاد HTML برای فیلد پنهان CSRF هستید، می توانید از روش  token استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">echo Form::token();
</code></pre>

<p>افزودن CSRF به Route :</p>
<pre><code class="language-php  line-numbers">Route::post('profile',
    [
        'before' => 'csrf',
        function()
        {
            //
        }
    ]
);
</code></pre>

<br>
<h3>ایجاد فرم با استفاده از Model</h3>
<p>
ممکن است بخواهید فرم را بر اساس محتویات یک model پر کنید. برای انجام این کار، از روش Form::model استفاده کنید:
</p>

<pre><code class="language-php  line-numbers">echo Form::model($user, ['route' => ['user.update', $user->id]])
</code></pre>

<p>
زمانیکه که شما یک عنصر در فرم همانند یک text input را تولید می کنید، ، مقدار این text input به صورت خودکار برابر با مقدار نام آیتم موجود در Model تنظیم می گردد. با این حال موارد دیگری نیز وجود دارد. اگر یک آیتم در Session flash data مطابق با نام ورودی وجود داشته باشد، بر روی مقدار مدل اولویت خواهد داشت. بنابراین ترتیب اولویت ها بدین صورت است :
</p>

<ol style="direction:ltr">
<li >Session Flash Data (Old Input)</li>
<li>Explicitly Passed Value</li>
<li>Model Attribute Data</li>
</ol>
<p>
این امر به شما اجازه می دهد تا سریعا فرم هایی را ایجاد کنید که نه تنها به مقادیر model پیوند دارند، بلکه اگر خطای اعتبارسنجی روی سرور وجود داشته باشد، به راحتی مقادیر فرم دوباره پر شود!
</p>

<blockquote>
توجه: هنگام استفاده از Form :: model، باید فرم خود را با Form::close ببندید!
</blockquote>

<br>
<h3>بررسی Form Model Accessors</h3>
<p>
Eloquent Accessor  در Laravel  اجازه می دهد ویژگی model را تا قبل از بازگشت دستکاری کنید. برای مثال، برای تعیین فرمت های تاریخ جهانی، این می تواند بسیار مفید باشد. با این حال، فرمت تاریخ مورد استفاده برای نمایش ممکن است با قالب تاریخی مورد استفاده برای عناصر فرم سازگار نباشد. شما می توانید این را با ایجاد دو دسترسی جداگانه حل کنید: یک دسترسی استاندارد و یا یک فرم دسترسی.
</p>

<p>
برای تعریف دسترسی به فرم، یک متد formFooAttribute را در model خود ایجاد کنید که در آن Foo نام ستونی است که میخواهید به آن دسترسی پیدا کنید.
</p>
<p>
در این مثال، یک accessor برای ویژگی date_of_birth تعریف می کنیم. زمانیکه   ()Form::model جهت پر کردن  فرم استفاده شود، Accessor به صورت خودکار توسط ایجاد کننده فرم فراخوانی می  گردد.
</p>

<p>
شما باید با استفاده از روش trait کلاس FormAccessible را به model خود include  نمایید.
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Carbon\Carbon;
use Illuminate\Database\Eloquent\Model;
use Collective\Html\Eloquent\FormAccessible;

class User extends Model
{
    use FormAccessible;     

    /**
     * Get the user's first name.
     *
     * @param  string  $value
     * @return string
     */
    public function getDateOfBirthAttribute($value)
    {
        return Carbon::parse($value)->format('m/d/Y');
    }

    /**
     * Get the user's first name for forms.
     *
     * @param  string  $value
     * @return string
     */
    public function formDateOfBirthAttribute($value)
    {
        return Carbon::parse($value)->format('Y-m-d');
    }
}
</code></pre>



<br>
<h3>ایجاد Label در فرم</h3>

<pre><code class="language-php  line-numbers">echo Form::label('email', 'E-Mail Address');
</code></pre>

<p>
تعیین ویژگی های HTML  در Label :
</p>

<pre><code class="language-php  line-numbers">echo Form::label('email', 'E-Mail Address', ['class' => 'awesome']);
</code></pre>

<blockquote>
نکته: پس از ایجاد یک Label، هر عنصر فرم که با نامی مطابق با نام Label ایجاد می کند، به طور خودکار یک شناسه مطابق با نام Label نیز دریافت می کند.
</blockquote>


<h3>ایجاد Text Input</h3>

<pre><code class="language-php  line-numbers">echo Form::text('username');
</code></pre>

<p>
تعیین  یک مقدار پیش فرض در Text Input :
</p>

<pre><code class="language-php  line-numbers">echo Form::text('email', 'example@gmail.com');
</code></pre>

<br>
<h3>ایجاد Password Input</h3>

<pre><code class="language-php  line-numbers">echo Form::password('password', ['class' => 'awesome']);
</code></pre>

<br>
<h3>ایجاد Input  های Email  و File</h3>

<pre><code class="language-php  line-numbers">echo Form::email('name', $value = null, $attributes = []);
echo Form::file('name', $attributes = []);
</code></pre>

<br>
<h3>ایجاد Checkbox و Radio Input</h3>

<pre><code class="language-php  line-numbers">echo Form::checkbox('name', 'value');

echo Form::radio('name', 'value');
</code></pre>

<p>
Checkbox Or Radio Input که ویژگی تیک خورده اند :
</p>

<pre><code class="language-php  line-numbers">echo Form::checkbox('name', 'value', true);

echo Form::radio('name', 'value', true);
</code></pre>

<br>
<h3>ایجاد Number Input</h3>

<pre><code class="language-php  line-numbers">echo Form::number('name', 'value');
</code></pre>

<br>
<h3>ایجاد Date Input</h3>

<pre><code class="language-php  line-numbers">echo Form::date('name', \Carbon\Carbon::now());
</code></pre>

<br>
<h3>ایجاد File Input</h3>

<pre><code class="language-php  line-numbers">echo Form::file('image');
</code></pre>

<blockquote>
نکته: خاصیت files  باید در هنگام ایجاد فرم برابر true باشد
</blockquote>

<br>
<h3>ایجاد Drop-Down List</h3>

<pre><code class="language-php  line-numbers">echo Form::select('size', ['L' => 'Large', 'S' => 'Small']);
</code></pre>

<p>ایجاد Drop-Down List با گزینه انتخاب شده پیش فرض :</p>

<pre><code class="language-php  line-numbers">echo Form::select('size', ['L' => 'Large', 'S' => 'Small'], 'S');
</code></pre>

<p>ایجاد Drop-Down List همراه با placeholder : </p>

<pre><code class="language-php  line-numbers">echo Form::select('size', ['L' => 'Large', 'S' => 'Small'], null, ['placeholder' => 'Pick a size...']);
</code></pre>


<p>در زیر نحوه ایجاد Grouped List را مشاهده می کنید :</p>

<pre><code class="language-php  line-numbers">echo Form::select('animal',[
    'Cats' => ['leopard' => 'Leopard'],
    'Dogs' => ['spaniel' => 'Spaniel'],
]);</code></pre>

<p>ایجاد Grouped List با یک رنج عددی :</p>

<pre><code class="language-php  line-numbers">echo Form::selectRange('number', 10, 20);
</code></pre>

<br>
<h3>ایجاد List  با یک Month Names :</h3>

<pre><code class="language-php  line-numbers">echo Form::selectMonth('month');
</code></pre>

<br>
<h3>ایجاد Submit Button</h3>

<pre><code class="language-php  line-numbers">echo Form::submit('Click Me!');
</code></pre>

<br>
<h3>ایجاد Form Macro</h3>
<p>
به Form class helpers شخصی، اصطلاحا macro  گفته می شود.
</p>
<p>جهت ایجاد Macro  همانند زیر عمل می کنیم :</p>

```html
Form::macro('myField', function()
{
    return '<input type="awesome">';
});
```

<p>هم اکنون با استفاده از نام Macro  می توانیم آنرا فراخوانی کنیم :</p>

<pre><code class="language-php  line-numbers">echo Form::myField();
</code></pre>

<br>
<h3>ایجاد Custom Component</h3>
<p>
component  شبیه macro  های سفارشی می باشند با این تفاوت که در ماکروها خروجی مستقیما تگ های HTML  است ولی در کامپوننت محتوای موجود در قالب Laravel Blade Templates  می باشد
</p>

<p>
جهت ثبت یک کامپوننت از روش زیر استفاده می کنیم
</p>
<pre><code class="language-php  line-numbers">Form::component('bsText', 'components.form.text', ['name', 'value', 'attributes']);
</code></pre>

<p>
برای مثال فرض کنید یک view  مانند زیر داشته باشیم :
</p>

```html
// resources/views/components/form/text.blade.php
<div class="form-group">
    {% raw %}{{{% endraw %} Form::label($name, null, ['class' => 'control-label']) {% raw %}}}{% endraw %}
    {% raw %}{{{% endraw %} Form::text($name, $value, array_merge(['class' => 'form-control'], $attributes)) {% raw %}}}{% endraw %}
</div>
```

<p>آنگاه جهت ثبت آن بصورت component  خواهیم داشت :</p>

<pre><code class="language-php  line-numbers">Form::component('bsText', 'components.form.text', ['name', 'value' => null, 'attributes' => []]);
</code></pre>

<p>
و برای فراخوانی components  ثبت شده :
</p>

```html
{% raw %}{{{% endraw %} Form::bsText('first_name') {% raw %}}}{% endraw %}
```

<p>که خروجی آن HTML  موجود در view  مشخص شده می باشد :</p>

```html
<div class="form-group">
    <label for="first_name">First Name</label>
    <input type="text" name="first_name" value="" class="form-control">
</div>
```


<br>
<h3>ایجاد URLs</h3>
<p>ایجاد یک لینک به یک آدرس URL  مشخص :</p>

<pre><code class="language-php  line-numbers">echo link_to('foo/bar', $title = null, $attributes = [], $secure = null);
</code></pre>

<p>ایجاد یک لینک برای اتصل به یک asset :</p>

<pre><code class="language-php  line-numbers">echo link_to_asset('foo/bar.zip', $title = null, $attributes = [], $secure = null);
</code></pre>

<p>ایجاد لینک به یک route :</p>

<pre><code class="language-php  line-numbers">echo link_to_route('route.name', $title = null, $parameters = [], $attributes = []);
</code></pre>

<p>ایجاد لینک به یک controller action :</p>

<pre><code class="language-php  line-numbers">echo link_to_action('HomeController@getIndex', $title = null, $parameters = [], $attributes = []);
</code></pre>
