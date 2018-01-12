---
layout: documentation
title:   معرفی collections
cattitle: اصول آموزش Laravel
date:   2017-12-18 11:23:42 +0330
jdate: دوشنبه 27 آذر 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/collections
---
<p>
کلاس <span class="redbold">Illuminate\<span class="token package">Support<span class="token punctuation">\</span>Collection</span></span> یک بسته مناسب و راحت برای کار با آرایه های داده فراهم می کند. به عنوان مثال، کد زیر را بررسی کنید. از <span class="redbold">collect</span> helper برای ایجاد نمونه جدید از آرایه استفاده می کنیم، عملگر <span class="redbold">strtoupper</span> را بر روی هر عنصر اجرا می کنیم و سپس تمام عناصر خالی را حذف می کنیم:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['taylor', 'abigail', null])->map(function ($name) {
    return strtoupper($name);
})
->reject(function ($name) {
    return empty($name);
});</code></pre>

<p>
همانطور که می بینید، کلاس <span class="redbold">Collection</span> به شما اجازه می دهد تا روش های زنجیره ای خود را برای انجام نقشه های روان  به کار ببرید. به طور كلي مجموعه ها تغيير نمي كنند، به اين معني كه هر متد <span class="redbold">Collection</span> نمونه كاملا جديدي از کلاس <span class="redbold">Collection</span> را باز مي گرداند.
</p>

<br>
<h3>متد Creating Collections</h3>

<p>
همانطور که در بالا ذکر شد، <span class="redbold">collect</span> helper نمونه جدید <span class="redbold">Illuminate\<span class="token package">Support<span class="token punctuation">\</span>Collection</span></span> را برای آرایه دریافت شده بر می گرداند. بنابراین ایجاد یک collection  ساده است:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3]);
</code></pre>


<blockquote>
نکته :نتایج کوئری های Eloquent همیشه نمونه ای از کلاس <span class="redbold">Collection</span> را بر می گرداند
</blockquote>

<br>
<h3>توسعه Collections</h3>

<p>
Collection ها "macroable" هستند، به این معنی که می توان متد هایی را به کلاس <span class="redbold">Collection</span> در زمان اجرا می توان اضافه نمود. برای مثال در کد زیر متدی به نام <span class="redbold">toUpper</span> را به این مجموعه اضافه نمودیم :
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Support\Str;

Collection::macro('toUpper', function () {
    return $this->map(function ($value) {
        return Str::upper($value);
    });
});

$collection = collect(['first', 'second']);

$upper = $collection->toUpper();

// ['FIRST', 'SECOND']
</code></pre>


<p>به طور معمول جهت معرفی نمودن متد ایجاد شده از service provider استفاده شده تا در تمامی قسمت های برنامه قابل دسترس باشد.</p>

<br>
<h3>متد های موجود در کلاس Collection</h3>
<p>متدهای از پیش تعریف شده این کلاس عبارتند از :</p>
<style>
    #collection-method-list > p {
        column-count: 3; -moz-column-count: 3; -webkit-column-count: 3;
        column-gap: 2em; -moz-column-gap: 2em; -webkit-column-gap: 2em;
        direction:ltr;

    }
#collection-method-list{
 text-align:center;
}
    #collection-method-list a {
        display: block;
        direction:ltr;
        color: #f4645f;
        text-align:left;
    }
</style>
<div id="collection-method-list">
<p><a href="#method-all">all</a>
<a href="#method-average">average</a>
<a href="#method-avg">avg</a>
<a href="#method-chunk">chunk</a>
<a href="#method-collapse">collapse</a>
<a href="#method-combine">combine</a>
<a href="#method-concat">concat</a>
<a href="#method-contains">contains</a>
<a href="#method-containsstrict">containsStrict</a>
<a href="#method-count">count</a>
<a href="#method-crossjoin">crossJoin</a>
<a href="#method-dd">dd</a>
<a href="#method-diff">diff</a>
<a href="#method-diffassoc">diffAssoc</a>
<a href="#method-diffkeys">diffKeys</a>
<a href="#method-dump">dump</a>
<a href="#method-each">each</a>
<a href="#method-eachspread">eachSpread</a>
<a href="#method-every">every</a>
<a href="#method-except">except</a>
<a href="#method-filter">filter</a>
<a href="#method-first">first</a>
<a href="#method-first-where">firstWhere</a>
<a href="#method-flatmap">flatMap</a>
<a href="#method-flatten">flatten</a>
<a href="#method-flip">flip</a>
<a href="#method-forget">forget</a>
<a href="#method-forpage">forPage</a>
<a href="#method-get">get</a>
<a href="#method-groupby">groupBy</a>
<a href="#method-has">has</a>
<a href="#method-implode">implode</a>
<a href="#method-intersect">intersect</a>
<a href="#method-intersectbykeys">intersectByKeys</a>
<a href="#method-isempty">isEmpty</a>
<a href="#method-isnotempty">isNotEmpty</a>
<a href="#method-keyby">keyBy</a>
<a href="#method-keys">keys</a>
<a href="#method-last">last</a>
<a href="#method-macro">macro</a>
<a href="#method-make">make</a>
<a href="#method-map">map</a>
<a href="#method-mapinto">mapInto</a>
<a href="#method-mapspread">mapSpread</a>
<a href="#method-maptogroups">mapToGroups</a>
<a href="#method-mapwithkeys">mapWithKeys</a>
<a href="#method-max">max</a>
<a href="#method-median">median</a>
<a href="#method-merge">merge</a>
<a href="#method-min">min</a>
<a href="#method-mode">mode</a>
<a href="#method-nth">nth</a>
<a href="#method-only">only</a>
<a href="#method-pad">pad</a>
<a href="#method-partition">partition</a>
<a href="#method-pipe">pipe</a>
<a href="#method-pluck">pluck</a>
<a href="#method-pop">pop</a>
<a href="#method-prepend">prepend</a>
<a href="#method-pull">pull</a>
<a href="#method-push">push</a>
<a href="#method-put">put</a>
<a href="#method-random">random</a>
<a href="#method-reduce">reduce</a>
<a href="#method-reject">reject</a>
<a href="#method-reverse">reverse</a>
<a href="#method-search">search</a>
<a href="#method-shift">shift</a>
<a href="#method-shuffle">shuffle</a>
<a href="#method-slice">slice</a>
<a href="#method-sort">sort</a>
<a href="#method-sortby">sortBy</a>
<a href="#method-sortbydesc">sortByDesc</a>
<a href="#method-splice">splice</a>
<a href="#method-split">split</a>
<a href="#method-sum">sum</a>
<a href="#method-take">take</a>
<a href="#method-tap">tap</a>
<a href="#method-times">times</a>
<a href="#method-toarray">toArray</a>
<a href="#method-tojson">toJson</a>
<a href="#method-transform">transform</a>
<a href="#method-union">union</a>
<a href="#method-unique">unique</a>
<a href="#method-uniquestrict">uniqueStrict</a>
<a href="#method-unless">unless</a>
<a href="#method-unwrap">unwrap</a>
<a href="#method-values">values</a>
<a href="#method-when">when</a>
<a href="#method-where">where</a>
<a href="#method-wherestrict">whereStrict</a>
<a href="#method-wherein">whereIn</a>
<a href="#method-whereinstrict">whereInStrict</a>
<a href="#method-wherenotin">whereNotIn</a>
<a href="#method-wherenotinstrict">whereNotInStrict</a>
<a href="#method-wrap">wrap</a>
<a href="#method-zip">zip</a></p>
</div>

<p><a name="method-all"></a></p>
<br>
<h3>متد all</h3>
<p>
این متد تمامی زیر آرایه های تعریف شده موجود در این مجموعه را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3])->all();

// [1, 2, 3]
</code></pre>


<p><a name="method-avg"></a></p>
<br>
<h3>متد avg</h3>
<p>
این متد معدل مقادیر را بر اساس کلید دریافتی بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$average = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->avg('foo');

// 20

$average = collect([1, 1, 2, 4])->avg();

// 2
</code></pre>


<p><a name="method-chunk"></a></p>
<br>
<h3>متد chunk</h3>
<p>
با استفاده از این متد می توان مجموعه را به چند زیر مجموعه کوچکتر با سایز مشخص شده تقسیم کرد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5, 6, 7]);

$chunks = $collection->chunk(4);

$chunks->toArray();

// [[1, 2, 3, 4], [5, 6, 7]]
</code></pre>

<p>
از این روش می توان در view  ها زمان نمایش اطلاعات مانند زیر استفاده کرد :
</p>

<pre><code class="language-php  line-numbers">@foreach ($products->chunk(3) as $chunk)
    <div class="row">
        @foreach ($chunk as $product)
            <div class="col-xs-4">{!! $product->name !!}</div>
        @endforeach
    </div>
@endforeach</code></pre>


<p><a name="method-collapse"></a></p>
<br>
<h3>متد collapse</h3>
<p>
این متد ، زیر آرایه های موجود را بصورت یک آرایه تبدیل می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([[1, 2, 3], [4, 5, 6], [7, 8, 9]]);

$collapsed = $collection->collapse();

$collapsed->all();

// [1, 2, 3, 4, 5, 6, 7, 8, 9]
</code></pre>

<p><a name="method-combine"></a></p>
<br>
<h3>متد combine</h3>
<p>
این متد کلید آرایه یک مجموعه با مقدار آرایه مجموعه ای دیگر را ترکیب می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name', 'age']);

$combined = $collection->combine(['George', 29]);

$combined->all();

// ['name' => 'George', 'age' => 29]
</code></pre>

<p><a name="method-concat"></a></p>
<br>
<h3>متد concat</h3>
<p>
این متد آرایه دریافتی را به انتهای آرایه مجموعه اضافه می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['John Doe']);

$concatenated = $collection->concat(['Jane Doe'])->concat(['name' => 'Johnny Doe']);

$concatenated->all();

// ['John Doe', 'Jane Doe', 'Johnny Doe']
</code></pre>

<p><a name="method-contains"></a></p>
<br>
<h3>متد contains</h3>
<p>
این متد وجود یک آیتم مشخص را در مجموعه بررسی می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'Desk', 'price' => 100]);

$collection->contains('Desk');

// true

$collection->contains('New York');

// false
</code></pre>

<p>
شما همچنین می توانید یک key / value را همانند زیر ارسال نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
]);

$collection->contains('product', 'Bookcase');

// false
</code></pre>
<p>
همچنین می توانید یک متد callback  را جهت بررسی داده های خود اجرا کنید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->contains(function ($value, $key) {
    return $value > 5;
});

// false
</code></pre>

<p><a name="method-containsstrict"></a></p>
<br>
<h3>متد containsStrict</h3>
<p>
این متد همانند contains  می باشد و از "strict" comparisons جهت بررسی داده ها استفاده می کند.
</p>


<p><a name="method-count"></a></p>
<br>
<h3>متد count</h3>
<p>
تعداد آیتم های یک مجموعه را شمارش می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4]);

$collection->count();

// 4
</code></pre>

<p><a name="method-crossjoin"></a></p>
<br>
<h3>متد crossJoin</h3>

<p>
 این متد یک ماتریس را ایجاد می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b']);

$matrix->all();

/*
    [
        [1, 'a'],
        [1, 'b'],
        [2, 'a'],
        [2, 'b'],
    ]
*/

$collection = collect([1, 2]);

$matrix = $collection->crossJoin(['a', 'b'], ['I', 'II']);

$matrix->all();

/*
    [
        [1, 'a', 'I'],
        [1, 'a', 'II'],
        [1, 'b', 'I'],
        [1, 'b', 'II'],
        [2, 'a', 'I'],
        [2, 'a', 'II'],
        [2, 'b', 'I'],
        [2, 'b', 'II'],
    ]
*/
</code></pre>

<p><a name="method-dd"></a></p>
<br>
<h3>متد dd</h3>
<p>
این متد محتوای مجموعه را نمایش و روند اجرای برنامه را قطع میکند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['John Doe', 'Jane Doe']);

$collection->dd();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
</code></pre>

<p>
چنانچه بخواهیم روند اجرای برنامه متوقف نشود از متد <span class="redbold">dump</span>  می توان استفاده کرد.
</p>

<p><a name="method-diff"></a></p>
<br>
<h3>متد diff</h3>
<p>
این متد مقدار آیتم های یک مجموعه را با مجموعه دیگر مقایسه و در صورت عدم وجود،  برگشت داده خواهند شد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$diff = $collection->diff([2, 4, 6, 8]);

$diff->all();

// [1, 3, 5]
</code></pre>

<p><a name="method-diffassoc"></a></p>
<br>
<h3>متد diffAssoc</h3>
<p>
این متد همانند متد diff  می باشد با این تفاوت که براساس key / value عمل می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    'color' => 'orange',
    'type' => 'fruit',
    'remain' => 6
]);

$diff = $collection->diffAssoc([
    'color' => 'yellow',
    'type' => 'fruit',
    'remain' => 3,
    'used' => 6
]);

$diff->all();

// ['color' => 'orange', 'remain' => 6]
</code></pre>

<p><a name="method-diffkeys"></a></p>
<br>
<h3>متد diffKeys</h3>
<p>
این متد همانند متد diff  بوده با این تفاوت که براساس  کلید عمل می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    'one' => 10,
    'two' => 20,
    'three' => 30,
    'four' => 40,
    'five' => 50,
]);

$diff = $collection->diffKeys([
    'two' => 2,
    'four' => 4,
    'six' => 6,
    'eight' => 8,
]);

$diff->all();

// ['one' => 10, 'three' => 30, 'five' => 50]
</code></pre>


<p><a name="method-dump"></a></p>
<br>
<h3>متد dump</h3>
<p>
این متد محتویات مجموعه را بدون متوقف کردن اجرای برنامه متوقف می کند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['John Doe', 'Jane Doe']);

$collection->dump();

/*
    Collection {
        #items: array:2 [
            0 => "John Doe"
            1 => "Jane Doe"
        ]
    }
*/
</code></pre>


<p><a name="method-each"></a></p>
<br>
<h3>متد each</h3>
<p>
  این متد هر بار یک آیتم از مجموعه دریافت و به متد callback جهت اجرای عملیات بر روی آن ارسال می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = $collection->each(function ($item, $key) {
    //
});
</code></pre>

<p>
با برگشت مقدار false  می توان روند اجرای each  را متوقف کرد :
</p>

<pre><code class="language-php  line-numbers">$collection = $collection->each(function ($item, $key) {
    if (/* some condition */) {
        return false;
    }
});
</code></pre>


<p><a name="method-eachspread"></a></p>
<br>
<h3>متد eachSpread</h3>

<p>
همانند متد each  بوده و آیتم های تودرتو را به callback  ارسال می کند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([['John Doe', 35], ['Jane Doe', 33]]);

$collection->eachSpread(function ($name, $age) {
    //
});
</code></pre>

<p><a name="method-every"></a></p>
<br>
<h3>متد every</h3>
<p>
از این متد جهت تایید آزمون مشخص روی تمامی عناصر استفاده می شود :

</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4])->every(function ($value, $key) {
    return $value > 2;
});

// false
</code></pre>


<p><a name="method-except"></a></p>
<br>
<h3>متد except</h3>
<p>
این متد تمامی عناصر به غیر از آیتم های مشخص شده را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['product_id' => 1, 'price' => 100, 'discount' => false]);

$filtered = $collection->except(['price', 'discount']);

$filtered->all();

// ['product_id' => 1]
</code></pre>

<p><a name="method-filter"></a></p>
<br>
<h3>متد filter</h3>
<p>
از این متد جهت فیلتر کردن عناصر با یک شرط معین استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4]);

$filtered = $collection->filter(function ($value, $key) {
    return $value > 2;
});

$filtered->all();

// [3, 4]
</code></pre>

<p>
اگر شرطی تعیین نشود تمامی عناصر معادل false حذف خواهند شد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, null, false, '', 0, []]);

$collection->filter()->all();

// [1, 2, 3]
</code></pre>

<p><a name="method-first"></a></p>
<br>
<h3>متد first</h3>
<p>
اولین آیتم که با گذر از شرط تعیین شده تایید شود برگشت داده خواهد شد :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4])->first(function ($value, $key) {
    return $value > 2;
});

// 3
</code></pre>


<p>
چنانچه شرطی تعیین نگردد اولین آیتم برگشت داده خواهد شد :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4])->first();

// 1
</code></pre>

<p><a name="method-first-where"></a></p>
<br>
<h3>متد firstWhere</h3>
<p>
متد firstWhere اولین عنصر مجموعه که با کلید / ارزش معادل باشد باز می گرداند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'Regena', 'age' => 12],
    ['name' => 'Linda', 'age' => 14],
    ['name' => 'Diego', 'age' => 23],
    ['name' => 'Linda', 'age' => 84],
]);

$collection->firstWhere('name', 'Linda');

// ['name' => 'Linda', 'age' => 14]
</code></pre>

<p>
همچنین می توانید این متد را با یک اپراتور فراخوانی کنید :
</p>

<pre><code class="language-php  line-numbers">$collection->firstWhere('age', '>=', 18);

// ['name' => 'Diego', 'age' => 23]
</code></pre>

<p><a name="method-flatmap"></a></p>
<br>
<h3>متد flatMap</h3>
<p>
متد flatmap داخل یک collection حلقه زده و سپس تک تک المان های آن را به تابع callback ارسال می کند. تابع بازفراخوان (callback) می تواند آیتم های ارسالی را ویرایش کرده و برگرداند. بدین وسیله مجموعه ای جدید از آیتم های ویرایش شده به عنوان خروجی برگردانده می شود. در نهایت آرایه ها flatten شده و در قالب یک آرایه ی واحد و تک بعدی در خروجی به نمایش در می آید:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'Sally'],
    ['school' => 'Arkansas'],
    ['age' => 28]
]);

$flattened = $collection->flatMap(function ($values) {
    return array_map('strtoupper', $values);
});

$flattened->all();

// ['name' => 'SALLY', 'school' => 'ARKANSAS', 'age' => '28'];
</code></pre>


<p><a name="method-flatten"></a></p>
<br>
<h3>متد flatten</h3>
<p>
این متد یک مجموعه ی چند بعدی را به یک collection تک بعدی تبدیل می کند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'taylor', 'languages' => ['php', 'javascript']]);

$flattened = $collection->flatten();

$flattened->all();

// ['taylor', 'php', 'javascript'];
</code></pre>


<p>
می توانید میزان "depth"  را به عنوان آرگومان ارسال کنیم :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    'Apple' => [
        ['name' => 'iPhone 6S', 'brand' => 'Apple'],
    ],
    'Samsung' => [
        ['name' => 'Galaxy S7', 'brand' => 'Samsung']
    ],
]);

$products = $collection->flatten(1);

$products->values()->all();

/*
    [
        ['name' => 'iPhone 6S', 'brand' => 'Apple'],
        ['name' => 'Galaxy S7', 'brand' => 'Samsung'],
    ]
*/
</code></pre>

<p><a name="method-flip"></a></p>
<br>
<h3>متد flip</h3>
<p>
این متد کلید و مقدار را جا به جا می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$flipped = $collection->flip();

$flipped->all();

// ['taylor' => 'name', 'laravel' => 'framework']
</code></pre>

<p><a name="method-forget"></a></p>
<br>
<h3>متد forget</h3>
<p>
این متد آیتم را با استفاده از کلید مشخص شده حذف می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$collection->forget('name');

$collection->all();

// ['framework' => 'laravel']
</code></pre>

<p><a name="method-forpage"></a></p>
<br>
<h3>متد forPage</h3>
<p>
روش forPage یک مجموعه جدید را که حاوی آیتم هایی است که در یک شماره صفحه مشخص وجود دارد، بازمی گرداند. این روش شماره صفحه را به عنوان اولین آرگومان و تعداد آیتم هایی که در هر صفحه نشان می دهد را  به عنوان آرگومان دوم می پذیرد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunk = $collection->forPage(2, 3);

$chunk->all();

// [4, 5, 6]
</code></pre>

<p><a name="method-get"></a></p>
<br>
<h3>متد get</h3>
<p>
این متد یک آیتم را با استفاده از کلید آن باز می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$value = $collection->get('name');

// taylor
</code></pre>

<p>
چنانچه کلیدی یافت نشود مقدار پیش فرض null  برگشت داده خواهد شد . می توان مقدار پیش فرض را مانند زیر تعیین کرد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);

$value = $collection->get('foo', 'default-value');

// default-value
</code></pre>

<p>
همچنین می توانید مقدار پیش فرض را با استفاده از روش callback  همانند زیر تعیین کنید :

</p>

<pre><code class="language-php  line-numbers">$collection->get('email', function () {
    return 'default-value';
});

// default-value
</code></pre>

<p><a name="method-groupby"></a></p>
<br>
<h3>متد groupBy</h3>
<p>
این متد آیتم های مجموعه را با استفاده از کلید گروه بندی می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['account_id' => 'account-x10', 'product' => 'Chair'],
    ['account_id' => 'account-x10', 'product' => 'Bookcase'],
    ['account_id' => 'account-x11', 'product' => 'Desk'],
]);

$grouped = $collection->groupBy('account_id');

$grouped->toArray();

/*
    [
        'account-x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'account-x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
</code></pre>

<p>
همچنین می توانید از روش callback  در این متد همانند زیر استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$grouped = $collection->groupBy(function ($item, $key) {
    return substr($item['account_id'], -3);
});

$grouped->toArray();

/*
    [
        'x10' => [
            ['account_id' => 'account-x10', 'product' => 'Chair'],
            ['account_id' => 'account-x10', 'product' => 'Bookcase'],
        ],
        'x11' => [
            ['account_id' => 'account-x11', 'product' => 'Desk'],
        ],
    ]
*/
</code></pre>

<p><a name="method-has"></a></p>
<br>
<h3>متد has</h3>
<p>
این متد بررسی می کند که آیا کلید مورد نظر وجود دارد یا خیر :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['account_id' => 1, 'product' => 'Desk']);

$collection->has('product');

// true
</code></pre>

<p><a name="method-implode"></a></p>
<br>
<h3>متد implode</h3>
<p>
متد implode عناصر موجود را به هم اتصال می دهد. اگر مجموعه شامل آرایه ها یا اشیاء باشد، باید کلید عناصر و رشته "اتصال" که می خواهید بین مقادیر قرار دهید را تعیین نمایید:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['account_id' => 1, 'product' => 'Desk'],
    ['account_id' => 2, 'product' => 'Chair'],
]);

$collection->implode('product', ', ');

// Desk, Chair
</code></pre>

<p>
آگر مجموعه شامل یک آرایه ساده باشد فقط کافی است رشته "اتصال" را مشخص نمایید :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4, 5])->implode('-');

// '1-2-3-4-5'
</code></pre>

<p><a name="method-intersect"></a></p>
<br>
<h3>متد intersect</h3>
<p>
این متد هر آیتمی را از مجموعه اصلی که در آرایه یا مجموعه داده شده موجود نباشد حذف می کند. مجموعه نتیجه، کلیدهای اصلی مجموعه را حفظ خواهد کرد:
</p>


<pre><code class="language-php  line-numbers">$collection = collect(['Desk', 'Sofa', 'Chair']);

$intersect = $collection->intersect(['Desk', 'Chair', 'Bookcase']);

$intersect->all();

// [0 => 'Desk', 2 => 'Chair']
</code></pre>

<p><a name="method-intersectbykeys"></a></p>
<br>
<h3>متد intersectByKeys</h3>
<p>
متد intersectByKeys هر کلید از مجموعه اصلی که در آرایه یا مجموعه داده شده موجود نباشد را حذف می کند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    'serial' => 'UX301', 'type' => 'screen', 'year' => 2009
]);

$intersect = $collection->intersectByKeys([
    'reference' => 'UX404', 'type' => 'tab', 'year' => 2011
]);

$intersect->all();

// ['type' => 'screen', 'year' => 2009]
</code></pre>



<br>
<p><a name="method-isempty"></a></p>
<h3>متد isEmpty</h3>
<p>
این متد چنانچه مجموعه خالی باشد مقدار true در غیر اینصورت مقدار false  بر می گرداند:
</p>
<pre><code class="language-php  line-numbers">collect([])->isEmpty();

// true
</code></pre>


<p><a name="method-isnotempty"></a></p>
<br>
<h3>متد isNotEmpty</h3>
<p>
چنانچه مجموعه خالی نباشد مقدار true  در غیر اینصورت مقدار false  بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">collect([])->isNotEmpty();

// false
</code></pre>


<p><a name="method-keyby"></a></p>
<br>
<h3>متد keyBy</h3>
<p>
متد keyBy مجموعه را با کلید داده شده کلید می کند. اگر چندین عنصر همان کلید را داشته باشند، تنها آخرین در مجموعه جدید ظاهر می شود:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keyed = $collection->keyBy('product_id');

$keyed->all();

/*
    [
        'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
</code></pre>

<p>
همچنین می توانید از روش callback  در این متد استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$keyed = $collection->keyBy(function ($item) {
    return strtoupper($item['product_id']);
});

$keyed->all();

/*
    [
        'PROD-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
        'PROD-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
    ]
*/
</code></pre>


<p><a name="method-keys"></a></p>
<br>
<h3>متد keys</h3>
<p>
این متد کلیه کلیدها را بر می گرداند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    'prod-100' => ['product_id' => 'prod-100', 'name' => 'Desk'],
    'prod-200' => ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$keys = $collection->keys();

$keys->all();

// ['prod-100', 'prod-200']
</code></pre>


<p><a name="method-last"></a></p>
<br>
<h3>متد last</h3>
<p>
آخرین آیتم که مصداق یک شرط مشخص باشد را برمی گرداند :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4])->last(function ($value, $key) {
    return $value < 3;
});

// 2
</code></pre>

<p>
اگر شرطی تعیین نشود آخرین عنصر برگشت داده خواهد شد.
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4])->last();

// 4
</code></pre>


<p><a name="method-macro"></a></p>
<br>
<h3>متد macro</h3>
<p>
با استفتده از این متد می توان متدهای مورد نظر خود را به این کلاس اضافه نماییم.
</p>

<p><a name="method-make"></a></p>
<br>
<h3>متد make</h3>
<p>
این متد نمونه ای از کلاس collection  را ایجاد می کند.
</p>

<p><a name="method-map"></a></p>
<br>
<h3>متد map</h3>
<p>
متد map داخل مجموعه حلقه زده و سپس هر مقدار را به تابع callback پاس می دهد. callback می تواند آیتم را ویرایش نموده و آن را در خروجی برگرداند. بدین وسیله یک نمونه ی جدید از مجموعه، متشکل از آیتم های ویرایش شده ی collection اصلی در خروجی ارائه می شود:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$multiplied = $collection->map(function ($item, $key) {
    return $item * 2;
});

$multiplied->all();

// [2, 4, 6, 8, 10]
</code></pre>


<p><a name="method-mapinto"></a></p>
<br>
<h3>متد mapInto</h3>
<p>
روش mapInto  بر روی مجموعه حلقه می زند، و  یک نمونه جدید از کلاس داده شده با گذراندن مقدار به سازنده ایجاد می کند:
</p>

<pre><code class="language-php  line-numbers">class Currency
{
    /**
     * Create a new currency instance.
     *
     * @param  string  $code
     * @return void
     */
    function __construct(string $code)
    {
        $this->code = $code;
    }
}

$collection = collect(['USD', 'EUR', 'GBP']);

$currencies = $collection->mapInto(Currency::class);

$currencies->all();

// [Currency('USD'), Currency('EUR'), Currency('GBP')]
</code></pre>


<p><a name="method-mapspread"></a></p>
<br>
<h3>متد mapSpread</h3>
<p>The <span class="redbold">mapSpread</span> method iterates over the collection's items, passing each nested item value into the given callback. The callback is free to modify the item and return it, thus forming a new collection of modified items:</p>


<pre><code class="language-php  line-numbers">$collection = collect([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]);

$chunks = $collection->chunk(2);

$sequence = $chunks->mapSpread(function ($odd, $even) {
    return $odd + $even;
});

$sequence->all();

// [1, 5, 9, 13, 17]
</code></pre>


<p><a name="method-maptogroups"></a></p>
<br>
<h3>متد mapToGroups</h3>
<p>The <span class="redbold">mapToGroups</span> method groups the collection's items by the given callback. The callback should return an associative array containing a single key / value pair, thus forming a new collection of grouped values:</p>


<pre><code class="language-php  line-numbers">$collection = collect([
    [
        'name' => 'John Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Jane Doe',
        'department' => 'Sales',
    ],
    [
        'name' => 'Johnny Doe',
        'department' => 'Marketing',
    ]
]);

$grouped = $collection->mapToGroups(function ($item, $key) {
    return [$item['department'] => $item['name']];
});

$grouped->toArray();

/*
    [
        'Sales' => ['John Doe', 'Jane Doe'],
        'Marketing' => ['Johhny Doe'],
    ]
*/

$grouped->get('Sales')->all();

// ['John Doe', 'Jane Doe']
</code></pre>


<p><a name="method-mapwithkeys"></a></p>
<br>
<h3>متد mapWithKeys</h3>
<p>The <span class="redbold">mapWithKeys</span> method iterates through the collection and passes each value to the given callback. The callback should return an associative array containing a single key / value pair:</p>


<pre><code class="language-php  line-numbers">$collection = collect([
    [
        'name' => 'John',
        'department' => 'Sales',
        'email' => 'john@example.com'
    ],
    [
        'name' => 'Jane',
        'department' => 'Marketing',
        'email' => 'jane@example.com'
    ]
]);

$keyed = $collection->mapWithKeys(function ($item) {
    return [$item['email'] => $item['name']];
});

$keyed->all();

/*
    [
        'john@example.com' => 'John',
        'jane@example.com' => 'Jane',
    ]
*/
</code></pre>


<p><a name="method-max"></a></p>
<br>
<h3>متد max</h3>
<p>
مقدار max  را براساس کلید بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$max = collect([['foo' => 10], ['foo' => 20]])->max('foo');

// 20

$max = collect([1, 2, 3, 4, 5])->max();

// 5
</code></pre>


<p><a name="method-median"></a></p>
<br>
<h3>متد median</h3>
<p>
این متد مقدار متوسط مجموعه را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$median = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->median('foo');

// 15

$median = collect([1, 1, 2, 4])->median();

// 1.5
</code></pre>


<p><a name="method-merge"></a></p>
<br>
<h3>متد merge</h3>
<p>
این متد عناصر یک مجموعه را با مجموعه دیگر ادغام می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['product_id' => 1, 'price' => 100]);

$merged = $collection->merge(['price' => 200, 'discount' => false]);

$merged->all();

// ['product_id' => 1, 'price' => 200, 'discount' => false]
</code></pre>

<p>
مثال :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['Desk', 'Chair']);

$merged = $collection->merge(['Bookcase', 'Door']);

$merged->all();

// ['Desk', 'Chair', 'Bookcase', 'Door']
</code></pre>


<p><a name="method-min"></a></p>
<br>
<h3>متد min</h3>
<p>
مقدار min  را براساس کلید بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$min = collect([['foo' => 10], ['foo' => 20]])->min('foo');

// 10

$min = collect([1, 2, 3, 4, 5])->min();

// 1
</code></pre>


<p><a name="method-mode"></a></p>
<br>
<h3>متد mode</h3>
<p>
مقدار mode  را براساس کلید بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$mode = collect([['foo' => 10], ['foo' => 10], ['foo' => 20], ['foo' => 40]])->mode('foo');

// [10]

$mode = collect([1, 1, 2, 4])->mode();

// [1]
</code></pre>


<p><a name="method-nth"></a></p>
<br>
<h3>متد nth</h3>
<p>The <span class="redbold">nth</span> method creates a new collection consisting of every n-th element:</p>


<pre><code class="language-php  line-numbers">$collection = collect(['a', 'b', 'c', 'd', 'e', 'f']);

$collection->nth(4);

// ['a', 'e']
</code></pre>


<p>You may optionally pass an offset as the second argument:</p>


<pre><code class="language-php  line-numbers">$collection->nth(4, 1);

// ['b', 'f']
</code></pre>


<p><a name="method-only"></a></p>
<br>
<h3>متد only</h3>
<p>
آیتم های موجود در مجموعه را با کلیدهای مشخصی باز می گرداند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['product_id' => 1, 'name' => 'Desk', 'price' => 100, 'discount' => false]);

$filtered = $collection->only(['product_id', 'name']);

$filtered->all();

// ['product_id' => 1, 'name' => 'Desk']
</code></pre>

<p><a name="method-pad"></a></p>
<br>
<h3>متد pad</h3>
<p>
روش pad آرایه را با مقدار داده شده پر می کند تا آرایه به اندازه مشخصی برسد. این روش مانند تابع array_pad PHP عمل می کند.
</p>

<p>
برای قرار دادن به سمت چپ، شما باید اندازه منفی را مشخص کنید. در صورتی که مقدار مطلق اندازه داده شده کمتر یا برابر طول آرایه باشد، هیچ مکانی وجود نخواهد داشت:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['A', 'B', 'C']);

$filtered = $collection->pad(5, 0);

$filtered->all();

// ['A', 'B', 'C', 0, 0]

$filtered = $collection->pad(-5, 0);

$filtered->all();

// [0, 0, 'A', 'B', 'C']
</code></pre>


<p><a name="method-partition"></a></p>
<br>
<h3>متد partition</h3>
<p>
از این متد جهت جدا کردن آیتم هایی که مورد تایید شرط مورد نظر هستند و آیتم هایی که مورد تایید شرط نیستند استفاده می گردد:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5, 6]);

list($underThree, $aboveThree) = $collection->partition(function ($i) {
    return $i < 3;
});
</code></pre>


<p><a name="method-pipe"></a></p>
<br>
<h3>متد pipe</h3>
<p>
این متد نوع داده collection را به callback ارسال می کند و نتیجه آنرا بر می گرداند  :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3]);

$piped = $collection->pipe(function ($collection) {
    return $collection->sum();
});

// 6
</code></pre>


<p><a name="method-pluck"></a></p>
<br>
<h3>متد pluck</h3>
<p>
این متد تمام مقادیر به ازای کلید معین شده بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product_id' => 'prod-100', 'name' => 'Desk'],
    ['product_id' => 'prod-200', 'name' => 'Chair'],
]);

$plucked = $collection->pluck('name');

$plucked->all();

// ['Desk', 'Chair']
</code></pre>


<p>
همچنین می توانید کلید خروجی را برای نتایج تعیین کنید :
</p>

<pre><code class="language-php  line-numbers">$plucked = $collection->pluck('name', 'product_id');

$plucked->all();

// ['prod-100' => 'Desk', 'prod-200' => 'Chair']
</code></pre>


<p><a name="method-pop"></a></p>
<br>
<h3>متد pop</h3>
<p>
این متد آخرین عنصر مجموعه را بر می گرداند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->pop();

// 5

$collection->all();

// [1, 2, 3, 4]
</code></pre>


<p><a name="method-prepend"></a></p>
<br>
<h3>متد prepend</h3>
<p>
این متد یک آیتم را به ابتدای مجموعه اضافه می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->prepend(0);

$collection->all();

// [0, 1, 2, 3, 4, 5]
</code></pre>


<p>
همچنین از آرگومان دوم به عنوان کلید استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['one' => 1, 'two' => 2]);

$collection->prepend(0, 'zero');

$collection->all();

// ['zero' => 0, 'one' => 1, 'two' => 2]
</code></pre>


<p><a name="method-pull"></a></p>
<br>
<h3>متد pull</h3>
<p>
این متد یک آیتم را با کلید تعیین شده بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['product_id' => 'prod-100', 'name' => 'Desk']);

$collection->pull('name');

// 'Desk'

$collection->all();

// ['product_id' => 'prod-100']
</code></pre>


<p><a name="method-push"></a></p>
<br>
<h3>متد push</h3>
<p>
این متد آیتم را به انتهای مجموعه اضافه می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4]);

$collection->push(5);

$collection->all();

// [1, 2, 3, 4, 5]
</code></pre>


<p><a name="method-put"></a></p>
<br>
<h3>متد put</h3>
<p>
این متد یک عنصر را با کلید و مقدار مشخص در مجموعه قرار می دهد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['product_id' => 1, 'name' => 'Desk']);

$collection->put('price', 100);

$collection->all();

// ['product_id' => 1, 'name' => 'Desk', 'price' => 100]
</code></pre>


<p><a name="method-random"></a></p>
<br>
<h3>متد random</h3>
<p>
یک آیتم را بصورت تصادفی از مجموعه بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->random();

// 4 - (retrieved randomly)
</code></pre>

<p>
همچنین می توانید تعداد آیتم هایی که بصورت تصادفی برگشت دا می شوند را تعیین کنید :
</p>

<pre><code class="language-php  line-numbers">$random = $collection->random(3);

$random->all();

// [2, 4, 5] - (retrieved randomly)
</code></pre>


<p><a name="method-reduce"></a></p>
<br>
<h3>متد reduce</h3>
<p>
این متد مجموعه را به یک مقدار کاهش می دهد ، و نتیجه هر تکرار را به تکرار بعدی انتقال می دهد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3]);

$total = $collection->reduce(function ($carry, $item) {
    return $carry + $item;
});

// 6
</code></pre>


<p>
مقدار  carry$ در تکرار اول null است؛ با این وجود، شما می توانید مقدار اولیه آن را تعیین نمایید:
</p>

<pre><code class="language-php  line-numbers">$collection->reduce(function ($carry, $item) {
    return $carry + $item;
}, 4);

// 10
</code></pre>


<p><a name="method-reject"></a></p>
<br>
<h3>متد reject</h3>

<p>
این متد آیتم که مصداق شرط تعیین شده نباشند را بر می گرداند و برعکس متد filter  عمل می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4]);

$filtered = $collection->reject(function ($value, $key) {
    return $value > 2;
});

$filtered->all();

// [1, 2]
</code></pre>


<p><a name="method-reverse"></a></p>
<br>
<h3>متد reverse</h3>
<p>
این متد ترتیب عناصر مجموعه را با حفظ کلید آن معکوس می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['a', 'b', 'c', 'd', 'e']);

$reversed = $collection->reverse();

$reversed->all();

/*
    [
        4 => 'e',
        3 => 'd',
        2 => 'c',
        1 => 'b',
        0 => 'a',
    ]
*/
</code></pre>


<p><a name="method-search"></a></p>
<br>
<h3>متد search</h3>
<p>
مجموعه ای را برای مقدار داده شده جستجو می کند و کلید آن را در صورت وجود بر می گرداند. اگر مورد یافت نشد، مقدار false  را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([2, 4, 6, 8]);

$collection->search(4);

// 1
</code></pre>

<p>
در حالت پیش فرض از روش "loose" جهت مقایسه و جستجو استفاده می شود برای جستجو با استفاده از روش "strict مقدار true  را به عنوان آرگومان دوم ارسال می کنیم :

</p>

<pre><code class="language-php  line-numbers">$collection->search('4', true);

// false
</code></pre>

<p>
همچنین می توانید از callback  در این متد استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection->search(function ($item, $key) {
    return $item > 5;
});

// 2
</code></pre>


<p><a name="method-shift"></a></p>
<br>
<h3>متد shift</h3>
<p>
عملیات shift  به راست را در مجموعه انجام می دهد و اولین عنصر را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->shift();

// 1

$collection->all();

// [2, 3, 4, 5]
</code></pre>


<p><a name="method-shuffle"></a></p>
<br>
<h3>متد shuffle</h3>
<p>
عناصر موجود در مجموعه را بصورت تصادفی جا به جا می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$shuffled = $collection->shuffle();

$shuffled->all();

// [3, 2, 5, 1, 4] - (generated randomly)
</code></pre>


<p><a name="method-slice"></a></p>
<br>
<h3>متد slice</h3>
<p>
تکه ای از مجموعه را با شروع از index معین شده بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);

$slice = $collection->slice(4);

$slice->all();

// [5, 6, 7, 8, 9, 10]
</code></pre>


<p>
چنانچه بخواهیم تعداد نیز مشخص شود آن را به عنوان آرگومان دوم در متد مشخص می نماییم :
</p>

<pre><code class="language-php  line-numbers">$slice = $collection->slice(4, 2);

$slice->all();

// [5, 6]
</code></pre>

<p>
تکه بازگشتی به طور پیش فرض کلید را حفظ می کند. اگر نمی خواهید کلیدهای اصلی را حفظ کنید، میتوانید از متد values برای reindex استفاده کنید :
</p>

<p><a name="method-sort"></a></p>
<br>
<h3>متد sort</h3>
<p>
از این متد جهت مرتب سازی مجموعه استفاده می شود. مجموعه مرتب شده،  کلیدهای آرایه را حفظ می کند، بنابراین در این مثال از متد values برای reindex کلیدها استفاده کردیم :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([5, 3, 1, 2, 4]);

$sorted = $collection->sort();

$sorted->values()->all();

// [1, 2, 3, 4, 5]
</code></pre>


<p><a name="method-sortby"></a></p>
<br>
<h3>متد sortBy</h3>
<p>
از این متد جهت مرتب سازی بر اساس کلید معین شده استفاده می گردد :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'Desk', 'price' => 200],
    ['name' => 'Chair', 'price' => 100],
    ['name' => 'Bookcase', 'price' => 150],
]);

$sorted = $collection->sortBy('price');

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'price' => 100],
        ['name' => 'Bookcase', 'price' => 150],
        ['name' => 'Desk', 'price' => 200],
    ]
*/
</code></pre>


<p>
همچنین می توان از روش callback در این متد استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$sorted = $collection->sortBy(function ($product, $key) {
    return count($product['colors']);
});

$sorted->values()->all();

/*
    [
        ['name' => 'Chair', 'colors' => ['Black']],
        ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
        ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
    ]
*/
</code></pre>


<p><a name="method-sortbydesc"></a></p>
<br>
<h3>متد sortByDesc</h3>
<p>
همانند متد sort می باشد با این تفاوت که بصورت نزولی مرتب می کند :
</p>

<p><a name="method-splice"></a></p>
<br>
<h3>متد splice</h3>
<p>
این متد تکه ای از مجموعه را با شروع از index  مشخص  حذف و آن را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2);

$chunk->all();

// [3, 4, 5]

$collection->all();

// [1, 2]
</code></pre>

<p>
همچنین می توانید تعداد آیتم ها را در آرگومان دوم مشخص نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 4, 5]
</code></pre>

<p>
در ادامه می توان در آرگومان سوم آیتم هایی را به عنوان جایگزین مشخص نمود :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$chunk = $collection->splice(2, 1, [10, 11]);

$chunk->all();

// [3]

$collection->all();

// [1, 2, 10, 11, 4, 5]
</code></pre>


<p><a name="method-split"></a></p>
<br>
<h3>متد split</h3>
<p>
از این متد جهت گروه بندی کردن مجموعه به تعداد مشخص استفاده می شود :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$groups = $collection->split(3);

$groups->toArray();

// [[1, 2], [3, 4], [5]]
</code></pre>


<p><a name="method-sum"></a></p>
<br>
<h3>متد sum</h3>
<p>
این متد مقادیر آیتم ها را جمع و باز می گرداند :
</p>

<pre><code class="language-php  line-numbers">collect([1, 2, 3, 4, 5])->sum();

// 15
</code></pre>

<p>
همچنین می توانید کلید معینی جهت جمع کردن مقادیر آیتم ها مشخص نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'JavaScript: The Good Parts', 'pages' => 176],
    ['name' => 'JavaScript: The Definitive Guide', 'pages' => 1096],
]);

$collection->sum('pages');

// 1272
</code></pre>


<p>
همچنین می توانید از روش callback در این متد استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'Chair', 'colors' => ['Black']],
    ['name' => 'Desk', 'colors' => ['Black', 'Mahogany']],
    ['name' => 'Bookcase', 'colors' => ['Red', 'Beige', 'Brown']],
]);

$collection->sum(function ($product) {
    return count($product['colors']);
});

// 6
</code></pre>


<p><a name="method-take"></a></p>
<br>
<h3>متد take</h3>
<p>
یک مجموعه جدید با تعداد مشخص آیتم را برمی گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(3);

$chunk->all();

// [0, 1, 2]
</code></pre>


<p>
همچنین می توانید از یک عدد منفی به عنوان آرگومان استفاده نمایید که در اینصورت از انتهای مجموعه آیتم ها برگردانده می شوند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([0, 1, 2, 3, 4, 5]);

$chunk = $collection->take(-2);

$chunk->all();

// [4, 5]
</code></pre>


<p><a name="method-tap"></a></p>
<br>
<h3>متد tap</h3>
<p>The <span class="redbold">tap</span> method passes the collection to the given callback, allowing you to "tap" into the collection at a specific point and do something with the items while not affecting the collection itself:</p>


<pre><code class="language-php  line-numbers">collect([2, 4, 3, 1, 5])
    ->sort()
    ->tap(function ($collection) {
        Log::debug('Values after sorting', $collection->values()->toArray());
    })
    ->shift();

// 1
</code></pre>


<p><a name="method-times"></a></p>
<br>
<h3>متد times</h3>
<p>
این متد تعداد دفعات اجرای متد callback  تعیین شده را تعیین می کند و مقدار آن را در هر بار اجرا بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = Collection::times(10, function ($number) {
    return $number * 9;
});

$collection->all();

// [9, 18, 27, 36, 45, 54, 63, 72, 81, 90]
</code></pre>

<p>
این متد می تواند در Eloquent models مانند زیر مفید باشد :
</p>

<pre><code class="language-php  line-numbers">$categories = Collection::times(3, function ($number) {
    return factory(Category::class)->create(['name' => 'Category #'.$number]);
});

$categories->all();

/*
    [
        ['id' => 1, 'name' => 'Category #1'],
        ['id' => 2, 'name' => 'Category #2'],
        ['id' => 3, 'name' => 'Category #3'],
    ]
*/
</code></pre>


<p><a name="method-toarray"></a></p>
<br>
<h3>متد toArray</h3>
<p>
عناصر موجود در مجموعه را بصورت آرایه بر می گرداند:
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toArray();

/*
    [
        ['name' => 'Desk', 'price' => 200],
    ]
*/
</code></pre>


<p><a name="method-tojson"></a></p>
<br>
<h3>متد toJson</h3>
<p>
عناصر موجود در مجموعه را بصورت JSON  بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect(['name' => 'Desk', 'price' => 200]);

$collection->toJson();

// '{"name":"Desk", "price":200}'
</code></pre>


<p><a name="method-transform"></a></p>
<br>
<h3>متد transform</h3>
<p>
این متد مقادیر تغییر یافته بعد از عملیات صورت گرفته بر روی عناصر مجموعه در  callback را جایگزین عناصر اصلی مجموعه می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3, 4, 5]);

$collection->transform(function ($item, $key) {
    return $item * 2;
});

$collection->all();

// [2, 4, 6, 8, 10]
</code></pre>

<p>
اگر بخواهیم بر روی مجموعه اصلی تغییری حاصل نشود از متد map  استفاده نمایید.
</p>

<p><a name="method-union"></a></p>
<br>
<h3>متد union</h3>
<p>
این متد آرایه داده شده را به مجموعه اضافه می کند. اگر آرایه داده شده شامل کلید هایی است که قبلا در مجموعه اصلی هستند، مقادیر اصلی مجموعه ترجیح داده می شود:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1 => ['a'], 2 => ['b']]);

$union = $collection->union([3 => ['c'], 1 => ['b']]);

$union->all();

// [1 => ['a'], 2 => ['b'], 3 => ['c']]
</code></pre>


<p><a name="method-unique"></a></p>
<br>
<h3>متد unique</h3>
<p>
این متد تمام اقلام منحصر به فرد در مجموعه را باز می گرداند. مجموعه بازگشتی،  کلیدهای آرایه را نگه می دارد، بنابراین در این مثال از متد values برای بازنشانی کلید استفاده می کنیم:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 1, 2, 2, 3, 4, 2]);

$unique = $collection->unique();

$unique->values()->all();

// [1, 2, 3, 4]
</code></pre>

<p>
هنگام استفاده با آرایه های تودرتو و یا اشیاء ، می توانید کلید مورد استفاده برای تعیین منحصر به فرد بودن را تعیین کنید:
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'iPhone 5', 'brand' => 'Apple', 'type' => 'phone'],
    ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
    ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
]);

$unique = $collection->unique('brand');

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
    ]
*/
</code></pre>

<p>
همچنین می توانید از callback  در این متد استفاده نمایید :
</p>

<pre><code class="language-php  line-numbers">$unique = $collection->unique(function ($item) {
    return $item['brand'].$item['type'];
});

$unique->values()->all();

/*
    [
        ['name' => 'iPhone 6', 'brand' => 'Apple', 'type' => 'phone'],
        ['name' => 'Apple Watch', 'brand' => 'Apple', 'type' => 'watch'],
        ['name' => 'Galaxy S6', 'brand' => 'Samsung', 'type' => 'phone'],
        ['name' => 'Galaxy Gear', 'brand' => 'Samsung', 'type' => 'watch'],
    ]
*/
</code></pre>

<p><a name="method-uniquestrict"></a></p>
<br>
<h3>متد uniqueStrict</h3>
<p>
همانند متد unique  عمل می کند با این تفاوت که از روش  "strict"  در مقایسه های خود استفاده می کند.
</p>


<p><a name="method-unless"></a></p>
<br>
<h3>متد unless</h3>
<p>
این متد callback  مشخص شده را اجرا می کند مگر اینگه آرگومان اول true  باشد.
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3]);

$collection->unless(true, function ($collection) {
    return $collection->push(4);
});

$collection->unless(false, function ($collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 5]
</code></pre>

<p>
این متد عکس متد when  عمل می کند.
</p>


<p><a name="method-unwrap"></a></p>
<br>
<h3>متد unwrap</h3>
<p>The static <span class="redbold">unwrap</span> method returns the collection's underlying items from the given value when applicable:</p>


<pre><code class="language-php  line-numbers">Collection::unwrap(collect('John Doe'));

// ['John Doe']

Collection::unwrap(['John Doe']);

// ['John Doe']

Collection::unwrap('John Doe');

// 'John Doe'
</code></pre>


<p><a name="method-values"></a></p>
<br>
<h3>متد values</h3>
<p>
این متد کلیدها را بصورت اعداد صحیح متوالی باز نشانی می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    10 => ['product' => 'Desk', 'price' => 200],
    11 => ['product' => 'Desk', 'price' => 200]
]);

$values = $collection->values();

$values->all();

/*
    [
        0 => ['product' => 'Desk', 'price' => 200],
        1 => ['product' => 'Desk', 'price' => 200],
    ]
*/
</code></pre>


<p><a name="method-when"></a></p>
<br>
<h3>متد when</h3>
<p>
این متد callback  مورد نظر را زمانیکه آرگومان اول برابر true  باشد را اجرا می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([1, 2, 3]);

$collection->when(true, function ($collection) {
    return $collection->push(4);
});

$collection->when(false, function ($collection) {
    return $collection->push(5);
});

$collection->all();

// [1, 2, 3, 4]
</code></pre>

<p>
این متد عکس متد unless  عمل می کند.
</p>

<p><a name="method-where"></a></p>
<br>
<h3>متد where</h3>
<p>
این متد عملیات filter  را براساس key /value  اعمال می کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->where('price', 100);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
</code></pre>



<p><a name="method-wherestrict"></a></p>
<br>
<h3>متد whereStrict</h3>
<p>
این متد همانند متد where  بوده با این تفاوت که از روش مقایسه "strict" استفاده می کند :
</p>


<p><a name="method-wherein"></a></p>
<br>
<h3>متد whereIn</h3>
<p>
این متد همانند متد where  بوده با این تفاوت که مقادیر را می تواند بصورت آرایه دریافت کند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Bookcase', 'price' => 150],
        ['product' => 'Desk', 'price' => 200],
    ]
*/
</code></pre>



<p><a name="method-whereinstrict"></a></p>
<br>
<h3>متد whereInStrict</h3>
<p>
این متد همانند متد where  بوده با این تفاوت که از روش مقایسه "strict" استفاده می کند :
</p>


<p><a name="method-wherenotin"></a></p>
<br>
<h3>متد whereNotIn</h3>
<p>
این متد عکس متد where  عمل می کند و مقادیری که در آرایه تعیین شده نباشند را بر می گرداند :
</p>

<pre><code class="language-php  line-numbers">$collection = collect([
    ['product' => 'Desk', 'price' => 200],
    ['product' => 'Chair', 'price' => 100],
    ['product' => 'Bookcase', 'price' => 150],
    ['product' => 'Door', 'price' => 100],
]);

$filtered = $collection->whereNotIn('price', [150, 200]);

$filtered->all();

/*
    [
        ['product' => 'Chair', 'price' => 100],
        ['product' => 'Door', 'price' => 100],
    ]
*/
</code></pre>



<p><a name="method-wherenotinstrict"></a></p>
<br>
<h3>متد whereNotInStrict</h3>
<p>
این متد همانند متد whereNotIn  بوده با این تفاوت که از روش مقایسه "strict" استفاده می کند :
</p>


<p><a name="method-wrap"></a></p>
<br>
<h3>متد wrap</h3>
<p>The static <span class="redbold">wrap</span> method wraps the given value in a collection when applicable:</p>


<pre><code class="language-php  line-numbers">$collection = Collection::wrap('John Doe');

$collection->all();

// ['John Doe']

$collection = Collection::wrap(['John Doe']);

$collection->all();

// ['John Doe']

$collection = Collection::wrap(collect('John Doe'));

$collection->all();

// ['John Doe']
</code></pre>


<p><a name="method-zip"></a></p>
<br>
<h3>متد zip</h3>
<p>
متد zip() مقادیر آرایه ی ورودی را با مقادیر موجود در مجموعه در اندیس منطبق (که دارای شماره ی مکان قرارگیری یکسان است) ترکیب می کند:
</p>


<pre><code class="language-php  line-numbers">$collection = collect(['Chair', 'Desk']);

$zipped = $collection->zip([100, 200]);

$zipped->all();

// [['Chair', 100], ['Desk', 200]]
</code></pre>

