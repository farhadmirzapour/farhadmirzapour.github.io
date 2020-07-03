---
layout: documentation-vuejs
title:    ایجاد نمونه Vue
cattitle:   اصول آموزش vuejs
date:   2019-09-24 08:08:42 +0330
jdate: سه شنبه 02 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/instance.html
---
<h3>  ایجاد نمونه Vue (Creating a Vue Instance)</h3>
<p>
هر برنامه Vue با ایجاد یک نمونه جدید از Vue با استفاده از تابع Vue شروع می شود:
</p>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  // options
})
</code></pre>

<p>
اگرچه کاملاً با الگوی MVVM مطابقت ندارد ، اما طراحی Vue تا حدی از آن الهام گرفته شده است. به عنوان یک قرارداد ، ما اغلب از متغیر vm (کوتاه برای ViewModel) برای مراجعه به نمونه Vue استفاده می کنیم.
</p>

<p>
برنامه Vue شامل یک نمونه root Vue  است که با new Vue ایجاد شده و به صورت اختیاری در یک درخت از اجزای تودرتو و قابل استفاده مجدد قرار گرفته است. به عنوان مثال ، ممکن است درخت کامپوننت یک برنامه todo مانند زیر باشد:
</p>

<pre><code class="language-text">Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
</code></pre>

<p>
بعداً در مورد سیستم کامپوننت صحبت خواهیم کرد. در حال حاضر ، فقط بدانید که همه کامپوننت های Vue نیز نمونه های Vue هستند 
</p>

<br>

<h3>  اصول دیتا و متد (Data and Methods)</h3>
<p>
هنگامی که یک نمونه Vue ایجاد شد ، تمام خصوصیاتی را که در شیء داده آن یافت می شود به سیستم واکنش پذیری Vue اضافه می شود. هنگامی که مقادیر آن خصوصیات تغییر می کند ، لایه view "واکنش" می دهد و به روز می شود تا با مقادیر جدید مطابقت داشته باشد.
</p>

<pre><code class="language-javascript  line-numbers">// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
</code></pre>

<p>
با تغییر این داده ها ، view  مجددا رندر می شود. لازم به ذکر است که خواص موجود در data تنها در صورتی واکنش نشان می دهند که در زمان ایجاد نمونه وجود داشته باشند. این بدان معناست که اگر یک ویژگی جدید اضافه کنید ، مانند:
</p>

<pre><code class="language-javascript  line-numbers">vm.b = 'hi'
</code></pre>

<p>
تغییرات در b باعث بروز رسانی در  view نمی شود. اگر بدانید بعداً به یک خاصیت احتیاج دارید ، اما در ابتداخالی یا غیر موجود است ، شما در ابتدا باید مقداری اولیه تعیین کنید. برای مثال:
</p>


<pre><code class="language-javascript  line-numbers">data: {
  newTodoText: '',
  visitCount: 0,
  hideCompletedTodos: false,
  todos: [],
  error: null
}
</code></pre>

<p>
تنها استثنا در این مورد استفاده از ()Object.freeze  است که از تغییر خصوصیات موجود جلوگیری می کند و این بدان معنی است که سیستم واکنش پذیری نمی تواند تغییرات را ردیابی کند.
</p>

<pre><code class="language-javascript  line-numbers">var obj = {
  foo: 'bar'
}

Object.freeze(obj)

new Vue({
  el: '#app',
  data: obj
})
</code></pre>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;app&#x22;&#x3E;
  &#x3C;p&#x3E;&#123;&#123; foo &#125;&#125;&#x3C;/p&#x3E;
  &#x3C;!-- this will no longer update &#x60;foo&#x60;! --&#x3E;
  &#x3C;button v-on:click=&#x22;foo = &#x27;baz&#x27;&#x22;&#x3E;Change it&#x3C;/button&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
علاوه بر خواص داده ، نمونه های Vue دارای تعدادی از خصوصیات و متد های مفید می باشند. آنها دارای پیشوند $ می باشند تا از ویژگیهای تعریف شده توسط کاربر متمایز شوند. مثلا:
</p>

<pre><code class="language-javascript  line-numbers">var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
</code></pre>

<p>
می توانید برای داشتن لیست کاملی از خصوصیات و متدها ،  به  <a href="https://vuejs.org/v2/api/#Instance-Properties" target="_blank"> API reference</a> مراجعه نمایید.
</p>
<br>

<h3>  چرخه سیستم (Instance Lifecycle Hooks)</h3>

<p>
هر نمونه Vue هنگام ایجاد یک سری مراحل اولیه سازی را طی می کند - برای مثال ، باید data را تنظیم کند ، قالب را کامپایل کند ، نمونه را روی DOM سوار کند و در هنگام تغییر داده ها DOM را به روز کند. در طی مسیر ، توابعی به نام  lifecycle hooks نیز فراخوانی می شوند و به کاربران این امکان را می دهد تا در مراحل خاص کد مخصوص خود را اضافه کنند.
</p>

<p>
به عنوان مثال ، هاک created می تواند پس از ایجاد نمونه ، برای اجرای کد استفاده شود:
</p>

<pre><code class="language-javascript  line-numbers">new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
</code></pre>

<p>
Hook های دیگری نیز وجود دارد که در مراحل مختلف چرخه حیات مانند mounted (نصب و سوار شدن)  ، updated ( به روزرسانی) و destroyed (از بین رفتن) فراخوانی می شوند.
</p>

<blockquote class="has-icon tip">
<div>
از arrow functions در property و callback همانند زیر استفاده نکنید
</div>
<div align="left">
created: () => console.log(this.a)
</div>
<div align="left">
vm.$watch('a', newValue => this.myMethod())
</div>
<div>
باعث خطاهایی نظیر زیر می شود:
</div>
<div align="left">
Uncaught TypeError: Cannot read property of undefined
</div>
<div align="left">
Uncaught TypeError: this.myMethod is not a function
</div>
</blockquote>
<br>

<h3>  دیاگرام چرخه سیستم (Lifecycle Diagram)</h3>
<p>
چرخه سیستم را بصورت دیاگرام در زیر نمایش داده شده است :
</p>

<p>
<img src="/images/post/vuejs/lifecycle.png" alt="دیاگرام چرخه سیستم" />
</p>