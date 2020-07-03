---
layout: documentation-vuejs
title:    اتصال Class و Style
cattitle:   اصول آموزش vuejs
date:   2019-10-04 08:08:42 +0330
jdate: جمعه 12 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/class-and-style.html
---
<h3> بررسی (Class and Style Bindings)</h3>
<p>
یک نیاز مشترک برای اتصال داده ها ، دستکاری در لیست class المان ها و style  درون خطی آن است. از آنجا که هر دو صفت هستند ، می توانیم از v-bind برای دستکاری آنها استفاده کنیم: ما فقط نیاز داریم که رشته نهایی را با عبارات خود ایجاد کنیم. با این حال ، استفاده از  اتصال رشته ها  خسته کننده و مستعد خطا است. به همین دلیل ، Vue ابزار ویژه ای را جهت  استفاده از v-bind  در class و style ارائه می دهد.
</p>
<br>

<h3>Binding HTML Classes</h3>

<h4>استفاده از روش Object Syntax</h4>

<p>
ما می توانیم یک object  را از طریف v-bind:class منتقل کنیم:
</p>
<pre><code class="language-javascript  line-numbers">&#x3C;div v-bind:class=&#x22;{ active: isActive }&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
نحو فوق به این معنی است که فعل بودن کلاس active  توسط مقدار boolean  خاصیت  isActive مشخص خواهد شد.
</p>

<p>همچنین جهت اتصال چندین کلاس می توانیم  بدینصورت عمل کنیم:</p>
<pre><code class="language-html  line-numbers">&#x3C;div
  class=&#x22;static&#x22;
  v-bind:class=&#x22;{ active: isActive, &#x27;text-danger&#x27;: hasError }&#x22;
&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>و داده های زیر:</p>

<pre><code class="language-javascript  line-numbers">data: {
  isActive: true,
  hasError: false
}
</code></pre>

<p>
و بدینصورت  رندر خواهد شد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div class=&#x22;static active&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
زمانیکه مقدار isActive  یا hasError تغییر می کند، لیست class براین اساس بروزرسانی می گردد. برای مثال اگر مقدار  hasError برابر true باشد،  لیست class  بدین صورت خواهد بود : "static active text-danger".
</p>

<p>
لازم نیست object بصورت inline محدود گردد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:class=&#x22;classObject&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
</code></pre>

<p>
این نتیجه همان نتیجه را خواهد داد. ما همچنین می توانیم به یک خاصیت computed  که یک شی را برمی گرداند متصل شویم که الگوی رایج و مناسبی است:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:class=&#x22;classObject&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
</code></pre>

<h4>استفاده از روش Array Syntax</h4>

<p>
ما می توانیم یک آرایه رابه v-bind:class  انتقال دهیم تا یک لیست از کلاس ایجاد نماییم :
</p>
<pre><code class="language-html  line-numbers">&#x3C;div v-bind:class=&#x22;[activeClass, errorClass]&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
</code></pre>

<p>
که بصووت زیر رندر می شود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div class=&#x22;active text-danger&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
اگر بخواهید یک کلاس را بصورت  مشروط از یک لیست انتخاب نمایید می توانید بصورت زیر عمل  نمایید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:class=&#x22;[isActive ? activeClass : &#x27;&#x27;, errorClass]&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
چنانچه isActive برابر true باشد مقدار class برابر activeClass  در غیر اینصورت errorClass خواهد بود.
</p>

<p>
با این وجود ، اگر چندین کلاس مشروط داشته باشید ، می تواند کمی طولانی گردد. به همین دلیل امکان استفاده از object syntax در ترکیب با array syntax نیز وجود دارد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:class=&#x22;[{ active: isActive }, errorClass]&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<h4>استفاده از Components</h4>

<blockquote class="has-icon tip">
در این بخش فرض بر این است که شما با مبحث <a href="Components-Basics" target="_blank" > Vue Component</a> آشنا باشید. 
</blockquote>

<p>
زمانیکه شما از صفت class در یک کامپوننت سفارشی استفاده می کنید، آن کلاس ها به عنصر root کامپوننت اضافه می گردند. کلاس های موجود در این عنصر بازنویسی نخواهند شد.
</p>

<p>
برای مثال چنانچه کامپوننت زیر را اعلام کنیم:
</p>

<pre><code class="language-javascript  line-numbers">Vue.component('my-component', {
  template: '&#x3C;p class=&#x22;foo bar&#x22;&#x3E;Hi&#x3C;/p&#x3E;'
})
</code></pre>
<p>
چنانچه داشته باشیم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;my-component class=&#x22;baz boo&#x22;&#x3E;&#x3C;/my-component&#x3E;
</code></pre>

<p>
HTML رندر شده بدین صورت خواهد بود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;p class=&#x22;foo bar baz boo&#x22;&#x3E;Hi&#x3C;/p&#x3E;
</code></pre>

<p>
در مورد اتصال کلاس نیز همین موضوع صادق است:
</p>

<pre><code class="language-html  line-numbers">&#x3C;my-component v-bind:class=&#x22;{ active: isActive }&#x22;&#x3E;&#x3C;/my-component&#x3E;
</code></pre>

<p>
زمانیکه isActive برابر true باشد، HTML رندر شده بدین صورت خواهد بود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;p class=&#x22;foo bar active&#x22;&#x3E;Hi&#x3C;/p&#x3E;
</code></pre>
<br>

<h4>اتصال  Inline Styles</h4>

<h4>استفاده از Object Syntax</h4>

<p>
استفاده از object syntax برای  v-bind:style کاملاً ساده است - تقریباً شبیه CSS است ، با این تفاوت که یک شیء JavaScript است. می توانید جهت نامگذاری خاصیت های CSS از هریک از روش های  camelCase و یا kebab-case استفاده نمایید :
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:style=&#x22;{ color: activeColor, fontSize: fontSize + &#x27;px&#x27; }&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  activeColor: &#x27;red&#x27;,
  fontSize: 30
}
</code></pre>

<p>
غالباً ایده ی خوبی است که مستقیماً به یک شیء style   متصل شوید تا قالب تمیزتر شود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:style=&#x22;styleObject&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-html  line-numbers">data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
</code></pre>

<p>
روش object syntax اغلب در رابطه با خاصیت های computed  که یک شی را برمی گردانند استفاده می شود.
</p>
<br>
<h4>استفاده از Array Syntax</h4>
<p>
روش array syntax برای  v-bind:style  این اجازه را می دهد تا چندین style objects را اعمال نمایید.
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:style=&#x22;[baseStyles, overridingStyles]&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<h4>Auto-prefixing</h4>
<p>
زمانیکه شما بخواهید یک خاصیت CSS که شامل <a href="https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix" target="_blank"> vendor prefixes</a>  را در  v-bind:style استفاده نمایید، برای مثال (-webkit-،transformT)،Vue بصورت اتوماتیک آنها را شناسایی و پیشوندهای مناسب را به آن ها اضافه و به style اعمال می نماید.
</p>

<br>
<h4>Multiple Values</h4>

<p>
از نسخه 2.3.0+ شما می توانید آرایه ای از پیشوندها را جهت اعمال در خواص Style فراهم نمایید. برای مثال : 
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-bind:style=&#x22;{ display: [&#x27;-webkit-box&#x27;, &#x27;-ms-flexbox&#x27;, &#x27;flex&#x27;] }&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<p>
تنها آخرین مقدار در آرایه که مرورگر از آن پشتیبانی می کند رندر خواهد شد. در این مثال ،display: flex      برای مرورگرهایی که نسخه غیر پیشوند از flexbox را پشتیبانی می کنند رندر خواهد شد.
</p>


