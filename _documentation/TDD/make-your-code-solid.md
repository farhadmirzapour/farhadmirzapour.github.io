---
layout: documentation
title:   نوشتن کد به سبک S.O.L.I.D
cattitle: اصول Test Driven Development
date:   2017-11-19 09:40:42 +0330
jdate: یکشنبه 28 آبان 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: https://www.telerik.com/blogs/30-days-of-tdd-day-five-make-your-code-solid
---
<p>
پیش از این ، با مطلبی تحت عنوان
 <a href="/2017/11/10/solid-object-oriented-design.html" title="اصول S.O.L.I.D در طراحی شی گرا (OOD)" target="_blank">اصول S.O.L.I.D در طراحی شی گرا (OOD)</a>
 با این مبحث آشنا شدیم و به تفکیک به بررسی 5 اصل اساسی آن پرداختیم.
</p>

<p>
در این قسمت بررسی خواهیم کرد که چگونه کدهای S.O.L.I.D به ما در TDD کمک خواهد کرد. این اصول بطور خلاصه عبارتند از :
</p>

<p>
<ul>
<li>
<a href="/documentation/solid-object-oriented-design/single-responsibility-principle" target="_blank">SRP) Single responsibility principle)</a>
</li>
<li>
<a href="/documentation/solid-object-oriented-design/open-closed-principle" target="_blank">OCP) Open closed principle)</a>
</li>
<li>
<a href="/documentation/solid-object-oriented-design/liskov-substitution-principle" target="_blank">LSP) Liskov substitution principle)</a>
</li>
<li>
<a href="/documentation/solid-object-oriented-design/interface-segregation-principle" target="_blank">ISP) Interface segregation principle)</a>
</li>
<li>
<a href="/documentation/solid-object-oriented-design/dependency-inversion-principle" target="_blank">DIP) Dependency inversion principle)</a>
</li>
</ul>
</p>


<h3>نقش SRP) Single responsibility principle) در TDD</h3>
<p>
یک کلاس باید یک و تنها یک دلیل برای تغییر داشته باشد، به این معنی که یک کلاس فقط باید یک مسئولیت داشته باشد
</p>
<p>
 متدهایی که بیش از یک وظیفه را برعهده دارند، نیاز به آزمون هایی با پیچیدگی و دقت بیشتری دارند همچنین باید دقت شود به تنوع وظایف، ترتیب اجرای وظایف در آزمون حفظ گردد،  که آزمون را طولانی تر می کنند و برای درک و حفظ آن با مشکلات بیشتری مواجه خواهیم شد.
</p>

<p>
به دلیل اینکه در SRP تنها یک وظیفه انجام می شود بنابراین نوشتن و درک آزمون ساده تر و همچنین درصورتیکه آزمون با شکست مواجه شد تنها نیاز به بررسی یک مکان برای بررسی عدم صحیح اجرا خواهد بود.
 </p>


<h3>نقش OCP) Open closed principle) در TDD</h3>
<p>

<p>


<h3>نقش LSP) Liskov substitution principle) در TDD</h3>
<p>
کلاسهای به ارث رفته (مشتق شده) باید بتوانند جایگزین کلاسهای اصلی شوند.
</p>

<p>
LSP به ما کمک می کند که کدمان را با ایجاد جایگزین تست کنیم.</p>

<h3>نقش ISP) Interface segregation principle) در TDD</h3>
<p>
تعداد بیشتری اینترفیس کوچک و خاص، بهتر از یک اینترفیس بزرگ (چاق) با متدهای بیشتر است.
</p>

<p>
ISP  سبب می شود اینترفیس ها و کلاس ها کوچکتر شوند بنابراین تست های نوشته نیز ساده تر و دارای پیچیدگی کمتری باشند.
</p>
<h3>نقش DIP) Dependency inversion principle) در TDD</h3>


<h3>dependency-injection</h3>