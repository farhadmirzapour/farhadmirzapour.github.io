---
layout: documentation-vuejs
title:    اصول پایه Component
cattitle:   اصول آموزش vuejs
date:   2019-10-11 22:08:42 +0330
jdate: جمعه 19 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/components.html
---

<h3>  مثال پایه (Base Example)</h3>

<p>در این قسمت یک مثال از یک کامپوننت را مشاهده می کنید:</p>
<pre><code class="language-javascript  line-numbers">// Define a new component called button-counter
Vue.component(&#x27;button-counter&#x27;, {
  data: function () {
    return {
      count: 0
    }
  },
  template: &#x27;&#x3C;button v-on:click=&#x22;count++&#x22;&#x3E;You clicked met &#123;&#123; count &#125;&#125; times.&#x3C;/button&#x3E;&#x27;
})
</code></pre>

<p>
کامپوننت ها با نام خود قابلیت استفاده مجدد در نمونه های vue را دارند. در این مثال &#x3C;button-counter&#x3E; .می توانیم از این کامپوننت به عنوان یک عنصر سفارشی در نمونه ایجاد شده Vue استفاده کنیم :
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;components-demo&#x22;&#x3E;
  &#x3C;button-counter&#x3E;&#x3C;/button-counter&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({ el: '#components-demo' })
</code></pre>

<div id="app1" class="result-example">
   <button-counter></button-counter>
</div>

<p>
از آنجا که کامپوننت ها قابلیت استفاده مجدد در نمونه های Vue را دارا می باشند، آنها تعدادی خاصیت را در یک new Vue همانند data, computed, watch, methods و lifecycle hooks را می پذیرند.
</p>
<br>

<h3>  استقاده مجدد از کامپوننت ها (Reusing Components)</h3>
<p>
کامپوننت ها را می توانید هربار که بخواهید استفاده نمایید :
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;components-demo&#x22;&#x3E;
  &#x3C;button-counter&#x3E;&#x3C;/button-counter&#x3E;
  &#x3C;button-counter&#x3E;&#x3C;/button-counter&#x3E;
  &#x3C;button-counter&#x3E;&#x3C;/button-counter&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<div id="app2" class="result-example">
   <button-counter></button-counter>
   <button-counter></button-counter>
   <button-counter></button-counter>
</div>

<p>
توجه کنید که با کلیک بر روی دکمه ها ، هر یک مقدار جداگانه خود را حفظ می کنند. به این دلیل است که هر بار که از یک کامپوننت استفاده می کنید ، نمونه جدیدی از آن ایجاد می شود.
</p>

<h4>  خاصیت data باید بصورت تابع پیاده سازی شود (data Must Be a Function)</h4>
<p>
وقتی ما عنصر  &#x3C;button-counter&#x3E; را تعریف کردیم ، ممکن است متوجه شده باشید که داده ها به طور مستقیم یک شی را ارائه نمی دهند ، مانند این:
</p>

<pre><code class="language-json  line-numbers">data: {
  count: 0
}
</code></pre>

<p>
در عوض ، خاصیت data یک کامپوننت باید یک تابع باشد ، به طوری که هر نمونه می تواند یک نسخه مستقل از شی داده داده شده را حفظ کند:
</p>

<pre><code class="language-javascript  line-numbers">data: function () {
  return {
    count: 0
  }
}
</code></pre>

<p>
اگر Vue این قانون را نداشت ، کلیک کردن بر روی یک دکمه می توانست بر داده های سایر موارد دیگر  تأثیر بگذارد.
</p>

<br>

<h4>  سازماندهی کامپوننت ها (Organizing Components)</h4>
<p>
معمولا برنامه ها به صورت قطعات و اجزای تو در تو و درختی پیاده سازی می شوند :
</p>

<p>
<img src="/images/post/vuejs/components.png" alt="ساختار app" />
</p>

<p>
به عنوان مثال ، شما ممکن است کامپوننت هایی  برای header ،  sidebar و همچنین نمایش محتوا داشته باشید که معمولاً خود شامل اجزای دیگری همانند لینک ها، پست های وبلاگ و غیره است.
</p>

<p>
جهت استفاده از کامپوننت ها در قالب ها ، آنها باید ابتدا ثبت شوند تا Vue از وجود آنها آگاه شود. دو نوع روش ثبت کامپوننت وجود دارد: سراسری و محلی. تاکنون توانستیم با استفاده از Vue.component  کامپوننت هایی را بصورت سراسری ایجاد کنیم :
</p>

<pre><code class="language-javascript  line-numbers">Vue.component('my-component-name', {
  // ... options ...
})
</code></pre>

<p>
کامپوننت هایی که بصورت سراسری ایجاد می شوند، می توانند در قالب نمونه های اصلی (new Vue) ایجاد شده و حتی در داخل همه subcomponents مورد استفاده قرار گیرند.
</p>

<br>

<h3>  انتقال داده به کامپوننت ها با استفاده از Props(Passing Data to Child Components with Props)</h3>
<p>
در ابتدا ، ما به ایجاد یک کامپوننت  برای نمایش  پست های وبلاگ اشاره کردیم. جهت انتقال داده هایی همانند عنوان و محتوای پست به کامپوننت، می توانیم از Props استفاده نماییم. 
</p>
<p>
Props ویژگی های سفارشی هستند که می توانید روی یک کامپوننت ثبت نمایید.
</p>

<pre><code class="language-javascript  line-numbers">Vue.component(&#x27;blog-post&#x27;, {
  props: [&#x27;title&#x27;],
  template: &#x27;&#x3C;h3&#x3E;&#123;&#123; title &#125;&#125;&#x3C;/h3&#x3E;&#x27;
})
</code></pre>

<p>
یک کامپوننت می تواند دارای تعداد زیادی props باشد و مقادیر می توانند به props انتقال یابند.  در قالب بالا ، خواهید دید که ما می توانیم دقیقاً به این مقادیر همانند data دسترسی داشته باشیم.
</p>

<p>
پس از ثبت props ، می توانید داده ها را به عنوان یک ویژگی سفارشی همانند زیر منتقل کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post title=&#x22;My journey with Vue&#x22;&#x3E;&#x3C;/blog-post&#x3E;
&#x3C;blog-post title=&#x22;Blogging with Vue&#x22;&#x3E;&#x3C;/blog-post&#x3E;
&#x3C;blog-post title=&#x22;Why Vue is so fun&#x22;&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<div id="app3" class="result-example" style="direction: ltr;text-align: left">
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
</div>
<p>
همچنین چنانچه مجموعه داده هایی بدین صورت داشته باشیم:
</p>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
</code></pre>

<p>سپس جهت رند کامپوننت برای هر یک از آنها بدین صورت پیاده سازی می کنیم:</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  v-for=&#x22;post in posts&#x22;
  v-bind:key=&#x22;post.id&#x22;
  v-bind:title=&#x22;post.title&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<div id="app4" class="result-example">
   <blog-post v-for="post in posts" v-bind:title="post.title" v-bind:key="post.id"></blog-post>
</div>

<p>
در مثال بالا مشاهده کردید که می توانیم از v-bind  جهت انتقال مقادیر بصورت داینامیک استفاده کنیم. این امر به ویژه هنگامی مفید است که شما محتوای دقیقی که می خواهید ارائه دهید را نمی دانید ، مانند هنگام ارسال پیامک از یک API.
</p>
<br>

<h4>  بررسی  قالب های چند خطی و المان ریشه  (A Single Root Element)</h4>
<p>
هنگام ساخت کامپوننت  &#x3C;blog-post&#x3E; در مثال قبل،  قالب شما حاوی مواردی غیر از عنوان همانند محتوا ، تاریخ ایجاد پست و ... می باشد : 
</p>

<pre><code class="language-html  line-numbers">&#x3C;h3&#x3E;&#123;&#123; title &#125;&#125;&#x3C;/h3&#x3E;
&#x3C;div v-html=&#x22;content&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
اگر این مورد را در الگوی خود امتحان کنید ، Vue خطایی با  متن  هر مؤلفه باید یک عنصر ریشه واحد داشته باشد را نمایش خواهد داد. می توانید با قرار دادن قالب خود در یک المان والد  مشکل را رفع نمایید :
</p>

<pre><code class="language-html  line-numbers">&#x3C;div class=&#x22;blog-post&#x22;&#x3E;
  &#x3C;h3&#x3E;&#123;&#123; title &#125;&#125;&#x3C;/h3&#x3E;
  &#x3C;div v-html=&#x22;content&#x22;&#x3E;&#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
با بزرگ شدن کامپوننت ، به احتمال زیاد به غیر از عنوان و محتوای یک پست به نمایش عناصر دیگری همچون تاریخ انتشار ، نظرات و ..  نیز احتیاج خواهیم داشت. تعریف prop برای هر قطعه از اطلاعات مرتبط می تواند بسیار آزار دهنده باشد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  v-for=&#x22;post in posts&#x22;
  v-bind:key=&#x22;post.id&#x22;
  v-bind:title=&#x22;post.title&#x22;
  v-bind:content=&#x22;post.content&#x22;
  v-bind:publishedAt=&#x22;post.publishedAt&#x22;
  v-bind:comments=&#x22;post.comments&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<p>
بنابراین می توانیم کامپوننت بالا را مجددا  دوباره سازی کرده تا تنها نیاز به تعریف یک prop داشته باشیم : 
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  v-for=&#x22;post in posts&#x22;
  v-bind:key=&#x22;post.id&#x22;
  v-bind:post=&#x22;post&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">Vue.component(&#x27;blog-post&#x27;, {
  props: [&#x27;post&#x27;],
  template: &#x60;
    &#x3C;div class=&#x22;blog-post&#x22;&#x3E;
      &#x3C;h3&#x3E;&#123;&#123; post.title &#125;&#125;&#x3C;/h3&#x3E;
      &#x3C;div v-html=&#x22;post.content&#x22;&#x3E;&#x3C;/div&#x3E;
    &#x3C;/div&#x3E;
  &#x60;
})
</code></pre>

<blockquote class="has-icon tip">
مثال فوق و برخی مثال ها در آینده از <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals" target="_blank">template literal</a> جاوا اسکریپت جهت خواندن قالب های چند خطی استفاده می نمایند که  توسط Internet Explorer (IE) پشتیبانی نمی شوند ، چنانچه بخواهیم در IE پشتیبانی شود از <a href=" https://css-tricks.com/snippets/javascript/multiline-string-variables-in-javascript/" target="_blank"> newline escapes</a> استفاده نمایید.
</blockquote>
<br>

<h4>  اجرای رویدادهای کامپوننت های فرزند  (Listening to Child Components Events)</h4>
<p>
هنگامی که کامپوننت &#x3C;blog-post&#x3E; را توسعه می دهیم ، برخی از ویژگیها ممکن است نیاز به برقراری ارتباط با والدین داشته باشند. به عنوان مثال ، ممکن است تصمیم بگیریم قابلیت بزرگتر کردن متن پست های وبلاگ را اضافه نماییم : 
</p>
<p>
می توانیم با اضافه کردن خاصیت   postFontSize به data در والد،  این عمل را انجام نماییم :
</p>
<pre><code class="language-javascript  line-numbers">new Vue({
  el: &#x27;#blog-posts-events-demo&#x27;,
  data: {
    posts: [/* ... */],
    postFontSize: 1
  }
})
</code></pre>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;blog-posts-events-demo&#x22;&#x3E;
  &#x3C;div :style=&#x22;{ fontSize: postFontSize + &#x27;em&#x27; }&#x22;&#x3E;
    &#x3C;blog-post
      v-for=&#x22;post in posts&#x22;
      v-bind:key=&#x22;post.id&#x22;
      v-bind:post=&#x22;post&#x22;
    &#x3E;&#x3C;/blog-post&#x3E;
  &#x3C;/div&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
اکنون بیایید یک دکمه برای بزرگنمایی متن درست قبل از محتوای هر پست اضافه کنیم:
</p>

<pre><code class="language-javascript  line-numbers">Vue.component(&#x27;blog-post&#x27;, {
  props: [&#x27;post&#x27;],
  template: &#x60;
    &#x3C;div class=&#x22;blog-post&#x22;&#x3E;
      &#x3C;h3&#x3E;&#123;&#123; post.title &#125;&#125;&#x3C;/h3&#x3E;
      &#x3C;button&#x3E;
        Enlarge text
      &#x3C;/button&#x3E;
      &#x3C;div v-html=&#x22;post.content&#x22;&#x3E;&#x3C;/div&#x3E;
    &#x3C;/div&#x3E;
  &#x60;
})
</code></pre>

<p>
مشکل این است که این دکمه کاری نمی کند:
</p>

<pre><code class="language-html  line-numbers">&#x3C;button&#x3E;
  Enlarge text
&#x3C;/button&#x3E;
</code></pre>

<p>
زمانیکه روی دکمه کلیک می کنیم ، باید به والدین ارتباط برقرار کرده تا متن تمام پست ها را بزرگتر کند. خوشبختانه ، نمونه های Vue سیستم رویدادهای سفارشی را برای حل این مشکل ارائه می دهند. کامپوننت والد می تواند با استفاده از دایرکتیو v-on انتخاب نماید که در بین رویدادها در نمونه کامپوننت فرزند به کدامیک گوش دهد. دقیقاً همانطور که با یک رویداد محلی DOM انجام می دهیم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  ...
  v-on:enlarge-text=&#x22;postFontSize += 0.1&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<p>
سپس کامپوننت فرزند می تواند با فرخوانی متد emit$، نام رویداد را منتقل می کند:
</p>

<pre><code class="language-html  line-numbers">&#x3C;button v-on:click=&#x22;$emit(&#x27;enlarge-text&#x27;)&#x22;&#x3E;
  Enlarge text
&#x3C;/button&#x3E;
</code></pre>

<p>
کامپوننت والد با استفاده از v-on :large-text = "postFontSize + = 0.1" این رویداد را دریافت کرده و مقدار postFontSize را به روز می کنند.
</p>

<div class="result-example">
<div id="app5">
    <div :style="{ fontSize:postFontSize+'em' }">
    <blog-post2 v-for="post in posts" v-bind:key="post.id" v-bind:post="post" v-on:enlarge-text="postFontSize += 0.1" ></blog-post2>
    </div>
</div>
</div>


<h4>  انتشار یک مقدار با رویداد  (Emitting a Value With an Event)</h4>
<p>
انتشار مقادیر خاص با یک رویداد گاهی اوقات مفید است. به عنوان مثال ، ممکن است بخواهیم کامپوننت &#x3C;blog-post&#x3E; مسئولیت این را داشته باشد که متن را تا چه اندازه بزرگنمایی کند. در این موارد ، ما می توانیم از دومین پارامتر متد  emit$  استفاده نماییم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;button v-on:click=&#x22;$emit(&#x27;enlarge-text&#x27;, 0.1)&#x22;&#x3E;
  Enlarge text
&#x3C;/button&#x3E;
</code></pre>

<p>
زمانیکه  ما به رویداد در والد گوش فرا می دهیم ، می توانیم به مقدار رویداد با استفاده از event$ دسترسی داشته باشیم :
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  ...
  v-on:enlarge-text=&#x22;postFontSize += $event&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<p>
یا اگر کنترل کننده رویداد یک متد باشد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;blog-post
  ...
  v-on:enlarge-text=&#x22;onEnlargeText&#x22;
&#x3E;&#x3C;/blog-post&#x3E;
</code></pre>

<p>
سپس مقدار به اولین پارامتر متد انتقال می یابد:
</p>

<pre><code class="language-javascript  line-numbers">new Vue({
        el: '#blog-posts-events-demo',
        data: {
            posts: [/* ... */],
            postFontSize : 1
        },
        methods: {
            onEnlargeText: function (enlargeAmount) {
                this.postFontSize += enlargeAmount
            }
        }
    })
</code></pre>

<br>

<h4> استفاده از v-model   (Using v-model on Components)</h4>

<p>
رویدادهای سفارشی همچنین می توانند در ایجاد ورودی های سفارشی که با v-model کار می کنند استفاده شوند. به خاطر دارید که :
</p>

<pre><code class="language-html  line-numbers">&#x3C;input v-model=&#x22;searchText&#x22;&#x3E;
</code></pre>

<p>
همان کاری را انجام می دهد که:
</p>

<pre><code class="language-html  line-numbers">&#x3C;input
  v-bind:value=&#x22;searchText&#x22;
  v-on:input=&#x22;searchText = $event.target.value&#x22;
&#x3E;
</code></pre>

<p>
در صورت استفاده از v-model در کامپوننت چنین عمل می کند :
</p>

<pre><code class="language-html  line-numbers">&#x3C;custom-input
  v-bind:value=&#x22;searchText&#x22;
  v-on:input=&#x22;searchText = $event&#x22;
&#x3E;&#x3C;/custom-input&#x3E;
</code></pre>

<p>
جهت اجرای کار input باید چنین عمل کند :
</p>
<ul>
<li>مقید کردن مقدار خاصیت به مقدار prop</li>
<li>در ورودی ، رویداد ورودی سفارشی خود را با مقدار جدید منتشر کنید</li>
</ul>

<p>
در عمل چنین خواهیم داشت :
</p>

<pre><code class="language-javascript  line-numbers">Vue.component(&#x27;custom-input&#x27;, {
  props: [&#x27;value&#x27;],
  template: &#x60;
    &#x3C;input
      v-bind:value=&#x22;value&#x22;
      v-on:input=&#x22;$emit(&#x27;input&#x27;, $event.target.value)&#x22;
    &#x3E;
  &#x60;
})
</code></pre>

<p>
اکنون v-model باید با این کامپوننت کاملاً کار کند:
</p>

<pre><code class="language-html  line-numbers">&#x3C;custom-input v-model=&#x22;searchText&#x22;&#x3E;&#x3C;/custom-input&#x3E;
</code></pre>

<br>

<h4> انتشار محتوا با Slots (Content Distribution with Slots)</h4>
<p>
درست همانند عناصر HTML ، بسیار مفید است که بتوانید محتوای را به یک کامپوننت منتقل کنید ، مانند زیر:
</p>

<pre><code class="language-html  line-numbers">&#x3C;alert-box&#x3E;
  Something bad happened.
&#x3C;/alert-box&#x3E;
</code></pre>

<p>
که ممکن است چیزی مانند زیر باشد:
</p>

<p>
خوشبختانه ، این کار توسط عنصر &#x3C;slot&#x3E; سفارشی Vue انجام شده است:
</p>

<pre><code class="language-javascript  line-numbers">Vue.component(&#x27;alert-box&#x27;, {
  template: &#x60;
    &#x3C;div class=&#x22;demo-alert-box&#x22;&#x3E;
      &#x3C;strong&#x3E;Error!&#x3C;/strong&#x3E;
      &#x3C;slot&#x3E;&#x3C;/slot&#x3E;
    &#x3C;/div&#x3E;
  &#x60;
})
</code></pre>

<p>
همانطور که در بالا مشاهده  میکنید ، ما فقط slot را جایی که می خواهیم آن را استفاده کنیم قرار می دهیم و تمام !
</p>

<div id="app6" class="result-example">
    <alert-box>
        Something bad happened.
    </alert-box>
</div>


<br>

<h4> نکاتی در عملیات تجزیه قالب  (DOM Template Parsing Caveats)</h4>

<p>
برخی از عناصر HTML مانند &#x3C;ul&#x3E; &#x60C; &#x3C;ol&#x3E; &#x60C; &#x3C;table&#x3E; &#x648; &#x3C;select&#x3E; محدودیت هایی در مورد عناصر موجود در داخل آنها دارند و برخی از عناصر مانند &#x3C;li&#x3E; &#x60C; &#x3C;tr&#x3E; و &#x3C;option&#x3E; فقط می توانند در داخل بعضی از عناصر دیگر ظاهر شوندد.
</p>

<p>
این مسئله هنگام استفاده از کامپوننت هایی با چنین عناصری محدودیت هایی به دنبال خواهد داشت. مثلا:
</p>

<pre><code class="language-html  line-numbers">&#x3C;table&#x3E;
  &#x3C;blog-post-row&#x3E;&#x3C;/blog-post-row&#x3E;
&#x3C;/table&#x3E;
</code></pre>

<p>
کامپوننت سفارشی &#x3C;blog-post-row&#x3E; به عنوان محتوای نامعتبر حذف می شود و باعث ایجاد خطا در خروجی نهایی ارائه شده می شود. خوشبختانه ،  ویژگی ویژه is یک راه حل ارائه می دهد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;table&#x3E;
  &#x3C;tr is=&#x22;blog-post-row&#x22;&#x3E;&#x3C;/tr&#x3E;
&#x3C;/table&#x3E;
</code></pre>

<p>
لازم به ذکر است در صورت استفاده از قالب های رشته از یکی از منابع زیر این محدودیت اعمال نمی شود:
</p>
<div style="text-align: left">
<ul style="direction: ltr;text-align: left">
<li>
String templates (e.g. template: '...')
</li>
<li>
Single-file (.vue) components
</li>
<li>
&#x3C;script type=&#x22;text/x-template&#x22;&#x3E;
</li>
</ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<style>
.demo-alert-box {
    padding: 10px 20px;
    background: #f3beb8;
    border: 1px solid #f09898;
}
</style>
<script>
    Vue.component('button-counter',{
        data: function(){
                return {
                    count:0
                };
            },
        template : '<button v-on:click="count++" >You clicked me &#123;&#123; count &#125;&#125; times.</button>'
    })
    Vue.component('blog-post',{
        props:['title'],
        template : '<p>&#123;&#123; title &#125;&#125;</p>'
    })
    
    Vue.component('blog-post2',{
        props: ['post'],
        template: `<div >
                    <h1>&#123;&#123; post.title }}</h1>
                    <button v-on:click="$emit('enlarge-text')">Enlarge Text</button>
                    <div v-html="post.content"></div>
                    </div>
                  `
    })
Vue.component('alert-box', {
    template: `
    <div class="demo-alert-box">
      <strong>Error!</strong>
      <slot></slot>
    </div>
  `
})
    var app1 = new Vue({
        el: '#app1'
    })
    var app2 = new Vue({
        el: '#app2'
    });    
    var app3 = new Vue({
        el: '#app3'
    });   
    var app4 = new Vue({
        data: {
            posts: [
                { id: 1, title: 'My journey with Vue' },
                { id: 2, title: 'Blogging with Vue' },
                { id: 3, title: 'Why Vue is so fun' }
            ]
        },    
        el: '#app4'
    });
    
    var app5 = new Vue({
        el: '#app5',
        data: {
            posts: [
                      { id: 1, title: 'My journey with Vue' },
                      { id: 2, title: 'Blogging with Vue' },
                      { id: 3, title: 'Why Vue is so fun' }
            ],
            postFontSize : 1
        }
    });    
  
      var app6 = new Vue({
          el: '#app6'
      })    
    
</script>