---
layout: documentation
title:  چندریختی (Polymorphism)
cattitle: آموزش شی گرایی در php
date:   2017-11-02 21:50:42 +0330
jdate: پنج شنبه 11 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
refrence: http://tahlildadeh.com/ArticleDetails/آموزش-چند-ریختی-Polymorphism-در-PHP
---
<div align="center">
<img src="/images/original/php-oop-course.png" alt="{{page.title}}" />
</div>

<h3>چندریختی (Polymorphism) در PHP</h3>
<p>
بر اساس اصل چندریختی، متدهایی که در کلاس های مختلف عملیات یکسانی را انجام می دهند، باید نام یکسانی هم داشته باشند. توسعه دهنده می تواند با بهره گیری از این اصل کد اپلیکیشن خود را به صورت منسجم و با قابلیت استفاده ی آسان تعریف کند.
</p>

<p>
یک مثال کاربردی کلاس هایی است که نمایانگر اشکال مختلف هندسی (نظیر مستطتیل، دایره و هشت گوشی) مختلف بوده که از نظر ظاهر، تعداد ضلع و همچنین فرمولی که مساحت بر اساس آن محاسبه می شود با یکدیگر فرق دارند. با این وجود تمامی این کلاس ها دارای یک ویژگی مشترک به نام مساحت هستند که باید محاسبه شود. اصل چندریختی بیان می دارد که در چنین شرایطی تمامی متدهایی که عملیات (یکسان) محاسبه ی مساحت شکل هندسی را انجام می دهند (مهم نیست برای کدام کلاس یا شکل هندسی) می بایست دارای اسمی یکسان باشند.
</p>

<p>
به طور مثال، می توانیم متدی که مساحت را محاسبه می کند calcArea() نام گذاری کرده و سپس در هر کلاسی که نمایانگر یک شکل هندسی است، متدی با همین اسم که عملیات محاسبه ی مساحت را با توجه به ظاهر شکل هندسی انجام می دهد پیاده سازی یا فراخوانی کنیم. حال هر زمان که می خواهیم مساحت اشکال هندسی مختلف را محاسبه کنیم، متدی به نام calcArea() را فراخوانی می کنیم. بدین وسیله دیگر لازم نیست نگران جزئیات فنی و پیاده سازی عملیات محاسبه ی مساحت اشکال هندسی مختلف باشیم. به عبارت ساده تر کافی است برای محاسبه ی مساحت شکل هندسی مورد نظر (خواه مستطیل باشد خواه دایره) یک متد واحد به نام calcArea() را صدا بزنیم.
</p>

<h3>نحوه ی پیاده سازی اصل چندریختی (polymorphism)</h3>

<p>
به منظور پیاده سازی اصل چندریختی، می توانیم از کلاس های انتزاعی (abstract) یا interface ها یکی را انتخاب کنیم. به عبارت دیگر، برای اینکه مطمئن شویم کلاس های مشتق (فرزند) اصل چندریختی را پیاده سازی می کنند، می بایست بین کلاس های abstract یا interface ها یکی را انتخاب کنیم. در مثال زیر، interface هایی که shape نام گذاری شده اند، کلیه ی کلاس هایی که این interface را پیاده سازی می کنند مجاب به تعریف بدنه ی متدی به نام calcArea() می نمایند.
</p>


<pre><code class="language-php  line-numbers">interface Shape {
  public function calcArea();
}
</code></pre>

<p>
در مثال زیر، کلاس circle با تعریف بدنه و قرار دادن فرمول محسابه ی مساحت داخل ساختمان متد ()calcArea، مساحت شکل هندسی دایره را محاسبه کرده و در خروجی برمی گرداند.
</p>

<pre><code class="language-php  line-numbers">class Circle implements Shape {
  private $radius;
  public function __construct($radius)
  {
    $this -> radius = $radius;
  }
  // calcArea calculates the area of circles
  public function calcArea()
  {
    return $this -> radius * $this -> radius * pi();
  }
}
</code></pre>


<p>
کلاس Rectangle در مثال زیر نیز اینترفیس Shape را پیاده سازی کرده و متد calcArea() را فراخوانی می کند، اما این بار منطق متد نام برده را طوری می نویسد که مساحت شکل هندسی مستطیل را محسابه نموده و در خروجی برگرداند.
</p>

<pre><code class="language-php  line-numbers">class Rectangle implements Shape {
  private $width;
  private $height;
  public function __construct($width, $height)
  {
    $this -> width = $width;
    $this -> height = $height;
  }
  // calcArea calculates the area of rectangles  
  public function calcArea()
  {
    return $this -> width * $this -> height;
  }
}
</code></pre>


<p>
اکنون می توانیم از روی کلاس های غیر انتزاعی نام برده، به صورت زیر نمونه سازی کنیم:
</p>

<pre><code class="language-php  line-numbers">$circ = new Circle(3);
$rect = new Rectangle(3,4);
</code></pre>


<p>
بنابراین تا زمانی که کلاس های ما اینترفیس Shape را پیاده سازی می کنند، دو آبجکت فوق مساحت را با فراخوانی متدی که هم نام با caclArea() (اعلان شده در اینترفیس Shape) است، محاسبه نموده و در خروجی برمی گردانند، خواه این شکل هندسی مستطیل باشد و خواه دایره.
</p>

<p>
حال با فراخوانی متد calcArea() از آبجکت های $circ و $rect مساحت مستطیل و دایره محاسبه می شوند.
</p>

<pre><code class="language-php  line-numbers">echo $circ -> calcArea();
echo $rect -> calcArea();
</code></pre>

<p>خروجی:</p>

<pre><code class="language-php">28.274333882308
12</code></pre>