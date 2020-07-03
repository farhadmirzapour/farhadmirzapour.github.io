---
layout: documentation-laravel
title:   Blade Templates
cattitle: اصول آموزش Laravel
date:   2018-02-13 19:50:42 +0330
jdate: سه شنبه 24 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/Frontend
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/blade <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/288/blade-template-لاراول-5-5
---
<p>
Blade یک موتور قالب بندی ساده و کارآمد است که به همراه لاراول ارائه می‌شود. بر خلاف دیگر موتورهای محبوب قالب بندی PHP،  موتور قالب بندی Blade محدودیتی برای شما در استفاده از کدهای ساده PHP در view ایجاد نمی‌کند. در واقع، تمام ویوهای Blade به کد PHP ساده تبدیل شده و تا زمانی که تغییر نکرده‌اند در حافظه کش ذخیره می‌شوند؛ در این حالت می‌توان گفت که Blade هیچ سرباری به برنامه شما اضافه نمی‌کند. فایل‌های ویوی Blade از پسوند فایل .blade.php استفاده می‌کنند و معمولاً در دایرکتوری resources/views ذخیره می‌شوند.
</p>

<p>
<a name="template-inheritance"></a>
</p>

<br>
<h3><a href="#template-inheritance">ارث بری قالب view در لاراول</a></h3>
<p>
<a name="defining-a-layout"></a>
</p>

<br>
<h3>تعریف یک Layout</h3>
<p>
دو مزیت اصلی استفاده از Blade، ارث بری قالب و بخش‌ها است. برای شروع کار، یک مثال ساده را در نظر می‌گیریم؛ در ابتدا، یک صفحه master را در نظر می‌گیریم. از آنجایی که اکثر برنامه‌های کاربردی وب از یک layout کلی برای صفحات مختلف استفاده می‌کنند، برای راحتی کار می‌توان این layout را به عنوان یک ویو Blade به صورت زیر تعریف کرد:
</p>

```html
<!-- Stored in resources/views/layouts/app.blade.php -->

<html>
    <head>
        <title>App Name - @yield('title')</title>
    </head>
    <body>
        @section('sidebar')
            This is the master sidebar.
        @show

        <div class="container">
            @yield('content')
        </div>
    </body>
</html>
```

<p>
همانطور که مشاهده می‌کنید، این فایل شامل عبارت‌های HTML است. با این حال، به دستورات @section و @yield توجه داشته باشید. دستورالعمل @section ، همانطور که از نامش پیداست، قسمتی از محتوا را تعریف می‌کند، در حالی که دستورالعمل @yield جهت نمایش محتوای یک بخش خاص استفاده می‌شود.
</p>

<p>
پس از اینکه یک layout برای برنامه ایجاد کردیم، اجازه دهید یک صفحه فرزند که از layout ارث بری می‌کند، ایجاد کنیم.
</p>

<p>
<a name="extending-a-layout"></a>
</p>

<br>
<h3>ارث بری از یک Layout در لاراول</h3>
<p>
در زمان تعریف یک ویو فرزند، جهت تعیین اینکه ویو فرزند از کدام layout ارث بری کند، می‌توانیم از دستور @extends استفاده کنیم. ویوهایی که از layout Blade ارث بری می‌کنند، می‌توانند با استفاده از دستور @section محتوای خاصی را به قسمت‌های مختلف layout تزریق کنند. توجه داشته باشید، همان طور که در مثال بالا مشاهده کردید، می‌توان با استفاده از دستور @yield محتویات این قسمت‌ها را در layout نمایش داد:
</p>

```html
<!-- Stored in resources/views/child.blade.php -->

@extends('layouts.app')

@section('title', 'Page Title')

@section('sidebar')
    @parent

    <p>This is appended to the master sidebar.</p>
@endsection

@section('content')
    <p>This is my body content.</p>
@endsection
```

<p>
در این مثال، قسمت sidebar از دستور @parent برای افزودن محتوا (به جای بازنویسی) به قسمت sidebar در layout استفاده می‌کند. زمانی که ویو نمایش داده می‌شود، دستورالعمل @parent با محتوای layout کلی جایگزین می‌شود.
</p>
<blockquote class="has-icon tip">
برخلاف مثال قبلی، در این مثال sidebar به جای show@ با endsection@ به پایان می‌رسد. دستور endsection@ فقط یک بخش را تعریف می‌کند در حالی که دستور show@ یک بخش را تعریف کرده و بلافاصله اجرا می‌کند.
</blockquote>
<p>
ویوهای blade می‌توانند توسط تابع کمکی view عمومی به صورت زیر از مسیرها بازگردانده شوند:
</p>

<pre><code class="language-php  line-numbers">Route::get('blade', function () {
    return view('child');
});
</code></pre>

<p>
<a name="components-and-slots"></a>
</p>

<br>
<h3><a href="#components-and-slots">کامپوننت ‌ها و اسلات ‌ها در view لاراول</a></h3>
<p>
کامپوننت‌ها و اسلات‌ها روش‌ مناسبی برای تعریف بخش‌ها و layoutها ارائه می‌دهند؛ شاید برای برخی افراد درک ذهنی کامپوننت‌ها و اسلات‌ها ساده‌تر باشد. در ابتدا، اجازه دهید یک کامپوننت «alert» را در نظر بگیریم که قصد داریم توسط برنامه از آن استفاده مجدد کنیم:
</p>

```html
<!-- /resources/views/alert.blade.php -->

<div class="alert alert-danger">
    {% raw %}{{ $slot }}{% endraw %}
</div>
```

<p>
متغیر <code class=" language-php"><span class="token punctuation">{</span><span class="token punctuation">{</span> <span class="token variable">$slot</span> <span class="token punctuation">}</span><span class="token punctuation">}</span></code> دارای محتوایی است که می‌خواهید به کامپوننت تزریق کنید. می‌توان از دستور @component برای ساختن این کامپوننت استفاده کرد:
</p>

```html
@component('alert')
    <strong>Whoops!</strong> Something went wrong!
@endcomponent
```

<p>
گاهی اوقات، تعریف چند اسلات برای یک کامپوننت می‌تواند مفید باشد. اجازه دهید ماژول alert را تغییر دهیم تا امکان تزریق «title» را نیز داشته باشیم. اسلات‌های نامگذاری شده ممکن است با متغیری که با نام آن مطابقت دارد نمایش داده شوند:
</p>

```html
<!-- /resources/views/alert.blade.php -->

<div class="alert alert-danger">
    <div class="alert-title">{% raw %} {{ $title }} {% endraw %} </div>

{% raw %}    {{ $slot }} {% endraw %} 
</div>
```

<p>
اکنون می‌توانیم محتوا را به اسلات نامگذاری شده با استفاده از دستور @slot تزریق کنیم. هر محتوایی که در دستور @slot نیست، در درون متغیر $slot به کامپوننت منتقل می‌شود:
</p>

```html
@component('alert')
    @slot('title')
        Forbidden
    @endslot

    You are not allowed to access this resource!
@endcomponent
```

<br>
<h3>انتقال داده های اضافی به کامپوننت‌های view در لاراول</h3>
<p>
گاهی اوقات، ممکن است بخواهید داده‌های اضافی دیگری را به یک کامپوننت انتقال دهید. به همین دلیل، می‌توانید آرایه‌ای از داده‌ها را به عنوان آرگومان دوم به دستور @component منتقل کنید. تمام داده‌های آرایه به عنوان متغیر در قالب کامپوننت در دسترس خواهند بود:
</p>

```html
@component('alert', ['foo' => 'bar'])
    ...
@endcomponent
```

<br>
<h3>Aliasing Components</h3>
<p>
If your Blade components are stored in a sub-directory, you may wish to alias them for easier access. For example, imagine a Blade component that is stored at <code class=" language-php">resources<span class="token operator">/</span>views<span class="token operator">/</span>components<span class="token operator">/</span>alert<span class="token punctuation">.</span>blade<span class="token punctuation">.</span>php</code>. You may use the <code class=" language-php">component</code> method to alias the component from <code class=" language-php">components<span class="token punctuation">.</span>alert</code> to <code class=" language-php">alert</code>. Typically, this should be done in the <code class=" language-php">boot</code> method of your <code class=" language-php">AppServiceProvider</code>:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Support\Facades\Blade;

Blade::component('components.alert', 'alert');
</code></pre>

<p>
Once the component has been aliased, you may render it using its alias:
</p>

```html
@alert('alert', ['type' => 'danger'])
    You are not allowed to access this resource!
@endalert
```

<p>
Or, if the component has no additional slots, you may use the component's name as a Blade directive:
</p>

```html
@alert
    You are not allowed to access this resource!
@endalert
```

<p>
<a name="displaying-data"></a>
</p>

<br>
<h3><a href="#displaying-data">نمایش داده‌ها در view لاراول</a></h3>

<p>
برای نمایش داده‌های منتقل شده به ویوهای Blade می‌توان متغیر را در درون {% raw %}{{ }}{% endraw %} قرار داد. برای مثال، به مسیر زیر توجه کنید:
</p>

<pre><code class="language-php  line-numbers">Route::get('greeting', function () {
    return view('welcome', ['name' => 'Samantha']);
});
</code></pre>

<p>
می‌توان محتویات متغیر name را به صورت زیر نمایش داد:
</p>

```html
Hello,{% raw %} {{ $name }} {% endraw %} .
```

<p>
البته، کار شما تنها به نمایش محتویات متغیرهای منتقل شده به ویو محدود نمی‌شود؛ بلکه، می‌توانید نتیجه حاصل از هر تابع PHP را نیز نمایش دهید. می‌توانید هر کد PHP را که بخواهید درون یک عبارت Blade echo قرار دهید:
</p>

```html
The current UNIX timestamp is {% raw %} {{ time() }} {% endraw %} .
```
<blockquote class="has-icon tip">
عبارت‌های {% raw %}{{ }}{% endraw %} Blade، جهت جلوگیری از حملات XSS به صورت خودکار توسط تابع پی اچ پی htmlspecialchars ارسال می‌شوند.
</blockquote>
<br>
<h3>نمایش داده های unescaped در view</h3>
<p>
به صورت پیش‌فرض، عبارت‌های {% raw %}{{ }}{% endraw %} Blade، جهت جلوگیری از حملات XSS به صورت خودکار توسط تابع پی اچ پی htmlspecialchars ارسال می‌شوند. اگر نخواهید داده‌هایتان را escaped کنید، می‌توانید از سینتکس زیر استفاده کنید:
</p>

```html
Hello, {!! $name !!}.
```
<blockquote class="has-icon note">
در هنگام بازخوانی محتوا که توسط کاربران برنامه ارائه شده‌اند، بسیار دقت کنید. جهت جلوگیری از حملات XSS هنگام نمایش داده‌های ارائه شده توسط کاربران از {% raw %}{{ }}{% endraw %}  استفاده کنید.
</blockquote>
<br>
<h3>نمایش JSON</h3>
<p>
گاهی اوقات، می‌توانید یک آرایه را به ویو خود انتقال دهید تا بتوانید آن را به عنوان یک JSON ارائه دهید و به عنوان یک متغیر جاوا اسکریپت از آن استفاده کنید. برای مثال:
</p>

```html
<script>
    var app = <?php echo json_encode($array); ?>;
</script>
```

<p>
با این حال، به جای اینکه به صورت دستی json_encode را فراخوانی کنید، می‌توانید از دستور @json استفاده کنید:
</p>

```html
<script>
    var app = @json($array);
</script>
```

<br>
<h3>HTML Entity Encoding</h3>
ddd

<pre><code class="language-php  line-numbers">

namespace App\Providers;

use Illuminate\Support\Facades\Blade;
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
        Blade::withoutDoubleEncoding();
    }
}
</code></pre>

<p>
<a name="blade-and-javascript-frameworks"></a>
</p>

<br>
<h3>Blade و فریم ورک های جاوا اسکریپت در لاراول</h3>
<p>
از آنجایی که بسیاری از فریم ورک‌های جاوا اسکریپت نیز از {% raw %}{{ }}{% endraw %} برای نمایش یک عبارت در مرورگر استفاده می‌کنند، می‌توانید از نماد @  استفاده کنید تا به موتور رندر Blade اعلان کنید که عبارت داخل آن را تفسیر نکند. برای مثال:
</p>

```html
<h1>Laravel</h1>

Hello, @{% raw %} {{ name }} {% endraw %} .
```

<p>
در این مثال، نماد @ توسط Blade حذف می‌شود؛ با این حال، عبارت {% raw %}{{ name }}{% endraw %} توسط موتور Blade تفسیر نمی‌شود و این کار مستقیماً توسط فریم ورک جاوا اسکریپت انجام می‌شود.
</p>

<br>
<h3>The دستور @verbatim در Blade لاراول</h3>
<p>
اگر متغیرهای جاوا اسکریپت را در بخش بزرگی از قالب خود نمایش می‌دهید، می‌توانید عبارت‌های HTML را در دستور @verbatim قرار دهید، بنابراین لازم نیست که پیش از هر عبارت Blade echo علامت @ قرار دهید:
</p>

```html
@verbatim
    <div class="container">
        Hello,{% raw %} {{ name }} {% endraw %} .
    </div>
@endverbatim
```

<p>
<a name="control-structures"></a>
</p>

<br>
<h3><a href="#control-structures">استفاده از ساختارهای کنترلی در Blade لاراول</a></h3>
<p>
علاوه بر ارث بری قالب‌ها و نمایش داده‌ها در ویو، Blade لاراول میانبرهای مناسبی را نیز ارائه می‌دهد که می‌توان از آن‌ها جهت ایجاد ساختارهای کنترلی رایج PHP مانند دستورات شرطی و حلقه‌ها در ویوهای Blade استفاده کرد. این میانبرها یک روش بسیار ساده و کارآمد برای کار با ساختارهای کنترلی PHP ارائه می‌دهند، در حالی که همچنان، شباهت خود را با معادل‌های PHP حفظ می‌کنند.
</p>

<p>
<a name="if-statements"></a>
</p>

<br>
<h3>استفاده از عبارات If در Blade لاراول</h3>
<p>
برای ساخت عبارات if در یک ویوی Blade، می‌توانید از دستوالعمل‌های @if و @elseif و @else و @endif استفاده کنید. این دستورالعمل‌ها بسیار شبیه به معادل‌های PHP خود عمل می‌کنند:
</p>

```html
@if (count($records) === 1)
    I have one record!
@elseif (count($records) > 1)
    I have multiple records!
@else
    I don't have any records!
@endif
```

<p>
برای راحتی کار، Blade یک دستورالعمل @unless نیز ارائه می‌دهد:
</p>

```html
@unless (Auth::check())
    You are not signed in.
@endunless
```

<p>
علاوه بر دستورالعمل‌های شرطی که اشاره شد، دستورالعمل‌های @isset و @empty نیز ممکن است به عنوان میانبرهای مناسب برای توابع PHP مربوط به آن‌ها در یک ویوی Blade مورد استفاده قرار گیرند:
</p>

```html
@isset($records)
    // $records is defined and is not null...
@endisset

@empty($records)
    // $records is "empty"...
@endempty
```

<br>
<h3>دستورالعمل‌های احراز هویت (authentication) در Blade لاراول</h3>
<p>
جهت تعیین فوری اینکه کاربر جاری یک کاربر تایید شده است یا یک کاربر مهمان، می‌توان از دستورالعمل‌های احراز هویت @auth و @guest به صورت زیر استفاده کرد:
</p>

```html
@auth
    // The user is authenticated...
@endauth

@guest
    // The user is not authenticated...
@endguest
```

<p>
در صورت لزوم، می‌توانید مسئول احراز هویت را به عنوان پارامتر مشخص کنید که در زمان استفاده از دستورات @auth و @guest جهت تعیین اینکه کاربر از طریق مسئول احراز هویت وارد سیستم شده است، بررسی می‌شود:
</p>

```html
@auth('admin')
    // The user is authenticated...
@endauth

@guest('admin')
    // The user is not authenticated...
@endguest
```

<br>
<h3>استفاده از دستورالعمل‌های section در Blade لاراول</h3>
<p>
می‌توانید محتوای یک section را با استفاده از دستور @hasSection بررسی نمایید:
</p>

```html
@hasSection('navigation')
    <div class="pull-right">
        @yield('navigation')
    </div>

    <div class="clearfix"></div>
@endif
```

<p>
<a name="switch-statements"></a>
</p>

<br>
<h3>استفاده از عبارت‌های switch در Blade لاراول</h3>
<p>
عبارات switch می‌توانند با استفاده از دستورالعمل‌های @switch و @case و @default و @endswitch ایجاد شوند:
</p>

```html
@switch($i)
    @case(1)
        First case...
        @break

    @case(2)
        Second case...
        @break

    @default
        Default case...
@endswitch
```

<p>
<a name="loops"></a>
</p>

<br>
<h3>استفاده از دستورات حلقه‌ در Blade لاراول</h3>
<p>
علاوه بر دستورات شرطی، Blade دستورالعمل‌های ساده‌ای را نیز برای کار با ساختارهای حلقه‌ای زبان PHP ارائه می‌دهد. در این مورد هم، هر کدام از این دستورات مشابه معادل‌های خود در زبان PHP عمل می‌کنند:
</p>

```html
@for ($i = 0; $i < 10; $i++)
    The current value is {% raw %} {{ $i }} {% endraw %} 
@endfor

@foreach ($users as $user)
    <p>This is user {% raw %} {{ $user->id }} {% endraw %} </p>
@endforeach

@forelse ($users as $user)
    <li>{% raw %} {{ $user->name }} {% endraw %} </li>
@empty
    <p>No users</p>
@endforelse

@while (true)
    <p>I'm looping forever.</p>
@endwhile
```
<blockquote class="has-icon tip">
در زمان کار با حلقه‌ها، می‌توانید از متغیر حلقه برای بدست آوردن اطلاعات مهم در مورد حلقه موردنظر استفاده کنید، اطلاعاتی مانند اینکه در اولین یا آخرین گام تکرار حلقه قرار دارید.
</blockquote>
<p>
همچنین، هنگام استفاده از حلقه‌ها می‌توانید حلقه را پایان دهید یا گام تکرار فعلی را حفظ کنید:
</p>

```html
@foreach ($users as $user)
    @if ($user->type == 1)
        @continue
    @endif

    <li>{% raw %} {{ $user->name }} {% endraw %} </li>

    @if ($user->number == 5)
        @break
    @endif
@endforeach
```

<p>
همچنین، می‌توانید شرط اجرای حلقه را با اعلان دستور حلقه در یک خط کد قرار دهید:
</p>

```html
@foreach ($users as $user)
    @continue($user->type == 1)

    <li>{% raw %} {{ $user->name }} {% endraw %} </li>

    @break($user->number == 5)
@endforeach
```

<p>
<a name="the-loop-variable"></a>
</p>

<br>
<h3>متغیر حلقه در Blade لاراول</h3>
<p>
هنگام استفاده از حلقه‌ها، یک متغیر $loop در داخل حلقه در دسترس است که این متغیر اطلاعات مهمی مانند گام تکرار فعلی و اینکه آیا در اولین یا آخرین گام تکرار حلقه قرار دارید را مشخص می‌کند:
</p>

```html
@foreach ($users as $user)
    @if ($loop->first)
        This is the first iteration.
    @endif

    @if ($loop->last)
        This is the last iteration.
    @endif

    <p>This is user {% raw %} {{ $user->id }} {% endraw %} </p>
@endforeach
```

<p>
اگر در یک حلقه تکرار تو در تو باشید، می‌توانید به متغیر $loop حلقه والد از طریق خصوصیت parent دسترسی داشته باشید:
</p>

```html
@foreach ($users as $user)
    @foreach ($user->posts as $post)
        @if ($loop->parent->first)
            This is first iteration of the parent loop.
        @endif
    @endforeach
@endforeach
```

<p>
همچنین، متغیر $loop خصوصیت‌های مفید دیگری را نیز شامل می‌شود که در جدول زیر لیست شده اند:
</p>
<table>
<thead>
<tr>
<th>Property</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">index</span></code></td>
<td>The index of the current loop iteration (starts at 0).</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">iteration</span></code></td>
<td>The current loop iteration (starts at 1).</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">remaining</span></code></td>
<td>The iteration remaining in the loop.</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">count</span></code></td>
<td>The total number of items in the array being iterated.</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">first</span></code></td>
<td>Whether this is the first iteration through the loop.</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">last</span></code></td>
<td>Whether this is the last iteration through the loop.</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">depth</span></code></td>
<td>The nesting level of the current loop.</td>
</tr>
<tr>
<td><code class=" language-php"><span class="token variable">$loop</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token keyword">parent</span></code></td>
<td>When in a nested loop, the parent's loop variable.</td>
</tr>
</tbody>
</table>
<p>
<a name="comments"></a>
</p>

<br>
<h3>نحوه ثبت کامنت‌ در ‌Blade لاراول</h3>
<p>
Blade این امکان را می‌دهد که بتوانید در ویوهای خود کامنت‌ها را تعریف کنید. با این حال، بر خلاف کامنت‌های HTML، کامنت‌های Blade در ساختار HTML که توسط برنامه برگردانده می‌شود، قرار نمی‌‌گیرند:
</p>

```html
{% raw %} {{-- This comment will not be present in the rendered HTML --}} {% endraw %} 
```

<p>
<a name="php"></a>
</p>

<br>
<h3>PHP استفاده از کد</h3>
<p>
در برخی موارد، لازم است که از کد PHP در ویوهای ‌Blade خود استفاده کنید. برای این کار، می‌توانید از دستورالعمل @php برای اجرای یک بلوک ساده از کد PHP در قالب ویو خود استفاده کنید:
</p>

```html
@php
    //
@endphp
```
<blockquote class="has-icon tip">
در حالی که Blade امکان استفاده از این ویژگی را فراهم کرده است ولی با این حال استفاده از این ویژگی می‌تواند نشان دهنده این باشد که شما در گنجاندن منطق در قالب ویو خود زیاده روی کرده‌اید.
</blockquote>
<p>
<a name="including-sub-views"></a>
</p>

<br>
<h3><a href="#including-sub-views">sub-view نحوه استفاده از</a></h3>
<p>
دستور @include این امکان را می‌دهد که بتوانید یک ویو Blade را در داخل یک ویو دیگر اضافه کنید. تمام متغیرهایی که در ویو والد موجود هستند در ویو فرزند نیز قابل دسترس خواهند بود:
</p>

```html
<div>
    @include('shared.errors')

    <form>
        <!-- Form Contents -->
    </form>
</div>
```

<p>
با اینکه ویو فرزند تمام داده‌های موجود در ویو والد را به ارث می‌برد؛ با این حال، می‌توانید یک آرایه شامل داده‌های اضافی را به این ویو انتقال دهید:
</p>

```html
@include('view.name', ['some' => 'data'])
```

<p>
البته، اگر سعی در اضافه کردن یک ویو با دستور @include داشته باشید که موجود نیست، لاراول یک خطا صادر می‌کند. اگر بخواهید یک ویو را با احتمال موجود بودن یا موجود نبودن آن اضافه کنید، بایستی از دستور @includeIf به صورت زیر استفاده کنید:
</p>

```html
@includeIf('view.name', ['some' => 'data'])
```

<p>
اگر بخواهید یک sub-view را براساس یک عبارت شرطی بولین با دستور @include اضافه کنید، می‌توانید از دستورالعمل @includeWhen به صورت زیر استفاده کنید:
</p>

```html
@includeWhen($boolean, 'view.name', ['some' => 'data'])
```

<p>
جهت اضافه کردن اولین ویو موجود در آرایه‌ای از ویو‌ها، می‌توانید از دستورالعمل includeFirst به صورت زیر استفاده کنید:
</p>

```html
@includeFirst(['custom.admin', 'admin'], ['some' => 'data'])
```
<blockquote class="has-icon note">
از بکار بردن ثابت‌های __DIR__ و __FILE__ در ویوهای Blade خود اجتناب کنید، زیرا به مکان ویو کامپایل شده و کش شده اشاره می‌کنند.
</blockquote>
<p>
<a name="rendering-views-for-collections"></a>
</p>

<br>
<h3>رندر کردن view با مجموعه‌‌ها در Blade</h3>
<p>
می‌توانید حلقه‌ها و includeها را با دستورالعمل @each در یک خط کد ترکیب کنید:
</p>

```html
@each('view.name', $jobs, 'job')
```

<p>
اولین آرگومان، view partial جهت رندر کردن هر آیتم موجود در آرایه یا مجموعه است. آرگومان دوم، آرایه یا مجموعه‌ای است که می‌خواهید آن را تکرار کنید. سومین آرگومان، نام متغیری است که به گام تکرار فعلی در ویو نسبت داده می‌شود. برای مثال، اگر درون یک آرایه از jobها از حلقه استفاده می‌کنید، ممکن است به هر job به عنوان یک متغیر job در view partial خود دسترسی داشته باشید. کلید گام تکرار فعلی به عنوان متغیر key  در ویو در دسترس خواهد بود.
</p>

<p>
همچنین، می‌توانید چهارمین آرگومان را نیز به دستور @each انتقال دهید. در صورتی که آرایه داده شده خالی بود، این آرگومان مشخص می‌کند که کدام ویو نمایش داده شود:
</p>

```html
@each('view.name', $jobs, 'job', 'view.empty')
```
<blockquote class="has-icon note">
ویوهای نمایش داده شده از طریق دستور each@ متغیرها را از ویو والد به ارث نمی‌برند. اگر ویو فرزند به این متغیرها نیاز داشته باشد، می‌توانید به جای آن از دستور foreach@ و include@ استفاده کنید.
</blockquote>
<p>
<a name="stacks"></a>
</p>

<br>
<h3><a href="#stacks">پشته Stacks</a></h3>
<p>
‌Blade این امکان را می‌دهد تا پشته‌های نامگذاری شده را push کنید که در جایی دیگر در داخل یک ویو یا layout رندر شود. این موضوع می‌تواند جهت تعیین کتابخانه جاوا اسکریپت موردنیاز ویوهای فرزند مفید باشد.
</p>

```html
@push('scripts')
    <script src="/example.js"></script>
@endpush
```

<p>
می‌توانید یک پشته موردنیاز را چندین مرتبه push کنید. برای ارائه کامل محتویات پشته، نام پشته را به دستور @stack انتقال دهید:
</p>

```html
<head>
    <!-- Head Contents -->

    @stack('scripts')
</head>
```

<p>
<a name="service-injection"></a>
</p>

<br>
<h3><a href="#service-injection">تزریق خدمات یا service Injection در Blade</a></h3>
<p>
خروجی دستور @inject می‌تواند برای بازیابی یک سرویس از service container لاراول مورد استفاده قرار گیرد. اولین آرگومان ارسالی به @inject ، نام متغیری است که سرویس در آن قرار داده می‌شود؛ در حالی که، دومین آرگومان نام کلاس یا رابط سرویسی است که قصد دارید آن را resolve کنید.
</p>

```html
@inject('metrics', 'App\Services\MetricsService')

<div>
    Monthly Revenue: {% raw %} {{ $metrics->monthlyRevenue() }} {% endraw %} .
</div>
```

<p>
<a name="extending-blade"></a>
</p>

<br>
<h3><a href="#extending-blade">گسترش دستورالعمل‌های Blade در لاراول</a></h3>
<p>
Blade این امکان را می‌دهد که بتوانید دستورالعمل‌های سفارشی خود را با استفاده از متد directive تعریف کنید. زمانی که کامپایلر Blade با دستورالعمل سفارشی مواجه می‌شود، تابع callback ارائه شده را با پارامترهای آن فراخوانی می‌کند.
</p>

<p>
مثال زیر یک دستور @datetime($var) ایجاد می‌کند که یک متغیر $var را فرمت دهی می‌کند که باید یک نمونه از DateTime باشد:
</p>

<pre><code class="language-php  line-numbers">namespace App\Providers;

use Illuminate\Support\Facades\Blade;
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
        Blade::directive('datetime', function ($expression) {
            return "<?php echo ($expression)->format('m/d/Y H:i'); ?>";
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
همانطور که مشاهده می‌کنید، ما متد format  را بر روی هر عبارت به ترتیبی که در دستورالعمل قرار دارد، اعمال می‌کنیم. بنابراین، در این مثال کد PHP تولید شده توسط این دستور به این صورت خواهد بود:
</p>

<pre><code class="language-php  line-numbers"><?php echo ($var)->format('m/d/Y H:i'); ?>
</code></pre>
<blockquote class="has-icon note">
پس از بروز رسانی منطق یک دستورالعمل Blade، باید تمام ویوهای Blade کش شده را حذف کنید. می‌توانید با استفاده از دستور آرتیسان view:clear ویوهای Blade کش شده را پاک کنید.
</blockquote>
<p>
<a name="custom-if-statements"></a>
</p>

<br>
<h3>عبارات if سفارشی در Blade</h3>
<p>
برنامه نویسی یک دستورالعمل سفارشی، پیچیده‌تر از تعیین عبارات شرطی ساده به صورت سفارشی است. به همین دلیل Blade متد Blade::if را ارائه می‌دهد که توسط آن می‌توانید به سرعت دستورات شرطی سفارشی خود را با استفاده از Closure تعریف کنید. برای مثال، اجازه دهید یک دستور شرطی سفارشی را تعریف کنیم که محیط برنامه فعلی را بررسی می‌کند. می‌توانیم این کار را در متد boot از AppServiceProvider خود انجام دهیم:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Support\Facades\Blade;

/**
 * Perform post-registration booting of services.
 *
 * @return void
 */
public function boot()
{
    Blade::if('env', function ($environment) {
        return app()->environment($environment);
    });
}
</code></pre>

<p>
زمانی که دستور شرطی سفارشی ایجاد شد، می‌توانیم به راحتی از آن در قالب‌های ویو خود استفاده کنیم:
</p>

```html
@env('local')
    // The application is in the local environment...
@elseenv('testing')
    // The application is in the testing environment...
@else
    // The application is not in the local or testing environment...
@endenv
```