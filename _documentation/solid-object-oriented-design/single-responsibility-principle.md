---
layout: documentation
title:   Single Responsibility Principle
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا (OOD)
date:   2017-11-10 11:04:42 +0330
jdate: جمعه 19 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
# permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: https://scotch.io/bar-talk/s-o-l-i-d-the-first-five-principles-of-object-oriented-design <br> http://alihossein.ir/tutorials/آموزش-single-responsibility-principle-solid
---
<h3>مفهوم Single Responsibility Principle</h3>
<p>
SRP  به طور خلاصه این مفهوم را می رساند :
</p>

<p>
<blockquote>
<p>
یک کلاس باید یک و تنها یک دلیل برای تغییر داشته باشد، به این معنی که یک کلاس فقط باید یک مسئولیت داشته باشد
</p>
</blockquote>
</p>

<p>
وقتی دو دلیل مختلف برای تغییر یک کلاس وجود داشته باشد بنابراین ممکن است دو تیم مختلف این کار را انجام دهند. در نهایت یک کلاس توسط دو تیم مختلف ویرایش می شود و این سبب می شود تا پروسه سوال و جواب برای هماهنگی و … طولانی شود.
</p>


<p>
برای مثال ما تعدادی اشکال هندسی داریم (مربع و دایره و …) و می خواهیم مجموع محیط های این اشکال را حساب نماییم .
</p>

<pre><code class="language-php  line-numbers">class Circle {
    public $radius;

    public function __construct($radius) {
        $this->radius = $radius;
    }
}

class Square {
    public $length;

    public function __construct($length) {
        $this->length = $length;
    }
}
</code></pre>

<p>
در کد بالا کلاس های اشکال مورد نظر خود را ایجاد کردیم و سپس  کلاس AreaCalculator را مطابق زیر جهت محاسبه مجموع محیط اشکال می نویسیم :
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

    public function output() {
        return implode('', array(
            "",
                "Sum of the areas of provided shapes: ",
                $this->sum(),
            ""
        ));
    }
}
</code></pre>

<p>
برای استفاده از کلاس AreaCalculator، ما به سادگی نمونه ای از آن ایجاد و سپس آرایه ای از اشکال  را به آن منتقل می کنیم و سپس با استفاده از متد sum مجموع آنها را حساب می کنیم و در نهایت با استفاده از متد output نتیجه را در قالب HTML چاپ می کنیم .
</p>

<pre><code class="language-php  line-numbers">$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);

echo $areas->output();
</code></pre>

<p>
خب حالا مشکل چی هست ؟ مشکل اینجاست که اگر بخواهیم به جای خروجی HTML به ما json بدهد چیکار باید بکنیم ؟
</p>
<p>
طبق اصل Single responsiblity principle که هر کلاس فقط باید یک کار انجام دهد عمل می کنیم .کلاس AreaCalculator فقط باید مجموعهای از اشکال ارائه شده را جمع کند و نباید نگرانی داشت که آیا کاربر میخواهد خروجی json یا HTML داشته باشد .
</p>

<p>
بنابراین، برای رفع این مشکل شما می توانید یک کلاس به نام SumCalculatorOutputter ایجاد کنید و از این کلاس برای رسیدگی به هر منطقی جهت نمایش داده ها استفاده کنید:
</p>


<pre><code class="language-php   line-numbers">class SumCalculatorOutputter {

    protected $calculator;

    public function __constructor(AreaCalculator $calculator) {
        $this->calculator = $calculator;
    }

    public function JSON() {
        // logic to show json data
    }

     public function HTML() {
            // logic to show html data
     }
}
</code></pre>


<p>
بنابراین تابع output() را از کلاس AreaCalculator حدف می نمائیم :
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
بنابراین جهت محاسبه و سپس نمایش نتیجه بدین صورت عمل می کنیم :
</p>

<pre><code class="language-php  line-numbers">$shapes = array(
    new Circle(2),
    new Square(5),
    new Square(6)
);

$areas = new AreaCalculator($shapes);
$output = new SumCalculatorOutputter($areas);

echo $output->JSON();
echo $output->HTML();
</code></pre>