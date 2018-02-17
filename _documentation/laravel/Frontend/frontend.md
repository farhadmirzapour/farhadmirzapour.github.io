---
layout: documentation
title:   JavaScript & CSS Scaffolding
cattitle: اصول آموزش Laravel
date:   2018-02-16 20:45:42 +0330
jdate: جمعه 27 بهمن 1396
caturl: 2017/12/18/laravel.html
#  permalink: /:categories/:title.html
directory : laravel
menu : false
thumbnail: laravel.png
refrence: https://laravel.com/docs/5.6/localization <br> www.tahlildadeh.com/ArticleDetails/آموزش-Localization-در-لاراول
---

<br>
<h3>CSS</h3>
<p>
<a href="/docs/5.6/mix">Laravel Mix</a> provides a clean, expressive API over compiling SASS or Less, which are extensions of plain CSS that add variables, mixins, and other powerful features that make working with CSS much more enjoyable. In this document, we will briefly discuss CSS compilation in general; however, you should consult the full <a href="/docs/5.6/mix">Laravel Mix documentation</a> for more information on compiling SASS or Less.
</p>

<br>
<h3>JavaScript</h3>
<p>
Laravel does not require you to use a specific JavaScript framework or library to build your applications. In fact, you don't have to use JavaScript at all. However, Laravel does include some basic scaffolding to make it easier to get started writing modern JavaScript using the <a href="https://vuejs.org">Vue</a> library. Vue provides an expressive API for building robust JavaScript applications using components. As with CSS, we may use Laravel Mix to easily compile JavaScript components into a single, browser-ready JavaScript file.
</p>

<br>
<h3>Removing The Frontend Scaffolding</h3>
<p>
If you would like to remove the frontend scaffolding from your application, you may use the <code class=" language-php">preset</code> Artisan command. This command, when combined with the <code class=" language-php">none</code> option, will remove the Bootstrap and Vue scaffolding from your application, leaving only a blank SASS file and a few common JavaScript utility libraries:
</p>

<pre><code class="language-php  line-numbers">php artisan preset none
</code></pre>

<p>
<a name="writing-css"></a>
</p>

<br>
<h3><a href="#writing-css">Writing CSS</a></h3>
<p>
Laravel's <code class=" language-php">package<span class="token punctuation">.</span>json</code> file includes the <code class=" language-php">bootstrap</code> package to help you get started prototyping your application's frontend using Bootstrap. However, feel free to add or remove packages from the <code class=" language-php">package<span class="token punctuation">.</span>json</code> file as needed for your own application. You are not required to use the Bootstrap framework to build your Laravel application - it is provided as a good starting point for those who choose to use it.
</p>

<p>
Before compiling your CSS, install your project's frontend dependencies using the <a href="https://www.npmjs.org">Node package manager (NPM)</a>:
</p>

<pre><code class="language-php  line-numbers">npm install
</code></pre>

<p>
Once the dependencies have been installed using <code class=" language-php">npm install</code>, you can compile your SASS files to plain CSS using <a href="/docs/5.6/mix#working-with-stylesheets">Laravel Mix</a>. The <code class=" language-php">npm run dev</code> command will process the instructions in your <code class=" language-php">webpack<span class="token punctuation">.</span>mix<span class="token punctuation">.</span>js</code> file. Typically, your compiled CSS will be placed in the <code class=" language-php"><span class="token keyword">public</span><span class="token operator">/</span>css</code> directory:
</p>

<pre><code class="language-php  line-numbers">npm run dev
</code></pre>

<p>
The default <code class=" language-php">webpack<span class="token punctuation">.</span>mix<span class="token punctuation">.</span>js</code> included with Laravel will compile the <code class=" language-php">resources<span class="token operator">/</span>assets<span class="token operator">/</span>sass<span class="token operator">/</span>app<span class="token punctuation">.</span>scss</code> SASS file. This <code class=" language-php">app<span class="token punctuation">.</span>scss</code> file imports a file of SASS variables and loads Bootstrap, which provides a good starting point for most applications. Feel free to customize the <code class=" language-php">app<span class="token punctuation">.</span>scss</code> file however you wish or even use an entirely different pre-processor by <a href="/docs/5.6/mix">configuring Laravel Mix</a>.
</p>

<p>
<a name="writing-javascript"></a>
</p>

<br>
<h3><a href="#writing-javascript">Writing JavaScript</a></h3>
<p>
All of the JavaScript dependencies required by your application can be found in the <code class=" language-php">package<span class="token punctuation">.</span>json</code> file in the project's root directory. This file is similar to a <code class=" language-php">composer<span class="token punctuation">.</span>json</code> file except it specifies JavaScript dependencies instead of PHP dependencies. You can install these dependencies using the <a href="https://www.npmjs.org">Node package manager (NPM)</a>:
</p>

<pre><code class="language-php  line-numbers">npm install
</code></pre>

<blockquote class="has-icon tip">
<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> By default, the Laravel <code class=" language-php">package<span class="token punctuation">.</span>json</code> file includes a few packages such as <code class=" language-php">vue</code> and <code class=" language-php">axios</code> to help you get started building your JavaScript application. Feel free to add or remove from the <code class=" language-php">package<span class="token punctuation">.</span>json</code> file as needed for your own application.
</p>
</blockquote>
<p>
Once the packages are installed, you can use the <code class=" language-php">npm run dev</code> command to <a href="/docs/5.6/mix">compile your assets</a>. Webpack is a module bundler for modern JavaScript applications. When you run the <code class=" language-php">npm run dev</code> command, Webpack will execute the instructions in your <code class=" language-php">webpack<span class="token punctuation">.</span>mix<span class="token punctuation">.</span>js</code> file:
</p>

<pre><code class="language-php  line-numbers">npm run dev
</code></pre>

<p>
By default, the Laravel <code class=" language-php">webpack<span class="token punctuation">.</span>mix<span class="token punctuation">.</span>js</code> file compiles your SASS and the <code class=" language-php">resources<span class="token operator">/</span>assets<span class="token operator">/</span>js<span class="token operator">/</span>app<span class="token punctuation">.</span>js</code> file. Within the <code class=" language-php">app<span class="token punctuation">.</span>js</code> file you may register your Vue components or, if you prefer a different framework, configure your own JavaScript application. Your compiled JavaScript will typically be placed in the <code class=" language-php"><span class="token keyword">public</span><span class="token operator">/</span>js</code> directory.
</p>
<blockquote class="has-icon tip">
<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> The <code class=" language-php">app<span class="token punctuation">.</span>js</code> file will load the <code class=" language-php">resources<span class="token operator">/</span>assets<span class="token operator">/</span>js<span class="token operator">/</span>bootstrap<span class="token punctuation">.</span>js</code> file which bootstraps and configures Vue, Axios, jQuery, and all other JavaScript dependencies. If you have additional JavaScript dependencies to configure, you may do so in this file.
</p>
</blockquote>
<p>
<a name="writing-vue-components"></a>
</p>

<br>
<h3>Writing Vue Components</h3>
<p>
By default, fresh Laravel applications contain an <code class=" language-php">ExampleComponent<span class="token punctuation">.</span>vue</code> Vue component located in the <code class=" language-php">resources<span class="token operator">/</span>assets<span class="token operator">/</span>js<span class="token operator">/</span>components</code> directory. The <code class=" language-php">ExampleComponent<span class="token punctuation">.</span>vue</code> file is an example of a <a href="https://vuejs.org/guide/single-file-components">single file Vue component</a> which defines its JavaScript and HTML template in the same file. Single file components provide a very convenient approach to building JavaScript driven applications. The example component is registered in your <code class=" language-php">app<span class="token punctuation">.</span>js</code> file:
</p>

<pre><code class="language-php  line-numbers">Vue.component(
    'example-component',
    require('./components/ExampleComponent.vue')
);
</code></pre>

<p>
To use the component in your application, you may drop it into one of your HTML templates. For example, after running the <code class=" language-php">make<span class="token punctuation">:</span>auth</code> Artisan command to scaffold your application's authentication and registration screens, you could drop the component into the <code class=" language-php">home<span class="token punctuation">.</span>blade<span class="token punctuation">.</span>php</code> Blade template:
</p>

<pre><code class="language-php  line-numbers">@extends('layouts.app')

@section('content')
    <example-component></example-component>
@endsection
</code></pre>

<blockquote class="has-icon tip">
<p>
<div class="flag"><span class="svg"><svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:a="http://ns.adobe.com/AdobeSVGViewerExtensions/3.0/" version="1.1" x="0px" y="0px" width="56.6px" height="87.5px" viewBox="0 0 56.6 87.5" enable-background="new 0 0 56.6 87.5" xml:space="preserve"><path fill="#FFFFFF" d="M28.7 64.5c-1.4 0-2.5-1.1-2.5-2.5v-5.7 -5V41c0-1.4 1.1-2.5 2.5-2.5s2.5 1.1 2.5 2.5v10.1 5 5.8C31.2 63.4 30.1 64.5 28.7 64.5zM26.4 0.1C11.9 1 0.3 13.1 0 27.7c-0.1 7.9 3 15.2 8.2 20.4 0.5 0.5 0.8 1 1 1.7l3.1 13.1c0.3 1.1 1.3 1.9 2.4 1.9 0.3 0 0.7-0.1 1.1-0.2 1.1-0.5 1.6-1.8 1.4-3l-2-8.4 -0.4-1.8c-0.7-2.9-2-5.7-4-8 -1-1.2-2-2.5-2.7-3.9C5.8 35.3 4.7 30.3 5.4 25 6.7 14.5 15.2 6.3 25.6 5.1c13.9-1.5 25.8 9.4 25.8 23 0 4.1-1.1 7.9-2.9 11.2 -0.8 1.4-1.7 2.7-2.7 3.9 -2 2.3-3.3 5-4 8L41.4 53l-2 8.4c-0.3 1.2 0.3 2.5 1.4 3 0.3 0.2 0.7 0.2 1.1 0.2 1.1 0 2.2-0.8 2.4-1.9l3.1-13.1c0.2-0.6 0.5-1.2 1-1.7 5-5.1 8.2-12.1 8.2-19.8C56.4 12 42.8-1 26.4 0.1zM43.7 69.6c0 0.5-0.1 0.9-0.3 1.3 -0.4 0.8-0.7 1.6-0.9 2.5 -0.7 3-2 8.6-2 8.6 -1.3 3.2-4.4 5.5-7.9 5.5h-4.1h38h-0.5 -3.6c-3.5 0-6.7-2.4-7.9-5.7l-0.1-0.4 -1.8-7.8c-0.4-1.1-0.8-2.1-1.2-3.1 -0.1-0.3-0.2-0.5-0.2-0.9 0.1-1.3 1.3-2.1 2.6-2.1h31C42.4 67.5 43.6 68.2 43.7 69.6zM37.7 72.5h36.9c-4.2 0-7.2 3.9-6.3 7.9 0.6 1.3 1.8 2.1 3.2 2.1h3.1 0.5 0.5 3.6c1.4 0 2.7-0.8 3.2-2.1L37.7 72.5z"></path></svg></span></div> Remember, you should run the <code class=" language-php">npm run dev</code> command each time you change a Vue component. Or, you may run the <code class=" language-php">npm run watch</code> command to monitor and automatically recompile your components each time they are modified.
</p>
</blockquote>
<p>
Of course, if you are interested in learning more about writing Vue components, you should read the <a href="https://vuejs.org/guide/">Vue documentation</a>, which provides a thorough, easy-to-read overview of the entire Vue framework.
</p>

<p>
<a name="using-react"></a>
</p>

<br>
<h3>Using React</h3>
<p>
If you prefer to use React to build your JavaScript application, Laravel makes it a cinch to swap the Vue scaffolding with React scaffolding. On any fresh Laravel application, you may use the <code class=" language-php">preset</code> command with the <code class=" language-php">react</code> option:
</p>

<pre><code class="language-php  line-numbers">php artisan preset react
</code></pre>

<p>
This single command will remove the Vue scaffolding and replace it with React scaffolding, including an example component.
</p>
