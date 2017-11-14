---
layout: documentation
title:   اصول S.O.L.I.D  در طراحی شی گرا  (OOD)
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا  (OOD)
date:   2017-11-10 10:00:42 +0330
jdate: جمعه 19 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
#  permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: http://peyvasteh.net/5-اصل-پایه-ای-طراحی-شی-گرا-solid-به-زبان-ساده/ <br> http://alihossein.ir/tutorials/اصول-طراحی-شی-گرا-solid-چیست  <br> https://code.tutsplus.com/tutorials/solid-part-1-the-single-responsibility-principle--net-36074
---
<p>
SOLID مخفف پنج اصل زیر بنایی طراحی شی گرا (Object-Oriented Design) است که اوایل سال ۲۰۰۰ میلادی توسط Robert C. Martin یا همان Uncle Bob معرفی شد.
</p>

<p>
یکی از مشکلاتی که طراحی نامناسب برنامه های شی گرا برای برنامه نویسان ایجاد می کند موضوع مدیریت وابستگی در اجزای برنامه می باشد. اگر این وابستگی به درستی مدیریت نشود مشکلاتی شبیه موارد زیر در برنامه ایجاد می شوند:
</p>

<ul>
<li>
<p>
<strong>مشکل  Rigidity : </strong>
برنامه ی نوشته شده را نمی توان تغییر داد و یا قابلیت جدید اضافه کرد. دلیل آن هم این است که با ایجاد تغییر در قسمتی از برنامه، این تغییر به صورت آبشاری در بقیه ی قسمت ها منتشر می شود و مجبور خواهیم بود که قسمت های زیادی از برنامه را تغییر دهیم.
</p>
</li>
<li>
<p>
<strong>مشکل  Fragility : </strong>
تغییر دادن برنامه مشکل است و آن هم به این دلیل که با ایجاد تغییر در یک قسمت از برنامه، قسمت های دیگر برنامه از کار می افتند و دچار مشکل می شوند.
</p>
</li>
<li>
<p>
<strong>مشکل  Immobility : </strong>
قابلیت استفاده مجدد از اجزای برنامه وجود ندارد. در واقع، قسمت های مجدد برنامه ی شی گرای شما آنچنان به هم وابستگی تو در تو دارند که به هیچ وجه نمی توانید یک قسمت را جدا کرده و در برنامه ی دیگری استفاده کنید.
</p>
</li>
</ul>

<p>
اگر هنگام طراحی و برنامه نویسی از این اصول پیروی کنیم و رعایت این اصول و قوانین را مد نظر داشته باشیم، نرم افزار توسعه یافته شده را میتوان راحتتر در طول زمان توسعه (Extensibility) داد و نگهداری (Maintainable) کرد.
</p>

<p>
اگر با Unit Testing یا همان تست واحد! آشنا باشید و در یک پروژه نسبتا قدیمی کار کنید و بخواهید برای این سیستم Unit Test بنویسید به زودی متوجه میشود که به راحتی نمیتوان برای آن تست درست حسابی نوشت. امکان دست زدن به کلاسها، جدا کردن کلاسها، جایگزین کردن آنها توسط Mock ها و Stub ها وجود ندارد و عملا امکان نوشتن Unit Test استاندارد و مفید وجود ندارد.
</p>

<p>
دلیل این مشکل این است که هنگام توسعه اولیه نرم افزار، نوشتن کدی که قابل تست باشد مد نظر نبوده است (یا اولویت نداشته) و یا اصول و قوانین پایه ای طراحی شی گرا رعایت نشده است. خب در این مرحله اکثر برنامه نویسان و توسعه دهندگان شروع به Refactoring یا بهینه سازی کدها میکنند. ولی باید از کجا شروع کرد؟
</p>

<p>
دقیقا در این نقطه این ۵ اصل به کمک ما می آیند و با استفاده از آنها میتوانیم کدهای فعلی را تا حد زیادی بهبود داده و مرحله به مرحله قابلیت تست را به آنها اضافه کنیم.
</p>

<p>
اگر هم از متدهای جدید برنامه نویسی مانند توسعه آزمایش محور (Test Driven Development) استفاده کنیم و واقعا TDD کار کنیم! ناخودآگاه بسیاری از این اصول در کدهای و کلاس های نوشته شده وجود دارند.
</p>

<p>Solid پنج اصل پایه ای است که به ما کمک می کند معماری نرم افزار خوبی داشته باشیم. Solid مخفف:</p>
<ul>
<li>
<a href="/documentation/solid-object-oriented-design/single-responsibility-principle" target="_blank" >S مخفف SRP) Single responsibility principle)</a>
<p>
یک کلاس باید یک و تنها یک دلیل برای تغییر داشته باشد، به این معنی که یک کلاس فقط باید یک مسئولیت داشته باشد
</p>
</li>
<li><a href="/documentation/solid-object-oriented-design/open-closed-principle" target="_blank">O مخفف OCP) Open closed principle)</a>
<p>
شما باید بتوانید رفتار یک کلاس را توسعه دهید بدون اینکه آنرا تغییر دهید. (کد آنرا دستکاری کنید)
</p>
</li>
<li><a href="/documentation/solid-object-oriented-design/liskov-substitution-principle" target="_blank">L مخفف LSP) Liskov substitution principle)</a>
<p>
کلاسهای به ارث رفته (مشتق شده) باید بتوانند جایگزین کلاسهای اصلی شوند.
</p>
</li>
<li><a href="/documentation/solid-object-oriented-design/interface-segregation-principle" target="_blank">I مخفف ISP) Interface segregation principle)</a>
<p>
تعداد بیشتری اینترفیس کوچک و خاص، بهتر از یک اینترفیس بزرگ (چاق) با متدهای بیشتر است.
</p>
</li>
<li><a href="/documentation/solid-object-oriented-design/dependency-inversion-principle" target="_blank">D مخفف DIP) Dependency inversion principle)</a>
<p>
وابستگی به Abstraction و نه Concrete Implementation
</p>
</li>
</ul>