---
layout: documentation
title:   آشنایی با eloquent collections
cattitle: اصول آموزش Laravel
date:   2018-01-19 21:40:42 +0330
jdate: جمعه 29 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/eloquent-collections <br> www.tahlildadeh.com/ArticleDetails/آموزش-collection-در-Laravel
---
<p>
تمامی مجموعه های چند نتیجه ای که در خروجی کوئری های Eloquent دریافت می کنیم، در واقع نمونه ای از شیIlluminate\Database\Eloquent\Collection هستند. نتایج واکشی شده توسط متد get یا رابطه ها (relationship method) نیز از این قاعده مستثنی نیستند. شی نام برده کلاس پایه ی (base) collection لاراول را به ارث می برد. بنابراین تمامی توابعی که برای کار با Eloquentاستفاده می شوند نیز به ارث برده می شوند.
</p>
<p>
می توان در تمامی collection ها مانند آرایه حلقه زد (collection ها قابل iterate هستند).
</p>

<pre><code class="language-php  line-numbers">$users = App\User::where('active', 1)->get();

foreach ($users as $user) {
    echo $user->name;
}
</code></pre>

<p>
البته collection ها بسیار قدرتمندتر از آرایه ها هستند و می توان از آن ها برای عملیات نگاشت / سازماندهی و کاهش (map/reduce) بهره گرفت. این عملیات را می توان از طریق یک رابط (intuitive interface) مانند زنجیر به هم متصل کرد. در نمونه ی زیر تمامی مدل های غیرفعال را حذف کرده و اسم تمامی کاربران باقی مانده را بیرون می کشیم:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::all();

$names = $users->reject(function ($user) {
    return $user->active === false;
})
->map(function ($user) {
    return $user->name;
});
</code></pre>

<blockquote class="has-icon note">
بیشتر توابع collection در Eloquent یک نمونه ی جدید از collection در خروجی برمی گردانند، این درحالی است که متدهایpluck، keys، zip، collapse، flatten و flip یک نمونه از collection پایه را به عنوان نتیجه برمی گردانند.
</blockquote>
<p>
<a name="available-methods"></a>
</p>

<br>
<h3>کلاس پایه ی collection</h3>
<p>
تمامی collection های Eloquent شی پایه ی collection لاراول را به ارث می برند. از این رو، تمامی متدهای قدرتمند این کلاس نیز در اختیار کالکشن ها قرار می گیرد:
</p>

<div id="collection-method-list">
<p><a target="_blank" href="/documentation/laravel/collections#method-all">all</a>
<a target="_blank" href="/documentation/laravel/collections#method-average">average</a>
<a target="_blank" href="/documentation/laravel/collections#method-avg">avg</a>
<a target="_blank" href="/documentation/laravel/collections#method-chunk">chunk</a>
<a target="_blank" href="/documentation/laravel/collections#method-collapse">collapse</a>
<a target="_blank" href="/documentation/laravel/collections#method-combine">combine</a>
<a target="_blank" href="/documentation/laravel/collections#method-concat">concat</a>
<a target="_blank" href="/documentation/laravel/collections#method-contains">contains</a>
<a target="_blank" href="/documentation/laravel/collections#method-containsstrict">containsStrict</a>
<a target="_blank" href="/documentation/laravel/collections#method-count">count</a>
<a target="_blank" href="/documentation/laravel/collections#method-crossjoin">crossJoin</a>
<a target="_blank" href="/documentation/laravel/collections#method-dd">dd</a>
<a target="_blank" href="/documentation/laravel/collections#method-diff">diff</a>
<a target="_blank" href="/documentation/laravel/collections#method-diffkeys">diffKeys</a>
<a target="_blank" href="/documentation/laravel/collections#method-dump">dump</a>
<a target="_blank" href="/documentation/laravel/collections#method-each">each</a>
<a target="_blank" href="/documentation/laravel/collections#method-eachspread">eachSpread</a>
<a target="_blank" href="/documentation/laravel/collections#method-every">every</a>
<a target="_blank" href="/documentation/laravel/collections#method-except">except</a>
<a target="_blank" href="/documentation/laravel/collections#method-filter">filter</a>
<a target="_blank" href="/documentation/laravel/collections#method-first">first</a>
<a target="_blank" href="/documentation/laravel/collections#method-flatmap">flatMap</a>
<a target="_blank" href="/documentation/laravel/collections#method-flatten">flatten</a>
<a target="_blank" href="/documentation/laravel/collections#method-flip">flip</a>
<a target="_blank" href="/documentation/laravel/collections#method-forget">forget</a>
<a target="_blank" href="/documentation/laravel/collections#method-forpage">forPage</a>
<a target="_blank" href="/documentation/laravel/collections#method-get">get</a>
<a target="_blank" href="/documentation/laravel/collections#method-groupby">groupBy</a>
<a target="_blank" href="/documentation/laravel/collections#method-has">has</a>
<a target="_blank" href="/documentation/laravel/collections#method-implode">implode</a>
<a target="_blank" href="/documentation/laravel/collections#method-intersect">intersect</a>
<a target="_blank" href="/documentation/laravel/collections#method-isempty">isEmpty</a>
<a target="_blank" href="/documentation/laravel/collections#method-isnotempty">isNotEmpty</a>
<a target="_blank" href="/documentation/laravel/collections#method-keyby">keyBy</a>
<a target="_blank" href="/documentation/laravel/collections#method-keys">keys</a>
<a target="_blank" href="/documentation/laravel/collections#method-last">last</a>
<a target="_blank" href="/documentation/laravel/collections#method-map">map</a>
<a target="_blank" href="/documentation/laravel/collections#method-mapinto">mapInto</a>
<a target="_blank" href="/documentation/laravel/collections#method-mapspread">mapSpread</a>
<a target="_blank" href="/documentation/laravel/collections#method-maptogroups">mapToGroups</a>
<a target="_blank" href="/documentation/laravel/collections#method-mapwithkeys">mapWithKeys</a>
<a target="_blank" href="/documentation/laravel/collections#method-max">max</a>
<a target="_blank" href="/documentation/laravel/collections#method-median">median</a>
<a target="_blank" href="/documentation/laravel/collections#method-merge">merge</a>
<a target="_blank" href="/documentation/laravel/collections#method-min">min</a>
<a target="_blank" href="/documentation/laravel/collections#method-mode">mode</a>
<a target="_blank" href="/documentation/laravel/collections#method-nth">nth</a>
<a target="_blank" href="/documentation/laravel/collections#method-only">only</a>
<a target="_blank" href="/documentation/laravel/collections#method-pad">pad</a>
<a target="_blank" href="/documentation/laravel/collections#method-partition">partition</a>
<a target="_blank" href="/documentation/laravel/collections#method-pipe">pipe</a>
<a target="_blank" href="/documentation/laravel/collections#method-pluck">pluck</a>
<a target="_blank" href="/documentation/laravel/collections#method-pop">pop</a>
<a target="_blank" href="/documentation/laravel/collections#method-prepend">prepend</a>
<a target="_blank" href="/documentation/laravel/collections#method-pull">pull</a>
<a target="_blank" href="/documentation/laravel/collections#method-push">push</a>
<a target="_blank" href="/documentation/laravel/collections#method-put">put</a>
<a target="_blank" href="/documentation/laravel/collections#method-random">random</a>
<a target="_blank" href="/documentation/laravel/collections#method-reduce">reduce</a>
<a target="_blank" href="/documentation/laravel/collections#method-reject">reject</a>
<a target="_blank" href="/documentation/laravel/collections#method-reverse">reverse</a>
<a target="_blank" href="/documentation/laravel/collections#method-search">search</a>
<a target="_blank" href="/documentation/laravel/collections#method-shift">shift</a>
<a target="_blank" href="/documentation/laravel/collections#method-shuffle">shuffle</a>
<a target="_blank" href="/documentation/laravel/collections#method-slice">slice</a>
<a target="_blank" href="/documentation/laravel/collections#method-sort">sort</a>
<a target="_blank" href="/documentation/laravel/collections#method-sortby">sortBy</a>
<a target="_blank" href="/documentation/laravel/collections#method-sortbydesc">sortByDesc</a>
<a target="_blank" href="/documentation/laravel/collections#method-splice">splice</a>
<a target="_blank" href="/documentation/laravel/collections#method-split">split</a>
<a target="_blank" href="/documentation/laravel/collections#method-sum">sum</a>
<a target="_blank" href="/documentation/laravel/collections#method-take">take</a>
<a target="_blank" href="/documentation/laravel/collections#method-tap">tap</a>
<a target="_blank" href="/documentation/laravel/collections#method-toarray">toArray</a>
<a target="_blank" href="/documentation/laravel/collections#method-tojson">toJson</a>
<a target="_blank" href="/documentation/laravel/collections#method-transform">transform</a>
<a target="_blank" href="/documentation/laravel/collections#method-union">union</a>
<a target="_blank" href="/documentation/laravel/collections#method-unique">unique</a>
<a target="_blank" href="/documentation/laravel/collections#method-uniquestrict">uniqueStrict</a>
<a target="_blank" href="/documentation/laravel/collections#method-unless">unless</a>
<a target="_blank" href="/documentation/laravel/collections#method-values">values</a>
<a target="_blank" href="/documentation/laravel/collections#method-when">when</a>
<a target="_blank" href="/documentation/laravel/collections#method-where">where</a>
<a target="_blank" href="/documentation/laravel/collections#method-wherestrict">whereStrict</a>
<a target="_blank" href="/documentation/laravel/collections#method-wherein">whereIn</a>
<a target="_blank" href="/documentation/laravel/collections#method-whereinstrict">whereInStrict</a>
<a target="_blank" href="/documentation/laravel/collections#method-wherenotin">whereNotIn</a>
<a target="_blank" href="/documentation/laravel/collections#method-wherenotinstrict">whereNotInStrict</a>
<a target="_blank" href="/documentation/laravel/collections#method-zip">zip</a>
</p>
</div>

<p>
<a name="custom-collections"></a>
</p>

<br>
<h3><a target="_blank" href="#custom-collections">ایجاد collection های اختصاصی</a></h3>
<p>
چنانچه لازم است از یک شی Collection سفارشی به همراه متدهای الحاقی (extension method) خود استفاده کنید، در آن صورت می توانید متدnewCollection را در مدل بازنویسی (override) نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use App\CustomCollection;
use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Create a new Eloquent Collection instance.
     *
     * @param  array  $models
     * @return \Illuminate\Database\Eloquent\Collection
     */
    public function newCollection(array $models = [])
    {
        return new CustomCollection($models);
    }
}
</code></pre>

<p>
پس از اینکه متد newCollection را تعریف کردید، هر بار که Eloquent یک نمونه Collection از مدل مربوطه را برمی گرداند به همراش نمونه ای از collection سفارشی خود را در خروجی دریافت می کنید. اگر می خواهید به ازای هر مدل در اپلیکیشن خود یک collection سفارشی استفاده کنید، در آن صورت می بایست متد newCollection را بر روی کلاس مدل پایه که توسط تمامی مدل های برنامه به ارث برده می شود، بازنویسی نمایید.
</p>