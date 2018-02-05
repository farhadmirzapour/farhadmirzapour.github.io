---
layout: documentation
title:   اصول requests
cattitle: اصول آموزش Laravel
date:   2018-02-03 10:40:42 +0330
jdate: شنبه 14 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.5/requests <br> https://www.lydaweb.com/article/courses/laravel-5-5-tutorial/239/درخواست-http-لاراول
---
<p>
To obtain an instance of the current HTTP request via dependency injection, you should type-hint the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> class on your controller method. The incoming request instance will automatically be injected by the <a href="/docs/5.5/container">service container</a>:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Store a new user.
     *
     * @param  Request  $request
     * @return Response
     */
    public function store(Request $request)
    {
        $name = $request->input('name');

        //
    }
}
</code></pre>


<br>
<h3>Dependency Injection &amp; Route Parameters</h3>

<p>
If your controller method is also expecting input from a route parameter you should list your route parameters after your other dependencies. For example, if your route is defined like so:
</p>


<pre><code class="language-php  line-numbers">Route::put('user/{id}', 'UserController@update');
</code></pre>


<p>
You may still type-hint the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> and access your route parameter <code class=" language-php">id</code> by defining your controller method as follows:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class UserController extends Controller
{
    /**
     * Update the specified user.
     *
     * @param  Request  $request
     * @param  string  $id
     * @return Response
     */
    public function update(Request $request, $id)
    {
        //
    }
}
</code></pre>


<br>
<h3>Accessing The Request Via Route Closures</h3>

<p>
You may also type-hint the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> class on a route Closure. The service container will automatically inject the incoming request into the Closure when it is executed:
</p>


<pre><code class="language-php  line-numbers">use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    //
});
</code></pre>


<p>
<a name="request-path-and-method"></a>
</p>


<br>
<h3>Request Path &amp; Method</h3>

<p>
The <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> instance provides a variety of methods for examining the HTTP request for your application and extends the <code class=" language-php">Symfony\<span class="token package">Component<span class="token punctuation">\</span>HttpFoundation<span class="token punctuation">\</span>Request</span></code> class. We will discuss a few of the most important methods below.
</p>


<br>
<h3>Retrieving The Request Path</h3>

<p>
The <code class=" language-php">path</code> method returns the request's path information. So, if the incoming request is targeted at <code class=" language-php">http<span class="token punctuation">:</span><span class="token operator">/</span><span class="token operator">/</span>domain<span class="token punctuation">.</span>com<span class="token operator">/</span>foo<span class="token operator">/</span>bar</code>, the <code class=" language-php">path</code> method will return <code class=" language-php">foo<span class="token operator">/</span>bar</code>:
</p>


<pre><code class="language-php  line-numbers">$uri = $request->path();
</code></pre>


<p>
The <code class=" language-php">is</code> method allows you to verify that the incoming request path matches a given pattern. You may use the <code class=" language-php"><span class="token operator">*</span></code> character as a wildcard when utilizing this method:
</p>


<pre><code class="language-php  line-numbers">if ($request->is('admin/*')) {
    //
}
</code></pre>


<br>
<h3>Retrieving The Request URL</h3>

<p>
To retrieve the full URL for the incoming request you may use the <code class=" language-php">url</code> or <code class=" language-php">fullUrl</code> methods. The <code class=" language-php">url</code> method will return the URL without the query string, while the <code class=" language-php">fullUrl</code> method includes the query string:
</p>


<pre><code class="language-php  line-numbers">// Without Query String...
$url = $request->url();

// With Query String...
$url = $request->fullUrl();
</code></pre>


<br>
<h3>Retrieving The Request Method</h3>

<p>
The <code class=" language-php">method</code> method will return the HTTP verb for the request. You may use the <code class=" language-php">isMethod</code> method to verify that the HTTP verb matches a given string:
</p>


<pre><code class="language-php  line-numbers">$method = $request->method();

if ($request->isMethod('post')) {
    //
}
</code></pre>


<p>
<a name="psr7-requests"></a>
</p>


<br>
<h3>PSR-7 Requests</h3>

<p>
The <a href="http://www.php-fig.org/psr/psr-7/">PSR-7 standard</a> specifies interfaces for HTTP messages, including requests and responses. If you would like to obtain an instance of a PSR-7 request instead of a Laravel request, you will first need to install a few libraries. Laravel uses the <em>Symfony HTTP Message Bridge</em> component to convert typical Laravel requests and responses into PSR-7 compatible implementations:
</p>


<pre><code class="language-php  line-numbers">composer require symfony/psr-http-message-bridge
composer require zendframework/zend-diactoros
</code></pre>


<p>
Once you have installed these libraries, you may obtain a PSR-7 request by type-hinting the request interface on your route Closure or controller method:
</p>


<pre><code class="language-php  line-numbers">use Psr\Http\Message\ServerRequestInterface;

Route::get('/', function (ServerRequestInterface $request) {
    //
});
</code></pre>

<blockquote class="has-icon tip">

<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> If you return a PSR-7 response instance from a route or controller, it will automatically be converted back to a Laravel response instance and be displayed by the framework.
</p>

</blockquote>

<p>
<a name="input-trimming-and-normalization"></a>
</p>


<br>
<h3><a href="#input-trimming-and-normalization">Input Trimming &amp; Normalization</a></h3>

<p>
By default, Laravel includes the <code class=" language-php">TrimStrings</code> and <code class=" language-php">ConvertEmptyStringsToNull</code> middleware in your application's global middleware stack. These middleware are listed in the stack by the <code class=" language-php">App\<span class="token package">Http<span class="token punctuation">\</span>Kernel</span></code> class. These middleware will automatically trim all incoming string fields on the request, as well as convert any empty string fields to <code class=" language-php"><span class="token keyword">null</span></code>. This allows you to not have to worry about these normalization concerns in your routes and controllers.
</p>


<p>
If you would like to disable this behavior, you may remove the two middleware from your application's middleware stack by removing them from the <code class=" language-php"><span class="token variable">$middleware</span></code> property of your <code class=" language-php">App\<span class="token package">Http<span class="token punctuation">\</span>Kernel</span></code> class.
</p>


<p>
<a name="retrieving-input"></a>
</p>


<br>
<h3><a href="#retrieving-input">Retrieving Input</a></h3>

<br>
<h3>Retrieving All Input Data</h3>

<p>
You may also retrieve all of the input data as an <code class=" language-php"><span class="token keyword">array</span></code> using the <code class=" language-php">all</code> method:
</p>


<pre><code class="language-php  line-numbers">$input = $request->all();
</code></pre>


<br>
<h3>Retrieving An Input Value</h3>

<p>
Using a few simple methods, you may access all of the user input from your <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> instance without worrying about which HTTP verb was used for the request. Regardless of the HTTP verb, the <code class=" language-php">input</code> method may be used to retrieve user input:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('name');
</code></pre>


<p>
You may pass a default value as the second argument to the <code class=" language-php">input</code> method. This value will be returned if the requested input value is not present on the request:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('name', 'Sally');
</code></pre>


<p>
When working with forms that contain array inputs, use "dot" notation to access the arrays:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('products.0.name');

$names = $request->input('products.*.name');
</code></pre>


<br>
<h3>Retrieving Input From The Query String</h3>

<p>
While the <code class=" language-php">input</code> method retrieves values from entire request payload (including the query string), the <code class=" language-php">query</code> method will only retrieve values from the query string:
</p>


<pre><code class="language-php  line-numbers">$name = $request->query('name');
</code></pre>


<p>
If the requested query string value data is not present, the second argument to this method will be returned:
</p>


<pre><code class="language-php  line-numbers">$name = $request->query('name', 'Helen');
</code></pre>


<p>
You may call the <code class=" language-php">query</code> method without any arguments in order to retrieve all of the query string values as an associative array:
</p>


<pre><code class="language-php  line-numbers">$query = $request->query();
</code></pre>


<br>
<h3>Retrieving Input Via Dynamic Properties</h3>

<p>
You may also access user input using dynamic properties on the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> instance. For example, if one of your application's forms contains a <code class=" language-php">name</code> field, you may access the value of the field like so:
</p>


<pre><code class="language-php  line-numbers">$name = $request->name;
</code></pre>


<p>
When using dynamic properties, Laravel will first look for the parameter's value in the request payload. If it is not present, Laravel will search for the field in the route parameters.
</p>


<br>
<h3>Retrieving JSON Input Values</h3>

<p>
When sending JSON requests to your application, you may access the JSON data via the <code class=" language-php">input</code> method as long as the <code class=" language-php">Content<span class="token operator">-</span>Type</code> header of the request is properly set to <code class=" language-php">application<span class="token operator">/</span>json</code>. You may even use "dot" syntax to dig into JSON arrays:
</p>


<pre><code class="language-php  line-numbers">$name = $request->input('user.name');
</code></pre>


<br>
<h3>Retrieving A Portion Of The Input Data</h3>

<p>
If you need to retrieve a subset of the input data, you may use the <code class=" language-php">only</code> and <code class=" language-php">except</code> methods. Both of these methods accept a single <code class=" language-php"><span class="token keyword">array</span></code> or a dynamic list of arguments:
</p>


<pre><code class="language-php  line-numbers">$input = $request->only(['username', 'password']);

$input = $request->only('username', 'password');

$input = $request->except(['credit_card']);

$input = $request->except('credit_card');
</code></pre>

<blockquote class="has-icon tip">

<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> The <code class=" language-php">only</code> method returns all of the key / value pairs that you request; however, it will not return key / values pairs that are not present on the request.
</p>

</blockquote>

<br>
<h3>Determining If An Input Value Is Present</h3>

<p>
You should use the <code class=" language-php">has</code> method to determine if a value is present on the request. The <code class=" language-php">has</code> method returns <code class=" language-php"><span class="token boolean">true</span></code> if the value is present on the request:
</p>


<pre><code class="language-php  line-numbers">if ($request->has('name')) {
    //
}
</code></pre>


<p>
When given an array, the <code class=" language-php">has</code> method will determine if all of the specified values are present:
</p>


<pre><code class="language-php  line-numbers">if ($request->has(['name', 'email'])) {
    //
}
</code></pre>


<p>
If you would like to determine if a value is present on the request and is not empty, you may use the <code class=" language-php">filled</code> method:
</p>


<pre><code class="language-php  line-numbers">if ($request->filled('name')) {
    //
}
</code></pre>


<p>
<a name="old-input"></a>
</p>


<br>
<h3>Old Input</h3>

<p>
Laravel allows you to keep input from one request during the next request. This feature is particularly useful for re-populating forms after detecting validation errors. However, if you are using Laravel's included <a href="/docs/5.5/validation">validation features</a>, it is unlikely you will need to manually use these methods, as some of Laravel's built-in validation facilities will call them automatically.
</p>


<br>
<h3>Flashing Input To The Session</h3>

<p>
The <code class=" language-php">flash</code> method on the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> class will flash the current input to the <a href="/docs/5.5/session">session</a> so that it is available during the user's next request to the application:
</p>


<pre><code class="language-php  line-numbers">$request->flash();
</code></pre>


<p>
You may also use the <code class=" language-php">flashOnly</code> and <code class=" language-php">flashExcept</code> methods to flash a subset of the request data to the session. These methods are useful for keeping sensitive information such as passwords out of the session:
</p>


<pre><code class="language-php  line-numbers">$request->flashOnly(['username', 'email']);

$request->flashExcept('password');
</code></pre>


<br>
<h3>Flashing Input Then Redirecting</h3>

<p>
Since you often will want to flash input to the session and then redirect to the previous page, you may easily chain input flashing onto a redirect using the <code class=" language-php">withInput</code> method:
</p>


<pre><code class="language-php  line-numbers">return redirect('form')->withInput();

return redirect('form')->withInput(
    $request->except('password')
);
</code></pre>


<br>
<h3>Retrieving Old Input</h3>

<p>
To retrieve flashed input from the previous request, use the <code class=" language-php">old</code> method on the <code class=" language-php">Request</code> instance. The <code class=" language-php">old</code> method will pull the previously flashed input data from the <a href="/docs/5.5/session">session</a>:
</p>


<pre><code class="language-php  line-numbers">$username = $request->old('username');
</code></pre>


<p>
Laravel also provides a global <code class=" language-php">old</code> helper. If you are displaying old input within a <a href="/docs/5.5/blade">Blade template</a>, it is more convenient to use the <code class=" language-php">old</code> helper. If no old input exists for the given field, <code class=" language-php"><span class="token keyword">null</span></code> will be returned:
</p>


<pre><code class="language-php  line-numbers">{% highlight scala %}<input type="text" name="username" value="{!! old('username') !!}">{% endhighlight %}
</code></pre>


<p>
<a name="cookies"></a>
</p>


<br>
<h3>Cookies</h3>

<br>
<h3>Retrieving Cookies From Requests</h3>

<p>
All cookies created by the Laravel framework are encrypted and signed with an authentication code, meaning they will be considered invalid if they have been changed by the client. To retrieve a cookie value from the request, use the <code class=" language-php">cookie</code> method on a <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> instance:
</p>


<pre><code class="language-php  line-numbers">$value = $request->cookie('name');
</code></pre>


<p>
Alternatively, you may use the <code class=" language-php">Cookie</code> facade to access cookie values:
</p>


<pre><code class="language-php  line-numbers">$value = Cookie::get('name');
</code></pre>


<br>
<h3>Attaching Cookies To Responses</h3>

<p>
You may attach a cookie to an outgoing <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Response</span></code> instance using the <code class=" language-php">cookie</code> method. You should pass the name, value, and number of minutes the cookie should be considered valid to this method:
</p>


<pre><code class="language-php  line-numbers">return response('Hello World')->cookie(
    'name', 'value', $minutes
);
</code></pre>


<p>
The <code class=" language-php">cookie</code> method also accepts a few more arguments which are used less frequently. Generally, these arguments have the same purpose and meaning as the arguments that would be given to PHP's native <a href="https://secure.php.net/manual/en/function.setcookie.php">setcookie</a> method:
</p>


<pre><code class="language-php  line-numbers">return response('Hello World')->cookie(
    'name', 'value', $minutes, $path, $domain, $secure, $httpOnly
);
</code></pre>


<p>
Alternatively, you can use the <code class=" language-php">Cookie</code> facade to "queue" cookies for attachment to the outgoing response from your application. The <code class=" language-php">queue</code> method accepts a <code class=" language-php">Cookie</code> instance or the arguments needed to create a <code class=" language-php">Cookie</code> instance. These cookies will be attached to the outgoing response before it is sent to the browser:
</p>


<pre><code class="language-php  line-numbers">Cookie::queue(Cookie::make('name', 'value', $minutes));

Cookie::queue('name', 'value', $minutes);
</code></pre>


<br>
<h3>Generating Cookie Instances</h3>

<p>
If you would like to generate a <code class=" language-php">Symfony\<span class="token package">Component<span class="token punctuation">\</span>HttpFoundation<span class="token punctuation">\</span>Cookie</span></code> instance that can be given to a response instance at a later time, you may use the global <code class=" language-php">cookie</code> helper. This cookie will not be sent back to the client unless it is attached to a response instance:
</p>


<pre><code class="language-php  line-numbers">$cookie = cookie('name', 'value', $minutes);

return response('Hello World')->cookie($cookie);
</code></pre>


<p>
<a name="files"></a>
</p>


<br>
<h3><a href="#files">Files</a></h3>

<p>
<a name="retrieving-uploaded-files"></a>
</p>


<br>
<h3>Retrieving Uploaded Files</h3>

<p>
You may access uploaded files from a <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>Request</span></code> instance using the <code class=" language-php">file</code> method or using dynamic properties. The <code class=" language-php">file</code> method returns an instance of the <code class=" language-php">Illuminate\<span class="token package">Http<span class="token punctuation">\</span>UploadedFile</span></code> class, which extends the PHP <code class=" language-php">SplFileInfo</code> class and provides a variety of methods for interacting with the file:
</p>


<pre><code class="language-php  line-numbers">$file = $request->file('photo');

$file = $request->photo;
</code></pre>


<p>
You may determine if a file is present on the request using the <code class=" language-php">hasFile</code> method:
</p>


<pre><code class="language-php  line-numbers">if ($request->hasFile('photo')) {
    //
}
</code></pre>


<br>
<h3>Validating Successful Uploads</h3>

<p>
In addition to checking if the file is present, you may verify that there were no problems uploading the file via the <code class=" language-php">isValid</code> method:
</p>


<pre><code class="language-php  line-numbers">if ($request->file('photo')->isValid()) {
    //
}
</code></pre>


<br>
<h3>File Paths &amp; Extensions</h3>

<p>
The <code class=" language-php">UploadedFile</code> class also contains methods for accessing the file's fully-qualified path and its extension. The <code class=" language-php">extension</code> method will attempt to guess the file's extension based on its contents. This extension may be different from the extension that was supplied by the client:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->path();

$extension = $request->photo->extension();
</code></pre>


<br>
<h3>Other File Methods</h3>

<p>
There are a variety of other methods available on <code class=" language-php">UploadedFile</code> instances. Check out the <a href="http://api.symfony.com/3.0/Symfony/Component/HttpFoundation/File/UploadedFile.html">API documentation for the class</a> for more information regarding these methods.
</p>


<p>
<a name="storing-uploaded-files"></a>
</p>


<br>
<h3>Storing Uploaded Files</h3>

<p>
To store an uploaded file, you will typically use one of your configured <a href="/docs/5.5/filesystem">filesystems</a>. The <code class=" language-php">UploadedFile</code> class has a <code class=" language-php">store</code> method which will move an uploaded file to one of your disks, which may be a location on your local filesystem or even a cloud storage location like Amazon S3.
</p>


<p>
The <code class=" language-php">store</code> method accepts the path where the file should be stored relative to the filesystem's configured root directory. This path should not contain a file name, since a unique ID will automatically be generated to serve as the file name.
</p>


<p>
The <code class=" language-php">store</code> method also accepts an optional second argument for the name of the disk that should be used to store the file. The method will return the path of the file relative to the disk's root:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->store('images');

$path = $request->photo->store('images', 's3');
</code></pre>


<p>
If you do not want a file name to be automatically generated, you may use the <code class=" language-php">storeAs</code> method, which accepts the path, file name, and disk name as its arguments:
</p>


<pre><code class="language-php  line-numbers">$path = $request->photo->storeAs('images', 'filename.jpg');

$path = $request->photo->storeAs('images', 'filename.jpg', 's3');
</code></pre>


<p>
<a name="configuring-trusted-proxies"></a>
</p>


<br>
<h3><a href="#configuring-trusted-proxies">Configuring Trusted Proxies</a></h3>

<p>
When running your applications behind a load balancer that terminates TLS / SSL certificates, you may notice your application sometimes does not generate HTTPS links. Typically this is because your application is being forwarded traffic from your load balancer on port 80 and does not know it should generate secure links.
</p>


<p>
To solve this, you may use the <code class=" language-php">App\<span class="token package">Http<span class="token punctuation">\</span>Middleware<span class="token punctuation">\</span>TrustProxies</span></code> middleware that is included in your Laravel application, which allows you to quickly customize the load balancers or proxies that should be trusted by your application. Your trusted proxies should be listed as an array on the <code class=" language-php"><span class="token variable">$proxies</span></code> property of this middleware. In addition to configuring the trusted proxies, you may configure the headers that are being sent by your proxy with information about the original request:
</p>


<pre><code class="language-php  line-numbers"><?php

namespace App\Http\Middleware;

use Illuminate\Http\Request;
use Fideloper\Proxy\TrustProxies as Middleware;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     *
     * @var array
     */
    protected $proxies = [
        '192.168.1.1',
        '192.168.1.2',
    ];

    /**
     * The current proxy header mappings.
     *
     * @var array
     */
    protected $headers = [
        Request::HEADER_FORWARDED => 'FORWARDED',
        Request::HEADER_X_FORWARDED_FOR => 'X_FORWARDED_FOR',
        Request::HEADER_X_FORWARDED_HOST => 'X_FORWARDED_HOST',
        Request::HEADER_X_FORWARDED_PORT => 'X_FORWARDED_PORT',
        Request::HEADER_X_FORWARDED_PROTO => 'X_FORWARDED_PROTO',
    ];
}
</code></pre>


<br>
<h3>Trusting All Proxies</h3>

<p>
If you are using Amazon AWS or another "cloud" load balancer provider, you may not know the IP addresses of your actual balancers. In this case, you may use <code class=" language-php"><span class="token operator">*</span><span class="token operator">*</span></code> to trust all proxies:
</p>


<pre><code class="language-php  line-numbers">/**
 * The trusted proxies for this application.
 *
 * @var array
 */
protected $proxies = '**';
</code></pre>