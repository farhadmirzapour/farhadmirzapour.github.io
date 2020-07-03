---
layout: documentation-vuejs
title:    رندر List (List Rendering)
cattitle:   اصول آموزش vuejs
date:   2019-10-08 20:08:42 +0330
jdate: سه شنبه 16 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/list.html
---
<h3>نگاشت آرایه به عناصر HTML با v-for</h3>
<p>
ما می توانیم از دایرکتیو v-for برای تهیه لیست از آیتم های  موجود در آرایه استفاده کنیم. دایرکتیو v-for  به صورت item in items به یک نحو خاص  نیاز دارد ، که در آن items  منبع دیتای آرایه  و item   یک نام مستعار برای عنصر آرایه که در آن تکرار می شود:
</p>

<pre><code class="language-html  line-numbers">&#x3C;ul id=&#x22;example-1&#x22;&#x3E;
  &#x3C;li v-for=&#x22;item in items&#x22;&#x3E;
    &#123;&#123; item.message &#125;&#125;
  &#x3C;/li&#x3E;
&#x3C;/ul&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</code></pre>

<p>
نتیجه :
</p>

<div id="app1" class="result-example">
<ul>
<li v-for="item in items">&#123;&#123; item.message &#125;&#125;</li>
</ul>
</div>

<p>
در داخل بلوکهای v-for ، دسترسی کامل به ویژگیهای دامنه والدین داریم. v-for همچنین از یک آرگومان دوم اختیاری برای index  آیتم فعلی پشتیبانی می کند.
</p>

<pre><code class="language-html  line-numbers">&#x3C;ul id=&#x22;example-2&#x22;&#x3E;
  &#x3C;li v-for=&#x22;(item, index) in items&#x22;&#x3E;
    &#123;&#123; parentMessage &#125;&#125; - &#123;&#123; index &#125;&#125; - &#123;&#123; item.message &#125;&#125;
  &#x3C;/li&#x3E;
&#x3C;/ul&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
</code></pre>

<p>
نتیجه :
</p>

<div id="app2" class="result-example">
<ul>
  <li v-for="(item, index) in items">
    &#123;&#123; parentMessage &#125;&#125; -  &#123;&#123; index &#125;&#125; - &#123;&#123; item.message &#125;&#125;
  </li>
</ul>
</div>

<p>
شما همچنین می توانید از of به جای in استفاده نمایید که به نحو جاوا اسکریپت برای تکرار کنندگان(iterators) نزدیکتر می باشد:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-for=&#x22;item of items&#x22;&#x3E;&#x3C;/div&#x3E;
</code></pre>

<br>

<h3>استفاده از v-for به همراه object</h3>
<p>
همچنین می توانید از v-for برای تکرار از طریق خواص یک object استفاده کنید.
</p>

<pre><code class="language-html  line-numbers">&#x3C;ul id=&#x22;v-for-object&#x22; class=&#x22;demo&#x22;&#x3E;
  &#x3C;li v-for=&#x22;value in object&#x22;&#x3E;
    &#123;&#123; value &#125;&#125;
  &#x3C;/li&#x3E;
&#x3C;/ul&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
</code></pre>

<p>
نتیجه :
</p>

<div id="app3" class="result-example">
<ul>
  <li v-for="value in object">
    &#123;&#123; value &#125;&#125;
  </li>
</ul>
</div>

<p>
همچنین می توانید یک آرگومان دوم برای نام ویژگی ارائه دهید ( a.k.a. key):
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-for=&#x22;(value, name) in object&#x22;&#x3E;
  &#123;&#123; name &#125;&#125;: &#123;&#123; value &#125;&#125;
&#x3C;/div&#x3E;
</code></pre>
<div id="app4" class="result-example">
<div v-for="(value, name) in object">
  &#123;&#123; name &#125;&#125;: &#123;&#123; value &#125;&#125;
</div>
</div>


<p>
و برای  index:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-for=&#x22;(value, name, index) in object&#x22;&#x3E;
  &#123;&#123; index &#125;&#125;. &#123;&#123; name &#125;&#125;: &#123;&#123; value &#125;&#125;
&#x3C;/div&#x3E;
</code></pre>

<div id="app5" class="result-example">
<div v-for="(value, name, index) in object">
  &#123;&#123; index &#125;&#125;. &#123;&#123; name &#125;&#125;: &#123;&#123; value &#125;&#125;
</div>
</div>

<blockquote class="has-icon tip">
هنگام تکرار از طریق یک object ، مرتب سازی بر اساس ترتیب  ()Object.keys صورت می گیرد ، که تضمینی ندارد مطابق با پیاده سازی موتور جاوا اسکریپت باشد.
</blockquote>
<br>

<h3>حفظ وضعیت (Maintaining State)</h3>

<p>
هنگامی که Vue در حال به روزرسانی لیستی از عناصر رندر شده با v-for است ، به طور پیش فرض از استراتژی  " in-place patch " استفاده می کند. اگر ترتیب داده ها تغییر کند ، به جای حرکت دادن عناصر DOM جهت تطبیق با ترتیب آیتم ها ، Vue هر عنصر را در جای خود قرار داده و اطمینان می دهد آنچه باید در آن index خاص رندر شود انعکاس یابد. این شبیه به رفتار track-by="$index" در Vue 1.x می باشد.
</p>

<p>
این حالت پیش فرض کارآمد است ، اما تنها در شرایطی مناسب است که خروجی لیست رندر شده شما به وضعیت کامپوننت فرزند  یا وضعیت DOM موقتی متکی نباشد (مثلاً مقادیر ورودی فرم).
</p>

<p>
برای اشاره و راهنمایی به Vue که هویت هر گره را ردیابی کرده  تا بتواند از آن ها استفاده مجدد و عناصر موجود را دوباره مرتب سازی نماید ، نیاز دارید یک ویژگی key منحصر به فرد برای هر آیتم در نظر بگیرید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div v-for=&#x22;item in items&#x22; v-bind:key=&#x22;item.id&#x22;&#x3E;
  &#x3C;!-- content --&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
توصیه می شود در هر زمان ممکن یک ویژگی key با v-for ارائه دهید ، مگر اینکه محتوای DOM تکرار شونده ساده باشد ، یا عمداً جهت افزایش عملکرد ، بر رفتار پیش فرض تکیه می کنید.
</p>

<p>
از آنجا که این یک مکانیسم عمومی جهت شناسایی گره ها توسط Vue است ، این key دارای کاربردهای دیگری نیز می باشد که به طور خاص با V-for گره خورده اند ، همانطور که بعداً در این راهنما خواهیم دید.
</p>

<blockquote class="has-icon tip">
از مقادیری همانند اشیاء و آرایه ها به عنوان کلیدهای v-for استفاده نکنید. به جای آن از مقادیر رشته یا عددی استفاده کنید.
</blockquote>

<p>
جهت استفاده دقیق از ویژگی key، به <a href="https://vuejs.org/v2/api/#key"  target="_blank" >  API documentation </a>  مراجعه نمایید.
</p>

<br>

<h3>تشخیص تغییر در آرایه (Array Change Detection)</h3>

<h4> متدهای جهش (Mutation Methods)</h4>

<p>
Vue متدهای mutation  (جهش) آرایه ارائه داده  که باعث بروزرسانی های view نیز می شود. این متدها عبارتند از:
</p>

<p style="direction: ltr;text-align: left">
<ul style="direction: ltr;text-align: left">
<li><code>push()</code></li>
<li><code>pop()</code></li>
<li><code>shift()</code></li>
<li><code>unshift()</code></li>
<li><code>splice()</code></li>
<li><code>sort()</code></li>
<li><code>reverse()</code></li>
</ul>
</p>

<p>
شما می توانید console مرورگر خود را باز کرده و در مثال قبل با متدها کار کنید.برای مثال : example1.items.push({ message: 'Baz' }).
</p>

<h4>جایگیزینی یک آرایه (Replacing an Array)</h4>
<p>
متدهای Mutation ، همانطور که از نام آن مشخص است ، آرایه اصلی مورد نظر را جهش و دگرگون می کنند. در مقایسه ، روشهای غیر جهش نیز وجود دارد ، به عنوان مثال . ()filter(), concat  و ()slice، که آرایه اصلی را جهش نمی دهند ، اما همیشه یک آرایه جدید را برمی گردانند. هنگام کار با روش های غیر جهش ، می توانید آرایه قدیمی را با روش جدید جایگزین کنید:
</p>

<pre><code class="language-javascript  line-numbers">example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
</code></pre>

<p>
ممکن است فکر کنید این امر باعث خواهد شد که DOM،  Vue موجود را دور بیندازد و مجدداً لیست را ارائه دهد - خوشبختانه ، اینگونه نیست. Vue برای به حداکثر رساندن استفاده مجدد از عناصر DOM برخی از اکتشافات هوشمند را پیاده سازی می کند ، بنابراین جایگزینی یک آرایه با آرایه دیگری که حاوی اشیاء همپوشانی است ، عملیاتی بسیار کارآمد است.
</p>

<br>

<h4>هشدار</h4>

<p>
به دلیل محدودیت های موجود در JavaScript ، Vue نمی تواند تغییرات زیر را در یک آرایه تشخیص دهد:
</p>

<p>
<ul>
<li>
زمانیکه یک آیتم را مستقیما با index جایگزاری می کنید ، به عنوان مثال vm.items [indexOfItem] = newValue
</li>
<li>
زمانیکه طول آرایه را تغییر می دهید ، به عنوان مثال vm.items.length = newLength
</li>
</ul>
</p>

<p>
برای مثال:
</p>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // is NOT reactive
vm.items.length = 2 // is NOT reactive
</code></pre>

<p>
برای غلبه بر بر هشدار 1 ، هر دو کد زیر کار مشابه با vm.items [indexOfItem] = newValue انجام می دهند ، اما باعث بروزرسانی وضعیت در سیستم واکنش پذیری خواهد شد :
</p>

<pre><code class="language-javascript  line-numbers">// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
</code></pre>

<pre><code class="language-javascript  line-numbers">// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)
</code></pre>

<p>
همچنین می توانید از متد نمونه vm.$set  استفاده کنید که یک نام مستعار برای  Vue.set سراسری است:
</p>

<pre><code class="language-javascript  line-numbers">vm.$set(vm.items, indexOfItem, newValue)
</code></pre>

<p>
برای مقابله با هشدار 2 ، می توانید از splice استفاده کنید:
</p>

<pre><code class="language-javascript  line-numbers">vm.items.splice(newLength)
</code></pre>
<br>

<h4>Object Change Detection Caveats</h4>
<p>
مجددا به دلیل محدودیت های JavaScript مدرن ، Vue نمی تواند اضافه و یا حذف ویژگی (property)  را تشخیص دهد. مثلا:
</p>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` is now reactive

vm.b = 2
// `vm.b` is NOT reactive
</code></pre>

<p>
Vue اجازه نمی دهد به صورت پویا خصوصیات واکنشی سطح ریشه به یک نمونه ایجاد شده اضافه شود. با این وجود ، می توان با استفاده از روش Vue.set(object, propertyName, value)  ویژگیهای واکنشی را به یک شیء تو در تو اضافه کرد. به عنوان مثال ،
</p>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})
</code></pre>

<p>
می توانید یک ویژگی جدید سن age  را به شیء userProfile   تودرتو اضافه کنید با:
</p>

<pre><code class="language-javascript  line-numbers">Vue.set(vm.userProfile, 'age', 27)
</code></pre>

<p>
همچنین می توانید از متد نمونه vm.$set  استفاده کنید که یک نام مستعار برای Vue.set سراسری است:
</p>

<pre><code class="language-javascript  line-numbers">vm.$set(vm.userProfile, 'age', 27)
</code></pre>

<p>
بعضی اوقات ممکن است بخواهید تعدادی از ویژگی های جدید را به یک شی موجود اختصاص دهید ، به عنوان مثال با استفاده از ()Object.assign    یا ()extend._ . در چنین مواردی ، شما باید یک شیء تازه را با خصوصیات هر دو شی  ایجاد کنید. بنابراین به جای:
</p>

<pre><code class="language-javascript  line-numbers">Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
</code></pre>

<p>
شما می توانید خصوصیات واکنشی جدید را با:
</p>

<pre><code class="language-javascript  line-numbers">vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
</code></pre>
<br>

<h3>نمایش نتایج  Filtered/Sorted</h3>
<p>
بعضی اوقات می خواهیم نسخه مرتب شده یا فیلتر شده از آرایه را نمایش دهیم بدون اینکه در واقع داده های اصلی را تغییر داده یا مجددا تنظیم کنیم. در این حالت ، می توانید یک ویژگی computed  ایجاد کنید که آرایه فیلتر شده یا مرتب شده را برمی گرداند.
</p>

<p>
برای مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;li v-for=&#x22;n in evenNumbers&#x22;&#x3E;&#123;&#123; n &#125;&#125;&#x3C;/li&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
</code></pre>

<p>
در شرایطی که ویژگی های computed امکان پذیر نیست (به عنوان مثال در داخل حلقه های تودردتو v-for ) ، می توانید از یک متد استفاده کنید:
</p>


<pre><code class="language-html  line-numbers">&#x3C;li v-for=&#x22;n in even(numbers)&#x22;&#x3E;&#123;&#123; n &#125;&#125;&#x3C;/li&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
</code></pre>
<br>

<h3>v-for با یک رنج</h3>
<p>
v-for همچنین می تواند یک عدد صحیح را دریافت نماید. در این حالت این قالب را بارها تکرار می کند.
</p>

<pre><code class="language-html  line-numbers">&#x3C;div&#x3E;
  &#x3C;span v-for=&#x22;n in 10&#x22;&#x3E;&#123;&#123; n &#125;&#125; &#x3C;/span&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
نتیجه:
</p>

<p>
<div id="range" class="demo"><span>1 </span><span>2 </span><span>3 </span><span>4 </span><span>5 </span><span>6 </span><span>7 </span><span>8 </span><span>9 </span><span>10 </span></div>
</p>
<br>

<h3>v-for با &#x3C;template&#x3E;</h3>
<p>
شبیه به الگوی v-if ، می توانید از تگ &#x3C;template&#x3E; با v-for نیز استفاده کنید تا بتوانید از چندین عنصر از یک بلاک را رندر نمایید. مثلا:
</p>

<pre><code class="language-html  line-numbers">&#x3C;ul&#x3E;
  &#x3C;template v-for=&#x22;item in items&#x22;&#x3E;
    &#x3C;li&#x3E;&#123;&#123; item.msg &#125;&#125;&#x3C;/li&#x3E;
    &#x3C;li class=&#x22;divider&#x22; role=&#x22;presentation&#x22;&#x3E;&#x3C;/li&#x3E;
  &#x3C;/template&#x3E;
&#x3C;/ul&#x3E;
</code></pre>

<br>

<h3>v-for با v-if</h3>

<blockquote class="has-icon tip">
توجه داشته باشید که توصیه نمی شود از v-if و v-for با هم استفاده کنید. برای جزئیات بیشتر به <a href="https://vuejs.org/v2/style-guide/#Avoid-v-if-with-v-for-essential"   target="_blank">style guide</a>مراجعه کنید.
</blockquote>

<p>
هنگامی که آنها در همان گره وجود دارند ، v-for دارای اولویت بالاتری نسبت به v-if است. این بدان معنی است که v-if در هر تکرار حلقه به طور جداگانه اجرا می شود. این می تواند زمانی مفید باشد که می خواهید گره ها را فقط برای برخی موارد ، مانند زیر ارائه دهید:
</p>


<pre><code class="language-html  line-numbers">&#x3C;li v-for=&#x22;todo in todos&#x22; v-if=&#x22;!todo.isComplete&#x22;&#x3E;
  &#123;&#123; todo &#125;&#125;
&#x3C;/li&#x3E;
</code></pre>

<p>
موارد فوق فقط todos هایی را که کامل نیستند رندر می شوند.
</p>


<p>
اگر هدف شما از اجرای شرط حلقه بطور شرطی باشد ، می توانید v-if را روی یک عنصر بسته (یا &#x3C;template&#x3E;) قرار دهید. مثلا:
</p>

<pre><code class="language-html  line-numbers">&#x3C;ul v-if=&#x22;todos.length&#x22;&#x3E;
  &#x3C;li v-for=&#x22;todo in todos&#x22;&#x3E;
    &#123;&#123; todo &#125;&#125;
  &#x3C;/li&#x3E;
&#x3C;/ul&#x3E;
&#x3C;p v-else&#x3E;No todos left!&#x3C;/p&#x3E;
</code></pre>

<br>

<h3>v-for با Component</h3>

<p>
شما می توانید به طور مستقیم مانند هر عنصر عادی از v-for در یک کامپوننت سفارشی استفاده کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;my-component v-for=&#x22;item in items&#x22; :key=&#x22;item.id&#x22;&#x3E;&#x3C;/my-component&#x3E;
</code></pre>

<blockquote class="has-icon tip">
در 2.2.0+ ، هنگام استفاده از v-for با یک کامپوننت ، یک key لازم است.
</blockquote>

<p>
با این وجود ، داده ها به طور خودکار به کامپوننت منتقل نمی شوند ، زیرا کامپوننت دارای محدوده های جدا شده از خود هستند. برای انتقال داده های تکرار شونده در کامپوننت ، باید از props  نیز استفاده کنیم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;my-component
  v-for=&#x22;(item, index) in items&#x22;
  v-bind:item=&#x22;item&#x22;
  v-bind:index=&#x22;index&#x22;
  v-bind:key=&#x22;item.id&#x22;
&#x3E;&#x3C;/my-component&#x3E;
</code></pre>

<p>
دلیل عدم تزریق خودکار item  به کامپوننت این است که باعث می شود این کامپوننت با نحوه کار v-for  پیوستگی محکمی  ایجاد کند. صریح بودن اطلاعات مربوط به اینکه از کجا تهیه می شود باعث می شود کامپوننت در موقعیت های دیگر قابل استفاده مجدد باشد.
</p>

<p>
در اینجا یک مثال کامل از یک لیست ساده TODO آمده است:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;todo-list-example&#x22;&#x3E;
  &#x3C;form v-on:submit.prevent=&#x22;addNewTodo&#x22;&#x3E;
    &#x3C;label for=&#x22;new-todo&#x22;&#x3E;Add a todo&#x3C;/label&#x3E;
    &#x3C;input
      v-model=&#x22;newTodoText&#x22;
      id=&#x22;new-todo&#x22;
      placeholder=&#x22;E.g. Feed the cat&#x22;
    &#x3E;
    &#x3C;button&#x3E;Add&#x3C;/button&#x3E;
  &#x3C;/form&#x3E;
  &#x3C;ul&#x3E;
    &#x3C;li
      is=&#x22;todo-item&#x22;
      v-for=&#x22;(todo, index) in todos&#x22;
      v-bind:key=&#x22;todo.id&#x22;
      v-bind:title=&#x22;todo.title&#x22;
      v-on:remove=&#x22;todos.splice(index, 1)&#x22;
    &#x3E;&#x3C;/li&#x3E;
  &#x3C;/ul&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
توجه داشته باشید که ویژگی is="todo-item"  است. این در الگوهای DOM ضروری است ، زیرا فقط یک عنصر &#x3C;li&#x3E;
 درون یک &#x3C;ul&#x3E; معتبر است. این همان کار را با &#x3C;todo-item&#x3E;
 انجام می دهد ، اما در اطراف یک خطای تجزیه مرورگر بالقوه کار می کند. برای کسب اطلاعات بیشتر به الگوی <a href="/documentation/vuejs/Essentials/Components-Basics#نکاتی-در-عملیات-تجزیه-قالب-dom-template-parsing-caveats" target="_blank"> DOM Template Parsing Caveats</a>مراجعه کنید.
</p>

<pre><code class="language-javascript  line-numbers">Vue.component('todo-item', {
  template: '\
    &#x3C;li&#x3E;\
      &#123;&#123; title &#125;&#125;\
      &#x3C;button v-on:click=&#x22;$emit(\&#x27;remove\&#x27;)&#x22;&#x3E;Remove&#x3C;/button&#x3E;\
    &#x3C;/li&#x3E;\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
</code></pre>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
var app1 = new Vue({
    el: '#app1',
    data: {
        items: [
          { message: 'Foo' },
          { message: 'Bar' }
        ]
  }
})

    var app2 = new Vue({
        el: '#app2',
        data: {
            parentMessage: 'Parent',
            items: [
                { message: 'Foo' },
                { message: 'Bar' }
            ]
        }
    })


var app3 = new Vue({
            el: '#app3',
            data: {
              object: {
                title: 'How to do lists in Vue',
                author: 'Jane Doe',
                publishedAt: '2016-04-10'
              }
            }
          })
var app4 = new Vue({
            el: '#app4',
            data: {
              object: {
                title: 'How to do lists in Vue',
                author: 'Jane Doe',
                publishedAt: '2016-04-10'
              }
            }
          })
          
var app5 = new Vue({
            el: '#app5',
            data: {
              object: {
                title: 'How to do lists in Vue',
                author: 'Jane Doe',
                publishedAt: '2016-04-10'
              }
            }
          })          
</script>