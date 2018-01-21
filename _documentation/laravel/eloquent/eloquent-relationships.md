---
layout: documentation
title:   eloquent relationships  آموزش
cattitle: اصول آموزش Laravel
date:   2018-01-05 19:47:42 +0330
jdate: جمعه 15 دی 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/eloquent-relationships <br> www.tahlildadeh.com/ArticleDetails/آموزش-رابطه-ها-در-Eloquent
---

<p>
جداول پایگاه داده معمولا به هم مرتبط هستند. برای مثال یک پست در وبلاگ می تواند تعداد زیادی دیدگاه (مرتبط) داشته باشد یا سفارشی با کاربری که آن را داده رابطه داشته باشد. Eloquent مدیریت و کار با این رابطه ها را به مراتب آسان می سازد. Eloquent از رابطه های زیر پشتیبانی می کند:
</p>

<ul>
<li><a href="#one-to-one">رابطه ی یک به یک (one to one)</a></li>
<li><a href="#one-to-many">رابطه ی یک به چند (one to many)</a></li>
<li><a href="#many-to-many">رابطه ی چند به چند (many to many)</a></li>
<li><a href="#has-many-through">رابطه ی چند به چند با واسطه (Has Many Through)</a></li>
<li><a href="#polymorphic-relations">رابطه های polymorphic</a></li>
<li><a href="#many-to-many-polymorphic-relations">رابطه های polymorphic چند به چند</a></li>
</ul>

<br>
<h3>تعریف رابطه ها (relationships)</h3>
<p>
در Eloquent، رابطه ها به صورت توابعی داخل کلاس های مدل تعریف می شوند. از آنجایی که رابطه ها نیز همچون مدل های Eloquent، خود به عنوان یک query builder ایفای نقش می کنند، ویژگی تعریف رابطه ها به صورت توابع این امکان را فراهم می کند تا متدها را به صورت زنجیره ای صدا زده و قابلیت های پرس و جوی بیشتری را در کوئری گرفتن های خود لحاظ کنید. مثال:
</p>

<pre><code class="language-php  line-numbers">$user->posts()->where('active', 1)->get();
</code></pre>


<p>
<a name="one-to-one"></a>
</p>

<br>
<h3>رابطه ی یک به یک One To One</h3>
<p>
رابطه ی یک به یک ساده ترین نوع relationship است. به عنوان مثال یک مدل User ممکن است با تنها یک phone رابطه داشته باشد. برای تعریف این رابطه، یک متد phone در مدل User قرار می دهیم. متد phone بایستی خروجی متد hasOne را در کلاس base مدل Eloquentبرگرداند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Get the phone record associated with the user.
     */
    public function phone()
    {
        return $this->hasOne('App\Phone');
    }
}
</code></pre>

<p>
اولین آرگومان ارسالی به متد hasOne اسم مدل مربوطه هست. پس از اینکه رابطه تعریف شد، می توان رکورد مربوطه را با استفاده از امکانproperty های داینامیک Eloquent بازیابی کرد. property های داینامیک به شما اجازه می دهند به رابطه ها (که به صورت توابع تعریف می شوند) مانند property های تعریف شده در مدل دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">$phone = User::find(1)->phone;
</code></pre>

<p>
Eloquent اسم کلید خارجی (foreign key) رابطه را بر اساس اسم مدل مربوطه درنظر می گیرد. در این مثال، مدل Phone (با توجه به اسم مدل) باید دارای کلید خارجی به نام user_id باشد. برای شکستن و بازنویسی این قرارداد، می توانید اسم دلخواه برای کلید خارجی را به عنوان آرگومان دوم به متد پاس دهید:
</p>

<pre><code class="language-php  line-numbers">return $this->hasOne('App\Phone', 'foreign_key');
</code></pre>

<p>
علاوه بر آن Eloquent انتظار دارد کلید خارجی مقداری منطبق با مقدار ستون id (یا متغیر سفارشی primaryKey$) جدول پدر داشته باشد. به عبارت بهتر، Eloquent به دنبال مقدار ستون id کاربر در ستون user_id رکورد Phone می گردد . اگر می خواهید رابطه از مقداری غیر از id به عنوان کلید خارجی استفاده کند، بایستی یک آرگومان سوم به متد hasOne ارسال کرده و از طریق آن کلید سفارشی خود را مشخص کنید:
</p>

<pre><code class="language-php  line-numbers">return $this->hasOne('App\Phone', 'foreign_key', 'local_key');
</code></pre>


<br>
<h3>تعریف عکس رابطه (طرف دیگر رابطه)</h3>
<p>
حال می توان از طریق مدل User خود به Phone دسترسی داشت. در این بخش رابطه ای در مدل Phone تعریف می کنیم که به ما اجازه ی دسترسی به User ای که مالک یا مدل پدر phone هست را می دهد. طرف دیگر رابطه ی hasOne را با استفاده از متد belongsTo تعریف می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Phone extends Model
{
    /**
     * Get the user that owns the phone.
     */
    public function user()
    {
        return $this->belongsTo('App\User');
    }
}
</code></pre>

<p>
در نمونه ی بالا، Eloquent ستون user_id از مدل Phone را به id در مدل User متصل (match می کند). Eloquent اسم پیش فرض کلید خارجی را با در نظر گرفتن اسم متد (رابطه) و الصاق پسوند id_ تعریف می کند. حال اگر می خواهید کلید خارجی در مدل Phone اسمی غیر ازuser_id داشته باشد، می توانید اسم دلخواه کلید خارجی را به عنوان آرگومان دوم ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the user that owns the phone.
 */
public function user()
{
    return $this->belongsTo('App\User', 'foreign_key');
}
</code></pre>

<p>
بنابراین :
</p>

<pre><code class="language-php  line-numbers"> $user = Phone::where('phone', '0911*******')->first()->user;
</code></pre>

<p>
اگر مدل پدر از id به عنوان کلید اصلی استفاده نمی کند، یا می خواهید مدل فرزند را به ستون دیگری وصل کنید، در آن صورت می توانید کلید سفارشی جدول پدر (اسم ستون کلید اصلی در جدول پدر) را به عنوان آرگومان سوم به متد belongsTo ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the user that owns the phone.
 */
public function user()
{
    return $this->belongsTo('App\User', 'foreign_key', 'other_key');
}
</code></pre>

<p>
<a name="default-models"></a>
</p>

<br>
<h3>Default Models</h3>
<p>
The <code class=" language-php">belongsTo</code> relationship allows you to define a default model that will be returned if the given relationship is <code class=" language-php"><span class="token keyword">null</span></code>. This pattern is often referred to as the <a href="https://en.wikipedia.org/wiki/Null_Object_pattern">Null Object pattern</a> and can help remove conditional checks in your code. In the following example, the <code class=" language-php">user</code> relation will return an empty <code class=" language-php">App\<span class="token package">User</span></code> model if no <code class=" language-php">user</code> is attached to the post:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the author of the post.
 */
public function user()
{
    return $this->belongsTo('App\User')->withDefault();
}
</code></pre>

<p>
To populate the default model with attributes, you may pass an array or Closure to the <code class=" language-php">withDefault</code> method:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the author of the post.
 */
public function user()
{
    return $this->belongsTo('App\User')->withDefault([
        'name' => 'Guest Author',
    ]);
}

/**
 * Get the author of the post.
 */
public function user()
{
    return $this->belongsTo('App\User')->withDefault(function ($user) {
        $user->name = 'Guest Author';
    });
}
</code></pre>

<p>
<a name="one-to-many"></a>
</p>

<br>
<h3>رابطه ی یک به چند (one to many)</h3>
<p>
relationship یک به چند برای تعریف رابطه ای بکار می رود که در آن یک مدل مالک چندین مدل دیگر است. برای مثال، یک پست در وبلاگ می تواندn تا دیدگاه داشته باشد. برای تعریف این نوع رابطه نیز یک متد در مدل Eloquent تعریف می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * Get the comments for the blog post.
     */
    public function comments()
    {
        return $this->hasMany('App\Comment');
    }
}
</code></pre>

<p>
یادآور می شویم که Eloquent خود ستون کلید خارجی را در مدل Comment تعیین می کند. بر اساس قراردادهای از پیش تعیین شده،Eloquent فرم snake case اسم مدل مالک (post) را انتخاب کرده و صرفا یک پسوند id_ به آن اضافه می کند. در این مثال Eloquent فرض را بر این می گذارد که کلید خارجی در مدل Comment ستونی به نام post_id هست.
</p>
<p>
پس از تعریف رابطه ی مورد نیاز، می توان با دسترسی به پراپرتی comments به مجموعه دیدگاه های پست مربوطه دسترسی داشت. همان طور که پیش تر گفته شد، Eloquent از ویژگی dynamic properties پشتیبانی می کند. بنابراین می توان به رابطه ها (relationship function) همانند property های تعریف شده در مدل دسترسی داشت:
</p>

<pre><code class="language-php  line-numbers">$comments = App\Post::find(1)->comments;

foreach ($comments as $comment) {
    //
}
</code></pre>

<p>
و با توجه به اینکه رابطه ها نقش query builder را نیز ایفا می کنند، می توانید دیدگاه هایی که در خروجی واکشی می شوند را با اعمال constraintها محدود نمایید. برای این منظور متد comments را صدا زده و شرط ها را به صورت زنجیره ای به کوئری الحاق کنید:
</p>

<pre><code class="language-php  line-numbers">$comments = App\Post::find(1)->comments()->where('title', 'foo')->first();
</code></pre>

<p>
برای تعیین اسم کلید خارجی دلخواه و کلید محلی که در رابطه استفاده می شود می توانید آن ها را به ترتیب به عنوان آرگومان های دوم و سوم به متد ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">return $this->hasMany('App\Comment', 'foreign_key');

return $this->hasMany('App\Comment', 'foreign_key', 'local_key');
</code></pre>

<p>
<a name="one-to-many-inverse"></a>
</p>

<br>
<h3>تعریف طرف دیگر رابطه  One To Many Inverse</h3>
<p>
پس از تعریف رابطه ی لازم برای دسترسی به تمامی دیدگاه های پست، رابطه ای تعریف می کنیم که اجازه ی دسترسی دیدگاه به پست مربوطه (پست پدر) را فراهم می کند. برای تعریف طرف دیگر رابطه ی hasMany، یک تابع relationship در مدل فرزند اعلان می کنیم که متد belongsTo را صدا می زند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * Get the post that owns the comment.
     */
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
</code></pre>

<p>
پس از تعریف رابطه، می توان با دسترسی به property های داینامیک post مدل Post مربوط به یک Comment را واکشی نمود:
</p>

<pre><code class="language-php  line-numbers">$comment = App\Comment::find(1);

echo $comment->post->title;
</code></pre>

<p>
در مثال بالا، Eloquent ستون post_id از مدل Comment را به ستون id در مدل Post مچ می کند. همان طور که قبلا هم به آن اشاره شد،Eloquent اسم کلید خارجی را با درنظر گرفتن اسم رابطه و افزودن پسوند id_ به اسم متد تعیین می کند. حال اگر اسم ستون کلید خارجی در مدلComment، post_id نباشد در آن صورت یک اسم سفارشی به عنوان آرگومان دوم به متد belongsTo پاس می دهیم:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the post that owns the comment.
 */
public function post()
{
    return $this->belongsTo('App\Post', 'foreign_key');
}
</code></pre>

<p>
اگر اسم ستون کلید اصلی در جدول پدر id نباشد یا شما می خواهید مدل فرزند را به ستون دیگری (غیر از id) وصل کنید، در آن صورت می توانید اسم ستونی که کلید اصلی در جدول پدر هست را به عنوان آرگومان سوم به متد belongsTo ارسال نمایید:
</p>

<pre><code class="language-php  line-numbers">/**
 * Get the post that owns the comment.
 */
public function post()
{
    return $this->belongsTo('App\Post', 'foreign_key', 'other_key');
}
</code></pre>

<p>
<a name="many-to-many"></a>
</p>

<br>
<h3>رابطه ی چند به چند (many to many)</h3>
<p>
relation های چند به چند کمی پیچیده تر از رابطه های hasOne و hasMany هستند. به عنوان مثال می توان به رابطه ای اشاره کرد که در آن یک کاربر می تواند چندین نقش داشته باشد و نقش ها هم می توانند به کاربرهای متعددی داده شوند.
</p>
<p>
برای مثال کاربران متعددی می توانند نقش Admin را داشته باشند. برای تعریف این رابطه به سه جدول نیاز داریم: 1. users 2. roles 3.role_user. اسم جدولrole_user بر اساس ترتیب حروف الفبا از کنار هم قرار گرفتن نام دو مدل مرتبط گرفته شده است. این جدول حاوی دو ستون به نام های user_id و role_id می باشد.
</p>

<p>
برای تعریف رابطه ی چند به چند یک متد تعریف می کنیم که خود متد belongsToMany را در کلاس پایه Eloquent صدا می زند. در مثال زیر متدroles را در مدل User فراخوانی می کنیم:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * The roles that belong to the user.
     */
    public function roles()
    {
        return $this->belongsToMany('App\Role');
    }
}
</code></pre>

<p>
پس از تعریف رابطه، کافی است از طریق property داینامیک roles به نقش های کاربر دسترسی پیدا کنید:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

foreach ($user->roles as $role) {
    //
}
</code></pre>

<p>
می توان متد roles را فراخوانی نموده و محدودیت هایی را جهت واکشی نتایج مد نظر در ادامه ی آن متد به صورت زنجیره ای به کوئری الحاق کرد:
</p>

<pre><code class="language-php  line-numbers">$roles = App\User::find(1)->roles()->orderBy('name')->get();
</code></pre>

<p>
همان طور که قبلا گفته شد، اسم جدول واسط با کنار هم قرار گرفتن اسم مدل های مرتبط به ترتیب حروف الفبا مشخص می شود. می توانید اسم دلخواه خود را برای جدول واسط تعریف کنید. برای این منظور بایستی آن را به عنوان آرگومان دوم به متد belongsToMany ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Role', 'role_user');
</code></pre>

<p>
همچنین می توانید اسم ستون های کلید خارجی جدول را توسط آرگومان سوم و چهارم سفارشی تنظیم نمایید. آرگومان سوم اسم کلید خارجی مدلی است که رابطه را در آن تعریف می کنید و آرگومان چهارم اسم کلید خارجی مدلی است که به آن وصل می شوید:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Role', 'role_user', 'user_id', 'role_id');
</code></pre>


<br>
<h3>تعریف طرف دیگر رابطه Defining The Inverse Of The Relationship</h3>
<p>
برای تعریف طرف مقابل رابطه، متد belongsToMany را در مدل مربوطه صدا می زنیم. در ادامه ی مثال قبلی، یک متد users در مدل Roleتعریف می کنیم که متد belongsToMany را صدا می زند:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Role extends Model
{
    /**
     * The users that belong to the role.
     */
    public function users()
    {
        return $this->belongsToMany('App\User');
    }
}
</code></pre>

<p>
همان طور که می بینید رابطه ی مقابل درست مانند همتای User آن تعریف شده است. با این تفاوت که در آن به مدل App\User ارجاع می دهیم. از آن جایی که متد belongsToMany را مجددا در این رابطه استفاده می کنیم، اسم جدول و کلیدهای خارجی که به صورت سفارشی تعریف کرده بودیم در زمان تعریف طرف دیگر رابطه ی چند به چند قابل استفاده خواهند بود.
</p>

<br>
<h3>بازیابی ستون های جدول واسط Retrieving Intermediate Table Columns</h3>
<p>
همان طور که پیش تر گفته شد، کار با رابطه های چند به چند لازمه ی وجود جدول واسط است. Eloquent روش های آسان و بهینه ای برای کار با این جدول فراهم می کند. برای مثال، فرض بگیرید شی User تعداد زیادی شی Role مرتبط دارد. پس از دسترسی به این رابطه، می توان با بهره گیری از اتریبیوت pivot در مدل ها به راحتی به جدول واسط دسترسی داشت:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

foreach ($user->roles as $role) {
    echo $role->pivot->created_at;
}
</code></pre>

<p>
همان طور که می بینید به ازای هر مدل Role که واکشی می کنیم، یک اتریبیوت pivot تخصیص می یابد. این attribute حاوی یک مدل است که بیانگر جدول واسط می باشد و می توان از آن مانند هر مدل دیگری استفاده کرد.
</p>
<p>
به صورت پیش فرض، شی pivot فقط کلیدهای مدل را شامل می شود. چنانچه قرار است جدول pivot دارای attribute هایی ورای کلید های مدل باشد، بایستی آن ها را در زمان تعریف رابطه مشخص نمایید:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Role')->withPivot('column1', 'column2');
</code></pre>

<p>
اگر می خواهید جدول pivot به صورت خودکار ستون های created_at و updated_at را داشته و مدیریت کند، بایستی متد withTimestampsرا در تعریف رابطه بکار ببرید:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Role')->withTimestamps();
</code></pre>


<br>
<h3>Customizing The <code class=" language-php">pivot</code> Attribute Name</h3>
<p>
As noted earlier, attributes from the intermediate table may be accessed on models using the <code class=" language-php">pivot</code> attribute. However, you are free to customize the name of this attribute to better reflect its purpose within your application.
</p>
<p>
For example, if your application contains users that may subscribe to podcasts, you probably have a many-to-many relationship between users and podcasts. If this is the case, you may wish to rename your intermediate table accessor to <code class=" language-php">subscription</code> instead of <code class=" language-php">pivot</code>. This can be done using the <code class=" language-php"><span class="token keyword">as</span></code> method when defining the relationship:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Podcast')
                ->as('subscription')
                ->withTimestamps();
</code></pre>

<p>
Once this is done, you may access the intermediate table data using the customized name:
</p>

<pre><code class="language-php  line-numbers">$users = User::with('podcasts')->get();

foreach ($users->flatMap->podcasts as $podcast) {
    echo $podcast->subscription->created_at;
}
</code></pre>


<br>
<h3> فیلتر کردن رابطه ها از طریق جداول واسط Filtering Relationships Via Intermediate Table Columns</h3>
<p>
می توانید نتایج واکشی شده توسط belongsToMany را با استفاده از متدهای wherePivot و wherePivotIn در تعریف رابطه فیلتر و محدود نمایید:
</p>

<pre><code class="language-php  line-numbers">return $this->belongsToMany('App\Role')->wherePivot('approved', 1);

return $this->belongsToMany('App\Role')->wherePivotIn('priority', [1, 2]);
</code></pre>


<br>
<h3>Defining Custom Intermediate Table Models</h3>
<p>
If you would like to define a custom model to represent the intermediate table of your relationship, you may call the <code class=" language-php">using</code> method when defining the relationship. All custom models used to represent intermediate tables of relationships must extend the <code class=" language-php">Illuminate\<span class="token package">Database<span class="token punctuation">\</span>Eloquent<span class="token punctuation">\</span>Relations<span class="token punctuation">\</span>Pivot</span></code> class. For example, we may define a <code class=" language-php">Role</code> which uses a custom <code class=" language-php">UserRole</code> pivot model:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Role extends Model
{
    /**
     * The users that belong to the role.
     */
    public function users()
    {
        return $this->belongsToMany('App\User')->using('App\UserRole');
    }
}
</code></pre>

<p>
When defining the <code class=" language-php">UserRole</code> model, we will extend the <code class=" language-php">Pivot</code> class:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Relations\Pivot;

class UserRole extends Pivot
{
    //
}
</code></pre>

<p>
<a name="has-many-through"></a>
</p>

<br>
<h3>Has Many Through</h3>
<p>
The "has-many-through" relationship provides a convenient shortcut for accessing distant relations via an intermediate relation. For example, a <code class=" language-php">Country</code> model might have many <code class=" language-php">Post</code> models through an intermediate <code class=" language-php">User</code> model. In this example, you could easily gather all blog posts for a given country. Let's look at the tables required to define this relationship:
</p>

<pre><code class="language-php  line-numbers">countries
    id - integer
    name - string

users
    id - integer
    country_id - integer
    name - string

posts
    id - integer
    user_id - integer
    title - string
</code></pre>

<p>
Though <code class=" language-php">posts</code> does not contain a <code class=" language-php">country_id</code> column, the <code class=" language-php">hasManyThrough</code> relation provides access to a country's posts via <code class=" language-php"><span class="token variable">$country</span><span class="token operator">-</span><span class="token operator">&gt;</span><span class="token property">posts</span></code>. To perform this query, Eloquent inspects the <code class=" language-php">country_id</code> on the intermediate <code class=" language-php">users</code> table. After finding the matching user IDs, they are used to query the <code class=" language-php">posts</code> table.
</p>
<p>
Now that we have examined the table structure for the relationship, let's define it on the <code class=" language-php">Country</code> model:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Country extends Model
{
    /**
     * Get all of the posts for the country.
     */
    public function posts()
    {
        return $this->hasManyThrough('App\Post', 'App\User');
    }
}
</code></pre>

<p>
The first argument passed to the <code class=" language-php">hasManyThrough</code> method is the name of the final model we wish to access, while the second argument is the name of the intermediate model.
</p>
<p>
Typical Eloquent foreign key conventions will be used when performing the relationship's queries. If you would like to customize the keys of the relationship, you may pass them as the third and fourth arguments to the <code class=" language-php">hasManyThrough</code> method. The third argument is the name of the foreign key on the intermediate model. The fourth argument is the name of the foreign key on the final model. The fifth argument is the local key, while the sixth argument is the local key of the intermediate model:
</p>

<pre><code class="language-php  line-numbers">class Country extends Model
{
    public function posts()
    {
        return $this->hasManyThrough(
            'App\Post',
            'App\User',
            'country_id', // Foreign key on users table...
            'user_id', // Foreign key on posts table...
            'id', // Local key on countries table...
            'id' // Local key on users table...
        );
    }
}
</code></pre>

<p>
<a name="polymorphic-relations"></a>
</p>

<br>
<h3>رابطه های polymorphic</h3>

<h3>ساختار جداول مورد نیاز رابطه ی پلی مرفیک</h3>
<p>
رابطه های Polymorphic به یک مدل اجازه می دهند در یک رابطه همزمان به چندین مدل دیگر تعلق داشته باشد (امکان رابطه ی یک مدل با چندین مدل دیگر از طریق یک رابطه را فراهم می آورد). به عنوان مثال، فرض کنید کاربران اپلیکیشن شما می خواهند برای پست ها و هم برای ویدئو ها، کامنت بگذارند. با بهره گیری از رابطه های polymorphic می توان به راحتی یک جدول واحد comments برای هر دو این سناریو بکار برد. در گام نخست ساختار جداولی که برای این رابطه لازم است را مورد بررسی قرار می دهیم:
</p>

<pre><code class="language-php  line-numbers">posts
    id - integer
    title - string
    body - text

videos
    id - integer
    title - string
    url - string

comments
    id - integer
    body - text
    commentable_id - integer
    commentable_type - string
</code></pre>

<p>
دو ستون مورد توجه در این مثال، ستون های commentable_id و commentable_type در جدول comments هستند. ستون commentable_id حاوی مقدار ID پست یا ویدئو خواهد بود، در حالی که ستون commentable_type اسم کلاس مدل مالک (پدر) را نگه خواهد داشت. Eloquent در واقع به واسطه ی ستون commentable_type تشخیص می دهد در زمان دسترسی به رابطه ی commentable کدام مدل را بایستی برگرداند.
</p>

<br>
<h3>ساختار مدل Model Structure</h3>
<p>
در مرحله ی بعدی، تعریف مدل لازم برای ایجاد این رابطه را مورد بررسی قرار می دهیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * Get all of the owning commentable models.
     */
    public function commentable()
    {
        return $this->morphTo();
    }
}

class Post extends Model
{
    /**
     * Get all of the post's comments.
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}

class Video extends Model
{
    /**
     * Get all of the video's comments.
     */
    public function comments()
    {
        return $this->morphMany('App\Comment', 'commentable');
    }
}
</code></pre>


<br>
<h3>بازیابی رابطه های polymorphic (Retrieving Polymorphic Relations)</h3>
<p>
پس از تعریف جداول و مدل های مورد نیاز، می توانید از طریق مدل ها به رابطه ها دسترسی داشته باشید.برای مثال، جهت دسترسی به کامنت های های یک پست، می توان از property داینامیک comments استفاده کرد:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

foreach ($post->comments as $comment) {
    //
}
</code></pre>

<p>
می توان مالک یک رابطه ی polymorphic را از مدل polymorphic بازیابی کرد. برای این منظور بایستی به اسم متدی که متد morphTo را صدا می زند، دسترسی داشته داشت. در مثال ما، این همان متد commentable در مدل Like هست. حال کافی است به آن متد همانند یک dynamic property دسترسی داشته باشید:
</p>

<pre><code class="language-php  line-numbers">$comment = App\Comment::find(1);

$commentable = $comment->commentable;
</code></pre>

<p>
فراخوانی (متد) رابطه ی commentable  در مدل Comment ، بسته به مدلی که مالک آن comment می باشد یک نمونه از Post یا Video  را در خروجی برمی گرداند.
</p>

<br>
<h3>نوع های اختصاصی polymorphic</h3>
<p>
به صورت پیش فرض، Laravel اسم کامل کلاس را برای ذخیره ی نوع (اسم) مدل مربوطه بکار می برد. به عنوان نمونه، در ادامه ی مثال فوق که یک comment ممکن بود به یک Post یا Video تعلق داشته باشد، commentable_type پیش فرضApp\Post یا App\Video خواهد بود.
</p>

<p>
شما می توانید این قرار داد را شکسته و پایگاه داده را از ساختار داخلی اپلیکیشن جدا نمایید. این کار را با تعریف یک relationship "morph map" انجام می دهیم. morph map به Eloquent اعلان می کند که بجای اسم کلاس، اسم جدول متناظر با آن مدل را بکار ببرد.
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Database\Eloquent\Relations\Relation;

Relation::morphMap([
    'posts' => 'App\Post',
    'videos' => 'App\Video',
]);
</code></pre>

<p>
می توانید morphMap را در تابع boot از AppServiceProvider معرفی کنید یا در صورت تمایل یک service provider مجزا تعریف کنید.
</p>
<p>
<a name="many-to-many-polymorphic-relations"></a>
</p>

<br>
<h3>رابطه های polymorphic چند به چند (Many To Many Polymorphic Relations)</h3>

<h3>Table Structure</h3>
<p>
علاوه بر رابطه های polymorphic متعارف، می توانید رابطه های polymorphic چند به چند تعریف کنید. برای مثال، یک مدل Post و Video در یک وبلاگ می توانند یک رابطه ی polymorphic با مدل Tag داشته باشند. استفاده از رابطه ی polymorphic چند به چند این امکان را برای شما فراهم می کند تا یک لیست واحد از تگ های منحصر بفرد داشته باشید که تمامی پست ها و ویدئوهای وبلاگ از آن ها استفاده می کنند. در گام نسخت ساختار جدول را مورد بررسی قرار دهیم:
</p>

<pre><code class="language-php  line-numbers">posts
    id - integer
    name - string

videos
    id - integer
    name - string

tags
    id - integer
    name - string

taggables
    tag_id - integer
    taggable_id - integer
    taggable_type - string
</code></pre>


<br>
<h3>Model Structure</h3>
<p>
اکنون زمان آن فرا رسیده تا رابطه ها را در مدل تعریف کنیم. مدل های Post و Video هر دو متد tags را خواهند داشت. متد tags متدmorphToMany را در کلاس پایه ی Eloquent فراخوانی می کند
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    /**
     * Get all of the tags for the post.
     */
    public function tags()
    {
        return $this->morphToMany('App\Tag', 'taggable');
    }
}
</code></pre>


<br>
<h3>تعریف طرف دیگر رابطه Defining The Inverse Of The Relationship</h3>
<p>
سپس در مدل Tag می بایست یک متد برای هر یک از مدل های مربوطه تعریف نمایید. در این مثال، یک متد Posts و یک videos تعریف می کنیم:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Tag extends Model
{
    /**
     * Get all of the posts that are assigned this tag.
     */
    public function posts()
    {
        return $this->morphedByMany('App\Post', 'taggable');
    }

    /**
     * Get all of the videos that are assigned this tag.
     */
    public function videos()
    {
        return $this->morphedByMany('App\Video', 'taggable');
    }
}
</code></pre>


<br>
<h3>بازیابی رابطه (Retrieving The Relationship)</h3>
<p>
پس از تعریف جداول و مدل های مربوطه، می توان از طریق رابطه ها به مدل ها دسترسی داشت. برای مثال، جهت دسترسی به تمامی تگ های یک پست، کافی است property داینامیک tags را بکار ببرید:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

foreach ($post->tags as $tag) {
    //
}
</code></pre>

<p>
می توانید مالک یک رابطه ی polymorphic را از مدل polymorphic بازیابی کنید. کافی است به اسم متدی که تابع morphedByMany را صدا می زند، دسترسی داشته باشید. در این مثال منظور دو متد posts یا videos در مدل Tag هست. بنابراین می بایست به متدهای نام برده همانندproperty های داینامیک دسترسی پیدا کنید:
</p>

<pre><code class="language-php  line-numbers">$tag = App\Tag::find(1);

foreach ($tag->videos as $video) {
    //
}
</code></pre>

<p>
<a name="querying-relations"></a>
</p>

<br>
<h3><a href="#querying-relations">پرس و جو و گرفتن کوئری از رابطه ها (Querying Relations)</a></h3>
<p>
همان طور که می دانید رابطه های Eloquent به صورت توابعی تعریف می شوند. شما می توانید با فراخوانی این توابع بدون اینکه لازم باشد خود کوئری relationship را واقعا اجرا کنید، نمونه ای از آن رابطه را دریافت نمایید. بعلاوه همان طور که قبلا نیز گفته شد، تمامی رابطه های Eloquentخود مانند query builder عمل می کنند و به شما اجازه می دهند شرط ها و قیودی را برای محدود نمودن نتیجه به صورت زنجیره ای به کوئریrelationship (پیش از اجرای نهایی کوئری بر روی پایگاه داده) اعمال کنید:
</p>
<p>
به عنوان مثال، یک سیستم وبلاگ را در نظر بگیرید که در آن مدل User تعداد زیادی مدل Post مرتبط دارد:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    /**
     * Get all of the posts for the user.
     */
    public function posts()
    {
        return $this->hasMany('App\Post');
    }
}
</code></pre>

<p>
می توانید از رابطه ی posts کوئری گرفته و محدودیت های بیشتری را مانند زیر به رابطه اعمال کنید:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->posts()->where('active', 1)->get();
</code></pre>

<p>
می توانید تمامی متدهای query builder را بر روی رابطه بکار ببرید.
</p>
<p>
<a name="relationship-methods-vs-dynamic-properties"></a>
</p>

<br>
<h3>متدهای relationship در برابر property های Dynamic</h3>
<p>
اگر نیازی به اعمال قیود (constraint) اضافی بر سازمان بر روی کوئری relationship نیست، می توانید به راحتی به آن رابطه مانند یک propertyدسترسی داشته باشید. در ادامه ی مثال قبلی (مدل های User و Post)، می توان به تمامی پست های کاربر مانند زیر دسترسی داشت:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

foreach ($user->posts as $post) {
    //
}
</code></pre>

<p>
property های dynamic بار گذاری را با تاخیر انجام می دهند (lazy loading). بدین معنی که داده های relationship خود را تنها زمانی بارگذاری می کنند که شما به آن ها دسترسی پیدا می کنید. به همین خاطر توسعه دهندگان اغلب از eager loading استفاده کرده و بدین وسیله رابطه هایی که می دانند پس از بارگذاری مدل مورد دسترسی قرار می گیرند را از قبل لود می کنند. eager loading تعداد کوئری های SQL که باید برای بارگذاری رابطه های یک مدل اجرا شوند را به طور قابل توجهی کاهش می دهد.
</p>
<p>
<a name="querying-relationship-existence"></a>
</p>

<br>
<h3>محدود کردن نتایج یک کوئری بر اساس وجود یا عدم وجود یک رابطه Querying Relationship Existence</h3>
<p>
در زمان دسترسی به رکورد های یک مدل، می توانید نتایج را بر اساس وجود یک رابطه محدود نمایید. برای مثال، فرض کنید می خواهید تمامی پست های وبلاگ که دارای حداقل یک دیدگاه یا comment مرتبط هستند را واکشی نمایید. برای این منظور، اسم رابطه را به عنوان آرگومان به متد hasپاس دهید:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have at least one comment...
$posts = App\Post::has('comments')->get();
</code></pre>

<p>
می توانید یک عملگر به همراه تعداد دیدگاه ها (تعداد comment هایی که پست ها باید داشته باشد تا در خروجی لحاظ شوند) برای تنظیم بیشتر کوئری مطابق نیاز تعریف کنید:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have three or more comments...
$posts = Post::has('comments', '>=', 3)->get();
</code></pre>

<p>
با استفاده از عملگر نقطه می توانید دستورات has تودرتو بسازید. در نمونه ی زیر تمامی پست هایی که حداقل یک comment و vote دارند را واکشی می کنیم:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have at least one comment with votes...
$posts = Post::has('comments.votes')->get();
</code></pre>

<p>
اگر می خواهید نتایج دقیق تری را در خروجی داشته باشید، می توانید متدهای whereHas و orWhereHas را برای اعمال شرط های where بر روی کوئری های has فراخوانی کنید. این متدها به شما امکان می دهند قیود اختصاصی (constraint) خود را به constraint های رابطه اعمال کنید، مانند بررسی محتویات یک دیدگاه (comment). در زیر تمامی پست هایی که حداقل یک دیدگاه مرتبط دارند و علاوه بر آن دربردارنده ی کلماتی همچون foo می باشد را استخراج می کنیم:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts with at least one comment containing words like foo%
$posts = Post::whereHas('comments', function ($query) {
    $query->where('content', 'like', 'foo%');
})->get();
</code></pre>

<p>
<a name="querying-relationship-absence"></a>
</p>

<br>
<h3>Querying Relationship Absence</h3>
<p>
When accessing the records for a model, you may wish to limit your results based on the absence of a relationship. For example, imagine you want to retrieve all blog posts that <strong>don't</strong> have any comments. To do so, you may pass the name of the relationship to the <code class=" language-php">doesntHave</code> and <code class=" language-php">orDoesntHave</code> methods:
</p>

<pre><code class="language-php  line-numbers">$posts = App\Post::doesntHave('comments')->get();
</code></pre>

<p>
If you need even more power, you may use the <code class=" language-php">whereDoesntHave</code> and <code class=" language-php">orWhereDoesntHave</code> methods to put "where" conditions on your <code class=" language-php">doesntHave</code> queries. These methods allows you to add customized constraints to a relationship constraint, such as checking the content of a comment:
</p>

<pre><code class="language-php  line-numbers">$posts = Post::whereDoesntHave('comments', function ($query) {
    $query->where('content', 'like', 'foo%');
})->get();
</code></pre>

<p>
<a name="counting-related-models"></a>
</p>

<br>
<h3>Counting Related Models</h3>
<p>
If you want to count the number of results from a relationship without actually loading them you may use the <code class=" language-php">withCount</code> method, which will place a <code class=" language-php"><span class="token punctuation">{</span>relation<span class="token punctuation">}</span>_count</code> column on your resulting models. For example:
</p>

<pre><code class="language-php  line-numbers">$posts = App\Post::withCount('comments')->get();

foreach ($posts as $post) {
    echo $post->comments_count;
}
</code></pre>

<p>
You may add the "counts" for multiple relations as well as add constraints to the queries:
</p>

<pre><code class="language-php  line-numbers">$posts = Post::withCount(['votes', 'comments' => function ($query) {
    $query->where('content', 'like', 'foo%');
}])->get();

echo $posts[0]->votes_count;
echo $posts[0]->comments_count;
</code></pre>

<p>
You may also alias the relationship count result, allowing multiple counts on the same relationship:
</p>

<pre><code class="language-php  line-numbers">$posts = Post::withCount([
    'comments',
    'comments as pending_comments_count' => function ($query) {
        $query->where('approved', false);
    }
])->get();

echo $posts[0]->comments_count;

echo $posts[0]->pending_comments_count;
</code></pre>

<p>
<a name="eager-loading"></a>
</p>

<br>
<h3><a href="#eager-loading">Eager loading (بارگذاری زود هنگام)</a></h3>
<p>
زمانی که به (توابع) رابطه های Eloquent مانند property دسترسی پیدا می کنید، داده های رابطه در واقع با تاخیر بارگذاری می شوند (به اصطلاحlazy load می شوند). بدین معنی که داده های رابطه تنها زمانی به معنای واقعی لود می شوند که شما (برای اولین بار) به property دسترسی پیدا کنید. Eloquent می تواند رابطه ها را در زمانی که از مدل پدر کوئری می گیرید، به صورت زود هنگام بارگذاری (eager load) کند. Eager loading در حقیقت برای برطرف ساختن مشکل کوئری N + 1 در نظر گرفته شد. برای فهم بهتر این مشکل یک مدل Book را در نظر بگیرید که با مدل Author مرتبط است:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Book extends Model
{
    /**
     * Get the author that wrote the book.
     */
    public function author()
    {
        return $this->belongsTo('App\Author');
    }
}
</code></pre>

<p>
حال تمامی کتاب ها و نویسندنگان مرتبط آن ها بازیابی می کنیم:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::all();

foreach ($books as $book) {
    echo $book->author->name;
}
</code></pre>

<p>
این حلقه یک کوئری برای واکشی تمامی کتاب های داخل جدول اجرا کرده سپس یک کوئری دیگر به ازای هر کتاب تا نویسنده ی مربوطه ی آن نیز واکشی شود. بنابراین اگر 25 کتاب داشته باشیم، حلقه ی مزبور در کل 26 کوئری اجرا می کند: 1 کوئری برای کتاب اصلی و 25 کوئری جهت استخراج نویسندگان مربوط به هر کتاب.
</p>
<p>
با بهره گیری از امکان eager loading می توان این عملیات و تعداد کوئری را به تنها دو کوئری کاهش داد. به هنگام کوئری گرفتن، می توان مشخص کرد کدام رابطه ها بایستی زود هنگام بارگذاری شوند. این کار با فراخوانی متد with امکان پذیر خواهد بود:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::with('author')->get();

foreach ($books as $book) {
    echo $book->author->name;
}
</code></pre>

<p>
برای این عملیات دو کوئری بیشتر اجرا نمی شود:
</p>

<pre><code class="language-php  line-numbers">select * from books

select * from authors where id in (1, 2, 3, 4, 5, ...)
</code></pre>


<br>
<h3>بارگذاری زود هنگام چندین رابطه Eager Loading Multiple Relationships</h3>
<p>
گاهی لازم می شود چندین رابطه ی مختلف را در طی تنها یک عملیات به صورت زود هنگام بارگذاری کنید. برای نیل به این هدف کافی است رابطه ها را به عنوان آرگومان به متد with ارسال نمایید:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::with(['author', 'publisher'])->get();
</code></pre>


<br>
<h3>بارگذاری زود هنگام رابطه های تودرتو Nested Eager Loading</h3>
<p>
برای بارگذاری زود هنگام رابطه های تودرتو کافی است عملگر نقطه را بکار ببرید. در نمونه ی زیر تمامی نویسندگان کتاب و نیز کلیه ی تماس های شخصی نویسنده را در قالب یک دستور Eloquent به صورت زودهنگام بارگذاری می کنیم:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::with('author.contacts')->get();
</code></pre>


<br>
<h3>Eager Loading Specific Columns</h3>
<p>
You may not always need every column from the relationships you are retrieving. For this reason, Eloquent allows you to specify which columns of the relationship you would like to retrieve:
</p>

<pre><code class="language-php  line-numbers">$users = App\Book::with('author:id,name')->get();
</code></pre>

<blockquote class="has-icon note">
<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="90px" height="90px" viewBox="0 0 90 90" enable-background="new 0 0 90 90" xml:space="preserve"><path fill="#FFFFFF" d="M45 0C20.1 0 0 20.1 0 45s20.1 45 45 45 45-20.1 45-45S69.9 0 45 0zM45 74.5c-3.6 0-6.5-2.9-6.5-6.5s2.9-6.5 6.5-6.5 6.5 2.9 6.5 6.5S48.6 74.5 45 74.5zM52.1 23.9l-2.5 29.6c0 2.5-2.1 4.6-4.6 4.6 -2.5 0-4.6-2.1-4.6-4.6l-2.5-29.6c-0.1-0.4-0.1-0.7-0.1-1.1 0-4 3.2-7.2 7.2-7.2 4 0 7.2 3.2 7.2 7.2C52.2 23.1 52.2 23.5 52.1 23.9z"></path></svg></span></div> When using this feature, you should always include the <code class=" language-php">id</code> column in the list of columns you wish to retrieve.
</p>
</blockquote>
<p>
<a name="constraining-eager-loads"></a>
</p>

<br>
<h3>اعمال محدودیت بر روی بارگذاری زود هنگام Constraining Eager Loads</h3>
<p>
گاهی لازم می شود رابطه ای را به صورت زود هنگام بارگذاری کرده و این میان محدودیت هایی نیز را بر روی کوئری اعمال کنید. مثال:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::with(['posts' => function ($query) {
    $query->where('title', 'like', '%first%');
}])->get();
</code></pre>

<p>
در این مثال، Eloquent تنها پست هایی را بارگذاری می کند که عنوان (مقدار ستون title) آن ها دربردارنده ی کلمه ی first باشد. البته می توانید دیگر متدهای query builder را برای تنظیم دقیق تر عملیات eager loading بکار ببرید:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::with(['posts' => function ($query) {
    $query->orderBy('created_at', 'desc');
}])->get();
</code></pre>

<p>
<a name="lazy-eager-loading"></a>
</p>

<br>
<h3>Lazy Eager Loading</h3>
<p>
Sometimes you may need to eager load a relationship after the parent model has already been retrieved. For example, this may be useful if you need to dynamically decide whether to load related models:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::all();

if ($someCondition) {
    $books->load('author', 'publisher');
}
</code></pre>

<p>
If you need to set additional query constraints on the eager loading query, you may pass an array keyed by the relationships you wish to load. The array values should be <code class=" language-php">Closure</code> instances which receive the query instance:
</p>

<pre><code class="language-php  line-numbers">$books->load(['author' => function ($query) {
    $query->orderBy('published_date', 'asc');
}]);
</code></pre>

<p>
To load a relationship only when it has not already been loaded, use the <code class=" language-php">loadMissing</code> method:
</p>

<pre><code class="language-php  line-numbers">public function format(Book $book)
{
    $book->loadMissing('author');

    return [
        'name' => $book->name,
        'author' => $book->author->name
    ];
}
</code></pre>

<p>
<a name="inserting-and-updating-related-models"></a>
</p>

<br>
<h3><a href="#inserting-and-updating-related-models">Inserting &amp; Updating Related Models</a></h3>
<p>
<a name="the-save-method"></a>
</p>

<br>
<h3>متد save</h3>
<p>
Eloquent متدهای فراوانی برای افزودن مدل های جدید به رابطه ها فراهم می کند. برای مثال، ممکن است لازم شود یک مدل جدید Commentبرای مدل Post درج کنید. بجای اینکه attribute(ستون) post-id را در مدل Comment به صورت دستی مقداردهی کنید، می توانید این مدل را به صورت مستقیم از متد save رابطه وارد نمایید:
</p>

<pre><code class="language-php  line-numbers">$comment = new App\Comment(['message' => 'A new comment.']);

$post = App\Post::find(1);

$post->comments()->save($comment);
</code></pre>

<p>
همان طور که در مثال بالا می بینید به رابطه ی comments به صورت یک dynamic property دسترسی پیدا نکردیم. بلکه صرفا متدcomments را برای بدست آوردن نمونه ای از رابطه فراخوانی نمودیم. متد save خود به صورت اتوماتیک مقدار post_id را به مدل جدیدComment اضافه می کند (فیلد مزبور را در مدل Comment مقداردهی می کند).
</p>
<p>
برای ذخیره ی چندین مدل مرتبط، کافی است متد saveMany را بکار ببرید:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

$post->comments()->saveMany([
    new App\Comment(['message' => 'A new comment.']),
    new App\Comment(['message' => 'Another comment.']),
]);
</code></pre>

<p>
<a name="the-create-method"></a>
</p>

<br>
<h3>متد Create</h3>
<p>
علاوه بر متدهای save و saveMany، می توانید متد create را فراخوانی کنید. این متد آرایه ای از attribute ها را به عنوان آرگومان دریافت کرده، یک مدل جدید ایجاد می کند و سپس آن مدل را داخل پایگاه داده درج می نماید. تفاوت بین دو متد save و create در این است که save یک نمونه ی کامل از مدل Eloquent را به عنوان آرگومان می پذیرد، در حالی که create صرفا یک آرایه ی ساده و متعارف PHP را به عنوان پارامتر ورودی دریافت می کند:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

$comment = $post->comments()->create([
    'message' => 'A new comment.',
]);
</code></pre>

<blockquote class="has-icon tip">
<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> Before using the <code class=" language-php">create</code> method, be sure to review the documentation on attribute <a href="/docs/5.5/eloquent#mass-assignment">mass assignment</a>.
</p>
</blockquote>
<p>
You may use the <code class=" language-php">createMany</code> method to create multiple related models:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

$post->comments()->createMany([
    [
        'message' => 'A new comment.',
    ],
    [
        'message' => 'Another new comment.',
    ],
]);
</code></pre>

<p>
<a name="updating-belongs-to-relationships"></a>
</p>

<br>
<h3>بروز رسانی رابطه های "Belongs To" (مرتبط کردن مدل ها با متد associate)</h3>
<p>
به هنگام بروز رسانی رابطه ی belongsTo، می توانید متد associate را فراخوانی کنید. این متد کلید خارجی (foreign key) را در مدل فرزند مقداردهی می کند:
</p>

<pre><code class="language-php  line-numbers">$account = App\Account::find(10);

$user->account()->associate($account);

$user->save();
</code></pre>

<p>
و در زمان حذف یک رابطه ی belongsTo متد dissociate را بکار ببرید. این متد علاوه بر بازگردانی کلید خارجی، رابطه ی موجود در مدل فرزند راreset می کند:
</p>

<pre><code class="language-php  line-numbers">$user->account()->dissociate();

$user->save();
</code></pre>

<p>
<a name="updating-many-to-many-relationships"></a>
</p>

<br>
<h3>Many To Many Relationships</h3>

<br>
<h3>درج (attach) / حذف (detach)  در رابطه های چند به چند</h3>
<p>
به هنگام کار با رابطه های چند به چند، Eloquent تعداد زیادی متد کمکی (helper method) ارائه می کند که کار با مدل های مرتبط را به مراتب آسان ساخته. سناریویی را در نظر بگیرید که در آن یک کاربر می تواند نقش های متعددی داشته باشد و یک نقش نیز در مقابل به کاربران زیادی تعلق داشته باشد. برای اینکه بتوان یک نقش جدید را با درج یک رکورد در جدول واسطه (که مدل ها را به هم وصل می کند) به کاربر مورد نظر اضافه کرد، بایستی متد attach را بکار برد (می توان با درج یک رکورد در جدول واسطه که مدل ها را به هم پیوند می دهد، یک نقش جدید به کاربر مورد نظر اضافه کرد. برای این منظور کافی است متد attach را فراخوانی کنید):
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->roles()->attach($roleId);
</code></pre>

<p>
به هنگام اضافه کردن یک رابطه جدید به مدل، همچنین می توانید آرایه ای از داده های جدید برای درج در جدول واسطه به متد attach ارسال کنید:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->attach($roleId, ['expires' => $expires]);
</code></pre>

<p>
گاهی لازم می شود یک نقش را از کاربری حذف کنیم. برای حذف یک رکورد در رابطه ی چند به چند، متد detach را بکار می بریم. این متد رکورد مربوطه را از جدول واسطه حذف می کند. دقت داشته باشید که در پی حذف رکورد، دو مدل در پایگاه داده باقی مانده و حذف نمی شوند:
</p>

<pre><code class="language-php  line-numbers">// Detach a single role from the user...
$user->roles()->detach($roleId);

// Detach all roles from the user...
$user->roles()->detach();
</code></pre>

<p>
هر دو متدهای attach و detach آرایه ای از شناسه ها را به عنوان ورودی می گیرند:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->roles()->detach([1, 2, 3]);

$user->roles()->attach([
    1 => ['expires' => $expires],
    2 => ['expires' => $expires]
]);
</code></pre>


<br>
<h3>Syncing Associations</h3>
<p>
You may also use the <code class=" language-php">sync</code> method to construct many-to-many associations. The <code class=" language-php">sync</code> method accepts an array of IDs to place on the intermediate table. Any IDs that are not in the given array will be removed from the intermediate table. So, after this operation is complete, only the IDs in the given array will exist in the intermediate table:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->sync([1, 2, 3]);
</code></pre>

<p>
You may also pass additional intermediate table values with the IDs:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->sync([1 => ['expires' => true], 2, 3]);
</code></pre>

<p>
If you do not want to detach existing IDs, you may use the <code class=" language-php">syncWithoutDetaching</code> method:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->syncWithoutDetaching([1, 2, 3]);
</code></pre>


<br>
<h3>Toggling Associations</h3>
<p>
همچنین می توانید از متد sync برای ساخت رابطه های چند به چند استفاده نمایید. این متد آرایه ای از شناسه ها (ID) را به عنوان آرگومان دریافت کرده و در جدول واسطه قرار می دهد. هر ID ای که در آرایه ی ورودی موجود نباشد، از جدول واسطه حذف خواهد شد. پس از اتمام این عملیات، تنهاID های حاضر در آرایه ی ورودی داخل جدول واسطه موجود خواهند بود:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->toggle([1, 2, 3]);
</code></pre>


<br>
<h3>Saving Additional Data On A Pivot Table</h3>
<p>
When working with a many-to-many relationship, the <code class=" language-php">save</code> method accepts an array of additional intermediate table attributes as its second argument:
</p>

<pre><code class="language-php  line-numbers">App\User::find(1)->roles()->save($role, ['expires' => $expires]);
</code></pre>


<br>
<h3>بروز رسانی یک رکورد در جدول Pivot</h3>
<p>
برای بروز رسانی سطر موجود در جدول pivot، متد updateExistingPivot را بکار ببرید:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->roles()->updateExistingPivot($roleId, $attributes);
</code></pre>

<p>
<a name="touching-parent-timestamps"></a>
</p>

<br>
<h3><a href="#touching-parent-timestamps">بروز رسانی Timestamp مدل پدر</a></h3>
<p>
زمانی که مدلی به یک (belongsTo) یا چند (belongToMany) مدل دیگر تعلق دارد، مانند مدل Comment که به یک مدل Post تعلق دارد، بروز رسانی timestamp مدل پدر پس از بروز رسانی مدل فرزند می تواند مفید واقع شود. برای مثال زمانی که یک مدل Comment بروز رسانی می شود، ممکن است بخواهید تایم استمپ updated_at مدل مالک (Post) نیز بروز آوری شود. Eloquent این کار را برای شما آسان کرده است. کافی است پراپرتی touches که با اسم رابطه ها مقداردهی شده را به مدل فرزند اضافه نمایید:
</p>

<pre><code class="language-php  line-numbers"><?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Comment extends Model
{
    /**
     * All of the relationships to be touched.
     *
     * @var array
     */
    protected $touches = ['post'];

    /**
     * Get the post that the comment belongs to.
     */
    public function post()
    {
        return $this->belongsTo('App\Post');
    }
}
</code></pre>

<p>
حال زمانی که شما یک Comment را بروز رسانی می کنید، مقدار ستون های updated_at در مدل پدر مدل پدر (Post) نیز بروز آوری می شود:
</p>

<pre><code class="language-php  line-numbers">$comment = App\Comment::find(1);

$comment->text = 'Edit to this comment!';

$comment->save();
</code></pre>