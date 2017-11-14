---
layout: documentation
title:   Liskov Substitution Principle
cattitle:   اصول S.O.L.I.D  در طراحی شی گرا (OOD)
date:   2017-11-12 21:34:42 +0330
jdate: یکشنبه 21 آبان 1396
caturl: 2017/11/10/solid-object-oriented-design.html
# permalink: /:categories/:title.html
thumbnail: Solid.jpg
refrence: https://en.wikipedia.org/wiki/Liskov_substitution_principle <br> http://pikneek.com/programming/مفهوم-solid-در-برنامه-نویسی-lsp/ <br> http://roocket.ir/articles/solid-principles-in-object-oriented-programming-part-iii-liskov-substitution-principal
---
<h3>مفهوم Liskov Substitution Principle</h3>
<p>
lsp  به طور خلاصه این مفهوم را می رساند :
</p>

<blockquote>
<p>
به این معنی که هیچ کلاسی نباید رفتار کلاس والد را تغییر دهد. برای رعایت این اصل باید در نظر داشته باشیم که هر کلاسی میتواند از کلاس دیگر ارث بری کند به شرطی که رفتار کلاس والد را تغییر ندهند. (کلاسهای به ارث رفته (مشتق شده) باید بتوانند جایگزین کلاسهای اصلی شوند.)
</p>
</blockquote>

<p>
تعویض پذیری یک اصل در برنامه نویسی شی گرا است که در یک برنامه کامپیوتری ، اگر S یک زیرمجموعه از T باشد، پس از آن اشیاء نوع T ممکن است با اشیاء نوع S جایگزین شوند (یعنی یک شی از نوع T می تواند جایگزین هر هدف از یک زیر نوع S) بدون تغییر هیچ کدام از خواص مطلوب T .
</p>

<p>
 به طور رسمی، اصل تعویض لیسکوف ( LSP ) یک تعریف خاص از رابطه زیر مجموعه است ( زیرمجموعه قوی) که در ابتدا توسط باربارا لیسکوف در سخنرانی یک کنفرانس در سال 1987 با نام انتزاع داده ها و سلسله مراتب معرفی شد . این یک رابطه معنایی است و نه صرفا نحوی ، زیرا در نظر دارد قابلیت همکاری معنایی انواع در یک سلسله مراتب را تضمین کند.
</p>
<p>
  باربارا لیسکوف و ژنیت وینگ این اصل را به صورت خلاصه در مقاله ای در سال 1994 فرموله کردند:

</p>


<blockquote>
<p><i>Subtype Requirement</i>: Let <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="http://www.w3.org/1998/Math/MathML" >
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03D5;<!-- ϕ --></mi>
        <mo stretchy="false">(</mo>
        <mi>x</mi>
        <mo stretchy="false">)</mo>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \phi (x)}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/546b660b2f3cfb5f34be7b3ed8371d54f5c74227" class="mwe-math-fallback-image-inline" aria-hidden="true" style="vertical-align: -0.838ex; width:4.566ex; height:2.843ex;" alt="\phi (x)" /></span> be a property provable about objects <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="http://www.w3.org/1998/Math/MathML" >
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>x</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle x}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4" class="mwe-math-fallback-image-inline" aria-hidden="true" style="vertical-align: -0.338ex; width:1.34ex; height:1.676ex;" alt="x" /></span> of type <span class="texhtml">T</span>. Then <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="http://www.w3.org/1998/Math/MathML" >
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03D5;<!-- ϕ --></mi>
        <mo stretchy="false">(</mo>
        <mi>y</mi>
        <mo stretchy="false">)</mo>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \phi (y)}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/db7ffe2f7daf9bae8d3f2711b2fd67348aceb3dc" class="mwe-math-fallback-image-inline" aria-hidden="true" style="vertical-align: -0.838ex; width:4.392ex; height:2.843ex;" alt="{\displaystyle \phi (y)}" /></span> should be true for objects <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="http://www.w3.org/1998/Math/MathML" >
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>y</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle y}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d" class="mwe-math-fallback-image-inline" aria-hidden="true" style="vertical-align: -0.671ex; width:1.166ex; height:2.009ex;" alt="y" /></span> of type <span class="texhtml">S</span> where <span class="texhtml">S</span> is a subtype of <span class="texhtml">T</span>.</p>
</blockquote>


<p>
جهت مفهوم این مطلب به مثال زیر توجه کنید :
</p>

<pre><code class="language-php   line-numbers">class Rectangle
{
    protected $width;
    protected $height;

    public function setHeight($height)
    {
        $this->height = $height;
    }

    public function getHeight()
    {
        return $this->height;
    }

    public function setWidth($width)
    {
        $this->width = $width;
    }

    public function getWidth()
    {
        return $this->width;
    }
    public function area()
    {
        return $this->width * $this->height;
    }
}
</code></pre>

<p>
کلاس Rectangle (مستطیل) را به عنوان یک کلاس پایه در نظر بگیرید. در دنیای واقعی شکل مربع هم نوعی مستطیل است. اما آیا در دنیای برنامه نویسی هم این موضوع صحیح است؟
</p>

<pre><code class="language-php   line-numbers">class Square extends Rectangle
{
    public function setHeight($value)
    {
        $this->width = $value;
        $this->height = $value;
    }

    public function setWidth($value)
    {
        $this->width = $value;
        $this->height = $value;
    }
}
</code></pre>

<p>
کلاس Square (مربع) از کلاس پایه ارث بری میکند. اما تفاوت این کلاس با کلاس پایه در این است که اگر هر یک از ابعاد طول و عرض مقدار دریافت کنند بعد دیگر نیز همان مقدار را خواهد داشت. یعنی اگر مقدار طول را 4 در نظر بگیریم, قطعا مقدار عرض نیز 4 خواهد بود.(خاصیت مربع)
</p>

<p>
حالا قطعه کد زیر را در نظر بگیرید:
</p>


<pre><code class="language-php  line-numbers">function areaVerifier(Rectangle $obj)
{
    echo 'area = '.$obj->area();
}

$obj1 = new Rectangle();
$obj1->setWidth(5);
$obj1->setHeight(4);
areaVerifier($obj1);

echo '<br>---------------------------<br/>';

$obj2 = new Square();
$obj2->setWidth(5);
$obj2->setHeight(4);
areaVerifier($obj2);
</code></pre>

<p>
تابع areaVerifier ورودی از نوع Rectangle را دریافت کرده و طول و عرض آنرا مقدار دهی میکند. و در نهایت بررسی میکند که آیا مساحت درست حساب شده است یا خیر. در ظاهر همه چیز خیلی خوب کار خواهد کرد. اما وقتی کلاس Square که نوعی از Rectangle هست را به عنوان ورودی به تابع areaVerifier بدهیم پاسخ کمی تغییر می کند. وقتی به متد setHeight واقع در خط 15 می رسیم بر اساس کاری که Square انجام میدهد مقدار Width را نیز برابر ۴ قرار میدهد. بنابراین مساحت ۱۶ خواهد شد نه 20 (دقت کنید تابع areaVerifier شی از نوع Square دریافت کرده است اما رفتار از نوع شی Rectangle  انتظار می رود اما مشاهده می کنیم رفتار توابع setWidth و setHeight کلاس والد خود را تغییر داده است).
</p>

<pre><code class="language-php ">// result
area is correct ---- area = 20
---------------------------
Bad area! ---- area = 16
</code></pre>

<p>
خوب این به چه معنی است؟ به این معنی که ما نمیتوانیم کلاس پایه را با کلاس مشتق شده جایگزین کنیم و باز هم این معنی را میدهد که ما داریم اصل LSP را نقض میکنیم.
</p>

<p>
این موضوع هدف اصلی LSP است و به ما آموزش میدهد که هنگام ارث بری از یک کلاس نباید رفتار کلاس والد را تغییر دهیم.
</p>

<p>
<strong>
اما راه حل چیست؟
</strong>
</p>

<p>
یک کلاس انتزاعی (abstract) را به شکل زیر ایجاد و سپس دوکلاس Square و Rectangle  را از آن مشتق میکنیم :
</p>

<pre><code class="language-php  line-numbers"><?php
abstract class Shape
{
    protected $width;
    protected $height;
    abstract public function setHeight($height);
    abstract public function setWidth($width);
    public function getHeight()
    {
        return $this->height;
    }
    public function getWidth()
    {
        return $this->width;
    }
    public function area()
    {
        return $this->width * $this->height;
    }
}


class Rectangle extends Shape
{
    public function setWidth($width)
    {
        $this->width = $width;
    }
    public function setHeight($height)
    {
        $this->height = $height;
    }
}


class Square extends Shape
{
    public function setHeight($value)
    {
        $this->width = $value;
        $this->height = $value;
    }

    public function setWidth($value)
    {
        $this->width = $value;
        $this->height = $value;
    }
}
</code></pre>

<p>
همچنین تابع areaVerifier را مطابق زیر تغییر می دهیم :
</p>

<pre><code class="language-php  line-numbers"><?php
function areaVerifier(Shape $obj)
{
    echo 'area = '.$obj->area();
}

$obj1 = new Rectangle();
$obj1->setWidth(5);
$obj1->setHeight(4);
areaVerifier($obj1);

echo '<br>---------------------------<br/>';

$obj2 = new Square();
$obj2->setWidth(5);
$obj2->setHeight(4);
areaVerifier($obj2);
</code></pre>

<p>
مشاهده می کنیم اشیا کلاس های Square و Rectangle مطابق رفتار مورد انتظارشان عمل می کنند.
</p>


<pre><code class="language-php ">area = 20
---------------------------
area = 16
</code></pre>