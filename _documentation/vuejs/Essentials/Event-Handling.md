---
layout: documentation-vuejs
title:   کار با رویدادها(Event Handling)
cattitle:   اصول آموزش vuejs
date:   2019-10-11 09:21:42 +0330
jdate: جمعه 19 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/events.html
---
<h3>گوش دادن به وقایع (Listening to Events)</h3>
<p>
ما می توانیم از دستورالعمل v-on برای گوش دادن به رویدادهای DOM استفاده کنیم و برخی از کدهای جاوا اسکریپت را در هنگام بروز آنها اجرا کنیم.
</p>

<p>
برای مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;example-1&#x22;&#x3E;
  &#x3C;button v-on:click=&#x22;counter += 1&#x22;&#x3E;Add 1&#x3C;/button&#x3E;
  &#x3C;p&#x3E;The button above has been clicked &#123;&#123; counter &#125;&#125; times.&#x3C;/p&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
</code></pre>

<p>نتیجه:</p>
<div id="app1" class="result-example">
<button v-on:click="counter += 1">Add 1</button>
  <p>The button above has been clicked &#123;&#123; counter &#125;&#125; times.</p>
</div>


<br>

<h3>متدهای کار با رویدادها (Method Event Handlers)</h3>
<p>
منطق برای بسیاری از اداره کنندگان رویدادها پیچیده تر خواهد شد ، بنابراین نگه داشتن کد JavaScript در مقدار  ویژگی v-on امکان پذیر نیست. به همین دلیل v-on می تواند نام متدی را که می خواهید  آن را فراخوانی نیز بپذیرد.
</p>
<p>
برای مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;example-2&#x22;&#x3E;
  &#x3C;!-- &#x60;greet&#x60; is the name of a method defined below --&#x3E;
  &#x3C;button v-on:click=&#x22;greet&#x22;&#x3E;Greet&#x3C;/button&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // define methods under the `methods` object
  methods: {
    greet: function (event) {
      // `this` inside methods points to the Vue instance
      alert('Hello ' + this.name + '!')
      // `event` is the native DOM event
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// you can invoke methods in JavaScript too
example2.greet() // => 'Hello Vue.js!'
</code></pre>
<div id="app2" class="result-example">
  <!-- `greet` is the name of a method defined below -->
  <button v-on:click="greet">Greet</button>
</div>
<br>

<h3>کار با متدهای درون خطی (Methods in Inline Handlers)</h3>
<p>
به جای اتصال مستقیم به نام متد ، می توانیم از متدهایی در یک عبارت JavaScript به صورت خطی (inline)  نیز استفاده کنیم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;example-3&#x22;&#x3E;
  &#x3C;button v-on:click=&#x22;say(&#x27;hi&#x27;)&#x22;&#x3E;Say hi&#x3C;/button&#x3E;
  &#x3C;button v-on:click=&#x22;say(&#x27;what&#x27;)&#x22;&#x3E;Say what&#x3C;/button&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
</code></pre>

<p>
نتیجه:
</p>

<div id="app3" class="result-example">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>

<p>
بعضی اوقات ما نیاز داریم در یک کنترل کننده دستور داخلی  (inline statement handler)به رویداد اصلی DOM نیز دسترسی پیدا کنیم. می توانید با استفاده از متغیر ویژه $event آن رویداد را به متد منتقل کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;button v-on:click=&#x22;warn(&#x27;Form cannot be submitted yet.&#x27;, $event)&#x22;&#x3E;
  Submit
&#x3C;/button&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">// ...
methods: {
  warn: function (message, event) {
    // now we have access to the native event
    if (event) event.preventDefault()
    alert(message)
  }
}
</code></pre>
<div id="app4" class="result-example">
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
</div>
<br>

<h3>اصلاح کننده رویداد (Event Modifiers)</h3>

<p>
این یک نیاز بسیار متداول است که متدهای ()event.preventDefault   یا ()event.stopPropagation   را در مدیریت رویداد به کار بگیرید. اگرچه ما می توانیم این کار را به راحتی در داخل متدها انجام دهیم ، اما بهتر  است که متدها صرفاً در مورد منطق داده ها باشند نه اینکه بخواهند با جزئیات رویداد DOM سروکار داشته باشند.
</p>

<p>
برای رفع این مشکل ،  Vue اصلاح کننده رویداد  (event modifiers ) را برای v-on فراهم می کند. به یاد بیاورید که event modifiers پسوندهایی با عنوان هستند که توسط یک نقطه مشخص می شوند.
</p>

<p style="direction: ltr;text-align: left">
<ul style="direction: ltr;text-align: left">
<li><code>.stop</code></li>
<li><code>.prevent</code></li>
<li><code>.capture</code></li>
<li><code>.self</code></li>
<li><code>.once</code></li>
<li><code>.passive</code></li>
</ul>
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- the click event&#x27;s propagation will be stopped --&#x3E;
&#x3C;a v-on:click.stop=&#x22;doThis&#x22;&#x3E;&#x3C;/a&#x3E;

&#x3C;!-- the submit event will no longer reload the page --&#x3E;
&#x3C;form v-on:submit.prevent=&#x22;onSubmit&#x22;&#x3E;&#x3C;/form&#x3E;

&#x3C;!-- modifiers can be chained --&#x3E;
&#x3C;a v-on:click.stop.prevent=&#x22;doThat&#x22;&#x3E;&#x3C;/a&#x3E;

&#x3C;!-- just the modifier --&#x3E;
&#x3C;form v-on:submit.prevent&#x3E;&#x3C;/form&#x3E;

&#x3C;!-- use capture mode when adding the event listener --&#x3E;
&#x3C;!-- i.e. an event targeting an inner element is handled here before being handled by that element --&#x3E;
&#x3C;div v-on:click.capture=&#x22;doThis&#x22;&#x3E;...&#x3C;/div&#x3E;

&#x3C;!-- only trigger handler if event.target is the element itself --&#x3E;
&#x3C;!-- i.e. not from a child element --&#x3E;
&#x3C;div v-on:click.self=&#x22;doThat&#x22;&#x3E;...&#x3C;/div&#x3E;
</code></pre>

<blockquote class="has-icon tip">
هنگام استفاده از modifiers ، موارد مهم را مرتب کنید زیرا کد مربوطه به همان ترتیب تولید می شود. بنابراین استفاده از v-on:click.prevent.self  از کلیه کلیکها جلوگیری می کند در حالی که v-on:click.self.prevent   فقط از کلیک روی خود عنصر جلوگیری می کند.
</blockquote>

<p>
در نسخه New in 2.1.4+
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- the click event will be triggered at most once --&#x3E;
&#x3C;a v-on:click.once=&#x22;doThis&#x22;&#x3E;&#x3C;/a&#x3E;
</code></pre>

<p>
بر خلاف اصلاح کننده های دیگر ، که منحصر به رویدادهای DOM بومی هستند ، از اصلاح کننده  once.  نیز می توانید در رویدادهای کامپوننت استفاده کنید.
</p>

<p>
در نسخه New in 2.3.0+
</p>


<pre><code class="language-html  line-numbers">&#x3C;!-- the scroll event&#x27;s default behavior (scrolling) will happen --&#x3E;
&#x3C;!-- immediately, instead of waiting for &#x60;onScroll&#x60; to complete  --&#x3E;
&#x3C;!-- in case it contains &#x60;event.preventDefault()&#x60;                --&#x3E;
&#x3C;div v-on:scroll.passive=&#x22;onScroll&#x22;&#x3E;...&#x3C;/div&#x3E;
</code></pre>

<p>
اصلاح کننده passive.  مخصوصاً برای بهبود عملکرد در دستگاه های تلفن همراه مفید است.
</p>

<blockquote class="has-icon tip">
از  passive.  و .prevent  با یکدیگر استفاده نکنید ، زیرا prevent.  نادیده گرفته می شود و مرورگر شما احتمالاً هشداری را به شما نشان می دهد. به یاد داشته باشید ، passive.  با مرورگر ارتباط برقرار می کند که نمی خواهید از عملکرد پیش فرض رویداد جلوگیری کند.
</blockquote>

<br>

<h3>کار با کیبورد  (Key Modifiers)</h3>

<p>
هنگام گوش دادن به رویدادهای صفحه کلید ، اغلب باید کلیدهای خاص را بررسی کنیم. Vue اجازه می دهد تا هنگام گوش دادن به رویدادهای کلیدی ، اصلاح کننده های کیبورد (key modifiers) را برای v-on اضافه کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- only call &#x60;vm.submit()&#x60; when the &#x60;key&#x60; is &#x60;Enter&#x60; --&#x3E;
&#x3C;input v-on:keyup.enter=&#x22;submit&#x22;&#x3E;
</code></pre>

<p>
می توانید مستقیماً از نامهای کلید معتبری که از طریق KeyboardEvent.key در معرض مدیریت هستند با تغییر نام آنها بصورت  kebab-case استفاده کنید.
</p>

<pre><code class="language-html  line-numbers">&#x3C;input v-on:keyup.page-down=&#x22;onPageDown&#x22;&#x3E;
</code></pre>

<p>
در مثال بالا، مدیریت کننده رویداد زمانی فراخوانی می شود که event.key$  برابر با 'PageDown' باشد.
</p>

<br>

<h4>استفاده از   (Key Codes)</h4>

<blockquote class="has-icon tip">
استفاده از رویدادهای keyCode  ممکن است در مرورگرهای جدید پشتیبانی نشود.
</blockquote>

<p>
استفاده از ویژگی های keyCode نیز مجاز است:
</p>

<pre><code class="language-html  line-numbers">&#x3C;input v-on:keyup.13=&#x22;submit&#x22;&#x3E;
</code></pre>

<p>
Vue در صورت لزوم برای پشتیبانی از مرورگرها ، نامهای متداول را برای متداول ترین کدهای کلیدی فراهم می کند:
</p>

<p style="direction: ltr;text-align: left">
<ul style="direction: ltr;text-align: left">
<li><code>.enter</code></li>
<li><code>.tab</code></li>
<li><code>.delete</code> (captures both “Delete” and “Backspace” keys)</li>
<li><code>.esc</code></li>
<li><code>.space</code></li>
<li><code>.up</code></li>
<li><code>.down</code></li>
<li><code>.left</code></li>
<li><code>.right</code></li>
</ul>
</p>

<p>
چند کلید (esc. و تمام کلیدهای جهت دار) در IE9 دارای مقادیر کلیدی متناقض هستند ، بنابراین در صورت نیاز به پشتیبانی از IE9 ، این مستعارهای داخلی ترجیح داده می شوند.
</p>

<p>
همچنین می توانید نام مستعار اصلاح کننده کلید سفارشی   (<a href="https://vuejs.org/v2/api/#keyCodes"  target="_blank"  >define custom key modifier aliases</a>) را از طریق شیء سراسری config.keyCodes تعریف کنید:
</p>

<pre><code class="language-javascript  line-numbers">// enable `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
</code></pre>

<br>

<h3>System Modifier Keys</h3>

<p>
در New in 2.1.0+ :
</p>

<p>
شما می توانید از modifiers زیر جهت شنود رویدادهای موس یا کیبورد فقط زمانیکه کلید اصلاح کننده مربوطه فشرده شد استفاده نمایید:
</p>

<p style="direction: ltr;text-align: left">
<ul style="direction: ltr;text-align: left">
<li><code>.ctrl</code></li>
<li><code>.alt</code></li>
<li><code>.shift</code></li>
<li><code>.meta</code></li>
</ul>
</p>

<blockquote class="has-icon tip">
توجه: در کیبوردهایMacintosh ، دکمه meta کلید (⌘) است. در کیبوردهای ویندوز ، دکمه meta کلید (⊞) است. در کیبوردSun Microsystems ، متا به عنوان یک الماس جامد (◆) مشخص شده است. در کیبوردهای خاص ، به طور خاص صفحه کلیدها و جانشینان دستگاههای MIT و Lisp ، مانند کیبورد نایت ، کیبوردcadet فضایی ، متا با عنوان "META" شناخته می شوند. در کیبوردهایSymbolics ، متا با "META" یا "متا" شناخته می شوند.
</blockquote>

<p>
برای مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- Alt + C --&#x3E;
&#x3C;input @keyup.alt.67=&#x22;clear&#x22;&#x3E;

&#x3C;!-- Ctrl + Click --&#x3E;
&#x3C;div @click.ctrl=&#x22;doSomething&#x22;&#x3E;Do something&#x3C;/div&#x3E;
</code></pre>

<blockquote class="has-icon tip">
توجه داشته باشید که کلیدهای اصلاح کننده با کلیدهای معمولی متفاوت هستند و در هنگام استفاده از وقایع Keyup ، هنگام انتشار رویداد باید فشرده شوند. به عبارت دیگر ، keyup.ctrl تنها در صورت رها کردن کلید در حالی که ctrl را رها می کنید ، شروع می شود. اگر کلید ctrl را به تنهایی آزاد کنید ، رویدادی ایجاد نمی شود. اگر چنین رفتاری را می خواهید ، به جای آن از کلید keycode برای ctrl استفاده کنید: keyup.17.
</blockquote>

<br>

<h3>.exact Modifier</h3>
<p>
در نسخه New in 2.5.0+ :
</p>

<p>
اصلاح کننده exact.  اجازه می دهد تا کنترل ترکیب دقیق اصلاح کننده های سیستم مورد نیاز برای ایجاد یک رویداد را کنترل کنید.
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- this will fire even if Alt or Shift is also pressed --&#x3E;
&#x3C;button @click.ctrl=&#x22;onClick&#x22;&#x3E;A&#x3C;/button&#x3E;

&#x3C;!-- this will only fire when Ctrl and no other keys are pressed --&#x3E;
&#x3C;button @click.ctrl.exact=&#x22;onCtrlClick&#x22;&#x3E;A&#x3C;/button&#x3E;

&#x3C;!-- this will only fire when no system modifiers are pressed --&#x3E;
&#x3C;button @click.exact=&#x22;onClick&#x22;&#x3E;A&#x3C;/button&#x3E;
</code></pre>

<br>

<h3>Mouse Button Modifiers</h3>
<p>
در نسخه New in 2.2.0+ :
</p>

<p style="direction: ltr;text-align: left">
<ul style="direction: ltr;text-align: left">
<li><code>.left</code></li>
<li><code>.right</code></li>
<li><code>.middle</code></li>
</ul>
</p>

<p>
این اصلاح کننده ها کنترل کننده ها را محدود به حوادثی می کنند که توسط یک دکمه ماوس خاص ایجاد می شوند.
</p>

<br>

<h3>چرا از شنودندگان در HTML استفاده می کنیم؟</h3>

<P>
ممکن است شما نگران باشید که تمام این رویکرد گوش دادن به رویداد ، قوانین قدیمی خوب درباره "جدایی نگرانی ها - separation of concerns " را نقض می کند. مطمئن باشید - از آنجا که تمام عملکردها و اصطلاحات انتقال دهنده Vue کاملاً محدود به ViewModel هستند که نمای فعلی را کنترل می کند ، هیچ مشکلی برای نگهداری نخواهد داشت. در حقیقت ، استفاده از v-on فواید زیادی دارد:
</P>

<p>
<ul>
<li>
پیاده سازی تابع کنترل کننده بهمراه کد JS شما با استفاده از قالب html ساده تر است.
</li>
<li>
از آنجا که لازم نیست شنوندگان رویداد را به صورت دستی در JS وصل کنید ، کد ViewModel شما می تواند منطقی ناب و عاری از DOM باشد. این باعث می شود آزمایش آسان تر شود.
</li>
<li>
هنگامی که یک ViewModel از بین می رود ، تمام شنوندگان رویداد به طور خودکار حذف می شوند. نیازی به نگرانی در مورد پاک کردن توسط خودتان  نیست.
</li>
</ul>
</p>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
var app1 = new Vue({
  el: '#app1',
  data: {
    counter: 0
  }
})

    var app2 = new Vue({
        el: '#app2',
        data: {
            name: 'Vue.js'
        },
        // define methods under the `methods` object
        methods: {
            greet: function (event) {
                // `this` inside methods points to the Vue instance
                alert('Hello ' + this.name + '!')
                // `event` is the native DOM event
                if (event) {
                    alert(event.target.tagName)
                }
            }
        }
    })

    // you can invoke methods in JavaScript too
    app2.greet() // => 'Hello Vue.js!'
    var app3 = new Vue({
        el: '#app3',
        data: {
            name: 'Vue.js'
        },
        // define methods under the `methods` object
        methods: {
                say: function (message) {
                  alert(message)
                }
        }
    })
</script>