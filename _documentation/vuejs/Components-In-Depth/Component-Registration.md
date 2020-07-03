---
layout: documentation-vuejs
title:    ثبت Component
cattitle:   اصول آموزش vuejs
date:   2019-11-15 12:14:42 +0330
jdate: جمعه 24 آبان 1398
caturl: 2019/10/11/vuejs.html
#  permalink: /:categories/:title.html
directory : vuejs/Components-In-Depth
menu : false
thumbnail: vuejs.jpg
refrence: https://vuejs.org/v2/guide/components-registration.html
---
<p>
</p>


<h3 >نامگداری Component</h3>

<p>جهت ثبت یک کامپوننت نامی به آن اختصاص داده می شود:</p>
<pre><code class="language-javascript  line-numbers">Vue.component('my-component-name', { /* ... */ })
</code></pre>

<p>
نام کامپوننت اولین آرگومان <code>Vue.component</code> می باشد.
</p>

<p>
نامی که شما به یک component می دهید بستگی به مکانی دارد که قصد استفاده از آن را دارید. هنگام استفاده از یک component به طور مستقیم در DOM (بر خلاف یک template رشته ای یا component تک فایلی) ، ما به شدت توصیه می کنیم از قوانین W3C برای نام های تگ های سفارشی پیروی کنید (همه حروف کوچک و باید دارای خط ربط باشد). این به شما کمک می کند تا از تعارض با عناصر HTML فعلی و آینده جلوگیری کنید.
</p>
<p>
همچنین می توانید پیشنهادات نامگداری را در بخش <a target="_blank" href="https://vuejs.org/v2/style-guide/#Base-component-names-strongly-recommended">Style Guide</a> مشاهده نمایید.
</p>

<br>

<h3>سبک نامگذاری (Name Casing)</h3>
<p>شما دو انتخاب جهت سبک نامگذاری در اختیار دارید :</p>

<h4>روش kebab-case</h4>
<pre><code class="language-javascript  line-numbers">Vue.component('my-component-name', { /* ... */ })
</code></pre>

<p>
هنگام تعریف یک Component به سبک kebab ، جهت دسترسی به عنصر سفارشی آن ، همانند &#x3C;my-component-name&#x3E;  ، عمل می کنیم:
</p>

<h4>روش PascalCase</h4>
<pre><code class="language-javascript  line-numbers">Vue.component('MyComponentName', { /* ... */ })
</code></pre>
<p>
هنگام نامگداری یک Component به سبک PascalCase ، می توانید هنگام مراجعه به عنصرسفارشی آن ، از هر دو سبک استفاده کنید. این بدان معنی است که هر دو &#x3C;my-component-name&#x3E; و &#x3C;MyComponentName&#x3E; قابل قبول هستند. با این حال توجه داشته باشید که فقط نامهای به سبک kebab به طور مستقیم در DOM معتبر هستند (به عنوان مثال templates غیر رشته ای).
</p>
<br>

<h3> ثبت سراسری (Global Registration)</h3>

<p>تاکنون ما فقط با استفاده از Vue.component کامپوننت هایی  ایجاد کرده ایم:</p>
<pre><code class="language-javascript  line-numbers">Vue.component('my-component-name', {
  // ... options ...
})
</code></pre>

<p>
این  کامپوننت ها به صورت سراسری ثبت شده اند. این بدان معنی است که آنها می توانند در قالب نمونه های اصلی Vue (Vue new) ایجاد شده پس از ثبت  مورد استفاده قرار گیرند. مثلا:
</p>

<pre><code class="language-javascript  line-numbers">Vue.component('component-a', { /* ... */ })
Vue.component('component-b', { /* ... */ })
Vue.component('component-c', { /* ... */ })

new Vue({ el: '#app' })
</code></pre>

<pre><code class="language-html  line-numbers">&#x3C;div id=&#x22;app&#x22;&#x3E;
  &#x3C;component-a&#x3E;&#x3C;/component-a&#x3E;
  &#x3C;component-b&#x3E;&#x3C;/component-b&#x3E;
  &#x3C;component-c&#x3E;&#x3C;/component-c&#x3E;
&#x3C;/div&#x3E;
</code></pre>

<p>
این حتی برای همه subcomponents نیز صدق می کند ، بدین معنی که هر سه این کامپوننت ها نیز در داخل یکدیگر در دسترس خواهند بود.
</p>
<br>

<h3> ثبت محلی (Local Registration)</h3>
<p>
ثبت سراسری غالباً ایده آل نیست. به عنوان مثال ، اگر از سیستم ساختاری مانند Webpack استفاده می کنید ، ثبت سراسری همه کامپوننت ها بدین معنی است که حتی اگر استفاده از یک کامپوننت را متوقف کنید ، هنوز هم می تواند در ساخت نهایی شما گنجانده شود. این باعث افزایش ناخواسته جاوا اسکریپت کاربران شما برای بارگیری می شود.
</p>

<p>
در این موارد ، می توانید اجزای خود را به عنوان اشیاء ساده JavaScript تعریف کنید:
</p>

<pre><code class="language-javascript  line-numbers">var ComponentA = { /* ... */ }
var ComponentB = { /* ... */ }
var ComponentC = { /* ... */ }
</code></pre>

<p>
سپس کامپوننت هایی را که می خواهید از آنها استفاده کنید در خاصیت کامپوننت ها تعریف کنید:
</p>

<pre><code class="language-javascript  line-numbers">new Vue({
  el: '#app',
  components: {
    'component-a': ComponentA,
    'component-b': ComponentB
  }
})
</code></pre>

<p>
برای هر خاصیت در آبجکت components، کلید نام المان سفارشی خواهد بود.
</p>

<p>
توجه داشته باشید که کامپوننت های محلی ثبت شده در subcomponents در دسترس نیستند. به عنوان مثال ، اگر می خواهید ComponentA در ComponentB موجود باشد ، باید استفاده کنید:
</p>

<pre><code class="language-javascript  line-numbers">var ComponentA = { /* ... */ }

var ComponentB = {
  components: {
    'component-a': ComponentA
  },
  // ...
}
</code></pre>

<p>
یا اگر از ماژول های ES2015 استفاده می کنید ، از جمله از طریق Babel و Webpack ، بدین صورت خواهد بود:
</p>

<pre><code class="language-javascript  line-numbers">import ComponentA from './ComponentA.vue'

export default {
  components: {
    ComponentA
  },
  // ...
}
</code></pre>
<br>

<h3>  سیستم ماژول (Module Systems)</h3>
<p>
چنانچه در سیستم ماژول از import/require استفاده نمی کنید ، احتمالاً اکنون می توانید از این بخش استفاده کنید:
</p>

<h4>کامپوننت محلی در سیستم ماژول (Local Registration in a Module System)</h4>
<p>
چنانچه شما از یک سیستم ماژول مانند Babel و Webpack. استفاده می کنید در این موارد ، ما توصیه می کنیم که یک دایرکتوری components ایجاد و هر یک از کامپوننت ها را در فایل خود قرار دهید.
</p>

<p>
سپس هر کامپوننتی که قرار است مورد استفاده قرار گیرد را قبل از ثبت بصورت محلی وارد نمایید.به عنوان مثال در فایل ComponentB.js یا ComponentB.vue :
</p>

<pre><code class="language-javascript  line-numbers">import ComponentA from './ComponentA'
import ComponentC from './ComponentC'

export default {
  components: {
    ComponentA,
    ComponentC
  },
  // ...
}
</code></pre>

<p>
هم اکنون هر دو کامپوننت ComponentA و ComponentC می توانند در قالب کامپوننت ComponentB مورد استفاده قرار گیرند.
</p>

<br>

<h3>  ثبت سراسری کامپوننت های پایه بصورت اتوماتیک  (Automatic Global Registration of Base Components)</h3>
<p>
بسیاری از کامپوننت های شما نسبتاً عمومی خواهند بود همانند دکمه ها و فیلدهای ورودی، ما بعضی اوقات به این موارد به عنوان اجزای پایه اشاره می کنیم و آنها معمولاً بسیار مورد استفاده قرار می گیرند.
</p>

<pre><code class="language-javascript  line-numbers">import BaseButton from './BaseButton.vue'
import BaseIcon from './BaseIcon.vue'
import BaseInput from './BaseInput.vue'

export default {
  components: {
    BaseButton,
    BaseIcon,
    BaseInput
  }
}
</code></pre>

<p>
خوشبختانه ، اگر از Webpack استفاده می کنید (یا Vue CLI 3+ ، که از Webpack به صورت داخلی استفاده می کند) ، می توانید از Requ.context جهت ثبت سراسری این کامپوننت های بسیار رایج استفاده کنید. (به عنوان مثال src / main.js):
</p>

<pre><code class="language-javascript  line-numbers">import Vue from 'vue'
import upperFirst from 'lodash/upperFirst'
import camelCase from 'lodash/camelCase'

const requireComponent = require.context(
  // The relative path of the components folder
  './components',
  // Whether or not to look in subfolders
  false,
  // The regular expression used to match base component filenames
  /Base[A-Z]\w+\.(vue|js)$/
)

requireComponent.keys().forEach(fileName => {
  // Get component config
  const componentConfig = requireComponent(fileName)

  // Get PascalCase name of component
  const componentName = upperFirst(
    camelCase(
      // Gets the file name regardless of folder depth
      fileName
        .split('/')
        .pop()
        .replace(/\.\w+$/, '')
    )
  )


  // Register component globally
  Vue.component(
    componentName,
    // Look for the component options on `.default`, which will
    // exist if the component was exported with `export default`,
    // otherwise fall back to module's root.
    componentConfig.default || componentConfig
  )
})
</code></pre>

<p>
توجه داشته باشید که ثبت سراسری باید قبل از ایجاد نمونه root Vue انجام شود. (با Vue new)
</p>