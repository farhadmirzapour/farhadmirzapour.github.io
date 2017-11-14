---
layout: documentation
title:   Open Closed Principle
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا (OOD)
date:   2017-11-10 12:59:42 +0330
jdate: جمعه 19 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
# permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design <br> http://alihossein.ir/tutorials/آموزش-open-close-principle-solid <br> http://edu7.ir/blog/Tutorials/اصول-پنج-گانه-SOLID-در-برنامه-نویسی
---
<h3>مفهوم Open Closed Principle</h3>
<p>
OCP  به طور خلاصه این مفهوم را می رساند :
</p>

<blockquote>
<p>
اشیاء یا کلاس ها باید برای گسترش پیدا کردن باز ، اما برای ویرایش و تغییرات بسته شده باشند.
</p>
</blockquote>


<p>
جهت مفهوم این مطلب به متد ()sum کلاس AreaCalculator از مثال قبل توجه کنید :
</p>

<pre><code class="language-php   line-numbers">class AreaCalculator {

    protected $shapes;

    public function __construct($shapes = array()) {
        $this->shapes = $shapes;
    }

    public function sum() {
        foreach($this->shapes as $shape) {
            if(is_a($shape, 'Square')) {
                $area[] = pow($shape->length, 2);
            } else if(is_a($shape, 'Circle')) {
                $area[] = pi() * pow($shape->radius, 2);
            }
        }

        return array_sum($area);
    }
}
</code></pre>

<p>
مشکل اینجاست که اگر ما بخواهیم اشکال دیگری اضافه کنیم  و  یا حتی  حدف کنیم باید  بلاک شرطی “if”  واقع در متد "sum" را ویرایش نمائیم.
</p>

<p>
بنابراین با هر بار تغییر در کلاس "AreaCalculator"  مجبوریم  بررسی کنیم که آیا کد قبلی با تغییری که داده شد باز هم درست کار می کند و یا خیر ؟!!!.
</p>

<p>
خب در اینجا ما باید Open closed principle  را اعمال کنیم، در حقیقت این قاعده به ما می گوید که یک کلاس برای تغییرات باید بسته باشد، یعنی ما اجازه تغییر کلاس برای افزودن امکانات جدید را نداریم، اما راه برای ایجاد Extension یا افزونه جدیدی برای کلاس باز است.
</p>


<p>
برای برطرف کردن این مشکل فقط کافی است یک interface بسازیم و کلاس های مورد نظر که اشکال ما هستند را از این interface در واقع implements نموده و وظیفه محاسبه  محیط اشکال را به کلاس های خودشان انتقال دهیم.
</p>

<pre><code class="language-php   line-numbers">interface ShapeInterface {
    public function area();
}

class Circle implements ShapeInterface{
    public $radius;
    public function __construct($radius) {
        $this->radius = $radius;
    }
    public function area() {
        return pi() * pow($this->radius, 2);
    }
}

class Square implements ShapeInterface{
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }
    public function area() {
        return pow($shape->length, 2);
    }
}
</code></pre>

<p>
در خط 1 اینترفیس ShapeInterface را تعریف کردیم و در ادامه کلاس هایمان را از این اینترفیس implements کردیم تا مجبور شود متد area را داشته باشد .
</p>

<p>
سپس متد ()sum را مطالق زیر تغییر می دهیم :
</p>


<pre><code class="language-php  line-numbers">class AreaCalculator {

    protected $shapes;

    public function __construct($shapes = array()) {
        $this->shapes = $shapes;
    }

    public function sum() {
        foreach($this->shapes as $shape) {
            if(is_a($shape, 'ShapeInterface')) {
                $area[] = $shape->area();
                continue;
            }

            throw new AreaCalculatorInvalidShapeException;
        }

        return array_sum($area);
    }
}</code></pre>

<p>
دراینجا توانستیم اصل Open Close Principle را در کلاسمان ایجاد نماییم تا وابستگی کلاس AreaCalculator به اضافه و یا کم شدن اشکال مختلف را از بین ببریم .
</p>
