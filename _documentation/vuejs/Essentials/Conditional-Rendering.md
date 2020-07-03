---
layout: documentation-vuejs
title:    رندرینگ شرطی  (Conditional Rendering)
cattitle:   اصول آموزش vuejs
date:   2019-10-04 20:08:42 +0330
jdate: جمعه 12 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/conditional.html
---
<h3>دایرکتیو v-if </h3>
<p>
دایرکتیو v-if برای رندر مشروط یک بلوک به کار می رود. این بلوک تنها در صورتیکه عبارت دایرکتیو یک مقدار true  برگرداند رندر می شود.
</p>

<pre><code class="language-html  line-numbers">&#x3C;h1 v-if=&#x22;awesome&#x22;&#x3E;Vue is awesome!&#x3C;/h1&#x3E;
</code></pre>

<p>
همچنین می توان "بلوک  else" را با v-else اضافه کرد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;h1 v-if=&#x22;awesome&#x22;&#x3E;Vue is awesome!&#x3C;/h1&#x3E;
&#x3C;h1 v-else&#x3E;Oh no &#x1F622;&#x3C;/h1&#x3E;
</code></pre>
<br>

<h3>گروههای شرطی با v-if در &#x3C;template&#x3E;</h3>

<p>
از آنجا که v-if یک دایرکتیو است ، باید به یک عنصر پیوست شود. اما اگر بخواهیم بیش از یک عنصر را تغییر دهیم چه می شود؟ در این حالت می توانیم از v-if در یک عنصر &#x3C;template&#x3E; که بقیه عناصر را در بر می گیرد ، استفاده کنیم. نتیجه ارائه شده نهایی عنصر &#x3C;template&#x3E; را شامل نمی شود.
</p>

<pre><code class="language-html  line-numbers">&#x3C;template v-if=&#x22;ok&#x22;&#x3E;
  &#x3C;h1&#x3E;Title&#x3C;/h1&#x3E;
  &#x3C;p&#x3E;Paragraph 1&#x3C;/p&#x3E;
  &#x3C;p&#x3E;Paragraph 2&#x3C;/p&#x3E;
&#x3C;/template&#x3E;
</code></pre>
<br>
<h3>دایرکتیو v-else </h3>
<p>
می توانید از دایرکتیو v-else  برای نشان دادن "بلاک else" برای دایرکتیو v-if استفاده نمایید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-if=&#x22;Math.random() &#x3E; 0.5&#x22;&#x3E;
  Now you see me
&#x3C;/div&#x3E;
&#x3C;div v-else&#x3E;
  Now you don&#x27;t
&#x3C;/div&#x3E;
</code></pre>


<br>
<h3>دایرکتیو v-else-if </h3>
<p>
v-other-if ، همانطور که از نام آن پیداست ، جهت بلاک "else if" برای دایرکتیو v-if استفاده می شود.این دایرکتیو همچنین بصورت زنجیره ای می تواند بکار رود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-if=&#x22;type === &#x27;A&#x27;&#x22;&#x3E;
  A
&#x3C;/div&#x3E;
&#x3C;div v-else-if=&#x22;type === &#x27;B&#x27;&#x22;&#x3E;
  B
&#x3C;/div&#x3E;
&#x3C;div v-else-if=&#x22;type === &#x27;C&#x27;&#x22;&#x3E;
  C
&#x3C;/div&#x3E;
&#x3C;div v-else&#x3E;
  Not A/B/C
&#x3C;/div&#x3E;
</code></pre>

<br>


<h3>کنترل عناصر قابل استفاده مجدد با key</h3>

<p>
Vue سعی می کند عناصر را تا حد ممکن کارآمدتر کند ، و اغلب به جای رها کردن عناصر ، دوباره از آنها استفاده می کند. گذشته از کمکی که به سرعت Vue می کند، این می تواند مزایای مفیدی داشته باشد. به عنوان مثال ، اگر به کاربران اجازه می دهید بین چندین ورود به سیستم جابجا شوند:
</p>

<pre><code class="language-html  line-numbers">&#x3C;template v-if=&#x22;loginType === &#x27;username&#x27;&#x22;&#x3E;
  &#x3C;label&#x3E;Username&#x3C;/label&#x3E;
  &#x3C;input placeholder=&#x22;Enter your username&#x22;&#x3E;
&#x3C;/template&#x3E;
&#x3C;template v-else&#x3E;
  &#x3C;label&#x3E;Email&#x3C;/label&#x3E;
  &#x3C;input placeholder=&#x22;Enter your email address&#x22;&#x3E;
&#x3C;/template&#x3E;
</code></pre>

<div id="app1" class="result-example">
    <template v-if="loginType === 'username'">
        <label>Username</label>
        <input placeholder="Enter your username">
    </template>
    <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address">
    </template>
    <div>
        <button v-on:click="click">Toggle login type</button>
    </div>
</div>

<p>
سپس با تعویض loginType در کد بالا خواهیم دید آنچه را که کاربر قبلاً وارد کرده پاک نخواهد کرد. از آنجا که هر دو قالب از عناصر یکسان استفاده می کنند ، &#x3C;input&#x3E; جایگزین نمی شود – و فقط placeholder  تغییر می کند.
</p>

<p>
این ویژگی همیشه مطلوب نیست ، بنابراین Vue راهی را برای شما فراهم می کند تا بگویید ، "این دو عنصر کاملاً جدا از هم هستند - از آنها استفاده مجدد نکنید." و یک ویژگی key با مقادیر منحصر به فرد اضافه نمایید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;template v-if=&#x22;loginType === &#x27;username&#x27;&#x22;&#x3E;
  &#x3C;label&#x3E;Username&#x3C;/label&#x3E;
  &#x3C;input placeholder=&#x22;Enter your username&#x22; key=&#x22;username-input&#x22;&#x3E;
&#x3C;/template&#x3E;
&#x3C;template v-else&#x3E;
  &#x3C;label&#x3E;Email&#x3C;/label&#x3E;
  &#x3C;input placeholder=&#x22;Enter your email address&#x22; key=&#x22;email-input&#x22;&#x3E;
&#x3C;/template&#x3E;
</code></pre>

<p>
حالا هر بار که وارد شوید ، این ورودی ها از ابتدا ارائه می شوند. مشاهده کنید:
</p>

<div id="app2" class="result-example">
    <template v-if="loginType === 'username'">
        <label>Username</label>
        <input placeholder="Enter your username" key="username-input">
    </template>
    <template v-else>
        <label>Email</label>
        <input placeholder="Enter your email address" key="email-input">
    </template>
    <div>
        <button v-on:click="click">Toggle login type</button>
    </div>
</div>

<p>
توجه داشته باشید که عناصر &#x3C;label&#x3E; هنوز به طور مؤثر دوباره مورد استفاده قرار می گیرند ، زیرا آنها ویژگی  key ندارند.
</p>

<br>
<h3>دایرکتیو v-show </h3>
<p>
گزینه دیگر برای نمایش مشروط یک عنصر ، استفاده از دایرکتیو v-show است. نحوه استفاده تقریباً یکسان است:
</p>

<pre><code class="language-html  line-numbers">&#x3C;h1 v-show=&#x22;ok&#x22;&#x3E;Hello!&#x3C;/h1&#x3E;
</code></pre>

<p>
تفاوت این است که عنصری که با v-show رندر می شود همیشه در DOM باقی می ماند. v-show فقط صفت display   مربوط به  CSS عنصر را ضمیمه می کند.
</p>

<blockquote class="has-icon tip">
توجه داشته باشید که v-show از عنصر &#x3C;template&#x3E; پشتیبانی نمی کند ، و همچنین با v-else کار نمی کند.
</blockquote>

<br>
<h3>مقایسه v-if با  v-show</h3>
<p>
 v-ifیک رندر شرطی "واقعی" است زیرا تضمین می کند که شنوندگان رویدادها و کامپوننت های فرزند در داخل بلوک شرطی به درستی از بین می روند و در تغییر وضعیت مجدداً ایجاد شوند.
</p>

<p>
v-if همچنین کند است: اگر شرط ارائه اولیه نادرست باشد ، رندری انجام نمی شود - بلوک شرطی رندر نخواهد شد تا زمانی که این شرط برای اولین بار صحیح شود.
</p>

<p>
در مقایسه ، v-show بسیار ساده تر است - این عنصر همیشه بدون در نظر گرفتن شرایط اولیه ، با ایجاد تغییر در CSS پایه رندر می شود.
</p>

<p>
به طور کلی ، v-if هزینه تغییر وضعیت بالاتری دارد در حالی که v-show دارای هزینه های رندر اولیه بالاتری است. بنابراین ، اگر لازم است عنصری را  اغلب تغییر وضعیت دهید ، v-show را ترجیح می دهیم و در صورت عدم احتمال تغییر در زمان اجرا ، v-if را ترجیح می دهیم.
</p>
<br>


<h3>استفاده v-if به همراه  v-for</h3>
<blockquote class="has-icon tip">
استفاده v-if و v-for با هم توصیه نمی شود. برای اطلاعات بیشتر به <a href="https://vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential" target="_blank" >  style guide </a>مراجعه کنید.
</blockquote>

<p>
درصورت استفاده همزمان، v-for دارای اواویت بالاتری نسبت به v-if است. برای اطلاعات بیشتر به <a href="https://vuejs.org/v2/guide/list.html#v-for-with-v-if" target="_blank" >  list rendering guide </a>مراجعه کنید.
</p>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>

 var app1 = new Vue({
     el: '#app1',
     data: {
         loginType : 'username'
     },
     methods:{
         click: function(){
             if(this.loginType == 'username'){
                 this.loginType = 'email';
             }else{
                 this.loginType = 'username';
             }
         }
     }
 })


 var app2 = new Vue({
     el: '#app2',
     data: {
         loginType : 'username'
     },
     methods:{
         click: function(){
             if(this.loginType == 'username'){
                 this.loginType = 'email';
             }else{
                 this.loginType = 'username';
             }
         }
     }
 })

</script>