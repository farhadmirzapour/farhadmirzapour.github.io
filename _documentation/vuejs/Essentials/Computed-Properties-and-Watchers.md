---
layout: documentation-vuejs
title:    خاصیت Computed و Watchers
cattitle:   اصول آموزش vuejs
date:   2019-10-03 08:08:42 +0330
jdate: پنج شنبه 11 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/computed.html
---
<h3>  بررسی (Computed Properties)</h3>
<p>
استفاده از عبارات در قالب ها بسیار راحت است ، اما بیشتر برای عملیات ساده مورد استفاده قرار می گیرند. قرار دادن منطق بیش از حد در الگوها نگه داری آن را سخت می کند. مثلا:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;example&#x22;&#x3E;
   &#123;&#123; message.split(&#x27;&#x27;).reverse().join(&#x27;&#x27;) &#125;&#125;
&#x3C;/div&#x3E;
</code></pre>

<p>
در این مثال ، قالب دیگر ساده و اعلامی نیست. وقتی می خواهید پیام معکوس شده را در الگوی خود بیش از یک بار بگنجانید این مشکل بدتر می شود.
</p>

<p>
به همین دلیل برای هر منطق پیچیده ، باید از یک خاصیت computed  (محاسبه شده) استفاده کنید.
</p>

<h4>مثال ساده :</h4>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;example&#x22;&#x3E;
  &#x3C;p&#x3E;Original message: &#x22; &#123;&#123; message &#125;&#125;&#x22;&#x3C;/p&#x3E;
  &#x3C;p&#x3E;Computed reversed message: &#x22; &#123;&#123; reversedMessage &#125;&#125;&#x22;&#x3C;/p&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
</code></pre>

<p>
نتیجه:
</p>

<div id="example" class="result-example">
  <p>Original message: "&#123;&#123; message &#125;&#125;"</p>
  <p>Computed reversed message: "&#123;&#123; reversedMessage &#125;&#125;"</p>
</div>

<p>
در اینجا ما یک  خاصیت computed  به نام reversedMessage اعلان کرده ایم. تابعی که ارائه دادیم به عنوان تابع getter  برای ویژگی vm.reversedMessage استفاده خواهد شد:
</p>


<pre><code class="language-javascript  line-numbers">console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
</code></pre>

<p>
می توانید console را باز کرده و با مثال vm خود کار کنید. مقدار vm.reversedMessage همیشه به مقدار vm.message بستگی دارد.
</p>

<p>
شما  می توانید داده ها را به  خاصیت های computed در قالب ها دقیقاً همانند یک ویژگی معمولی متصل کنید.  Vue می داند که vm.reversedMessage به vm.message بستگی دارد ، بنابراین هر اتصالی که وابسته به vm.reversedMessage  باشد را زمانیکه   vm.message تغییر کند به روز رسانی می کند. بهترین قسمت کار این است که ما این رابطه وابستگی را بصورت اعلانی ایجاد کرده ایم و این باعث می شود تست و درک آن آسان تر شود.
</p>

<br>

<h3>  مقایسه Computed Caching  با  Methods</h3>
<p>
شاید متوجه شده باشید که با استفاده از method در عبارت می توانیم به نتیجه مشابه برسیم:
</p>

<pre><code class="language-html  line-numbers">&#x3C;p&#x3E;Reversed message: &#x22;&#123;&#123; reverseMessage() &#125;&#125;&#x22;&#x3C;/p&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">// in component
methods: {
  reverseMessage: function () {
    return this.message.split('').reverse().join('')
  }
}
</code></pre>

<p>
به جای یک خاصیت computed ، می توانیم همان عملکرد را به عنوان یک method تعریف کنیم. نتایج برای هردو یکسان است. با این حال ، تفاوت در این است که خاصیت های computed بر اساس وابستگی واکنشی آنها  ذخیره موقت (cached)  می شوند. یک خاصیت computed  فقط در صورت تغییر برخی از وابستگی های واکنشی آن ، دوباره ارزیابی می شود.  این بدان معنی است تا زمانی که پیام تغییر نکرده ، دسترسی چندگانه به خاصیت computed (محاسبه شده)  reversedMessage بلافاصله بدون نیاز به اجرای دوباره عملکرد ، نتیجه قبلاً محاسبه شده را برمی گرداند.
</p>

<p>
این همچنین بدان معنی است که ویژگی computed  زیر هرگز به روز نمی شود ، زیرا  ()Date.now یک وابستگی واکنشی نیست:
</p>

<pre><code class="language-javascript  line-numbers">computed: {
  now: function () {
    return Date.now()
  }
}
</code></pre>

<p>
در مقایسه ،  method هر زمان که یک رندر دوباره(re-render) رخ دهد عملکرد را انجام می دهد.
</p>

<p>
چرا ما نیاز به ذخیره سازی موقت محاسبات داریم؟ تصور کنید که ما یک خاصیت  computed  به نام A  با محاسبات سنگین داشته باشیم  که نیاز به حلقه زدن در یک Array عظیم و انجام محاسبات زیادی دارد. بنابراین ممکن است ما خاصیت های  computed  دیگری داشته باشیم که به نوبه خود به A بستگی داشته باشند . بدون ذخیره کردن ، چند بار بیشتر از حد لازم A را اجرا  می کنیم! در مواردی که نمی خواهید حافظه پنهانی داشته باشید ، به جای آن از method استفاده کنید.
</p>

<br>

<h3>  مقایسه  Computed  با  watch</h3>
<p>
Vue روشی عمومی تر برای مشاهده و واکنش به تغییرات داده ها در یک نمونه فراهم می کند به نام خاصیت watch. چنانچه داده هایی دارید که بر اساس برخی داده های دیگر تغییر می کنند ، استفاده از watch می تواند بسیار وسوسه انگیز باشد  به خصوص اگر قبلا با محیط کار AngularJS  کار کرده باشید. با این حال ، اغلب ایده بهتر استفاده از خاصیت computed  می باشد. این مثال را در نظر بگیرید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;demo&#x22;&#x3E;&#123;&#123; fullName &#125;&#125;&#x3C;/div&#x3E;
</code></pre>


<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
</code></pre>

<p>
کد بالا ضروری و تکراری است.حال آن را با یک نسخه  computed مقایسه کنید:
</p>

<pre><code class="language-javascript  line-numbers">var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar'
  },
  computed: {
    fullName: function () {
      return this.firstName + ' ' + this.lastName
    }
  }
})
</code></pre>

<p>
خیلی بهتر است ، اینطور نیست؟
</p>

<br>

<h3> افزودن Setter به Computed</h3>
<p>
ویژگی های Computed به صورت پیش فرض فقط دارای getter می باشند ، اما می توانید در صورت نیاز به آن یک setter نیز اضافه نمایید:
</p>

<pre><code class="language-javascript  line-numbers">// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
</code></pre>

<p>
اکنون وقتی vm.fullName = 'John Doe' را اجرا کردید ،setter فراخوانی می شود و vm.firstName و vm.lastName به همین ترتیب به روز می شوند.
</p>

<br>

<h3> بررسی Watchers</h3>

<p>
در حالی که خاصیت های computed  در بیشتر موارد مناسب تر هستند ، مواقعی وجود دارد که یک watcher (مشاهده گر) سفارشی می تواند ضرورت داشته باشد. به همین دلیل است که Vue یک روش عمومی تر برای واکنش به تغییرات داده ها از طریق watch  ارائه می دهد. بیشترین کاربرد زمانیست که بخواهید عملیات ناهمزمان (asynchronous) در پاسخ به تغییر داده ها ، داشته باشید.
</p>

<p>
برای مثال:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;watch-example&#x22;&#x3E;
  &#x3C;p&#x3E;
    Ask a yes/no question:
    &#x3C;input v-model=&#x22;question&#x22;&#x3E;
  &#x3C;/p&#x3E;
  &#x3C;p&#x3E;&#123;&#123; answer &#125;&#125;&#x3C;/p&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-html  line-numbers">&#x3C;!-- Since there is already a rich ecosystem of ajax libraries    --&#x3E;
&#x3C;!-- and collections of general-purpose utility methods, Vue core --&#x3E;
&#x3C;!-- is able to remain small by not reinventing them. This also   --&#x3E;
&#x3C;!-- gives you the freedom to use what you&#x27;re familiar with.      --&#x3E;
&#x3C;script src=&#x22;https://cdn.jsdelivr.net/npm/axios@0.12.0/dist/axios.min.js&#x22;&#x3E;&#x3C;/script&#x3E;
&#x3C;script src=&#x22;https://cdn.jsdelivr.net/npm/lodash@4.13.1/lodash.min.js&#x22;&#x3E;&#x3C;/script&#x3E;
&#x3C;script&#x3E;
var watchExampleVM = new Vue({
  el: &#x27;#watch-example&#x27;,
  data: {
    question: &#x27;&#x27;,
    answer: &#x27;I cannot give you an answer until you ask a question!&#x27;
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = &#x27;Waiting for you to stop typing...&#x27;
      this.debouncedGetAnswer()
    }
  },
  created: function () {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500)
  },
  methods: {
    getAnswer: function () {
      if (this.question.indexOf(&#x27;?&#x27;) === -1) {
        this.answer = &#x27;Questions usually contain a question mark. ;-)&#x27;
        return
      }
      this.answer = &#x27;Thinking...&#x27;
      var vm = this
      axios.get(&#x27;https://yesno.wtf/api&#x27;)
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer)
        })
        .catch(function (error) {
          vm.answer = &#x27;Error! Could not reach the API. &#x27; + error
        })
    }
  }
})
&#x3C;/script&#x3E;
</code></pre>

<p>
در این حالت ، استفاده از گزینه Watch به ما این امکان را می دهد تا عملیاتی asynchronous  (ناهمزمان) (دسترسی به یک API) را انجام دهیم ، تعداد دفعات عملیات را می توان محدود و حالت های واسطه را تنظیم کنیم تا پاسخ نهایی را بدست آوریم. هیچ یک از این موارد با خاصیت computed  امکان پذیر نیست.
</p>

<p>
علاوه بر گزینه Watch ، می توانید از <a href="https://vuejs.org/v2/api/#vm-watch" target="_blank">vm.$watch API</a>  نیز استفاده کنید.
</p>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>

<script>
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
</script>    