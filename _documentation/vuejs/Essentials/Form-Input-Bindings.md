---
layout: documentation-vuejs
title:     اتصال داده به ورودی فرم (Form Input Bindings)
cattitle:   اصول آموزش vuejs
date:   2019-10-11 15:10:42 +0330
jdate: جمعه 19 مهر 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Essentials
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/forms.html
---
<h3>  اتصال داده به ورودی فرم (Form Input Bindings)</h3>
<p>
شما می توانید از دایرکتیوv-model برای ایجاد اتصالات داده دو طرفه در ورودی فرم ، textarea و عناصر انتخاب استفاده کنید. این به طور خودکار روش صحیح برای به روز رسانی عنصر را بر اساس نوع ورودی انتخاب می کند. 
</p>

<blockquote class="has-icon tip">
v-model مقدار اولیه ، صفات بررسی شده یا منتخب موجود در هر عنصر فرم را نادیده نمی گیرد. همیشه با داده های نمونه Vue به عنوان منبع درست رفتار خواهد کرد. شما باید مقدار اولیه را در قسمت JavaScript ، در داخل گزینه data (کامپوننت) خود اعلام کنید.
</blockquote>

<p>
v-model در داخل از خواص مختلفی استفاده می کند و رویدادهای مختلفی را برای عناصر ورودی مختلف منتشر می کند:
</p>

<ul>
<li>
text   و عناصر textarea از ویژگی value  و رویداد ورودی استفاده می کنند.
</li>
<li>
checkboxes  و radiobuttons  از ویژگیهای checked  استفاده کرده و رویداد را تغییر می دهند.
</li>
<li>
فیلدهای انتخابی  value  را به عنوان پایه استفاده کرده و به عنوان یک رویداد تغییر می دهند.
</li>
</ul>

<br>

<h3>Text</h3>

<pre><code class="language-html  line-numbers">&#x3C;input v-model=&#x22;message&#x22; placeholder=&#x22;edit me&#x22;&#x3E;
&#x3C;p&#x3E;Message is: &#123;&#123; message &#125;&#125;&#x3C;/p&#x3E;
</code></pre>
<pre><code class="language-javascript  line-numbers">var vm = new Vue({
        el: 'app',
        data:{
            message: ''
        }
    })
</code></pre>

<div id="app1" class="result-example">
<input v-model="message" placeholder="edit me">
<p>Message is: &#123;&#123; message &#125;&#125;</p>
</div>

<br>

<h3>Multiline text</h3>
<pre><code class="language-html  line-numbers">&#x3C;span&#x3E;Multiline message is:&#x3C;/span&#x3E;
&#x3C;p style=&#x22;white-space: pre-line;&#x22;&#x3E;&#123;&#123; message &#125;&#125;&#x3C;/p&#x3E;
&#x3C;br&#x3E;
&#x3C;textarea v-model=&#x22;message&#x22; placeholder=&#x22;add multiple lines&#x22;&#x3E;&#x3C;/textarea&#x3E;
</code></pre>
<div id="app2" class="result-example">
<span>Multiline message is:</span>
<p style="white-space: pre-line;">&#123;&#123; message &#125;&#125;</p>
<br>
<textarea v-model="message" placeholder="add multiple lines"></textarea>
</div>

<blockquote class="has-icon tip">
درج در textareas بصورت (&#x3C;textarea&#x3E;&#123;&#123;text&#125;&#125;&#x3C;/textarea&#x3E;) کار نمی کند. از v-model  استفاده نمایید.
</blockquote>
<br>

<h3>Checkbox</h3>
<p>
Checkbox تکی، مقدار boolean   :
</p>

<pre><code class="language-html  line-numbers">&#x3C;input type=&#x22;checkbox&#x22; id=&#x22;checkbox&#x22; v-model=&#x22;checked&#x22;&#x3E;
&#x3C;label for=&#x22;checkbox&#x22;&#x3E;&#123;&#123; checked &#125;&#125;&#x3C;/label&#x3E;
</code></pre>
<div id="app3" class="result-example">
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">&#123;&#123; checked &#125;&#125;</label>
</div>


<p>
checkboxes چک چندگانه ، محدود به همان آرایه:
</p>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x27;example-3&#x27;&#x3E;
  &#x3C;input type=&#x22;checkbox&#x22; id=&#x22;jack&#x22; value=&#x22;Jack&#x22; v-model=&#x22;checkedNames&#x22;&#x3E;
  &#x3C;label for=&#x22;jack&#x22;&#x3E;Jack&#x3C;/label&#x3E;
  &#x3C;input type=&#x22;checkbox&#x22; id=&#x22;john&#x22; value=&#x22;John&#x22; v-model=&#x22;checkedNames&#x22;&#x3E;
  &#x3C;label for=&#x22;john&#x22;&#x3E;John&#x3C;/label&#x3E;
  &#x3C;input type=&#x22;checkbox&#x22; id=&#x22;mike&#x22; value=&#x22;Mike&#x22; v-model=&#x22;checkedNames&#x22;&#x3E;
  &#x3C;label for=&#x22;mike&#x22;&#x3E;Mike&#x3C;/label&#x3E;
  &#x3C;br&#x3E;
  &#x3C;span&#x3E;Checked names: &#123;&#123; checkedNames &#125;&#125;&#x3C;/span&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
</code></pre>

<div id="app4" class="result-example">
    <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
    <label for="jack">Jack</label>
    <input type="checkbox" id="john" value="John" v-model="checkedNames">
    <label for="john">John</label>
    <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
    <label for="mike">Mike</label>
    <br>
    <span>Checked names: &#123;&#123; checkedNames &#125;&#125;</span>
</div>

<br>

<h3>Radio</h3>

<pre><code class="language-html  line-numbers">&#x3C;input type=&#x22;radio&#x22; id=&#x22;one&#x22; value=&#x22;One&#x22; v-model=&#x22;picked&#x22;&#x3E;
&#x3C;label for=&#x22;one&#x22;&#x3E;One&#x3C;/label&#x3E;
&#x3C;br&#x3E;
&#x3C;input type=&#x22;radio&#x22; id=&#x22;two&#x22; value=&#x22;Two&#x22; v-model=&#x22;picked&#x22;&#x3E;
&#x3C;label for=&#x22;two&#x22;&#x3E;Two&#x3C;/label&#x3E;
&#x3C;br&#x3E;
&#x3C;span&#x3E;Picked: &#123;&#123; picked &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<div id="app5" class="result-example">
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
<br>
<span>Picked: &#123;&#123; picked &#125;&#125;</span>
</div>
<br>

<h3>Select</h3>
<p>
انتخاب تکی:
</p>

<pre><code class="language-html  line-numbers">&#x3C;select v-model=&#x22;selected&#x22;&#x3E;
  &#x3C;option disabled value=&#x22;&#x22;&#x3E;Please select one&#x3C;/option&#x3E;
  &#x3C;option&#x3E;A&#x3C;/option&#x3E;
  &#x3C;option&#x3E;B&#x3C;/option&#x3E;
  &#x3C;option&#x3E;C&#x3C;/option&#x3E;
&#x3C;/select&#x3E;
&#x3C;span&#x3E;Selected: &#123;&#123; selected &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
</code></pre>

<div id="app6" class="result-example" >
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>Selected: &#123;&#123; selected &#125;&#125;</span>
</div>

<blockquote class="has-icon tip">
اگر مقدار اولیه  از v-model  شما با هیچ یک از گزینه ها مطابقت نداشته باشد ، عنصر &#x3C;select&#x3E; در حالت "انتخاب نشده" ارائه می شود. در iOS این باعث می شود کاربر نتواند اولین مورد را انتخاب کند. بنابراین توصیه می شود همانطور که در مثال بالا نشان داده شده است گزینه غیرفعال با مقدار خالی تهیه کنید.
</blockquote>

<p>
انتخاب چندگانه:
</p>

<pre><code class="language-html  line-numbers">&#x3C;select v-model=&#x22;selected&#x22; multiple&#x3E;
  &#x3C;option&#x3E;A&#x3C;/option&#x3E;
  &#x3C;option&#x3E;B&#x3C;/option&#x3E;
  &#x3C;option&#x3E;C&#x3C;/option&#x3E;
&#x3C;/select&#x3E;
&#x3C;br&#x3E;
&#x3C;span&#x3E;Selected: &#123;&#123; selected &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<div id="app7" class="result-example">
    <select v-model="selected" multiple>
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select>
    <span>Selected: &#123;&#123; selected &#125;&#125;</span>
</div>


<p>
گزینه های پویا ارائه شده با v-for:
</p>

<pre><code class="language-html  line-numbers">&#x3C;select v-model=&#x22;selected&#x22;&#x3E;
  &#x3C;option v-for=&#x22;option in options&#x22; v-bind:value=&#x22;option.value&#x22;&#x3E;
    &#123;&#123; option.text &#125;&#125;
  &#x3C;/option&#x3E;
&#x3C;/select&#x3E;
&#x3C;span&#x3E;Selected: &#123;&#123; selected &#125;&#125;&#x3C;/span&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
</code></pre>

<div id="app8" class="result-example">
    <select v-model="selected">
        <option v-for="option in options" v-bind:value="option.value">
            &#123;&#123; option.text &#125;&#125;
        </option>
    </select>
    <span>Selected: &#123;&#123; selected &#125;&#125;</span>
</div>

<br>

<h3> اتصال مقدار (Value Bindings)</h3>

<p>
برای radio, checkbox and select options ، مقادیر اتصال v-model  معمولاً رشته های استاتیک (یا booleans برای checkbox) است:
</p>

<pre><code class="language-javascript  line-numbers">&#x3C;!-- &#x60;picked&#x60; is a string &#x22;a&#x22; when checked --&#x3E;
&#x3C;input type=&#x22;radio&#x22; v-model=&#x22;picked&#x22; value=&#x22;a&#x22;&#x3E;

&#x3C;!-- &#x60;toggle&#x60; is either true or false --&#x3E;
&#x3C;input type=&#x22;checkbox&#x22; v-model=&#x22;toggle&#x22;&#x3E;

&#x3C;!-- &#x60;selected&#x60; is a string &#x22;abc&#x22; when the first option is selected --&#x3E;
&#x3C;select v-model=&#x22;selected&#x22;&#x3E;
  &#x3C;option value=&#x22;abc&#x22;&#x3E;ABC&#x3C;/option&#x3E;
&#x3C;/select&#x3E;
</code></pre>

<p>
اما گاهی اوقات ممکن است بخواهیم مقدار را به عنوان یک ویژگی پویا در نمونه Vue پیوند دهیم. برای دستیابی به آن می توانیم از v-bind استفاده کنیم. علاوه بر این ، استفاده از v-bind به ما امکان می دهد مقدار ورودی را به مقادیر غیر رشته وصل کنیم.
</p>
<br>
<h4>Checkbox</h4>

<pre><code class="language-html  line-numbers">&#x3C;input
  type=&#x22;checkbox&#x22;
  v-model=&#x22;toggle&#x22;
  true-value=&#x22;yes&#x22;
  false-value=&#x22;no&#x22;
&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">// when checked:
vm.toggle === 'yes'
// when unchecked:
vm.toggle === 'no'
</code></pre>

<blockquote class="has-icon tip">
ویژگی های true-value   و false-value   روی ویژگی value  ورودی تأثیر نمی گذارند ، زیرا مرورگرها چک باکس های چک نشده  را ارسال نمی کنند. 
</blockquote>

<br>
<h4>Radio</h4>

<pre><code class="language-html  line-numbers">&#x3C;input type=&#x22;radio&#x22; v-model=&#x22;pick&#x22; v-bind:value=&#x22;a&#x22;&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">// when checked:
vm.pick === vm.a
</code></pre>

<br>
<h4>Select Options</h4>

<pre><code class="language-html  line-numbers">&#x3C;select v-model=&#x22;selected&#x22;&#x3E;
  &#x3C;!-- inline object literal --&#x3E;
  &#x3C;option v-bind:value=&#x22;{ number: 123 }&#x22;&#x3E;123&#x3C;/option&#x3E;
&#x3C;/select&#x3E;
</code></pre>

<pre><code class="language-javascript  line-numbers">// when selected:
typeof vm.selected // => 'object'
vm.selected.number // => 123
</code></pre>

<br>

<h3>اصلاح کننده ها (Modifiers)</h3>
<h4>.lazy</h4>

<p>
به طور پیش فرض ، مدل v-model  بعد از هر رویداد ورودی ، ورودی را با داده ها همگام می کند (به استثنای ترکیب IME همانطور که گفته شد).می توانید از رویداد lazy به جای sync  بعد از رویداد change  استفاده نمایید :
</p>

<pre><code class="language-html  line-numbers">&#x3C;!-- synced after &#x22;change&#x22; instead of &#x22;input&#x22; --&#x3E;
&#x3C;input v-model.lazy=&#x22;msg&#x22; &#x3E;
</code></pre>
<br>
<h4>.number</h4>

<p>
اگر می خواهید ورودی کاربر به صورت خودکار بصورت عددی تایپ شود ، می توانید اصلاح کننده شماره را به ورودی های مدیریت شده v-model  خود اضافه کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;input v-model.number=&#x22;age&#x22; type=&#x22;number&#x22;&#x3E;
</code></pre>

<p>
این اغلب مفید است ، زیرا حتی با type="number" مقدار عناصر ورودی HTML همیشه یک رشته را برمی گرداند. اگر مقدار با ()parseFloat تجزیه نشود ، مقدار اصلی برمی گردد.
</p>

<br>
<h4>.trim</h4>

<p>
اگر می خواهید کاراکتر فضای خالی از ورودی کاربر به صورت خودکار حذف شود ، می توانید اصلاح کننده trim  را به ورودی های با مدیریت v-model خود اضافه کنید:
</p>

<pre><code class="language-html  line-numbers">&#x3C;input v-model.trim=&#x22;msg&#x22;&#x3E;
</code></pre>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
    var app1 = new Vue({
        el: '#app1',
        data:{
            message: ''
        }
    })
    
        var app2 = new Vue({
            el: '#app2',
            data:{
                message: ''
            }
        })
        
    var app3 = new Vue({
        el: '#app3',
        data:{
            checked: true
        }
    })        

    var app4 = new Vue({
        el: '#app4',
        data: {
            checkedNames: []
        }
    })
    
    var app5 = new Vue({
        el: '#app5',
        data: {
            picked: null
        }
    }) 
 
     var app6 = new Vue({
         el: '#app6',
         data: {
             selected: ''
         }
     })         
     
     var app7 = new Vue({
         el: '#app7',
         data: {
             selected: ''
         }
     })     
    var app8 = new Vue({
        el: '#app8',
        data: {
            selected: 'A',
            options: [
                { text: 'One', value: 'A' },
                { text: 'Two', value: 'B' },
                { text: 'Three', value: 'C' }
            ]
        }
    })     
         
</script>