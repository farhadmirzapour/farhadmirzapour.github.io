---
layout: documentation
title:  متدهای جادوئی در شی گرایی php
cattitle: آموزش شی گرایی در php
date:   2017-11-01 20:59:42 +0330
jdate: چهارشنبه 10 آبان 1396
caturl: 2017/11/01/object-oriented-programming-in-php.html
# permalink: /:categories/:title.html
thumbnail: phpOOPTutorialPAGE.png
refrence: https://roocket.ir/articles/object-oriented-programming-in-php-part-5
---
<h3>متدهای جادوئی در شی گرایی php</h3>
<p>
php برای ساده تر کردن بعضی از کارها در کلاس ها یک سری متدهای خاص و ویژه در شی گرایی قرار داده که این موضوع به توسعه دهندگان اجازه میده تا تعدادی از کارهای مفید رو به سهولت انجام بدن .
</p>

<h3>استفاده از Constructors , Destructors</h3>
<p>
بزارید اینطور براتون توضیح بدم. زمانی که شما یک object رو از کلاسی میسازید در همون ابتدای ساختن بطور اتوماتیک می خواید یک سری اعمال انجام بشه . شما اینکار رو به سادگی با متد <span class="notespan">()construct__</span>  می تونید انجام بدید. یه ویژگی در مورد متدهای جادوئی که یادم رفت در بالا بگم اینکه تمام متدهای جادوئی دارای دو Underscore یا ( __ ) در قبل اسم متد هستن که به این صورت میشه فهمید که اون متد یک متد جادوئیه .
</p>
<p>
حالا اگه شما بخواین راحت تر با کار <span class="notespan">()construct__</span> آشنا بشین در زیر یک مثال از این متد میزنم که در هنگام ایجاد شدن obj یک مقداری رو بطور اتوماتیک چاپ کنه .
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}

// Create a new object
$obj = new MyClass;

// Get the value of $prop1
echo $obj->getProperty();

// Output a message at the end of the file
echo "End of file.<br />";
</code></pre>

<p>
در بالا شما در یک قسمت با __CLASS__ مواجه شدید که این یک ثابت جادوئیه ( magic constant ) که دقیقا مثل متدهای جادوئی یک سری اعمال رو انجام میده مثلا در این جا __CLASS__ اسم کلاسی که توش قرار داره رو بر میگردونه . شما با مراجعه به این صفحه میتونید مابقی magic constant رو ببینید .
</p>
<p>
در زیر میتونید نتیجه کد بالا رو مشاهده کنید .
</p>

<pre><code class="language-php">
The class "MyClass" was initiated!
I'm a class property!
End of file.
</code></pre>

<p>
خب حالا میرسیم به متد <span class="notespan">()destruct__</span> . زمانی که یک Object نابود میشه این متد فراخونی و اجرا میشه یکی از مثال های این مورد میتونه زمانی که ارتباط با دیتابیس بسته میشه باشه که این متد اجرا بشه و یه سری کارها رو انجام بده .
</p>
<p>
در زیر با استفاده از این متد زمانی که یک obj نابود میشه یک پیام چاپ میشه . به مثال زیر دقت کنید .
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }

  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}

// Create a new object
$obj = new MyClass;

// Get the value of $prop1
echo $obj->getProperty();

// Output a message at the end of the file
echo "End of file.<br />";
</code></pre>

<p>
با تعریف یک destruct خروجی کد بالا به صورت زیر میشه .
</p>

<pre><code class="language-php">
The class "MyClass" was initiated!
I'm a class property!
End of file.
The class "MyClass" was destroyed.
</code></pre>

<p>
شما با یک تابع به اسم unset می تونید یک Obj رو زودتر از زمانی که قراره نابود بشه نابود کنید. در زیر یک مثال با این تابع میزنم تا بهتر بتونید این مسئله و درک کنید .
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }

  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}

// Create a new object
$obj = new MyClass;

// Get the value of $prop1
echo $obj->getProperty();

// Destroy the object
unset($obj);

// Output a message at the end of the file
echo "End of file.<br />";
</code></pre>

<p>
خب حالا به نتیجه کد بالا دقت کنید .
</p>

<pre><code class="language-php">
The class "MyClass" was initiated!
I'm a class property!
The class "MyClass" was destroyed.
End of file.
</code></pre>

<h3>تبدیل به یک رشته (Converting to a String)</h3>
<p>
اگر شما یک obj رو بخواین بطور مستقیم به عنوان رشته چاپ کنید قطعا با ارور مواجه میشید . اما با استفاده از متد جادوئی <span class="notespan">()toString__</span> میتونید یک obj رو بصورت یک رشته چاپ کنید .
</p>

<p>
در زیر مثالی می زنم که قصد دارم obj رو بصورت یک رشته با echo چاپ کنم اما با ارور مواجه میشم .
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }

  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}

// Create a new object
$obj = new MyClass;

// Output the object as a string
echo $obj;

// Destroy the object
unset($obj);

// Output a message at the end of the file
echo "End of file.<br />";
</code></pre>

<p>
خروجی کد بالا بصورت زیره که ارور رو هم میتونید ببینید :
</p>

<pre><code class="language-php">
The class "MyClass" was initiated!

Catchable fatal error: Object of class MyClass could not be converted to string in /Applications/XAMPP/xamppfiles/htdocs/testing/test.php on line 40
</code></pre>

<p>
برای رفع ارور ما میتونیم از متد <span class="notespan">()toString__</span> استفاده کنیم
</p>

<pre><code class="language-php  line-numbers">class MyClass
{
  public $prop1 = "I'm a class property!";

  public function __construct()
  {
      echo 'The class "', __CLASS__, '" was initiated!<br />';
  }

  public function __destruct()
  {
      echo 'The class "', __CLASS__, '" was destroyed.<br />';
  }

  public function __toString()
  {
      echo "Using the toString method: ";
      return $this->getProperty();
  }

  public function setProperty($newval)
  {
      $this->prop1 = $newval;
  }

  public function getProperty()
  {
      return $this->prop1 . "<br />";
  }
}

// Create a new object
$obj = new MyClass;

// Output the object as a string
echo $obj;

// Destroy the object
unset($obj);

// Output a message at the end of the file
echo "End of file.<br />";

</code></pre>

<p>
در مثال بالا ما با استفاده از <span class="notespan">()toString__</span> به علاوه اینکه یک رشته رو چاپ میکنیم ، یک متد از کلاس به اسم getProperty رو هم بر میگردونیم که شما برای واضح تر شدن و درک این موضوع به نتیجه کد بالا دقت کنید .
</p>

<pre><code class="language-php">
The class "MyClass" was initiated!
Using the toString method: I'm a class property!
The class "MyClass" was destroyed.
End of file.
</code></pre>

<p>
خب در این پست من چند تا از متدهای جادوئی رو معرفی کردم که امیدوارم باهشون آشنا شده باشید اگر میخواید با متدهای بیشتری از متدهای جادوئی آشنا بشین به این صفحه مراجعه کنید . "<a href="http://php.net/manual/en/language.oop5.magic.php" target="_blank" >Magic Methods</a>"
</p>