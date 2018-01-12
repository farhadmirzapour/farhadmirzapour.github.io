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
refrence: https://laravel.com/docs/5.5/collections <br> www.tahlildadeh.com/ArticleDetails/آموزش-ابزار-Eloquent-در-لاوارل
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
علاوه بر آن Eloquent انتظار دارد کلید خارجی مقداری منطبق با مقدار ستون id (یا متغیر سفارشی $primaryKey) جدول پدر داشته باشد. به عبارت بهتر، Eloquent به دنبال مقدار ستون id کاربر در ستون user_id رکورد Phone می گردد . اگر می خواهید رابطه از مقداری غیر از id به عنوان کلید خارجی استفاده کند، بایستی یک آرگومان سوم به متد hasOne ارسال کرده و از طریق آن کلید سفارشی خود را مشخص کنید:
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
در نمونه ی بالا، Eloquent ستون user_id از مدل Phone را به id در مدل User متصل (match می کند). Eloquent اسم پیش فرض کلید خارجی را با در نظر گرفتن اسم متد (رابطه) و الصاق پسوند _id تعریف می کند. حال اگر می خواهید کلید خارجی در مدل Phone اسمی غیر ازuser_id داشته باشد، می توانید اسم دلخواه کلید خارجی را به عنوان آرگومان دوم ارسال کنید:
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
یادآور می شویم که Eloquent خود ستون کلید خارجی را در مدل Comment تعیین می کند. بر اساس قراردادهای از پیش تعیین شده،Eloquent فرم snake case اسم مدل مالک (post) را انتخاب کرده و صرفا یک پسوند _id به آن اضافه می کند. در این مثال Eloquent فرض را بر این می گذارد که کلید خارجی در مدل Comment ستونی به نام post_id هست.
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
<h3>تعریف طرف دیگر رابطه  One To Many (Inverse)</h3>
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
در مثال بالا، Eloquent ستون post_id از مدل Comment را به ستون id در مدل Post مچ می کند. همان طور که قبلا هم به آن اشاره شد،Eloquent اسم کلید خارجی را با درنظر گرفتن اسم رابطه و افزودن پسوند _id به اسم متد تعیین می کند. حال اگر اسم ستون کلید خارجی در مدلComment، post_id نباشد در آن صورت یک اسم سفارشی به عنوان آرگومان دوم به متد belongsTo پاس می دهیم:
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
<h3>)بازیابی ستون های جدول واسط( Retrieving Intermediate Table Columns</h3>
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

<br>
<h3>ساختار جداول مورد نیاز رابطه ی پلی مرفیک</h3>
<p>
رابطه های Polymorphic به یک مدل اجازه می دهند در یک رابطه همزمان به چندین مدل دیگر تعلق داشته باشد (امکان رابطه ی یک مدل با چندین مدل دیگر از طریق یک رابطه را فراهم می آورد). به عنوان مثال، فرض کنید کاربران اپلیکیشن شما می خواهند هم پست ها و هم دیدگاه ها را بپسندد (like کنند). با بهره گیری از رابطه های polymorphic می توان به راحتی یک جدول واحد likes برای هر دو این سناریو بکار برد. در گام نخست ساختار جداولی که برای این رابطه لازم است را مورد بررسی قرار می دهیم:
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
دو ستون مورد توجه در این مثال، ستون های likeable_id و likeable_type در جدول likes هستند. ستون likeable_id حاوی مقدار ID پست یا دیدگاه خواهد بود، در حالی که ستون likeable_type اسم کلاس مدل مالک (پدر) را نگه خواهد داشت. Eloquent در واقع به واسطه ی ستونlikeable_type تشخیص می دهد در زمان دسترسی به رابطه ی likeable کدام مدل را بایستی برگرداند.
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
<h3>Retrieving Polymorphic Relations</h3>
<p>
Once your database table and models are defined, you may access the relationships via your models. For example, to access all of the comments for a post, we can simply use the <code class=" language-php">comments</code> dynamic property:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

foreach ($post->comments as $comment) {
    //
}
</code></pre>

<p>
You may also retrieve the owner of a polymorphic relation from the polymorphic model by accessing the name of the method that performs the call to <code class=" language-php">morphTo</code>. In our case, that is the <code class=" language-php">commentable</code> method on the <code class=" language-php">Comment</code> model. So, we will access that method as a dynamic property:
</p>

<pre><code class="language-php  line-numbers">$comment = App\Comment::find(1);

$commentable = $comment->commentable;
</code></pre>

<p>
The <code class=" language-php">commentable</code> relation on the <code class=" language-php">Comment</code> model will return either a <code class=" language-php">Post</code> or <code class=" language-php">Video</code> instance, depending on which type of model owns the comment.
</p>

<br>
<h3>Custom Polymorphic Types</h3>
<p>
By default, Laravel will use the fully qualified class name to store the type of the related model. For instance, given the example above where a <code class=" language-php">Comment</code> may belong to a <code class=" language-php">Post</code> or a <code class=" language-php">Video</code>, the default <code class=" language-php">commentable_type</code> would be either <code class=" language-php">App\<span class="token package">Post</span></code> or <code class=" language-php">App\<span class="token package">Video</span></code>, respectively. However, you may wish to decouple your database from your application's internal structure. In that case, you may define a relationship "morph map" to instruct Eloquent to use a custom name for each model instead of the class name:
</p>

<pre><code class="language-php  line-numbers">use Illuminate\Database\Eloquent\Relations\Relation;

Relation::morphMap([
    'posts' => 'App\Post',
    'videos' => 'App\Video',
]);
</code></pre>

<p>
You may register the <code class=" language-php">morphMap</code> in the <code class=" language-php">boot</code> function of your <code class=" language-php">AppServiceProvider</code> or create a separate service provider if you wish.
</p>
<p>
<a name="many-to-many-polymorphic-relations"></a>
</p>

<br>
<h3>Many To Many Polymorphic Relations</h3>

<br>
<h3>Table Structure</h3>
<p>
In addition to traditional polymorphic relations, you may also define "many-to-many" polymorphic relations. For example, a blog <code class=" language-php">Post</code> and <code class=" language-php">Video</code> model could share a polymorphic relation to a <code class=" language-php">Tag</code> model. Using a many-to-many polymorphic relation allows you to have a single list of unique tags that are shared across blog posts and videos. First, let's examine the table structure:
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
Next, we're ready to define the relationships on the model. The <code class=" language-php">Post</code> and <code class=" language-php">Video</code> models will both have a <code class=" language-php">tags</code> method that calls the <code class=" language-php">morphToMany</code> method on the base Eloquent class:
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
<h3>Defining The Inverse Of The Relationship</h3>
<p>
Next, on the <code class=" language-php">Tag</code> model, you should define a method for each of its related models. So, for this example, we will define a <code class=" language-php">posts</code> method and a <code class=" language-php">videos</code> method:
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
<h3>Retrieving The Relationship</h3>
<p>
Once your database table and models are defined, you may access the relationships via your models. For example, to access all of the tags for a post, you can simply use the <code class=" language-php">tags</code> dynamic property:
</p>

<pre><code class="language-php  line-numbers">$post = App\Post::find(1);

foreach ($post->tags as $tag) {
    //
}
</code></pre>

<p>
You may also retrieve the owner of a polymorphic relation from the polymorphic model by accessing the name of the method that performs the call to <code class=" language-php">morphedByMany</code>. In our case, that is the <code class=" language-php">posts</code> or <code class=" language-php">videos</code> methods on the <code class=" language-php">Tag</code> model. So, you will access those methods as dynamic properties:
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
<h3><a href="#querying-relations">Querying Relations</a></h3>
<p>
Since all types of Eloquent relationships are defined via methods, you may call those methods to obtain an instance of the relationship without actually executing the relationship queries. In addition, all types of Eloquent relationships also serve as <a href="/docs/5.5/queries">query builders</a>, allowing you to continue to chain constraints onto the relationship query before finally executing the SQL against your database.
</p>
<p>
For example, imagine a blog system in which a <code class=" language-php">User</code> model has many associated <code class=" language-php">Post</code> models:
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
You may query the <code class=" language-php">posts</code> relationship and add additional constraints to the relationship like so:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->posts()->where('active', 1)->get();
</code></pre>

<p>
You are able to use any of the <a href="/docs/5.5/queries">query builder</a> methods on the relationship, so be sure to explore the query builder documentation to learn about all of the methods that are available to you.
</p>
<p>
<a name="relationship-methods-vs-dynamic-properties"></a>
</p>

<br>
<h3>Relationship Methods Vs. Dynamic Properties</h3>
<p>
If you do not need to add additional constraints to an Eloquent relationship query, you may simply access the relationship as if it were a property. For example, continuing to use our <code class=" language-php">User</code> and <code class=" language-php">Post</code> example models, we may access all of a user's posts like so:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

foreach ($user->posts as $post) {
    //
}
</code></pre>

<p>
Dynamic properties are "lazy loading", meaning they will only load their relationship data when you actually access them. Because of this, developers often use <a href="#eager-loading">eager loading</a> to pre-load relationships they know will be accessed after loading the model. Eager loading provides a significant reduction in SQL queries that must be executed to load a model's relations.
</p>
<p>
<a name="querying-relationship-existence"></a>
</p>

<br>
<h3>Querying Relationship Existence</h3>
<p>
When accessing the records for a model, you may wish to limit your results based on the existence of a relationship. For example, imagine you want to retrieve all blog posts that have at least one comment. To do so, you may pass the name of the relationship to the <code class=" language-php">has</code> and <code class=" language-php">orHas</code> methods:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have at least one comment...
$posts = App\Post::has('comments')->get();
</code></pre>

<p>
You may also specify an operator and count to further customize the query:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have three or more comments...
$posts = Post::has('comments', '>=', 3)->get();
</code></pre>

<p>
Nested <code class=" language-php">has</code> statements may also be constructed using "dot" notation. For example, you may retrieve all posts that have at least one comment and vote:
</p>

<pre><code class="language-php  line-numbers">// Retrieve all posts that have at least one comment with votes...
$posts = Post::has('comments.votes')->get();
</code></pre>

<p>
If you need even more power, you may use the <code class=" language-php">whereHas</code> and <code class=" language-php">orWhereHas</code> methods to put "where" conditions on your <code class=" language-php">has</code> queries. These methods allow you to add customized constraints to a relationship constraint, such as checking the content of a comment:
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
<h3><a href="#eager-loading">Eager Loading</a></h3>
<p>
When accessing Eloquent relationships as properties, the relationship data is "lazy loaded". This means the relationship data is not actually loaded until you first access the property. However, Eloquent can "eager load" relationships at the time you query the parent model. Eager loading alleviates the N + 1 query problem. To illustrate the N + 1 query problem, consider a <code class=" language-php">Book</code> model that is related to <code class=" language-php">Author</code>:
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
Now, let's retrieve all books and their authors:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::all();

foreach ($books as $book) {
    echo $book->author->name;
}
</code></pre>

<p>
This loop will execute 1 query to retrieve all of the books on the table, then another query for each book to retrieve the author. So, if we have 25 books, this loop would run 26 queries: 1 for the original book, and 25 additional queries to retrieve the author of each book.
</p>
<p>
Thankfully, we can use eager loading to reduce this operation to just 2 queries. When querying, you may specify which relationships should be eager loaded using the <code class=" language-php">with</code> method:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::with('author')->get();

foreach ($books as $book) {
    echo $book->author->name;
}
</code></pre>

<p>
For this operation, only two queries will be executed:
</p>

<pre><code class="language-php  line-numbers">select * from books

select * from authors where id in (1, 2, 3, 4, 5, ...)
</code></pre>


<br>
<h3>Eager Loading Multiple Relationships</h3>
<p>
Sometimes you may need to eager load several different relationships in a single operation. To do so, just pass additional arguments to the <code class=" language-php">with</code> method:
</p>

<pre><code class="language-php  line-numbers">$books = App\Book::with(['author', 'publisher'])->get();
</code></pre>


<br>
<h3>Nested Eager Loading</h3>
<p>
To eager load nested relationships, you may use "dot" syntax. For example, let's eager load all of the book's authors and all of the author's personal contacts in one Eloquent statement:
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
<h3>Constraining Eager Loads</h3>
<p>
Sometimes you may wish to eager load a relationship, but also specify additional query constraints for the eager loading query. Here's an example:
</p>

<pre><code class="language-php  line-numbers">$users = App\User::with(['posts' => function ($query) {
    $query->where('title', 'like', '%first%');
}])->get();
</code></pre>

<p>
In this example, Eloquent will only eager load posts where the post's <code class=" language-php">title</code> column contains the word <code class=" language-php">first</code>. Of course, you may call other <a href="/docs/5.5/queries">query builder</a> methods to further customize the eager loading operation:
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
<h3>The Save Method</h3>
<p>
Eloquent provides convenient methods for adding new models to relationships. For example, perhaps you need to insert a new <code class=" language-php">Comment</code> for a <code class=" language-php">Post</code> model. Instead of manually setting the <code class=" language-php">post_id</code> attribute on the <code class=" language-php">Comment</code>, you may insert the <code class=" language-php">Comment</code> directly from the relationship's <code class=" language-php">save</code> method:
</p>

<pre><code class="language-php  line-numbers">$comment = new App\Comment(['message' => 'A new comment.']);

$post = App\Post::find(1);

$post->comments()->save($comment);
</code></pre>

<p>
Notice that we did not access the <code class=" language-php">comments</code> relationship as a dynamic property. Instead, we called the <code class=" language-php">comments</code> method to obtain an instance of the relationship. The <code class=" language-php">save</code> method will automatically add the appropriate <code class=" language-php">post_id</code> value to the new <code class=" language-php">Comment</code> model.
</p>
<p>
If you need to save multiple related models, you may use the <code class=" language-php">saveMany</code> method:
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
<h3>The Create Method</h3>
<p>
In addition to the <code class=" language-php">save</code> and <code class=" language-php">saveMany</code> methods, you may also use the <code class=" language-php">create</code> method, which accepts an array of attributes, creates a model, and inserts it into the database. Again, the difference between <code class=" language-php">save</code> and <code class=" language-php">create</code> is that <code class=" language-php">save</code> accepts a full Eloquent model instance while <code class=" language-php">create</code> accepts a plain PHP <code class=" language-php"><span class="token keyword">array</span></code>:
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
<h3>Belongs To Relationships</h3>
<p>
When updating a <code class=" language-php">belongsTo</code> relationship, you may use the <code class=" language-php">associate</code> method. This method will set the foreign key on the child model:
</p>

<pre><code class="language-php  line-numbers">$account = App\Account::find(10);

$user->account()->associate($account);

$user->save();
</code></pre>

<p>
When removing a <code class=" language-php">belongsTo</code> relationship, you may use the <code class=" language-php">dissociate</code> method. This method will set the relationship's foreign key to <code class=" language-php"><span class="token keyword">null</span></code>:
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
<h3>Attaching / Detaching</h3>
<p>
Eloquent also provides a few additional helper methods to make working with related models more convenient. For example, let's imagine a user can have many roles and a role can have many users. To attach a role to a user by inserting a record in the intermediate table that joins the models, use the <code class=" language-php">attach</code> method:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->roles()->attach($roleId);
</code></pre>

<p>
When attaching a relationship to a model, you may also pass an array of additional data to be inserted into the intermediate table:
</p>

<pre><code class="language-php  line-numbers">$user->roles()->attach($roleId, ['expires' => $expires]);
</code></pre>

<p>
Of course, sometimes it may be necessary to remove a role from a user. To remove a many-to-many relationship record, use the <code class=" language-php">detach</code> method. The <code class=" language-php">detach</code> method will remove the appropriate record out of the intermediate table; however, both models will remain in the database:
</p>

<pre><code class="language-php  line-numbers">// Detach a single role from the user...
$user->roles()->detach($roleId);

// Detach all roles from the user...
$user->roles()->detach();
</code></pre>

<p>
For convenience, <code class=" language-php">attach</code> and <code class=" language-php">detach</code> also accept arrays of IDs as input:
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
The many-to-many relationship also provides a <code class=" language-php">toggle</code> method which "toggles" the attachment status of the given IDs. If the given ID is currently attached, it will be detached. Likewise, if it is currently detached, it will be attached:
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
<h3>Updating A Record On A Pivot Table</h3>
<p>
If you need to update an existing row in your pivot table, you may use <code class=" language-php">updateExistingPivot</code> method. This method accepts the pivot record foreign key and an array of attributes to update:
</p>

<pre><code class="language-php  line-numbers">$user = App\User::find(1);

$user->roles()->updateExistingPivot($roleId, $attributes);
</code></pre>

<p>
<a name="touching-parent-timestamps"></a>
</p>

<br>
<h3><a href="#touching-parent-timestamps">Touching Parent Timestamps</a></h3>
<p>
When a model <code class=" language-php">belongsTo</code> or <code class=" language-php">belongsToMany</code> another model, such as a <code class=" language-php">Comment</code> which belongs to a <code class=" language-php">Post</code>, it is sometimes helpful to update the parent's timestamp when the child model is updated. For example, when a <code class=" language-php">Comment</code> model is updated, you may want to automatically "touch" the <code class=" language-php">updated_at</code> timestamp of the owning <code class=" language-php">Post</code>. Eloquent makes it easy. Just add a <code class=" language-php">touches</code> property containing the names of the relationships to the child model:
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
Now, when you update a <code class=" language-php">Comment</code>, the owning <code class=" language-php">Post</code> will have its <code class=" language-php">updated_at</code> column updated as well, making it more convenient to know when to invalidate a cache of the <code class=" language-php">Post</code> model:
</p>

<pre><code class="language-php  line-numbers">$comment = App\Comment::find(1);

$comment->text = 'Edit to this comment!';

$comment->save();
</code></pre>