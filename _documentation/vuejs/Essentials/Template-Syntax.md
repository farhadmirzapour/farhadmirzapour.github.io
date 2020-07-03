---
layout: documentation-vuejs
title:   اصول قالب در Vue
cattitle:   اصول آموزش vuejs
date:   2019-09-28 21:16:42 +0330
jdate: سه شنبه 02 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/syntax.html
---
<h3>  ایجاد نمونه  (Creating a Vue Instance)</h3>
<p>
Vue.js از نحو قالب مبتنی بر HTML استفاده می کند که به شما امکان می دهد DOM رندر شده را به داده های نمونه ایجاد شده Vue  متصل کنید. همه قالب های Vue.js دارای HTML معتبری هستند که می تواند توسط مرورگرهای خاص و سازگار با آن تجزیه و تحلیل شود.
</p>

<p>
Vue قالبها را به توابع DOM مجازی کامپایل می کند. همراه با سیستم واکنش پذیری ، Vue قادر است هوشمندانه به حداقل تعداد کامپوننت ها جهت رندر مجدد و اعمال حداقل دستکاری DOM هنگام تغییر وضعیت برنامه ، پی برد.
</p>
<br>

<h3>  متن (Text)</h3>

<p>
معمول ترین شکل اتصال داده ها استفاده از نحو Mustache (دابل براکت) می باشد :
</p>

<pre><code class="language-html  line-numbers">&#x3C;span&#x3E;Message: &#123;&#123; msg &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<p>
متن Mustache با مقدار ویژگی msg در شی داده مربوطه جایگزین می شود. همچنین هرگاه مقدار ویژگی خاصیت داده شی تغییر کند متن آن نیز بروزرسانی می شود.
</p>

<p>
شما همچنین می توانید عمل جایگذاری را یک بار انجام دهید که با استفاده از دستورالعمل v-once داده ها بیش از یکبار بروزرسانی نمی شود ، اما در نظر داشته باشید که این امر می تواند بر روی اتصال داده در سایر گره ها (node) نیز تأثیرگذار باشد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;span v-once&#x3E;This will never change: &#123;&#123; msg &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<br>

<h3> داده نوع (Raw HTML)</h3>

<p>
استفاده از نحو (mustaches) برای داده های متنی کاربرد دارد. جهت داده های HTML شما می توانید از دایرکتیو v-html استفاده نمایید :
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;app-1&#x22;&#x3E;
&#x3C;p&#x3E;Using mustaches: &#123;&#123; rawHtml &#125;&#125;&#x3C;/p&#x3E;
&#x3C;p&#x3E;Using v-html directive: &#x3C;span v-html=&#x22;rawHtml&#x22;&#x3E;&#x3C;/span&#x3E;&#x3C;/p&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers"> var app1 = new Vue({
    el: '#app-1',
    data: {
    rawHtml: '&#x3C;span style=&#x22;color: red&#x22;&#x3E;This should be red.&#x3C;/span&#x3E;'
    }
 })
</code></pre>

<div id="app-1" class="result-example">
   <p>Using mustaches: &#123;&#123; rawHtml &#125;&#125;</p>
   <p>Using v-html directive: <span v-html="rawHtml"></span></p>
</div>

<blockquote class="has-icon tip">
رندر داینامیکی HTML دلخواه در وب سایت شما می تواند بسیار خطرناک باشد زیرا به راحتی می تواند منجر به آسیب پذیری XSS شود. فقط از جایگذاری HTML بر روی محتوای قابل اعتماد استفاده شود و هرگز در محتوای ارائه شده توسط کاربر استفاده نکنید.
</blockquote>
<br>

<h3>   صفات (Attributes)</h3>
<p>
نحو(Mustaches) را نمی توان در صفات HTML استفاده کرد. در عوض می توانید از دستورالعمل v-bind استفاده نمایید:
</p>
<pre><code class="language-html  line-numbers">&#x3C;div v-bind:id=&#x22;dynamicId&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
در مورد صفات boolean ، جایی که شرط  آنها منتهی بر true  می شود ، v-bind کمی متفاوت عمل می کند. در این مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;button v-bind:disabled=&#x22;isButtonDisabled&#x22;&#x3E;Button&#x3C;/button&#x3E;
</code></pre>

<p>
اگر isButtonDisabled مقدار null،  undefined یا false داشته باشد ، ویژگی disabled حتی در عنصر &#x3C;button&#x3E;  نیز درج نخواهد شد.
</p>

<br>

<h3>    استفاداه از عبارات جاوااسکریپت(Using JavaScript Expressions)</h3>
<p>
Vue.js  از تمام عبارات جاوا اسکریپت در کلیه اتصالات داده پشتیبانی می کند:
 </p>
 
 <pre><code class="language-html  line-numbers"> &#123;&#123; number + 1 &#125;&#125;
 
 &#123;&#123; ok ? &#x27;YES&#x27; : &#x27;NO&#x27; &#125;&#125;
 
 &#123;&#123; message.split(&#x27;&#x27;).reverse().join(&#x27;&#x27;) &#125;&#125;
 
 &#x3C;div v-bind:id=&#x22;&#x27;list-&#x27; + id&#x22;&#x3E;&#x3C;/div&#x3E;

 </code></pre>
 
 <p>
 یک محدودیت این است که هر اتصال فقط می تواند یک عبارت واحد داشته باشد ، بنابراین موارد زیر کار نمی کند:
 </p>
 
 
  <pre><code class="language-html  line-numbers">  &#x3C;!-- this is a statement, not an expression: --&#x3E;
  &#123;&#123; var a = 1 &#125;&#125;
  
  &#x3C;!-- flow control won&#x27;t work either, use ternary expressions --&#x3E;
  &#123;&#123; if (ok) { return message } &#125;&#125;
  </code></pre>
  
  <blockquote class="has-icon tip">
  عبارات قالب  فقط به لیست سفید از توابع مانند Math و Date دسترسی دارند و شما نباید سعی کنید در عبارات به توابع  تعریف شده توسط کاربر دسترسی داشته باشید
  </blockquote>
  
  <br>
  
  <h3>    استفاداه از دایرکتیو(Directives)</h3>
  <p>
  دایرکتیوها  ویژگی های خاصی با پیشوند -v هستند و مقدار آن،  یک عبارت جاوا اسکریپت واحد  می باشد(به استثنای v-for ، که بعداً مورد بحث قرار خواهد گرفت).  کار یک دایرکتیو این است که وقتی مقدار یک عبارت تغییر کرد ، تاثیر آن را روی DOM اعمال نماید. بیایید مثالی را که در مقدمه دیدیم مرور کنیم:
  </p>
  
  <pre><code class="language-html  line-numbers">&#x3C;p v-if=&#x22;seen&#x22;&#x3E;Now you see me&#x3C;/p&#x3E;
  </code></pre>
  
  <p>
  در اینجا ، دستورالعمل v-if عنصر &#x3C;p&#x3E; را براساس نتیجه عبارتی که دیده می شود ، حذف یا درج می کند.
  </p>
  <br>
  
   <h3>    آرگومان ها در دایرکتیو(Arguments)</h3>
  <p>
  برخی از دایرکتیوها می توانند یک آرگومان دریافت کنند ، که پس از نام دایرکتیو توسط کاراکتر :  مشخص شده اند. به عنوان مثال ، دستورالعمل v-bind برای به روزرسانی واکنشی یک ویژگی HTML استفاده می شود:
  </p>
  
   <pre><code class="language-html  line-numbers">&#x3C;a v-bind:href=&#x22;url&#x22;&#x3E; ... &#x3C;/a&#x3E;
   </code></pre>
   
   <p>
   در اینجا href آرگومانی است که به دایرکتیو v-bind می گوید ویژگی href عنصر را به مقدار عبارت url متصل کند.
   </p>
   
   <p>
   مثال دیگر دایرکتیو v-on است که به رویدادهای DOM گوش می دهد:
   </p>
   
   <pre><code class="language-html  line-numbers">&#x3C;a v-on:click=&#x22;doSomething&#x22;&#x3E; ... &#x3C;/a&#x3E;
   </code></pre>
   
   <p>
   در اینجا آرگومان، نام رویدادی جهت گوش دادن به است. ما در مورد جزئیات رویدادها نیز با جزئیات بیشتری صحبت خواهیم کرد.
   </p>
   
   <br>
     
   <h3>    آرگومان های داینامیک(Dynamic Arguments)</h3>
   <p>
 از نسخه 2.6.0 ، همچنین می توانید از یک عبارات جاوا اسکریپت  به عنوان یک آرگومان دایرکتیو  در درون [] استفاده نمایید:
   </p>
   
  <pre><code class="language-html  line-numbers">&#x3C;!--
  Note that there are some constraints to the argument expression, as explained
  in the &#x22;Dynamic Argument Expression Constraints&#x22; section below.
  --&#x3E;
  &#x3C;a v-bind:[attributeName]=&#x22;url&#x22;&#x3E; ... &#x3C;/a&#x3E;
  </code></pre>
  
<pre><code class="language-javascript  line-numbers">var vm = new Vue({
     el: '#app',
     data: {
         attributename : 'href',
         url : 'http://farhadnote.ir'
     }
 })
</code></pre>
  
  <p>
  در اینجا  attributeName به صورت پویا به عنوان یک عبارت جاوا اسکریپت ارزیابی می شود و از مقدار ارزیابی شده آن به عنوان مقدار نهایی برای آرگومان استفاده می شود. به عنوان مثال ، اگر نمونه Vue شما دارای ویژگی داده attributename باشد و مقدار آن برابر "href" است ، این معادل است با v-bind: href.
  </p>
  
  <p>
  به طور مشابه ، می توانید از آرگومان های پویا برای پیوند دادن یک کنترل کننده به نام یک رویداد پویا استفاده کنید:
  </p>
  
  <pre><code class="language-html  line-numbers">&#x3C;a v-on:[eventName]=&#x22;doSomething&#x22;&#x3E; ... &#x3C;/a&#x3E;
  </code></pre>
<pre><code class="language-javascript  line-numbers">var vm = new Vue({
     el: '#app',
     data: {
         eventname : 'click'
     },
     methods: {
         doSomething : function(){
             alert('hello world');
         }
     }
 })
</code></pre>
  
  <p>
  در این مثال، زمانیکه مقدار eventname برابر "click" باشد آنگاه v-on:[eventName]  معادل v-on:click خواهد شد.
  </p>
 <br>
 
 <h4>محدودیت های مقدار  (Dynamic Argument Value Constraints)  </h4> 
  <p>
  انتظار می رود آرگومان های پویا ، یک رشته را به استثنای null ارزیابی کنند. از مقدار ویژه  null می توان جهت حذف صریح اتصال استفاده کرد. هر مقدار غیر رشته ای دیگر باعث ایجاد اخطار می گردد.
  </p>
  
 <h4>محدودیت های عبارات  (Dynamic Argument Expression Constraints)  </h4> 
  <p>
  عبارات آرگومان پویا محدودیت های نحوی دارند زیرا برخی از کاراکترها ها مانند فضاها و نقل قول ها در نام های ویژگی های HTML نامعتبر هستند. به عنوان مثال موارد زیر نامعتبر است:
  </p>
  
   <pre><code class="language-html  line-numbers">&#x3C;!-- This will trigger a compiler warning. --&#x3E;
   &#x3C;a v-bind:[&#x27;foo&#x27; + bar]=&#x22;value&#x22;&#x3E; ... &#x3C;/a&#x3E;
   </code></pre>  
  <p>
  راه حل این است که یا از عبارات بدون فاصله یا نقل قول استفاده کنید ، یا عبارت پیچیده را با یک ویژگی computed جایگزین کنید.
  </p>
  <p>
  هنگام استفاده از قالب ها در DOM (قالب هایی که مستقیماً در یک فایل HTML نوشته شده اند) ، باید از نامگذاری کلیدها با کاراکترهای بزرگ خودداری کنید ، زیرا مرورگرها اسم ویژگی ها را به حروف کوچک محدود می کنند:
  </p>
  
   <pre><code class="language-html  line-numbers">&#x3C;!--
 This will be converted to v-bind:[someattr] in in-DOM templates.
 Unless you have a &#x22;someattr&#x22; property in your instance, your code won&#x27;t work.
 --&#x3E;
 &#x3C;a v-bind:[someAttr]=&#x22;value&#x22;&#x3E; ... &#x3C;/a&#x3E;   
    </code></pre>  
  
  <br>
  <h3> اصلاح کننده ها(Modifiers)</h3>
   <p>
Modifiers (اصلاح کننده) پسوندهای خاصی هستند که با یک نقطه مشخص می شوند  و  بیانگر این است که یک دایرکتیو باید به روشی خاص محدود شود. به عنوان مثال ، اصلاح کننده prevent به دایرکتیو v-on می گوید  تابع ()preventDefault را در هنگام  آغاز رویداد فراخوانی کند:
   </p>
   
   <pre><code class="language-html  line-numbers">&#x3C;form v-on:submit.prevent=&#x22;onSubmit&#x22;&#x3E; ... &#x3C;/form&#x3E;
   </code></pre>
   
   <p>
   در آینده  این ویژگیها را بررسی کرده و نمونه های دیگری از اصلاح کننده ها را برای مدل v-on و v-model مشاهده خواهید کرد.
   </p>
   
   <br>
     
   <h3>    مختصرنویسی(Shorthands)</h3>
   <p>
پیشوند -v به عنوان یک نشانه بصری برای شناسایی ویژگی های خاص Vue در قالب های شما استفاده می شود. این زمانی مفید است که از Vue.js برای اعمال رفتار پویا در برخی از نشانه های(markup) موجود استفاده می کنید ، اما می تواند موجب طولانی شدن  برای برخی از دایرکتیوهایی که اغلب استفاده می شود  گردد. در عین حال نیاز به پیشوند v-، هنگام ساختن SPA ، جایی که Vue هر الگویی را مدیریت می کند اهمیت کمتری می یابد. بنابراین ، Vue برای دو مورد از دایرکتیوهای متداول ، v-bind و v-on اصلاحات ویژه ای را ارائه می دهد:
   </p>
   
  <br>
  <h3> اختصار v-bind</h3>
   
   <pre><code class="language-html  line-numbers">&#x3C;!-- full syntax --&#x3E;
   &#x3C;a v-bind:href=&#x22;url&#x22;&#x3E; ... &#x3C;/a&#x3E;
   
   &#x3C;!-- shorthand --&#x3E;
   &#x3C;a :href=&#x22;url&#x22;&#x3E; ... &#x3C;/a&#x3E;
   
   &#x3C;!-- shorthand with dynamic argument (2.6.0+) --&#x3E;
   &#x3C;a :[key]=&#x22;url&#x22;&#x3E; ... &#x3C;/a&#x3E;
   </code></pre> 
   
  <br>
  <h3> اختصار v-on</h3>
 
   <pre><code class="language-html  line-numbers">&#x3C;!-- full syntax --&#x3E;
&#x3C;a v-on:click=&#x22;doSomething&#x22;&#x3E; ... &#x3C;/a&#x3E;

&#x3C;!-- shorthand --&#x3E;
&#x3C;a @click=&#x22;doSomething&#x22;&#x3E; ... &#x3C;/a&#x3E;

&#x3C;!-- shorthand with dynamic argument (2.6.0+) --&#x3E;
&#x3C;a @[event]=&#x22;doSomething&#x22;&#x3E; ... &#x3C;/a&#x3E;
   </code></pre>    
   
<p>
آنها ممکن است کمی متفاوت از HTML نرمال به نظر برسند ، اما: و @ کاراکترهای برای نام ویژگی ها هستند و همه مرورگرهای دارای پشتیبانی Vue می توانند آن را به درستی تجزیه کنند. علاوه بر این ، آنها در مارک نهایی رندر شده ظاهر نمی شوند. نحو shorthand کاملاً اختیاری است.
</p>      

 <script src="https://cdn.jsdelivr.net/npm/vue"></script>
 <script>
 var app1 = new Vue({
    el: '#app-1',
    data: {
    rawHtml: '<span style="color: red">This should be red.</span>'
    }
 })
 </script>   