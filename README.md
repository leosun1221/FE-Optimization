#### <a name="s1"></a>1. Avoid empty src or href
*You may expect a browser to do nothing when it encounters an empty image src. However, it is not the case in most browsers. IE makes a request to the directory in which the page is located; Safari, Chrome, Firefox 3 and earlier make a request to the actual page itself. This behavior could possibly corrupt user data, waste server computing cycles generating a page that will never be viewed, and in the worst case, cripple your servers by sending a large amount of unexpected traffic.*

This is pretty simple to fix. Simply make sure you don’t have any sources that are left empty. This is an older rule and we rarely see many people fail this rule.

```html
<img src="">

```

#### <a name="s2"></a>2. Put CSS at Top
*Moving style sheets to the document HEAD element helps pages appear to load quicker since this allows pages to render progressively.*

The HTML specification states that stylesheets are to be included in the HEAD of the page. However, if you do this it will become render blocking. And while it will fix the warning , it will actually create one to appear in Google PageSpeed Insights. Completely eliminating the use of render blocking resources may not be possible in all cases. However, there do exist some recommendations to help prevent blocking resources such as lessening the amount of CSS files, inlining your CSS, minifying your CSS, and moving your scripts to the bottom of the page (just before your `</body>` tag), etc.

In our test, to fix our C grade for this rule we would need to move both our CSS footer stylesheets up to the top between our `<head></head>` tags.

```html
<head>
<link href='https://fonts.googleapis.com/css?family=Noto+Serif:400,400italic,700' rel='stylesheet' type='text/css'>
<link href="https://opensource.keycdn.com/fontawesome/4.5.0/font-awesome.min.css" rel="stylesheet">
</head>
```

#### <a name="s3"></a>3. Put JavaScript at Bottom
*JavaScript scripts block parallel downloads; that is, when a script is downloading, the browser will not start any other downloads. To help the page load faster, move scripts to the bottom of the page if they are deferrable.*

**Non-Render Blocking Javascript**

When it comes to Javascript there are some best practices to always keep in mind.

* Move your scripts to the bottom of the page right before your `</body>` tag.
* Use the async or defer directive to avoid render blocking.

**Loading Javascript Asynchronously**

Async allows the script to be downloaded in the background without blocking. Then, the moment it finishes downloading, rendering is blocked and that script executes. Render resumes when the script has executed.

```html
<script async src="foobar.js"></script>
```

**Deferring Javascript**

The defer directive does the same thing, except it guarantees that scripts execute in the order they were specified on the page. So, some scripts may finish downloading then sit and wait for scripts that downloaded later but appeared before them.

This is a good example of how to defer loading of javascript properly.

* Less the amount of Javascript files (concatenate your JS files into one file)
* Minify your Javascript (remove extra spaces, characters, etc)
* Inline your javascript if it is small
* Read more about what is blocking the DOM.


#### <a name="s4"></a>4. Avoid CSS Expressions
*CSS expressions (supported in IE beginning with Version 5) are a powerful, and dangerous, way to dynamically set CSS properties. These expressions are evaluated frequently: when the page is rendered and resized, when the page is scrolled, and even when the user moves the mouse over the page. These frequent evaluations degrade the user experience.*

CSS expressions can be used to set CSS properties dynamically, like the example below. If you need to change values like this it might be better to combine CSS with some JavaScript to achieve the same thing.

```html
background-color: expression( (new Date()).getHours()%2 ? "#B8D4FF" : "#F08A00" );
```

#### <a name="s5"></a>5. Minify JavaScript and CSS
*Minification removes unnecessary characters from a file to reduce its size, thereby improving load times. When a file is minified, comments and unneeded white space characters (space, newline, and tab) are removed. This improves response time since the size of the download files is reduced.*

To minify CSS, JS, and HTML involves removing any unnecessary characters from within a file to help **reduce its size and thus make it load faster**. Examples of what is removed during file minification includes:

* Whitespace characters
* Comments
* Line breaks
* Block delimiters
* Check out our in-depth post on how to minify CSS, JS, and HTML.


#### <a name="s6"></a>6. Remove Duplicate JavaScript and CSS
*Duplicate JavaScript and CSS files hurt performance by creating unnecessary HTTP requests (IE only) and wasted JavaScript execution (IE and Firefox). In IE, if an external script is included twice and is not cacheable, it generates two HTTP requests during page loading. Even if the script is cacheable, extra HTTP requests occur when the user reloads the page. In both IE and Firefox, duplicate JavaScript scripts cause wasted time evaluating the same scripts more than once. This redundant script execution happens regardless of whether the script is cacheable.*

We actually see this happen a lot with with both Google Fonts and Font Awesome scripts, especially with customers running WordPress. Usually a theme developer will include Google Fonts or Font Awesome and then if a user goes and adds the script themselves, or another plugin which utilizes its, the website then has HTTP requests to the same asset. Even with caching make sure you are only including references to your external scripts once.


#### <a name="s7"></a>7. Reduce Cookie Size
*HTTP cookies are used for authentication, personalization, and other purposes. Cookie information is exchanged in the HTTP headers between web servers and the browser, so keeping the cookie size small minimizes the impact on response time.*

Keeping your cookie sizes small might not be as important as it used to be but it is something you should always check, especially if you are a developer. Some additional tips are:

* Eliminate unnecessary cookies
* Be mindful of setting cookies at the appropriate domain level so other sub-domains are not affected
* Set an Expires date appropriately.


#### <a name="s7"></a>7. Do Not Scale Images in HTML
*Web page designers sometimes set image dimensions by using the width and height attributes of the HTML image element. Avoid doing this since it can result in images being larger than needed. For example, if your page requires image myimg.jpg which has dimensions 240×720 but displays it with dimensions 120×360 using the width and height attributes, then the browser will download an image that is larger than necessary.*

For the best performance, you should always upload your images at scale if possible. For example, if you have an image that you want to display at 200px wide, don’t upload an image that is 400px wide and then scale it with HTML. A better way to accomplish this is to use the srcset attribute in the `<img />` tag which allows you to define possible resolutions that the browser can choose from. Here an example:

```html
<img src="/img/keycdn-600.jpg"
      alt="KeyCDN"
      srcset="/img/keycdn-300.jpg 300w,
              /img/keycdn-600.jpg 600w,
              /img/keycdn-1200.jpg 1200w" />
```

Most modern [web browsers support srcset](http://caniuse.com/#search=srcset), except IE and opera mini.


#### <a name="s8"></a>8. Test Tool
* [KeyCDN Website Speed Test](https://tools.keycdn.com/speed)
* [Pingdom Website Speed Test](https://tools.pingdom.com/)