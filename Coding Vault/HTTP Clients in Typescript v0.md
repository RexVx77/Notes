CH1: Why HTTP?
# Communicating on the Web

Instagram would be pretty terrible if you had to manually copy your photos to your friend's phone when you wanted to share them. Modern applications need to be able to communicate information _between devices_ over the internet.

- Gmail doesn't just store your emails in variables on your computer, it stores them on computers in their data centers
- You don't lose your Slack messages if you drop your computer in a lake, those messages exist on Slack's [servers](https://en.wikipedia.org/wiki/Web_server)

## How Does Web Communication Work?

When two computers communicate with each other, they need to use the same rules. An English speaker can't communicate verbally with a Japanese speaker, similarly, two computers need to speak the same language to communicate.

This "language" that computers use is called a [protocol](https://en.wikipedia.org/wiki/Communication_protocol). The most popular protocol for web communication is [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), which stands for Hypertext Transfer Protocol.

# HTTP Requests and Responses

At the heart of HTTP is a simple request-response system. The "requesting" computer, also known as the ["client"](https://en.wikipedia.org/wiki/Client_\(computing\)), asks another computer for some information. That computer, ["the server"](https://en.wikipedia.org/wiki/Server_\(computing\)) sends back a response with the information that was requested.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/mi20b1O-850x400.png)

We'll talk about the specifics of how the "requests" and "responses" are formatted later. For now, just think of it as a simple question-and-answer system.

- Request: "What issues are on Jello?"
- Response: `["Fix bug", "Improve auth flow"]`

# HTTP Powers Websites

[HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview), or Hypertext Transfer Protocol, is a [protocol](https://developer.mozilla.org/en-US/docs/Glossary/Protocol) designed to transfer information between computers.

There are other protocols for communicating over the internet, but HTTP is the most popular and is _particularly great for websites and web applications_. Each time you visit a website, your browser is making an HTTP request to that website's server. The server responds with all the text, images, and styling information that your browser needs to render its pretty website!

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/7OQAN0m-1280x473.png)

# HTTP URLs

A URL, or [Uniform Resource Locator](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL), is the address of another computer, or "server" on the internet. Part of the URL specifies _where to reach_ the server, and part of it tells the server _what information we want_.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/ga7wRus-1019x720.png)

Put simply, a URL represents _a piece of information on some computer somewhere_. We can get access to it by making a _request_, and reading the _response_ that the server replies with.

# Using URLs in HTTP

The `http://` at the beginning of a [website URL](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL) specifies that the `http` protocol will be used for communication.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/kz0imYE-793x394.png)

Other communication protocols use URLs as well, (hence "Uniform Resource Locator"). That's why we need to be specific when we're making HTTP requests by prefixing the URL with `http://`

# Requests and Responses Quiz

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/JJN40ZV-816x400.png)

- A "client" is a computer making an HTTP request
- A "server" is a computer responding to an HTTP request
- A computer can be a client, a server, both, or neither. "Client" and "server" are just words we use to describe what computers are doing within a communication system.
- Clients send requests and receive responses
- Servers receive requests and send responses

# JavaScript's Fetch API

In this course, we'll be using JavaScript's built-in [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) to make HTTP requests. We already used it in the last two assignments!

The `fetch` function is made available to us by the JavaScript language running in the browser, all we have to do is provide it with the parameters it requires.

## Using Fetch

```ts
const response = await fetch(url, settings);
const responseData = await response.json();
```

We'll go in-depth on the various things happening in this standard `fetch` call later, but let's cover some basics for now.

- `response` is the data that comes back from the server
- `url` is the URL we are making a request to
- `settings` is an object containing some request-specific settings
- The first `await` tells JavaScript to wait until the response comes back from the server before continuing
- The second `await` waits for `response.json()` to convert the response data from the server into a JavaScript object
# Web Clients

As we've discussed, a web client is a device making requests to a web server.

A client can be any type of device but is often something users physically interact with. For example:

- A desktop computer
- A mobile phone
- A tablet

In a website or web application, we call the user's device the "front-end".

A front-end client makes requests to a back-end server.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/3RL8Os2-1273x720.png)

# Web Servers

Up to this point, most of the data you have worked with in your code has simply been generated and stored locally in variables.

While you'll always use variables to store and manipulate data while your program is running, most websites and apps use a web server to store, sort, and serve that data so that it sticks around for longer than a single session, and can be accessed by multiple devices.

## Listening and Serving Data

Similar to how a server at a restaurant brings your food to the table, a [web server](https://en.wikipedia.org/wiki/Web_server) serves web resources, such as web pages, images, and other data. The server is turned on and "listening" for inbound requests constantly so that the second it receives a new request, it can send an appropriate response.

## The Server Is the Back-End

While the "front-end" of a website or web application is the device the user interacts with, the "back-end" is the server that keeps all the data housed in a central location.

## A Server Is Just a Computer

"Server" is just the name we give to a computer that is taking on the role of serving data across a network connection. A good server is turned on and available 24 hours a day, 7 days a week. While your laptop _can_ be used as a server, it makes more sense to use a computer in a data center that's designed to be up and running constantly.

---

CH2: DNS
# Web Addresses

In the real world, we use physical addresses to help us find where a friend lives, where a business is located, or where a party is being thrown (well, I don't because I'm not invited to parties, but I digress).

In computing, web clients find other computers over the internet using [Internet Protocol](https://en.wikipedia.org/wiki/Internet_Protocol) (IP) addresses. Each device connected to the internet has a **unique IP address**.

## Domain Names and IP Addresses

When we browse the internet, we type in a human readable domain name. That domain is converted into an IP address. The IP address tells our computer where the server is located on the internet.

Click to hide video

An IP address typically looks like a sequence of numbers separated by periods, ranging from 0 to 255.

# Web Addresses Quiz

To recap, a [domain name](https://en.wikipedia.org/wiki/Domain_name) is part of a URL. It's the part that tells the computer _where the server is located on the internet_ by being converted into a numerical IP address.

We'll cover exactly how an IP address is used by your computer to find a path to the server in a later course. For now, it's just important to understand that an IP address is what your computer is using at a lower level to communicate on a network.

Deploying a real website to the internet is actually quite simple. It involves only a couple of steps:

1. Create a server that hosts your website files and connect it to the internet
2. Acquire a domain name
3. Connect the domain name to the IP address of your server
4. Your server is accessible via the internet!

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/MK3y0He-1280x590.png)

# DNS

A ["domain name"](https://en.wikipedia.org/wiki/Domain_name) or ["hostname"](https://en.wikipedia.org/wiki/Hostname) is just one portion of a URL. We'll get to the other parts of a URL later.

For example, the URL `https://homestarrunner.com/toons` has a hostname of `homestarrunner.com`. The `https://` and `/toons` portions aren't part of the `domain name -> IP address` mapping that we've been talking about.

## Using the URL API

The `URL` API is built into JavaScript. You can create a [new URL object](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL):

```js
const urlObj = new URL("https://homestarrunner.com/toons");
```

And then you can [extract just the hostname](https://developer.mozilla.org/en-US/docs/Web/API/URL):

```js
const hostname = urlObj.hostname;
console.log(hostname); // homestarrunner.com
```

# What Is the Domain Name System?

So we've talked about domain names, but we haven't talked about _the system_ that makes them work.

[DNS](https://en.wikipedia.org/wiki/Domain_Name_System), or the "Domain Name System", is the phonebook of the internet. Humans type easy-to-read [domain names](https://en.wikipedia.org/wiki/Domain_name) like [Boot.dev](https://boot.dev/). DNS "resolves" those domain names to their associated [IP addresses](https://en.wikipedia.org/wiki/Internet_Protocol) so that web clients can find the server they're looking for.

_Domain names are for humans, IP addresses are for computers._

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/lvjZb5u-1280x513.png)

## How Does DNS Work?

We'll go into more detail on DNS in a future course, but to give you a simplified idea, let's just introduce ICANN. [ICANN](https://www.icann.org/) is a not-for-profit organization that manages DNS for the entire internet.

Whenever your computer attempts to resolve a domain name, it contacts one of ICANN's ["root nameservers"](https://en.wikipedia.org/wiki/Root_name_server) whose address is included in your computer's networking configuration. From there, that nameserver can gather the domain records for a specific domain name from their distributed DNS database.

If you think of DNS as a phonebook, ICANN is the publisher that keeps the phonebook in print and available.

# Subdomains

We learned about how a domain name resolves to an IP address, which is just a computer on a network - often the internet.

A _subdomain_ prefixes a domain name, allowing a domain to route network traffic to many different servers and resources.

For example, [docs.github.com](https://docs.github.com/) is a subdomain of `github.com`. The `docs` subdomain likely routes to a different server than the main `github.com` domain.

---

CH3: URIs

# Uniform Resource Identifiers

We briefly touched on URLs earlier, let's dive a little deeper into the subject.

A [URI](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier), or Uniform Resource _Identifier_, is a unique character sequence that identifies a resource that is (almost always) accessed via the internet.

Just like TypeScript has syntax rules, so do URIs. These rules help ensure uniformity so that any program can interpret the meaning of the URI in the same way.

URIs come in two main types:

- [URLs](https://en.wikipedia.org/wiki/URL)
- [URNs](https://en.wikipedia.org/wiki/Uniform_Resource_Name)

We will focus specifically on URLs in this course, but it's important to know that URLs are only one kind of URI.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/T9uAKkw-1254x720.png)

# Sections of a URL

URLs have quite a few sections. Some are required, some are not.

## Assignment

Let's use the [URL API](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL) again. This time, we'll parse a URL and print all the different parts. We'll learn more about each part later, for now, let's just split and print a URL!

Complete the `printURLParts` function. It should print all the parts of a URL. For example, given this URL:

`http://testuser:testpass@testdomain.com:8080/testpath?testsearch=testvalue#testhash`

Your code should print:

```
protocol: http:
username: testuser
password: testpass
hostname: testdomain.com
port: 8080
pathname: /testpath
search: ?testsearch=testvalue
hash: #testhash
```

Use the following properties on the URL object:

- `protocol`
- `username`
- `password`
- `hostname`
- `port`
- `pathname`
- `search`
- `hash`

The URL object does not have enumerable properties to iterate over. You'll need to manually log each property.

# Further Dissecting a URL

There are 8 main parts to a URL, though not all the sections are always present. Each piece plays a role in helping clients locate the ~~droids~~ resources they're looking for.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/TpxX9Ei-1280x234.png)

|Part|Required|
|---|---|
|Protocol|Yes|
|Username|No|
|Password|No|
|Domain|Yes|
|Port|No (defaults to 80 or 443)|
|Path|No (defaults to /)|
|Query|No|
|Fragment|No|

## Don't Memorize

Because names for the different sections are often used... sloppily... and because not all the parts of the URL are always present, it can be hard to keep things straight.

Don't worry about memorizing this stuff! Like any good developer, now that you know about URL sections, you can always look up the specifics again in the future when you need them.

# The Protocol

The "protocol" (also referred to as the "scheme") is the first component of a URL. It defines the rules by which the data being communicated is displayed, encoded or formatted.

Some examples of different URL protocols:

- http
- ftp
- mailto
- https

For example:

- `http://example.com`
- `mailto:noreply@jello.app`

## Not All Schemes Require a “//”

The "http" in a URL is always followed by `://`. All URLs have the colon, but the `//` part is only included for schemes that have an [authority component](https://www.rfc-editor.org/rfc/rfc3986#section-3.2). As you can see above, the `mailto` scheme doesn't use an authority component, so it doesn't need the slashes.

# URL Ports

The port in a URL is a virtual point where network connections are made. Ports are managed by a computer's operating system and are numbered from `0` to `65,535` _(Though port `0` is reserved for the system API)_.

Whenever you connect to another computer over a network, you're connecting to a specific port on that computer, which is listened to by a program on that computer. A port can only be used by one program at a time, which is why there are so many possible ports.

The port component of a URL is often not visible when browsing normal sites on the internet, because 99% of the time you're using the default ports for the HTTP and HTTPS schemes: `80` and `443` respectively.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/gOCIIe4-632x283.png)

Whenever you aren't using a default port, you need to specify it in the URL. For example, port `8080` is often used by web developers when they're running their server in "test mode" on their own machines.

# URL Paths

On static sites (like blogs or documentation sites) a URL's path mirrors the server's filesystem hierarchy.

For example, if the website `https://exampleblog.com` had a static web server running in its `/home` directory, then a request to `https://exampleblog.com/site/index.html` would probably return the file located at `/home/site/index.html`.

_But technically, this is just a convention. The server could be configured to return any file or data given that path._

## It's Not Always That Simple

Paths in URLs are essentially just another type of parameter that can be passed to the server when making a request. For dynamic sites and web applications, the path is often used to denote a specific resource or endpoint.

# Query Parameters

Query parameters in a URL are _not_ always present. In the context of websites, query parameters are often used for marketing analytics or for changing a variable on the web page. With website URLs, query parameters _rarely_ change _which_ page you're viewing, though they often will change the page's _contents_.

That said, query parameters can be used for anything the server chooses to use them for, just like the URL's path.

## How Google Uses Query Parameters

1. Open a new tab and go to [https://google.com](https://google.com/).
2. Search for the term "hello world"
3. Take a look at your current URL. It should start with `https://www.google.com/search?q=hello+world`
4. Change the URL to say `https://www.google.com/search?q=hello+universe`
5. Press "enter"

You should see new search results for the query "hello universe". Google chose to use query parameters to represent the value of your search query. It makes sense - each search result page is _essentially_ the same page as far as HTML structure and CSS styling are concerned - it's just showing you different results based on the search query.

---

CH4: Errors

# Error Handling in TypeScript

When something goes wrong while a program is running, TypeScript uses the `try/catch` paradigm for handling those errors. Try/catch is fairly common, Python uses a similar mechanism.

## First, an Error Is Thrown

For example, let's say we try to access a property on an undefined variable. JavaScript will automatically "throw" an error.

```ts
const speed = car.speed;
// The code crashes with the following error:
// "ReferenceError: car is not defined"
```
## Trying and Catching Errors

By wrapping that code in a try/catch block, we can handle the case where `car` is not yet defined. In TypeScript, the catch block parameter is typed as unknown by default, so you need to check if the error is an instance of Error before accessing its properties, like message. This ensures type safety and prevents runtime errors when the error is not an Error object.

```ts
try {
  const speed = car.speed;
} catch (err: unknown) {
  if (err instanceof Error) {
    console.log(`An error was thrown: ${err}`);
    // the code cleanly logs:
    // "An error was thrown: ReferenceError: car is not defined"
  } else {
    console.log("An unknown error occurred:", err);
  }
}
```
## Handling a New Error Object

When handling a thrown `Error` object, you must access the `message` property of the error object to display it correctly to the console.

```ts
try {
  // computation
  throw new Error("This is the error message");
} catch (err) {
  if (err instanceof Error) {
    console.log(`An error was thrown: ${err.message}`);
    // the code cleanly logs:
    // "An error was thrown: This is the error message"
  }
}
```

# Bugs vs. Errors


Error handling via try/catch is **not** the same as debugging. Likewise, errors are **not** the same as bugs.

- Good code with no bugs can still produce errors that are gracefully handled
- Bugs are, by definition, bits of code that aren't working as intended

## Debugging

"Debugging" a program is the process of going through your code to find where it is not behaving as expected. Debugging is a manual process performed by the developer. Sometimes developers use special software called a "debugger" to help them find bugs, but often they just use `console.log()` statements to figure out what's going on.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/VBhqzp2-435x300.png)

Examples of debugging:

- Adding a missing parameter to a function call
- Updating a broken URL that an HTTP call was trying to reach
- Fixing a date-picker component in an app that wasn't displaying properly

## Error Handling

"Error handling" is code that can handle _expected_ edge cases in your program. Error handling is an automated process that we design into our production code to protect it from things like weak internet connections, bad user input, or bugs in other people's code that we have to interface with.

Examples of error handling:

- Using a try/catch block to detect an issue with user input
- Using a try/catch block to gracefully fail when no internet connection is available

## In Short, Don't Use Try/Catch to Try to Handle Bugs

If your code has a [bug](https://en.wikipedia.org/wiki/Software_bug), try/catch won't help you. You need to just go find the bug and fix it.

If something out of your control can produce issues in your code, you should use try/catch or other error-handling logic to deal with it.

For example, there could be a prompt in Jello for users to type in a new character name, but we don't want them to use punctuation. Validating their input and displaying an error message if something is wrong with it would be a form of "error handling".

# Async/Await Makes Error Handling Easier

`try` and `catch` are the standard way to handle errors, the trouble is, the original Promise API with `.then` didn't allow us to make use of `try` and `catch` blocks.

Luckily, the `async` and `await` keywords _do_ allow it, yet another reason to prefer the newer syntax.

## .catch() Callback On Promises

The [.catch()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/catch) method works similarly to the .then() method, but it fires when a promise is _rejected_ instead of resolved.

## Example With .then and .catch Callbacks

```ts
fetchUser()
  .then((user: User) => {
    console.log(`User fetched: ${user}`);
  })
  .catch((err: unknown) => {
    if (err instanceof Error) {
      console.log(`An error was thrown: ${err.message}`);
    } else {
      console.log("An unknown error occurred:", err);
    }
  });
```

## Example of Awaiting a Promise

```ts
try {
  const user = await fetchUser();
  console.log(`user fetched: ${user}`);
} catch (err) {
  if (err instanceof Error) {
    console.log(err.message);
  } else {
    console.log("An unexpected error occurred:", err);
  }
}
```

As you can see, the `async/await` version looks just like normal `try/catch` TypeScript!

---

CH5: Headers

# What Are HTTP Headers?

An [HTTP header](https://developer.mozilla.org/en-US/docs/Glossary/HTTP_header) allows clients and servers to pass _additional_ information with each request or response. Headers are just case-insensitive [key-value pairs](https://en.wikipedia.org/wiki/Name%E2%80%93value_pair) that pass additional [metadata](https://en.wikipedia.org/wiki/Metadata) about the request or response.

HTTP requests from a web browser automatically carry with them many headers, including but not limited to:

- The type of client (e.g. Google Chrome)
- The Operating system (e.g. Windows)
- The preferred language (e.g. US English)

As developers, we can also define custom headers in each request.

## Headers API

The [Headers](https://developer.mozilla.org/en-US/docs/Web/API/Headers) API allows us to perform various actions on our request and response headers such as retrieving, setting, and removing them. We can access the headers object through the `Request.headers` and `Response.headers` properties.

# Using the Browser's Developer Tools

Modern web browsers offer developers a powerful set of _developer tools_. The [Developer Tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools) are a front-end web developer's best friend! For example, using the dev tools you can:

- View the web page's JavaScript console output
- Inspect the page's HTML, CSS, and JavaScript code
- View network requests and responses, along with their headers.

The method for accessing dev tools varies from browser to browser.

- **Keyboard Shortcuts**:
    - Windows: Ctrl+Shift+I or F12
    - macOS: Cmd+Option+I

In most browsers, you can just right-click anywhere on a web page and click the "inspect" option. Follow this link for more info on [how to access dev tools](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Tools_and_setup/What_are_browser_developer_tools#how_to_open_the_devtools_in_your_browser).

## The Network Tab

While all of the tabs within the dev tools are useful, we will focus on the _network tab_ in this chapter so we can play with HTTP headers. The network tab monitors your browser's network activity and records all of the requests and responses the browser is making, including how long each of those requests and responses takes to fully process.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/STKdceG-580x372.png)


# Why Are Headers Useful?

Headers are useful for several reasons from design to security, but most often headers are used for [metadata](https://en.wikipedia.org/wiki/Metadata) _about_ the request or response itself. For example, let's say we wanted to ask for a specific project from the Jello server. We need to send that project's ID to the server so it knows which project to send back the information for. That ID _is my request_, it's not information _about my request_. However, we might include headers in that request to pass along extra information - like _who_ is making the request.

[Authentication](https://auth0.com/intro-to-iam/what-is-authentication/) is a common use case for headers. If I ask Jello to complete a project, I need to provide authentication information that I'm logged in, but that auth info isn't the request itself, it's just _additional information_ about the request.

# Network Tab Practice

1. Open your browser's Dev Tools
2. Navigate to the _Network_ tab at the top.
    - If you don't see the network tab, it should be in the dropdown menu. Click it.
3. Once you've opened the network tab, refresh this page.

Poke around through the different requests that you see. Notice that you can select a request and see its request and response headers. Request headers are sent from your browser to the server. Response headers are the headers sent back from the server to your browser.  
You will use the information you find within the headers tab to answer the following questions.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/STKdceG-580x372.png)

---

CH6: JSON

# JSON Syntax

[JSON](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/JSON) (JavaScript Object Notation), is a standard for representing _structured_ data based on JavaScript's object syntax. It is commonly used to transmit data in web apps via HTTP. For example, the HTTP `fetch()` requests we have been using in this course have been returning _Jello_ projects, users, and issues as JSON.

JSON supports the following primitive data types:

- Strings, e.g. `"Hello, World!"`
- Numbers, e.g. `42` or `3.14`
- Booleans, e.g. `true`
- Null, e.g. `null`

And the following collection types:

- Arrays, e.g. `[1, 2, 3]`
- Object literals, e.g. `{"key": "value"}`

Because we already understand what JavaScript objects look like, understanding JSON is easy! JSON is just a stringified JavaScript object. The following is valid JSON data:

```js
{
    "movies": [
        {
            "id": 1,
            "genre": "Action",
            "title": "Iron Man",
            "director": "Jon Favreau"
        },
        {
            "id": 2,
            "genre": "Action",
            "title": "The Avengers",
            "director": "Joss Whedon"
        }
    ]
}
```

## Parsing HTTP Responses As JSON

JavaScript provides us with some easy tools to help us work with JSON. After making an HTTP request with the [fetch() API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API), we get a [Response object](https://developer.mozilla.org/en-US/docs/Web/API/Response). That response object offers us some methods that help us interact with the response. One such method is the [`.json()`](https://developer.mozilla.org/en-US/docs/Web/API/Response/json) method. The `.json()` method takes the response stream returned by a fetch request and returns a [promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) that resolves into a JavaScript object parsed from the JSON body of the HTTP response!

```js
const resp = await fetch(...)
const javascriptObjectResponse = await resp.json()
```
## What type is .json()?

The `.json()` method returns a promise that resolves to an object typed as `any`. This means TypeScript doesn't enforce any specific shape or structure for the returned data, leaving it up to you to handle the type appropriately.

If you know the shape of the data, you can explicitly set the return type – like we've done so far – or you can cast it to a specific type:

```ts
interface Project {
  id: string;
  title: string;
}

const response = await fetch("https://api.example.com/projects");
const projects = (await response.json()) as Project[];
```

# JSON Review

JSON is a _stringified representation_ of a [JavaScript object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_objects), which makes it perfect for saving to a file or sending in an HTTP request. Remember, an actual JavaScript object is something that exists only within your program's variables. If we want to send an object outside our program, for example, across the internet in an HTTP request, we need to convert it to JSON first.

## JSON Isn't Just for JavaScript

Just because JSON is called _JavaScript_ Object Notation doesn't mean it's only used by JavaScript code! JSON is a common standard that is recognized and supported by every major programming language. For example, even though Boot.dev's backend is written in Go, we still use JSON as the communication format between the front-end and backend.

## Common Use-Cases

- In HTTP request and response bodies
- As formats for text files. `.json` files are often used as configuration files.
- In NoSQL databases like MongoDB, ElasticSearch and Firestore

## Pronouncing JSON

I pronounce it "Jay-sawn", but I've also heard people pronounce it "Jason" (like the name), and even "J'ai son" (Frenchily).

# Sending JSON

`JSON` isn't just something we get from the server, we can also _send_ JSON data!

In TypeScript, two of the main methods we have access to are [JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse), and [JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).

## JSON.stringify()

`JSON.stringify()` is particularly useful for _sending_ JSON.

As you may expect the JSON `stringify()` method does the opposite of parse. It takes a JavaScript object or value as input and converts it into a string. This is useful when we need to serialize the objects into strings to send them to our server or store them in a database.

In the context of using `fetch`, the main difference lies in how the data is handled and whether it is synchronous or asynchronous.

### 1. `response.json()`

When you use `fetch`, the `response` object is a Stream. The data doesn't all arrive at once; it's still being read from the network.

- **Asynchronous:** It returns a **Promise**. You must use `await` or `.then()` because it has to wait for the full body of the HTTP response to finish downloading before it can turn it into a JavaScript object.
- **Convenience:** It automatically reads the stream to completion and runs the parsing logic for you.

```ts
const data = await response.json();
```

### 2. `JSON.parse()`

This is a standard JavaScript method used to turn a **string** into an **object**.

- **Synchronous:** It happens instantly. It does not return a Promise.
- **Input:** It requires the data to already be a complete string in memory. It cannot handle a network stream directly.
- **Manual step:** If you wanted to use this with `fetch`, you would first have to resolve the body as text, and then parse it.

```ts
const text = await response.text(); // Wait for the string to download
const data = JSON.parse(text);      // Turn the string into an object
```

### Summary

You use `response.json()` because it is built into the Fetch API specifically to handle network data efficiently. You would use `JSON.parse()` if you already had a JSON string sitting in a variable (for example, if you pulled it out of `localStorage` or a file).

# Parsing JSON

## Parse

The [JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) method takes a JSON string as input and constructs the JavaScript value/object described by the string. This allows us to work with the JSON as an object!

```typescript
const json = '{"title": "Avengers Endgame", "Rating":4.7, "inTheaters":false}';
const obj: Movie = JSON.parse(json);

console.log(obj.title);
// Avengers Endgame
```

# XML

We can't talk about JSON without mentioning [XML](https://en.wikipedia.org/wiki/XML#:~:text=Extensible%20Markup%20Language%20\(XML\)%20is,%2Dreadable%20and%20machine%2Dreadable.). `XML`, or "Extensible Markup Language" is a text-based format for representing structured information, like JSON - but different (and a bit more verbose).

## XML Syntax

XML is a markup language like [HTML](https://en.wikipedia.org/wiki/HTML), but it's more generalized in that it does _not_ use predefined tags. Just like how in a JSON object keys can be called anything, XML tags can also have any name.

XML representing a movie:

```xml
<root>
  <id>1</id>
  <genre>Action</genre>
  <title>Iron Man</title>
  <director>Jon Favreau</director>
</root>
```

The same data in JSON:

```json
{
  "id": "1",
  "genre": "Action",
  "title": "Iron Man",
  "director": "Jon Favreau"
}
```

# Why Use XML?

XML and JSON both accomplish similar tasks, so which should you use?

XML used to be used for the same things that today JSON is primarily used for. Configuration files, HTTP bodies, and other data-transfer can work with either JSON or XML. Personally, I believe that if JSON works, you should favor it over XML. JSON is:

- Lighter-weight
- Easier to read
- Has better support in most programming languages

There are cases where XML might still be better, or maybe even _necessary_, but that tends to be the exception rather than the rule.

---

CH7: Methods

# HTTP Methods - GET

HTTP defines a set of [methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). We must choose one to use each time we make an HTTP request. The most common ones include:

- `GET`
- `POST`
- `PUT`
- `DELETE`

The [GET method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET) is used to "get" a _representation_ of a specified resource. It doesn't _take_ (remove) the data from the server but rather _gets_ a representation, or copy, of the resource in its current state. A GET request is considered a [_safe_](https://developer.mozilla.org/en-US/docs/Glossary/Safe/HTTP) method to call multiple times because it shouldn't alter the state of the server.

## Making a GET Request Using the Fetch API

In this course, we have been and will continue to use the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) to make HTTP requests. The [fetch()](https://developer.mozilla.org/en-US/docs/Web/API/fetch) method accepts an optional `init` object parameter as its second argument that we can use to define things like:

- `method`: The HTTP method of the request, like `GET`.
- `headers`: The headers to send.
- `mode`: Used for security, we'll talk about this in future courses.
- `body`: The body of the request. Often encoded as JSON.

Example `GET` request using fetch:

```js
await fetch(url, {
  method: "GET",
  mode: "cors",
  headers: {
    "sec-ch-ua-platform": "macOS",
  },
});
```

# Why Use HTTP Methods?

The primary purpose of HTTP methods is to indicate to the server what we want to do with the resource we're trying to interact with. At the end of the day, an HTTP method is just a string, like `GET`, `POST`, `PUT`, or `DELETE`, but by _convention_, backend developers write their server code so that the methods correspond with different "CRUD" actions.

The "CRUD" actions are:

- Create
- Read
- Update
- Delete

The bulk of the logic in most web applications is "CRUD" logic. Even a complex social media site is _mostly_ just CRUD. Users create, read, update and delete their accounts. They create, read, update, and delete their posts. _It's CRUD all the way down!_

The 4 most common HTTP methods map nicely to the CRUD actions:

|HTTP Method|CRUD Action|
|---|---|
|GET|Read|
|POST|Create|
|PUT|Update|
|DELETE|Delete|

These mappings follow convention, not law. Some APIs may use `POST` for updating and creating, some might just skip `DELETE`.

# POST Requests

An [HTTP POST request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST) _sends_ data to a server, typically to _create_ a new resource.

## Adding a Body

The `body` of the request is the _payload_ sent to the server. The special `Content-Type` header is used to tell the server the format of the body: `application/json` for JSON data in our case. `POST` requests are generally _not_ safe methods to call multiple times because that would create duplicate records. For example, you wouldn't want to accidentally send a tweet twice.

```js
await fetch(url, {
  method: "POST",
  mode: "cors",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(data),
});
```

# Status Codes

The `Status Code` of an HTTP _response_ tells the client whether or not the server was able to fulfill the request. Status codes are 3-digit numbers that are grouped into categories:

- `100-199`: Informational responses. These are very rare.
- `200-299`: Successful responses. Hopefully, most responses are 200's!
- `300-399`: Redirection messages. These are typically invisible because the browser or HTTP client will automatically do the redirect.
- `400-499`: Client errors. You'll see these often, especially when trying to debug a client application
- `500-599`: Server errors. You'll see these sometimes, usually only if there is a bug on the server.

Here are some of the most common status codes, but there is also a [full list here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) if you're interested.

- `200` - OK. This is by far the most common code, it just means that everything worked as expected.
- `201` - Created. This means that a resource was created successfully. Typically in response to a `POST` request.
- `301` - Moved permanently. This means the resource was moved to a new place, and the response will include where that new place is. Websites often use `301` redirects when they change their domain name, for example.
- `400` - Bad request. A general error indicating the client made a mistake in their request.
- `401` - Unauthorized. This means the client doesn't have the correct permissions. Maybe they didn't include a required authorization header, for example.
- `404` - Not found. You'll see this on websites quite often. It just means the resource doesn't exist.
- `500` - Internal server error. This means something went wrong on the server, likely a bug on their end.

## Don't Memorize

It's good to know the basics by heart, like "2XX is good", "4XX is a client error", and "5XX is a server error". But don't memorize all the status codes... they're easy to [look up](https://http.cat/).

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/FJl2z9O-549x454.jpg)

# Status Code Property

The [`Response`](https://developer.mozilla.org/en-US/docs/Web/API/Response) has a `.status` property that contains the status code of the response.

# PUT

The HTTP [`PUT`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PUT) method creates _or more commonly, updates_ a representation of the target resource with the contents of the request's `body`. In short, it updates a resource's properties.

```js
await fetch(url, {
  method: "PUT",
  mode: "cors",
  headers: {
    "Content-Type": "application/json",
  },
  body: JSON.stringify(data),
});
```

## POST vs. PUT

While `POST` and `PUT` are both used for creating resources, `PUT` is more common for updates and is [idempotent](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent), meaning it's safe to make the request multiple times without changing the server state. For example:

```
POST /users/bob (create bob user)
POST /users/bob (create duplicate bob user)
POST /users/bob (create duplicate bob user)
```

```
PUT /users/bob (create bob user if it doesn't exist)
PUT /users/bob (update bob user with the same data)
PUT /users/bob (update bob user with the same data)
```

# PATCH vs. PUT

You may encounter the [PATCH](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/PATCH) method from time to time. It's frankly not used very often. It's meant to _partially_ modify a resource, whereas `PUT` is meant to _completely replace_ a resource.

`PATCH` is not as popular as `PUT`, and many servers, even if they allow partial updates, just use `PUT`

# Delete

The `DELETE` method does exactly what you expect: it deletes a specified resource.

### Example

```js
// This deletes the location with ID: 52fdfc07-2182-454f-963f-5f0f9a621d72
const url =
  "https://api.boot.dev/v1/courses_rest_api/learn-http/locations/52fdfc07-2182-454f-963f-5f0f9a621d72";

await fetch(url, {
  method: "DELETE",
  mode: "cors",
});
```

---

CH8: Paths

# URL Paths

The URL Path comes right after the domain (or port if one is provided) in a URL string.

In this URL:

```
`http://testdomain.com/root/next`
```

The path is:

```
/root/next
```

## Path Conventions

Many static websites (websites where the content does not change, as opposed to dynamic web applications) use paths as a direct mapping to the path to the server's file system. For example, If I was running a static web server for "mystaticstate.com" from the root of my file system, a `GET` request to `http://mystaticstate.com/documents/hello.txt` would serve the file at `/documents/hello.txt` from my server.

Most dynamic web applications don't use this simple mapping of `URL path` -> `file path`. Technically, a URL path is just a string that the web server can do what it wants with, and modern websites take advantage of that flexibility. Some common examples of what paths are used for include:

- The hierarchy of pages on a website, whether or not that reflects a server's file structure
- Parameters passed into an HTTP request, like the ID of a resource
- The version of the API
- The type of resource being requested

# RESTful APIs

[Representational State Transfer, or REST,](https://developer.mozilla.org/en-US/docs/Glossary/REST) is a popular convention that many dynamic HTTP servers follow. Not all HTTP APIs are "REST APIs", or "RESTful", but it is _very_ common.

RESTful servers follow a loose set of rules that makes it easy to build reliable and predictable web APIs. REST is a set of conventions about how HTTP APIs _should_ be built.

## Separate and Agnostic

The big idea behind REST is that resources are transferred via well-recognized, language-agnostic client/server interactions. A RESTful style means the implementation of the client and server can be created independently of one another, as long as some simple standards, like the names of the available resources, have been established.

## Stateless

A RESTful architecture is _stateless_, which means the server does not need to know what state the client is in, nor does the client need to know what state the server is in. Statelessness in REST is enforced by interacting with _resources_ instead of _commands_. Keep in mind, this doesn't mean the _applications_ are stateless - what would "updating a resource" even mean if the server wasn't keeping track of its state?

## Paths in Rest

In a RESTful API, the last section of the `path` of a URL specifies the _resource_. In Jello, this means an `issue`, a `user`, or a `project`. Depending on whether the request is a `GET`, `POST`, `PUT` or `DELETE`, the resource is read, created, updated, or deleted.

The _Jello_ API we have been working with is a RESTful API! Take a look at the URLs we've been using:

- `https://api.boot.dev/v1/courses_rest_api/learn-http/projects`
- `https://api.boot.dev/v1/courses_rest_api/learn-http/users`
- `https://api.boot.dev/v1/courses_rest_api/learn-http/issues`

1. The first part of the path specifies the _version_. In this case, version 1, or `v1`.
2. The second part of the path tells our server that this is a REST API for the "Learn HTTP" course.
3. The last part denotes which _resource_ is being accessed, be it a `project`, `user`, or `issue`.

# URL Query Parameters

A URL's query parameters appear next in the URL structure but are _not_ always present - they're optional. For example:

[https://www.google.com/search?q=boot.dev](https://www.google.com/search?q=boot.dev)

`q=boot.dev` is a query parameter. Like headers, query parameters are `key / value` pairs. In this case, `q` is the key and `boot.dev` is the value.

# The Documentation of an HTTP Server

You may be wondering:

> How the heck am I supposed to memorize how all these different servers work???

The good news is that _you don't need to_. When you work with a backend server, it's the responsibility of that server's developers to provide you with instructions, or _documentation_ that explains how to interact with it. For example, the documentation should tell you:

- The domain of the server
- The resources you can interact with (HTTP paths)
- The supported query parameters
- The supported HTTP methods
- Anything else you'll need to know to work with the server

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/GIlWhYF-500x553.jpg)

## The Server Is the Captain Now

The server has _complete control_ over how the path in a URL is interpreted and used in a request. The same goes for query parameters. While there are a lot of strong _conventions_ around how servers _should_ interpret paths and query parameters, the server can do whatever it wants. That's why you need docs.

# Multiple Query Parameters

Query parameters are `key/value` pairs - that means there can be multiple pairs!

`http://example.com?firstName=lane&lastName=wagner`

In the example above:

- `firstName` = `lane`
- `lastName` = `wagner`

The `?` separates the query parameters from the rest of the URL. The `&` is then used to separate _each additional pair_ of query parameters after that.

---

# HTTPS

Hypertext Transfer Protocol _Secure_ or [HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/https) is an extension of the HTTP protocol. HTTPS secures the data transfer between client and server by [encrypting](https://developer.mozilla.org/en-US/docs/Glossary/Encryption) all of the communication.

HTTPS allows a client to safely share sensitive information with the server through an HTTP request, such as credit card information, passwords, or bank account numbers.

# Security and Encryption

HTTPS requires that the client use [SSL](https://developer.mozilla.org/en-US/docs/Glossary/SSL) or [TLS](https://developer.mozilla.org/en-US/docs/Glossary/TLS) to protect requests and traffic by encrypting the information in the request. HTTPS is just HTTP with extra security!

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/iOkQUdG-517x487.png)

## HTTPS Keeps Your Messages Private, but Not Your Identity

We won't cover _how_ encryption works in this course, but we will in later courses! For now, it's important to note that while HTTPS encrypts _what you are saying_, it doesn't necessarily protect _who you are_. Tools like [VPNs](https://nordvpn.com/what-is-a-vpn/) are needed for added privacy.

## HTTPS Ensures That You're Talking to the Right Person (or Server)

In addition to encrypting the information within a request, HTTPS uses [digital signatures](https://en.wikipedia.org/wiki/Digital_signature) to prove that you're communicating with the server that you think you are. If a hacker were to intercept an HTTPS request by tapping into a network cable, they wouldn't be able to successfully pretend they are your bank's web server.

---

CH10: Runtime Validation

# The Runtime Problem

TypeScript gives us compile-time type safety, but when data comes from external sources like APIs, we lose that safety. The result of `await res.json()` is typed as `any`.

```ts
const res = await fetch(
  "https://api.boot.dev/v1/courses_rest_api/learn-http/issues",
);
const data = await res.json(); // data is type 'any'
console.log(data[0].title.toUpperCase()); // might crash at runtime!
```

## Manual Validation

Without a library, you'd need to manually check every field, which is tedious, error-prone, and doesn't give you TypeScript types:

```ts
function validateUser(data: any): User {
  if (!data || typeof data !== "object") {
    throw new Error("Invalid data");
  }
  if (typeof data.name !== "string") {
    throw new Error("Invalid name");
  }
  if (typeof data.age !== "number") {
    throw new Error("Invalid age");
  }
  // ... and so on for every field
  return data;
}
```

Or, if you trust the backend or feel adventurous, you can forcefully assert it:

```ts
function validateUser(data: any): User {
  return data as User;
}
```

Since `any` is assignable to everything, you don't need `as User`, but writing it makes your intent clear.

# Validation Libraries

Manual validation is quite error-prone. Fortunately, many agree and thus someone wrote the [Zod library](https://zod.dev/):

## What Is Zod?

Zod provides a declarative way to describe data structures and get compile-time types and validate them at runtime. Instead of writing lengthy manual validation functions, you create schemas that describe what valid data looks like.

## Creating Basic Schemas

Zod provides validators for all JavaScript primitives:

```ts
import { z } from "zod";

const stringSchema = z.string();
const numberSchema = z.number();
const booleanSchema = z.boolean();
```
## Object Schemas

For validating objects, create schemas with `z.object()`:

```ts
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
  email: z.string(),
});
```
## Schema Refinements

You can add constraints to make validation more specific:

```ts
const UserSchema = z.object({
  id: z.number().positive(), // must be positive
  name: z.string().min(1), // must be non-empty
  email: z.email(), // must be valid email string
});
```

# Schemas and Type Inference

Zod schemas can both validate runtime data and generate TypeScript types, giving you a single source of truth for your data structures.

## Parsing With Schemas

Use your schema's `parse()` method to validate data. It returns the validated data or throws a `ZodError`:

```ts
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
});

try {
  const user = UserSchema.parse(unknownData);
  // user is now typed and validated
  console.log(user.name); // TypeScript knows this is a string
} catch (error) {
  if (error instanceof z.ZodError) {
    console.error("Validation failed:", error.errors);
  }
}
```

A [`safeParse`](https://zod.dev/basics?id=handling-errors) method exists which returns a result type instead of throwing.

## z.infer

Zod can automatically generate TypeScript types from your schemas using `z.infer`:

```ts
type User = z.infer<typeof UserSchema>;
// User is: { id: number; name: string }
```

This means you define your data structure once in the schema, and get both:

- Runtime validation via `parse()`
- Compile-time TypeScript types via `z.infer`

## Single Source of Truth

You don't need to maintain two definitions with separate TypeScript interfaces and validation logic:

```ts
interface User {
  id: number;
  name: string;
}

function validateUser(data: any): User {
  // Manual validation logic...
}
```

Instead, you can have a single definition that provides both runtime validation and compile-time types:

```ts
const UserSchema = z.object({
  id: z.number(),
  name: z.string(),
});

type User = z.infer<typeof UserSchema>;
```

# Why Validate?

Zod's only one of many runtime validation libraries. Popular alternatives include:

- [Joi](https://joi.dev/)
- [Yup](https://github.com/jquense/yup)
- [class-validator](https://github.com/typestack/class-validator).

## The Problem

TypeScript provides compile-time type safety, but can't validate external data at runtime. Data from APIs, user input, or databases arrives as `any` and TypeScript can't guarantee its shape.

External APIs are particularly problematic - you don't control them, and documentation is often outdated.

```ts
const userData = await response.json(); // 'any'
console.log(userData.profile.name.toUpperCase()); // Crashes if profile is null
```

## The Solution

Runtime validation creates a boundary between untrusted external data and your typed code:

```ts
const userData = UserSchema.parse(await response.json());
console.log(userData.profile.name.toUpperCase()); // Guaranteed to work
```

When validation fails, you can handle it gracefully instead of crashing.

## Do You Need It?

Runtime validation might not be necessary if you control all the data or work on a small team with insight into all changes.

