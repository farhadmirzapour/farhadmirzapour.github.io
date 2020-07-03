---
layout: documentation-laravel
title:   اصول localization
cattitle: اصول آموزش Laravel
date:   2018-02-16 18:36:42 +0330
jdate: جمعه 27 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel/Frontend
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/localization <br> www.tahlildadeh.com/ArticleDetails/آموزش-Localization-در-لاراول
---
<p>
امکان localization در فریم ورک Laravel به شما این اجازه را می دهد تا رشته ها را به زبان های مختلف در اپلیکیشن خود ترجمه و بازیابی کنید.
</p>
<p>
متغیرهای رشته ای language داخل فایل هایی در پوشه ی resources/lang ذخیره می شود. در این پوشه بایستی به ازای هر زبان که اپلیکیشن پشتیبانی می کند، یک subdirectory وجود داشته باشد:
</p>

<pre><code class="language-html  line-numbers">/resources
    /lang
        /en
            messages.php
        /es
            messages.php
</code></pre>

<p>
تمامی فایل های language صرفا یک آرایه از رشته های با کلید (keyed strings) را به عنوان خروجی برمی گردانند. مثال:
</p>
<pre><code class="language-php  line-numbers"><?php

return [
    'welcome' => 'Welcome to our application'
];
</code></pre>

<br>
<h3>تنظیم زبان جاری (locale)</h3>
<p>
زبان پیش فرض اپلیکیشن داخل فایل تنظیمات config/app.php ذخیره می شود. البته شما می توانید این مقدار را مطابق نیازهای اپلیکیشن خود ویرایش نمایید. همچنین می توانید با فراخوانی متد setLocale در App facade زبان فعلی برنامه را در زمان اجرا (runtime) تغییر دهید:
</p>
<pre><code class="language-php  line-numbers">Route::get('welcome/{locale}', function ($locale) {
    App::setLocale($locale);

    //
});
</code></pre>

<p>
همچنین می توانید یک زبان جایگزین (fallback) تنظیم کنید. این زبان جایگزین زمانی بکار می رود که زبان فعلی دربردارنده ی رشته مورد نظر نباشد. زبان جایگزین نیز مانند زبان فعلی برنامه در فایل تنظیمات config/app.php قابل دسترسی و تنظیم می باشد:
</p>
<pre><code class="language-php  line-numbers">'fallback_locale' => 'en',
</code></pre>

<br>
<h3>تعیین محل فعلی</h3>
<p>
می توانید با استفاده از متدهای getLocale و isLocale مرتبط با فاساد App، مکان فعلی تنظیم شده را بررسی نمایید :
</p>
<pre><code class="language-php  line-numbers">$locale = App::getLocale();

if (App::isLocale('en')) {
    //
}
</code></pre>

<p>
<a name="defining-translation-strings"></a>
</p>

<br>
<h3><a href="#defining-translation-strings">Defining Translation Strings</a></h3>
<p>
<a name="using-short-keys"></a>
</p>

<br>
<h3>Using Short Keys</h3>
<p>
به طور معمول، رشته های ترجمه در فایل ها در دایرکتوری resources / lang ذخیره می شوند. در این فهرست باید یک زیرپوشه برای هر زبان پشتیبانی شده توسط برنامه وجود داشته باشد:
</p>

<pre><code class="language-html  line-numbers">/resources
    /lang
        /en
            messages.php
        /es
            messages.php
</code></pre>

<p>
تمام فایل های زبان یک آرایه از رشته های کلید می گیرند. مثلا:
</p>
<pre><code class="language-php  line-numbers"><?php

// resources/lang/en/messages.php

return [
    'welcome' => 'Welcome to our application'
];
</code></pre>

<p>
<a name="using-translation-strings-as-keys"></a>
</p>

<br>
<h3>Using Translation Strings As Keys</h3>
<p>
برای برنامه های با نیازهای ترجمه سنگین، تعریف هر رشته با "کلید کوتاه" می تواند به سرعت در هنگام اشاره به آنها در نظرات شما گیج کننده باشد. به همین دلیل، Laravel همچنین پشتیبانی از تعریف رشته های ترجمه را با استفاده از «پیش فرض» ترجمه رشته به عنوان کلید ارائه می دهد.
</p>

<p>
فایل های ترجمه که از رشته های ترجمه به عنوان کلید استفاده می کنند، به عنوان فایل های JSON در دایرکتوری resource / lang ذخیره می شوند. به عنوان مثال، اگر درخواست شما یک ترجمه اسپانیایی باشد، باید یک فایل resource / lang / es.json ایجاد کنید:
</p>

<pre><code class="language-php  line-numbers">{
    "I love programming.": "Me encanta programar."
}
</code></pre>

<p>
<a name="retrieving-translation-strings"></a>
</p>

<br>
<h3><a href="#retrieving-translation-strings">Retrieving Translation Strings</a></h3>
<p>
می توانید با بهره گیری از تابع کمکی __ رشته هایی را از فایل های language بازیابی کنید. متد __ فایل و کلید مربوط به رشته ی زبان مورد نظر را به عنوان آرگوامان اول می پذیرد. در زیر با فراخوانی تابع مذکور رشته ی welcome را از فایل مربوط به زبانresources/lang/messages.php خوانده و بازیابی می کنیم.
</p>

<pre><code class="language-php  line-numbers">echo __('messages.welcome');

echo __('I love programming.');
</code></pre>

<p>
البته در صورتی که از موتور تولید قالب Blade استفاده می کنید، می توانید از سینتکس{% raw %}{{ }}{% endraw %} برای چاپ {% raw %}{{ }}{% endraw %}مورد نظر به زبان معین استفاده نمایید:
</p>
<pre><code class="language-php  line-numbers">{% raw %}{{{% endraw %} __('messages.welcome') {% raw %}}}{% endraw %}

@lang('messages.welcome')
</code></pre>

<p>
در صورتی که رشته ی مورد نظر در فایل language وجود نداشت، تابع trans کلید آن رشته را برمی گرداند. حال با توجه به مثال قبلی می توان نتیجه گرفت که در صورت عدم وجود رشته ی مورد نظر تابع نام برده messages.welcome را به عنوان خروجی برمی گرداند.
</p>

<p>
<a name="replacing-parameters-in-translation-strings"></a>
</p>

<br>
<h3>جایگزین کردن پارامترها در رشته های زبان </h3>
<p>
placeholder ها (مکان نگهدارها) ابزاری کارامدی هستند که می توانید در رشته های متنی زبان خود نیز از آن ها استفاده کنید. برای درجplaceholder کافی است ابتدا کاراکتر دو نقطه و سپس اسم مکان نگهدار دلخواه را مشخص نمایید. در زیر با استفاده از placeholder ای به نامname یک پیغام خوش آمدگویی نمایش می دهیم:
</p>
<pre><code class="language-php  line-numbers">'welcome' => 'Welcome, :name',
</code></pre>

<p>
به منظور جایگذاری مقدار مورد نظر در مکان نگهدار تعریف شده، در زمان بازیابی خط زبان (رشته ی متنی زبان)، آرایه ای از مقادیر جایگزین را به عنوان آرگومان دوم به تابع __ ارسال نمایید:
</p>
<pre><code class="language-php  line-numbers">echo __('messages.welcome', ['name' => 'dayle']);
</code></pre>

<p>
اگر دارنده مکان شما حاوی تمام حروف بزرگ است، یا فقط اولین حرف خود را با حروف بزرگ می نویسد، ارزش ترجمه شده به ترتیب به ترتیب سرمایه گذاری می شود:
</p>
<pre><code class="language-php  line-numbers">'welcome' => 'Welcome, :NAME', // Welcome, DAYLE
'goodbye' => 'Goodbye, :Name', // Goodbye, Dayle
</code></pre>

<p>
<a name="pluralization"></a>
</p>

<br>
<h3>Pluralization</h3>
<p>
از آنجایی که هر زبانی قواعد خاص خود را برای جمع بندی دارد، pluralizationمشکل پیچیده ای را در چند زبانه کردن اپلیکیشن به وجود آورده است. با بهره گیری از کاراکتر " | " می توان فرم های جمع و مفرد یک رشته را از هم تمایز بخشید:
</p>
<pre><code class="language-php  line-numbers">'apples' => 'There is one apple|There are many apples',
</code></pre>

<p>
مترجم Laravel توسط کامپوننت ترجمه ی Symphony طراحی و پشتیبانی می شود، بنابراین می توانید قواعد پیچیده تری برای جمع بندی رشته ها تعریف کنید:
</p>
<pre><code class="language-php  line-numbers">'apples' => '{0} There are none|[1,19] There are some|[20,*] There are many',
</code></pre>

<p>
پس از آن می توانید به وسیله ی تابع trans choice رشته مورد نظر را با توجه به آرگومان دوم (تعداد کلمات مشخص شده) ترجمه و بازیابی کنید. در این مثال، از آنجایی که مقدار آرگومان دوم از 1 بزرگتر است، فرم جمع رشته برگردانده می شود:
</p>
<pre><code class="language-php  line-numbers">echo trans_choice('messages.apples', 10);
</code></pre>

<p>
You may also define place-holder attributes in pluralization strings. These place-holders may be replaced by passing an array as the third argument to the <code class=" language-php">trans_choice</code> function:
</p>
<pre><code class="language-php  line-numbers">'minutes_ago' => '{1} :value minute ago|[2,*] :value minutes ago',

echo trans_choice('time.minutes_ago', 5, ['value' => 5]);
</code></pre>

<p>
<a name="overriding-package-language-files"></a>
</p>

<br>
<h3><a href="#overriding-package-language-files">Overriding Package Language Files</a></h3>
<p>
برخی از پکیج ها همراه با فایل های language اختصاصی خود ارائه می شوند. بجای اینکه فایل های اصلی پکیج را هک کرده و اقدام به ویرایش خط ها و رشته های متنی زبان کنید، می توانید با قرار دادن فایل های خود در پوشه ی resources/lang/vendor/{package}/{locale} به طور کامل آن ها را بازنویسی یا به اصطلاح override نمایید.
</p>

<p>
برای مثال اگر بخواهید متن های زبان انگلیسی را در فایل messages.php پکیج ای به نام skyrim/hearthfire بازنویسی کنید، در آن صورت بایستی فایل language دلخواه را در آدرس resources/lang/vendor/hearthfire/en/messages.php جایگذاری نمایید. یادآور می شویم که در این فایل فقط می بایست آن دسته از رشته های متنی (خط های زبان) که می خواهید بازنویسی شوند را تعریف کنید. هر رشته ی متنی یا خط زبانی (language line) که بازنویسی نکنید، از فایل های language اصلی پکیج خوانده و لود می شوند.
</p>
