---
layout: documentation
title:   مفهوم Refactoring (بازسازی)
cattitle: اصول Test Driven Development
date:   2017-11-19 23:10:42 +0330
jdate: یکشنبه 28 آبان 1396
caturl: 2017/11/15/test-driven-development.html
# permalink: /:categories/:title.html
thumbnail: stride-nyc-test-driven-development-chart-700x400.jpg
refrence: https://www.telerik.com/blogs/30-days-of-tdd-day-nine-refactoring-basics
---
<p>
بازسازی یک گام مهم در TDD می باشد. شما اغلب عبارت "Red، Green، Refactor" را می شنوید. "قرمز" به این ایده اشاره دارد که ما یک تست را نوشتیم و ابتدا آن را شکست دادیم. هنگامی که کد برای گذراندن آزمون نوشته شده است، ما به قسمت "سبز" گردش کارمان رسیده ایم؛ کد ما کار می کند و نتایج با توجه به داده های داده شده و نتایج مورد انتظار باهم مطابقت مطابقت دارد. Refactoring گام نهایی است. به طور منظم، کد ما را بهبود می بخشد و آن را قابل خواندن، بهینه سازی، کار را ساده تر و انعطاف پذیر تر می کند.
</p>


<p>در بخش نقص ها باتوجه به نقصی که گذارش شده بود جهت رفع آن تغییری در کد برنامه داده بودیم تا این مورد رفع گردد. در این قسمت به مرحله بازنگری و بازسازی کد می رسیم . تغییراتی که داده می شود ممکن است بهترین راه حل نباشند اما باعث اجرای تمام آزمون ها می گردد :</p>

<pre><code class="language-php   line-numbers"><?php
namespace Classes;

class StringUtils
{
    public function __construct()
    {
    }

    public function findNumberOfOccurences($sentenceToScan, $characterToScanFor)
    {
        $numberOfOccurenes = 0;
        for ($charIdx= 0; $charIdx < strlen($sentenceToScan); $charIdx++) {
            if ($sentenceToScan[$charIdx] === $characterToScanFor) {
                $numberOfOccurenes++;
            }
        }
        return $numberOfOccurenes;
    }
}
</code></pre>

<p>
تغییری که داده شده بدین صورت است که دستور return 0 حذف، همچنین از عملگر === به جای == استفاده است.
</p>

<p>
عملگر == مقدار دو متغیر را برای مساوی (معادل) بودن بررسی می‌کند و در صورت لزوم casting نیز صورت می‌دهد. === بررسی می‌کند که آیا مقدار دو متغیر دقیقا از یک type باشد و مقدار آن‌ها نیز دقیقا یکسان باشد.
</p>

<p>
هم اینک با اجرای آزمون ها مشاهده می کنیم تمامی آزمون ها pass خواهند شد.
</p>