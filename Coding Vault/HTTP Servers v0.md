CH1: Servers
# Welcome to Learn HTTP Servers

In a way, this course is where _everything_ you've learned so far on the Boot.dev back-end path comes together. Building HTTP servers is the bread-and-butter of a backend developer's day-to-day work.

This course assumes you already have a solid understanding of Go. If you don't, take a step back and take our [Go course](https://www.boot.dev/courses/learn-golang).

## Goals of This Course

- Understand what web servers are and how they power real-world web applications
- Build a production-style HTTP server in Go, without the use of a framework
- Use JSON, headers, and status codes to communicate with clients via a RESTful API
- Learn what makes Go a great language for building fast web servers
- Use type safe SQL to store and retrieve data from a Postgres database
- Implement a secure authentication/authorization system with well-tested cryptography libraries
- Build and understand webhooks and API keys
- Document the REST API with markdown

## What Is a Server?

A web [server](https://en.wikipedia.org/wiki/Server_%28computing%29) is just a computer that serves data over a network, typically the Internet. Servers run software that listens for incoming requests from clients. When a request is received, the server responds with the requested data.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/nUzP2kg-1280x720.png)

Any server worth its salt can handle _many_ requests at the same time. In Go, we use a new [goroutine](https://go.dev/tour/concurrency) for each request to handle them concurrently. Let's start by practicing with goroutines.

# Goroutines in Servers

In Go, _goroutines_ are used to serve _many_ requests at the same time, but not all servers are quite so performant.

Go was built by Google, and one of the purposes of its creation was to power Google's massive web infrastructure. Go's goroutines are a great fit for web servers because they're lighter weight than operating system threads, but still take advantage of multiple cores. Let's compare a Go web server's concurrency model to other popular languages and frameworks.

## Node.js / Express.js

In JavaScript land, servers are typically single-threaded. A [Node.js](https://nodejs.org/en/) server (often using the [Express](https://expressjs.com/) framework) only uses one CPU core at a time. It can still handle many requests at once by using an [async event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Event_loop). That just means whenever a request has to wait on I/O (like to a database), the server puts it on pause and does something else for a bit.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/bmsOkiQ-1280x530.png)

This might sound _horribly_ inefficient, but it's not _too_ bad. Node servers do just fine with the I/O workloads associated with most CRUD apps (Where processing is offloaded to the Database). You only start to run into trouble with this model when you need your server to do CPU-intensive work.

## Takeaways

- Go servers are great for performance whether the workload is I/O _or_ CPU-bound
- Node.js and Express work well for I/O-bound tasks, but struggle with CPU-bound tasks

I'm not saying Go is always "better" than JavaScript when it comes to back-end development, but it generally outperforms when it comes to computational speed.

# Server

We're building a fully-fledged web server from scratch _on your local machine_. The test suite will make HTTP requests to your local server over [localhost](https://www.hostinger.com/tutorials/what-is-localhost). Your server will run in one terminal, while you submit tests with the [Boot.dev CLI](https://github.com/bootdotdev/bootdev) in another terminal.

## Assignment

The Go standard library makes it easy to build a simple server. Your task is to build and run a server that binds to `localhost:8080` and always responds with a `404 Not Found` response.

### Steps

1. [ ] Create a [new http.ServeMux](https://pkg.go.dev/net/http#NewServeMux)
2. [ ] Create a new [http.Server](https://pkg.go.dev/net/http#Server) struct.
    - Use the new "ServeMux" as the server's handler
    - Set the `.Addr` field to ":8080"
3. [ ] Use the server's [ListenAndServe](https://pkg.go.dev/net/http#Server.ListenAndServe) method to start the server
4. [ ] Build and run your server (e.g. `go build -o out && ./out`)
5. [ ] Open `http://localhost:8080` in your browser. You should see a `404` error because we haven't connected any handler logic yet. Don't worry, that's what is expected for the tests to pass for now.

Being a programmer means reading documentation and searching for examples. This is an essential skill because a professional in any field should never stop learning. In fact, the best way to learn is [on the clock](https://www.boot.dev/expense). Check out the docs provided for examples and be sure to read the `http` package's [docs](https://pkg.go.dev/net/http) as a primer.

## Tips

- Use `go mod init` to create a Go module for your project
- Each time you change your code you'll need to rebuild and restart your server
- Use Git to save your work as you go

# Fileservers

A _fileserver_ is a kind of simple web server that serves static files from the host machine. Fileservers are often used to serve static assets for a website, things like:

- HTML
- CSS
- JavaScript
- Images

## Assignment

The Go standard library makes it super easy to build a simple fileserver. Build and run a fileserver that serves a file called `index.html` from its root at `http://localhost:8080`. That file should contain this HTML:

```html
<html>
  <body>
    <h1>Welcome to Chirpy</h1>
  </body>
</html>
```

## Steps

1. [ ] Add the HTML code above to a file called `index.html` in the same root directory as your server
2. [ ] Use the [http.NewServeMux](https://pkg.go.dev/net/http#NewServeMux)'s [.Handle()](https://pkg.go.dev/net/http#ServeMux.Handle) method to add a handler for the root path (`/`).
3. [ ] Use a standard [http.FileServer](https://pkg.go.dev/net/http#FileServer) as the handler
4. [ ] Use [http.Dir](https://pkg.go.dev/net/http#Dir) to convert a filepath (in our case a dot: `.` which indicates the current directory) to a directory for the `http.FileServer`.
5. [ ] Re-build and run your server
6. [ ] Test your server by visiting `http://localhost:8080` in your browser

What is http.Server?
	A struct that describes a server configuration
What happens when ListenAndServe() is called?
	The main function blocks until the server is shut down

# Serving Images

You may be wondering how the fileserver knew to serve the `index.html` file to the root of the server. It's _such_ a common convention on the web to use a file called `index.html` to serve the webpage for a given path, that the Go standard library's [FileServer](https://pkg.go.dev/net/http#FileServer) does it automatically.

When using a standard fileserver, the path to a file on disk is the same as its URL path. An exception is that `index.html` is served from `/` instead of `/index.html`.

## Try It Out

Run your chirpy server again, and open `http://localhost:8080/index.html` in a new browser tab. You'll notice that you're redirected to `http://localhost:8080/`.

This works for all directories, not just the root!

For example:

- `/index.html` will be served from `/`
- `/pages/index.html` will be served from `/pages`
- `/pages/about/index.html` will be served from `/pages/about`

Alternatively, try opening a URL that doesn't exist, like `http://localhost:8080/doesntexist.html`. You'll see that the fileserver returns a 404 error.

## Assignment

Let's serve another type of file from our server: an image. Chirpy has a slick logo, and we need to serve it so that our users can load it in their browsers and mobile apps.

Download the Chirpy logo from below and add it to your project directory.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/2CofkLc-256x256.png)

Configure its filepath so that it's accessible from this URL:

```
http://localhost:8080/assets/logo.png
```

# Workflow Tips

Servers are interesting because they're _always running._ A lot of the code we've written in Boot.dev up to this point has acted more like a command line tool: it runs, does its thing, and then exits.

Servers are different. They run forever, waiting for requests to come in, processing them, sending responses, and then waiting for the next request. If they didn't work this way, websites and apps would be down and unavailable _all the time_!

## Debugging a Server

Debugging a CLI app is simple:

1. Write some code.
2. Build and run the code.
3. See if it did what you expected.
4. If it didn't, add some logging or fix the code, and go back to step 2.

Debugging a server is a little different. The _simplest_ way (minimal tooling) is to:

1. Write some code.
2. Build and run the code.
3. _Send a request to the server using a browser or some other HTTP client._
4. See if it did what you expected.
5. If it didn't, add some logging or fix the code, and go back to step 2.

_Make sure you're testing your server by hitting endpoints in the browser before submitting your answers._

## Restarting a Server

I usually use a single command to build and run my servers, assuming I'm in my `main` package directory:

```bash
go run .
```

This builds the server and runs it in one command.

To stop the server, I use `ctrl+c`. This sends a signal to the server, telling it to stop. The server then exits.

To start it again, I just run the same command.

Alternatively, you can compile a binary and run it instead

```bash
go build -o out && ./out
```

## CLI Tip

If you didn't know, you can continuously press the up arrow key on the command line to see the commands you've previously run. That way you don't need to re-type commands that you use often!

# Custom Handlers

In the previous exercise, we used the [http.FileServer](https://pkg.go.dev/net/http#FileServer) function, which simply returns a built-in [http.Handler](https://pkg.go.dev/net/http#Handler).

An `http.Handler` is just an interface:

```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

Any type with a `ServeHTTP` method that matches the [http.HandlerFunc](https://pkg.go.dev/net/http#HandlerFunc) signature above is an `http.Handler`. Take a second to think about it: it makes a lot of sense! To handle an incoming HTTP request, all a function needs is a way to write a response and the request itself.

## Assignment

Let's add a readiness endpoint to the Chirpy server! Readiness endpoints are commonly used by external systems to check if our server is ready to receive traffic.

The endpoint should be accessible at the `/healthz` path using any HTTP method.

The endpoint should simply return a `200 OK` status code indicating that it has started up successfully and is listening for traffic. The endpoint should return a `Content-Type: text/plain; charset=utf-8` header, and the body will contain a message that simply says "OK" (the text associated with the 200 status code).

_Later this endpoint can be enhanced to return a `503 Service Unavailable` status code if the server is not ready._

### 1. Add the Readiness Endpoint

I recommend using the [mux.HandleFunc](https://pkg.go.dev/net/http#ServeMux.HandleFunc) to register your handler. Your handler can just be a function that matches the signature of [http.HandlerFunc](https://pkg.go.dev/net/http#HandlerFunc):

```go
func(http.ResponseWriter, *http.Request)
```

Your handler should do the following:

1. [ ] Write the `Content-Type` header
2. [ ] Write the status code using [w.WriteHeader](https://pkg.go.dev/net/http#ResponseWriter.WriteHeader)
3. [ ] Write the body text using [w.Write](https://pkg.go.dev/net/http#ResponseWriter.Write)

### 2. Update the Fileserver Path

Now that we've added a new handler, we don't want potential conflicts with the fileserver handler. Update the fileserver to use the `/app/` path instead of `/`.

Not only will you need to [mux.Handle](https://pkg.go.dev/net/http#ServeMux.Handle) the `/app/` path, you'll also need to strip the `/app` prefix from the request path before passing it to the fileserver handler. You can do this using the [http.StripPrefix](https://pkg.go.dev/net/http#StripPrefix) function.

# Handler Review

## Handler

An [http.Handler](https://pkg.go.dev/net/http#Handler) is any [defined type](https://go.dev/ref/spec#Type_definitions) that implements the set of methods defined by the `Handler` [interface](https://go.dev/tour/methods/9), specifically the `ServeHTTP` method.

```go
type Handler interface {
	ServeHTTP(ResponseWriter, *Request)
}
```

The [ServeMux](https://pkg.go.dev/net/http#ServeMux) you used in the previous exercise is an `http.Handler`.

You will typically use a `Handler` for more complex use cases, such as when you want to implement a custom router, middleware, or other custom logic.

## HandlerFunc

```go
type HandlerFunc func(ResponseWriter, *Request)
```

You'll typically use a `HandlerFunc` when you want to implement a simple handler. The `HandlerFunc` type is just a function that matches the `ServeHTTP` signature above.

## Why This Signature?

The `Request` argument is fairly obvious: it contains all the information about the incoming request, such as the HTTP method, path, headers, and body.

The `ResponseWriter` is less intuitive in my opinion. The response is an _argument_, not a _return type_. Instead of returning a value all at once from the handler function, we _write_ the response to the `ResponseWriter`.

Note: In Go, an interface is satisfied by any type that implements its required methods. Since you can attach methods to almost any type you define—not just structs—the following are all valid candidates to implement the `http.Handler` interface:

- **Structs**: The most common way to store data (like database pools) alongside methods.
- **Integers**: Useful if the handler's logic depends primarily on a numerical value.
- **Strings**: Useful if the handler simply needs to return a static message or ID.

As long as you use a **type definition** (e.g., `type MyType string`) and define the `ServeHTTP` method for it, that type becomes an `http.Handler`.

---

CH2: Routing

# Middleware

[Middleware](https://en.wikipedia.org/wiki/Middleware) is a way to wrap a handler with additional functionality. It is a common pattern in web applications that allows us to write DRY code.

For example, we can write a middleware that logs every request to the server. We can then wrap our handler with this middleware and every request will be logged without us having to write the logging code in every handler.

To do that, we can write the middleware function like this:

```go
func middlewareLog(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		log.Printf("%s %s", r.Method, r.URL.Path)
		next.ServeHTTP(w, r)
	})
}
```

Then, any handler that needs logging can be wrapped by this middleware function:

```go
mux.Handle("/app/", middlewareLog(handler))
```

# Stateful Handlers

It's frequently useful to have a way to store and access state in our handlers. For example, we might want to keep track of the number of requests we've received, or we may want to pass around an open connection to a database, or credentials to an API.

## Assignment

The product managers at Chirpy want to know how many requests are being made to serve our homepage - in essence, they want to know how many people are viewing the site!

They have asked for a simple HTTP endpoint they can hit to get the number of requests that have been processed. It will return the count as plain text in the response body.

For now, they just want the number of requests that have been processed since the last time the server was started, we don't need to worry about saving the data between restarts.

## Steps

1. [ ] Create a struct in `main.go` that will hold any stateful, in-memory data we'll need to keep track of. In our case, we just need to keep track of the number of requests we've received.

```go
type apiConfig struct {
	fileserverHits atomic.Int32
}
```

The [`atomic.Int32`](https://pkg.go.dev/sync/atomic#Int32) type is a really cool standard-library type that allows us to safely increment and read an integer value across multiple goroutines (HTTP requests).

2. [ ] Next, write a new middleware method on a `*apiConfig` that increments the `fileserverHits` counter every time it's called. Here's the method signature I used:

```go
func (cfg *apiConfig) middlewareMetricsInc(next http.Handler) http.Handler {
	// ...
}
```

The `atomic.Int32` type has an [`.Add()`](https://pkg.go.dev/sync/atomic#Int32.Add) method, use it to safely increment the number of `fileserverHits`.

3. [ ] [Wrap](https://en.wikipedia.org/wiki/Wrapper_function) the `http.FileServer` handler with the middleware method we just wrote. For example:

```go
mux.Handle("/app/", apiCfg.middlewareMetricsInc(handler))
```

4. [ ] Create a new handler that writes the number of requests that have been counted as plain text in this format to the HTTP response:

```
Hits: x
```

Where `x` is the number of requests that have been processed. This handler should be a method on the `*apiConfig` struct so that it can access the `fileserverHits` data.

5. [ ] Register that handler with the serve mux on the `/metrics` path.
6. [ ] Finally, create and register a handler on the `/reset` path that, when hit, will reset your `fileserverHits` back to `0`.

_It should follow the same design as the previous handlers._

Remember, similar to the metrics endpoint, `/reset` will need to be a method on the `*apiConfig` struct so that it can also access the `fileserverHits`

# Routing

The Go standard library has a lot of powerful HTTP features and, as of version 1.22, comes equipped with method-based pattern matching for routing.

In this lesson, we are going to limit which endpoints are available via which HTTP methods. In our current implementation, we can use any HTTP method to access any endpoint. _This is not ideal._

There are powerful routing libraries like [Gorilla Mux](https://github.com/gorilla/mux) and [Chi](https://github.com/go-chi/chi), however, the course will assume you are using Go's standard library. Just know that it isn't your only option!

## Try It!

Run this command to send an empty `POST` request to your running server:

```bash
curl -X POST http://localhost:8080/healthz
```

You should get an `OK` response - but we want this endpoint to only be available via `GET` requests!

## Method Specific Routing

Using the Go standard library, you can specify a method like this: `[METHOD ][HOST]/[PATH]`. For example:

```go
mux.HandleFunc("POST /articles", handlerArticlesCreate)
mux.HandleFunc("DELETE /articles", handlerArticlesDelete)
```

## Assignment

1. [ ] Update the following paths to only accept `GET` requests:
    - [ ] `/healthz`
    - [ ] `/metrics`

When a request is made to one of these endpoints with a method other than `GET`, the server should return a `405` (Method Not Allowed) response (this is handled automatically!).

2. [ ] Update the `/reset` endpoint to only accept `POST` requests.

# Patterns

A pattern is a string that specifies the set of URL paths that should be matched to handle HTTP requests. Go's `ServeMux` router uses these patterns to dispatch requests to the appropriate handler functions based on the URL path of the request. As we saw in the previous lesson, patterns help organize the handling of different routes efficiently.

As previously mentioned, patterns generally look like this: `[METHOD ][HOST]/[PATH]`. Note that all three parts are optional.

## Rules and Definitions

### Fixed URL Paths

A pattern that exactly matches the URL path. For example, if you have a pattern `/about`, it will match the URL path `/about` and no other paths.

### Subtree Paths

If a pattern ends with a slash `/`, it matches all URL paths that have the same prefix. For example, a pattern `/images/` matches `/images/`, `/images/logo.png`, and `/images/css/style.css`. As we saw with our `/app/` path, this is useful for serving a directory of static files or for structuring your application into sub-sections.

### Longest Match Wins

If more than one pattern matches a request path, the longest match is chosen. This allows more specific handlers to override more general ones. For example, if you have patterns `/` (root) and `/images/`, and the request path is `/images/logo.png`, the `/images/` handler will be used because it's the longest match.

### Host-Specific Patterns

We won't be using this but be aware that patterns can also start with a hostname (e.g., `www.example.com/`). This allows you to serve different content based on the Host header of the request. If both host-specific and non-host-specific patterns match, the host-specific pattern takes precedence.

If you're interested, you can read more in the [ServeMux docs](https://pkg.go.dev/net/http#ServeMux).

---

CH3: Architecture

# Monoliths and Decoupling

"Architecture" in software can mean _many_ different things, but in this lesson, we're talking about the high-level architecture of a web application from a structural standpoint. More specifically, we are concerned with the separation (or lack thereof) between the back-end and the front-end.

When we talk about "coupling" in this context, we're talking about the coupling between the _data_ and the _presentation logic_ of that data. Loosely speaking, when I say "a tightly coupled front-end and back-end", what I mean is:

### Front-End: the Presentation Logic

If it's a web app, then this is the HTML, CSS, and JavaScript that is served to the browser which will then be used to render any dynamic data. If it's a mobile app, then this is the compiled code that is downloaded on the mobile device.

### Back-End: Raw Data

For an app like YouTube, this would be videos and comments. For an app like Twitter, this might be tweets and users data. You can't embed the YouTube videos directly into the Youtube app, because a user's feed changes each time they open the app. The app downloads new raw data from Google's back-end each time the app is opened.

## Monolithic

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/8KTaJ9v-1280x720.png)

A monolith is a single, large program that contains all of the functionality for both the front-end and the back-end of an application. It's a common architecture for web applications, and it's what we're building here in this course.

Sometimes monoliths host a REST API for raw data (like JSON data) within a subpath, like `/api` as shown in the image. That said, there are even more tightly coupled kinds of monoliths that inject the dynamic data directly into the HTML as well. The nice thing about separate data endpoints is that they can be consumed by any client, (like a mobile app) and not just the website. That said, injection is typically more performant, so it's a trade-off. WordPress and other web_site_ builders typically work this way.

## Decoupled

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/VDJk0zU-1280x720.png)

A "decoupled" architecture is one where the front-end and back-end are separated into different codebases. For example, the front-end might be hosted by a static file server on one domain, and the back-end might be hosted on a subdomain by a different server.

Depending on whether or not a load balancer is sitting in front of a decoupled architecture, the API server might be hosted on a separate domain (as shown in the image) _or_ on a subpath, as shown in the monolithic architecture. A decoupled architecture allows for either approach.

# Which Is Better?

There is _always_ a trade-off.

## Pros for Monoliths

- Simpler to get started with
- Easier to deploy new versions because everything is always in sync
- In the case of the data being embedded in the HTML, the performance can result in better UX and SEO

## Pros for Decoupled Architectures

- Easier, and probably cheaper, to scale as traffic grows
- Easier to practice good separation of concerns as the codebase grows
- Can be hosted on separate servers and using separate technologies
- Embedding data in the HTML is still possible with pre-rendering (similar to how Next.js works), it's just more complicated

## Can We Have the Best of Both Worlds?

Perhaps. My recommendation to someone building a new application from scratch would be to start with a monolith, but to keep the API and the front-end decoupled logically within the project from the start (like we're doing with Chirpy).

That way, our app is easy to get started with, but we can migrate to a fully decoupled architecture later if we need to.

# Admin Namespace

Let's add an "admin" namespace. This is where we'll put endpoints intended for internal administrative use. Note: there's nothing inherently more secure about this namespace, it's just an organizational structure.

## Assignment

1. [ ] Swap out the `GET /api/metrics` endpoint, which just returns plain text, for a `GET /admin/metrics` that returns HTML to be rendered in the browser. Use this template with [`fmt.Sprintf()`](https://pkg.go.dev/fmt#Sprintf):

```html
<html>
  <body>
    <h1>Welcome, Chirpy Admin</h1>
    <p>Chirpy has been visited %d times!</p>
  </body>
</html>
```

Where `%d` is replaced with the number of times the page has been loaded.

- [ ] Make sure you use the `Content-Type` header to set the response type to `text/html` so that the browser knows how to render it.
- [ ] Try loading `http://localhost:8080/admin/metrics` in your browser, and in another tab load `http://localhost:8080/app` a few times. Refreshing the admin page should show the updated count.

2. [ ] Update the `POST /api/reset` to `POST /admin/reset`. Its functionality should not change.

# Deployment Options

We won't go in-depth with deployment instructions right now; that said, let's talk about how our choice of project architecture affects our deployment options, and how we _could_ deploy our application in the future. We'll only talk about cloud deployment options here, and by the "cloud" I'm just referring to a remote server that's managed by a third-party company like Google or Amazon.

![](https://imgs.xkcd.com/comics/the_cloud.png)

- [xkcd](https://xkcd.com/908/)

Using a cloud service to deploy applications is _super_ common these days because it's easy, fast, and cheap.

That said, it's still possible to deploy to a local or on-premise server, and some companies still do that, but it's not as common as it used to be.

## Monolithic Deployment

Deploying a monolith is straightforward. Because your server is just one program, you just need to get it running on a server that's exposed to the internet and point your DNS records to it.

You could upload and run it on classic server, something like:

- AWS EC2
- GCP Compute Engine (GCE)
- Digital Ocean Droplets
- Azure Virtual Machines

Alternatively, you could use a platform that's specifically designed to run web applications, like:

- Heroku
- Google App Engine
- Fly.io
- AWS Elastic Beanstalk

## Decoupled Deployment

With a decoupled architecture, you have _two_ different programs that need to be deployed. You would typically deploy your _back-end_ to the same kinds of places you would deploy a monolith.

For your front-end server, you can do the same, _or_ you can use a platform that's specifically designed to host static files and server-side rendered front-end apps, something like:

- Vercel
- Netlify
- GitHub Pages

Because the front-end bundle is likely just static files, you can host it easily on a [CDN (Content Delivery Network)](https://www.cloudflare.com/learning/cdn/what-is-a-cdn/) inexpensively.

## More Powerful Options

If you want to be able to scale your application up and down in specific ways, or you want to add other back-end servers to your stack, you might want to look into container orchestration options like Kubernetes and Docker Swarm.

## Don't Worry About All This Stuff!

I'm trying to gently introduce you to some popular technologies and how they work together, but you don't need to memorize all of these products and options.

---

CH4: JSON

# HTTP Clients

So far, you have _probably_ been using a browser to test your server. That works fine with simple `GET` requests (the kind of request a browser sends when you type a URL into the address bar), but it's not very useful for any other HTTP methods or requests with custom headers and bodies.

## Debugging Your Endpoints

Servers are built to be used by clients. As you develop your code, you should be using a tool that makes sending one-off requests to your server easy! Here are some of my favorites:

- [REST Client for VS Code](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)
- [Postman for VS Code](https://marketplace.visualstudio.com/items?itemName=Postman.postman-for-vscode)
- [cURL](https://curl.se/)
- [Postman](https://www.postman.com/)
- [Bruno](https://www.usebruno.com/)

Use whichever client you like, _but make sure you're using one!_

# JSON

Hopefully, by now you already know what JSON is. If not, you should go back and take the Learn HTTP Clients course [here first](https://www.boot.dev/courses/learn-http-clients-golang).

What you may be new to is handling and parsing JSON on the server side, rather than sending it as a client.

If you want to take a super deep dive into JSON in Go, then you can [read this post here](https://www.boot.dev/blog/golang/json-golang/). With that in mind, you don't need to! I'll give you the relevant info below.

## Decode JSON Request Body

It's _very_ common for `POST` requests to send JSON data in the request body. Here's how you can handle that incoming data:

```json
{
  "name": "John",
  "age": 30
}
```

```go
func handler(w http.ResponseWriter, r *http.Request){
    type parameters struct {
        Name string `json:"name"`
        Age int `json:"age"`
    }

    decoder := json.NewDecoder(r.Body)
    params := parameters{}
    err := decoder.Decode(&params)
    if err != nil {
		log.Printf("Error decoding parameters: %s", err)
		w.WriteHeader(500)
		return
    }
    // params is a struct with data populated successfully
    // ...
}
```

The struct tags (e.g., `` `json:"name"` ``) indicate how the keys in the JSON should be mapped to the struct fields. The struct fields themselves must be exported (start with a capital letter) if you want them parsed.

`decoder.Decode()` will return an error if the JSON is invalid or has the wrong types, and any missing fields will simply have their values in the struct set to their zero value.

## Encode JSON Response Body

```go
func handler(w http.ResponseWriter, r *http.Request){
    // ...

    type returnVals struct {
        CreatedAt time.Time `json:"created_at"`
        ID int `json:"id"`
    }
    respBody := returnVals{
        CreatedAt: time.Now(),
        ID: 123,
    }
    dat, err := json.Marshal(respBody)
	if err != nil {
			log.Printf("Error marshalling JSON: %s", err)
			w.WriteHeader(500)
			return
	}
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(200)
    w.Write(dat)
}
```

Again, we use struct tags to specify how the field names will be encoded in the JSON data. If you omit the tags, the keys will be encoded as the same names of struct fields (e.g., `CreatedAt`, `ID`).

## Assignment

At Chirpy, we have a silly rule that says all Chirps must be 140 characters long or less.

Add a new endpoint to the Chirpy API that accepts a `POST` request at `/api/validate_chirp`. It should expect a JSON body of this shape:

```json
{
  "body": "This is an opinion I need to share with the world"
}
```

If any errors occur, it should respond with an appropriate HTTP status code and a JSON body of this shape:

```json
{
  "error": "Something went wrong"
}
```

`"Something went wrong"` is just an example of the response shape. For example, if the Chirp is too long, respond with a `400` code and this body:

```json
{
  "error": "Chirp is too long"
}
```

If the Chirp is valid, respond with a `200` code and this body:

```json
{
  "valid": true
}
```

The JSON here has been prettified. The actual response body won't have newlines or spaces between the key and value. They'll look more like `{"valid":true}`

## Tips

Use an HTTP client to test your POST requests.

Use [json.Marshal()](https://pkg.go.dev/encoding/json#Marshal) like the example above to remove whitespace in your encoded data.

I'd recommend creating two helper functions:

- `respondWithError(w http.ResponseWriter, code int, msg string)`
- `respondWithJSON(w http.ResponseWriter, code int, payload interface{})`

These helpers are not _required_ but might help [DRY](https://www.boot.dev/blog/computer-science/dry-code/) up your code when we add more endpoints in the future. If you want `respondWithError` to also log the underlying error, it's fine for that helper to grow an extra `err` parameter later.

# The Profane

Not only do we validate that Chirps are under 140 characters, but we also have a list of words that are not allowed.

## Assignment

We need to update the `/api/validate_chirp` endpoint to replace all "profane" words with `4` asterisks: `****`.

Assuming the length validation passed, replace any of the following words in the Chirp with the static 4-character string `****`:

- kerfuffle
- sharbert
- fornax

Be sure to match against uppercase versions of the words as well, but not punctuation. "Sharbert!" does _not_ need to be replaced, we'll consider it a different word due to the exclamation point. Finally, instead of the `valid` boolean, your handler should return the cleaned version of the text in a JSON response:

### Example Input

```json
{
  "body": "This is a kerfuffle opinion I need to share with the world"
}
```

### Example Output

```json
{
  "cleaned_body": "This is a **** opinion I need to share with the world"
}
```

**Run and submit** the CLI tests.

## Tips

_Use an HTTP client to test your POST requests._

I'd recommend breaking the bad word replacement into a separate function. You can even write some unit tests for it!

Here are some useful standard library functions:

- [strings.ToLower](https://pkg.go.dev/strings#ToLower)
- [strings.Split](https://pkg.go.dev/strings#Split)
- [strings.Join](https://pkg.go.dev/strings#Join)

---

CH5: Storage

# Storage

Arguably the most important part of your typical web application is the storage of data. It would be pretty useless if each time you logged into your account on YouTube, Twitter or GitHub, all of your subscriptions, tweets, or repositories were gone.

## Memory vs. Disk

When you run a program on your computer (like our HTTP server), the program is loaded into _memory_. Memory is a lot like a scratch pad. It's fast, but it's not permanent. If the program terminates or restarts, the data in memory is _lost_.

When you're building a web server, any data you store in memory (in your program's variables) is lost when the server is restarted. Any important data needs to be saved to disk via the file system.

## Option 1: Raw Files

We _could_ take our user's data, serialize it to JSON, and save it to disk in `.json` files (or any other format for that matter). It's simple, and will even work for small applications. Trouble is, it will run into problems _fast_:

- **Concurrency**: If two requests try to write to the same file at the same time, you'll get overwritten data.
- **Scalability**: It's not efficient to read and write large files to disk for every request.
- **Complexity**: You'll have to write a lot of code to manage the files, and the chances of bugs are high.

## Option 2: a Database

At the end of the day, a database technology like MySQL, PostgreSQL, or MongoDB "just" writes files to disk. The difference is that they _also_ come with all the fancy code and algorithms that make managing those files efficient and safe. In the case of a SQL database, the files are abstracted away from us entirely. You just write SQL queries and let the DB handle the rest.

**We will be using option 2: [PostgreSQL](https://www.postgresql.org/).** It's a production-ready, open-source SQL database. It's a great choice for many web applications, and as a back-end engineer, it might be the single most important database to be familiar with.

## Assignment

1. [ ] Install Postgres v15 or later.

**macOS** with [brew](https://brew.sh/)

```bash
brew install postgresql@15
```

**Linux / WSL (Debian).** Here are the [docs from Microsoft](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-database#install-postgresql), but simply:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

2. [ ] Ensure the installation worked. The `psql` command-line utility is the default client for Postgres. Use it to make sure you're on version 15+ of Postgres:

```bash
psql --version
```

3. [ ] (Linux only) Update postgres password:

```bash
sudo passwd postgres
```

Enter a password, and be sure you won't forget it. You can just use something easy like `postgres`.

4. [ ] Start the Postgres server in the background
    - Mac: `brew services start postgresql@15`
    - Linux: `sudo service postgresql start`
5. [ ] Connect to the server. I recommend simply using the `psql` client. It's the "default" client for Postgres, and it's a great way to interact with the database. While it's not as user-friendly as a GUI like [PGAdmin](https://www.pgadmin.org/), it's a great tool to be able to do at least basic operations with.

Enter the `psql` shell:

- Mac: `psql postgres`
- Linux: `sudo -u postgres psql`

You should see a new prompt that looks like this:

```bash
postgres=#
```

6. [ ] Create a new database. I called mine `chirpy`:

```sql
CREATE DATABASE chirpy;
```

7. [ ] Connect to the new database:

```sql
\c chirpy
```

You should see a new prompt that looks like this:

```bash
chirpy=#
```

8. [ ] Set the user password (Linux only)

```sql
ALTER USER postgres WITH PASSWORD 'postgres';
```

For simplicity, I used `postgres` as the password. Before, we altered the _system_ user's password, now we're altering the _database_ user's password.

9. [ ] Query the database

From here you can run SQL queries against the `chirpy` database. For example, to see the version of Postgres you're running, you can run:

```sql
SELECT version();
```

# Goose Migrations

[Goose](https://github.com/pressly/goose) is a database migration tool written in Go. It runs migrations from a set of SQL files, making it a perfect fit for this project (we wanna stay close to the raw SQL).

## What Is a Migration?

A migration is just a set of changes to your database table. You can have as many migrations as needed as your requirements change over time. For example, one migration might create a new table, one might delete a column, and one might add 2 new columns.

An "up" migration moves the state of the database from its current schema to the schema that you want. So, to get a "blank" database to the state it needs to be ready to run your application, you run all the "up" migrations.

If something breaks, you can run one of the "down" migrations to revert the database to a previous state. "Down" migrations are also used if you need to reset a local testing database to a known state.

## Users

Our API needs to support the standard CRUD operations for "users" - the people logging into and using our application.

## Assignment

1. [ ] Install Goose.

Goose is just a command line tool that happens to be written in Go. I recommend [installing](https://github.com/pressly/goose#install) it using `go install`:

```bash
go install github.com/pressly/goose/v3/cmd/goose@latest
```

Run `goose -version` to make sure it's installed correctly.

2. [ ] Create a `users` migration in a new `sql/schema` directory.

A "migration" in Goose is just a `.sql` file with some SQL queries and some special comments. Our first migration should just create a `users` table. The simplest format for these files is:

```
number_name.sql
```

For example, I created a file in `sql/schema` called `001_users.sql` with the following contents:

```sql
-- +goose Up
CREATE TABLE ...

-- +goose Down
DROP TABLE users;
```

Write out the `CREATE TABLE` statement in full, I left it blank for you to fill in. A `user` should have 4 fields:

- `id`: a [`UUID`](https://www.boot.dev/blog/backend/what-are-uuids-and-should-you-use-them/) that will serve as the _primary key_
- `created_at`: a `TIMESTAMP` that can _not be null_
- `updated_at`: a `TIMESTAMP` that can _not be null_
- `email`: `TEXT` that can _not be null_ and must be _unique_

The `-- +goose Up` and `-- +goose Down` comments are required. They tell Goose how to run the migration in each direction.

3. [ ] Get your connection string. A connection string is just a URL with all of the information needed to connect to a database. The format is:

```
protocol://username:password@host:port/database
```

Here are examples:

- macOS (no password, your username): `postgres://wagslane:@localhost:5432/chirpy`
- Linux (password from last lesson, postgres user): `postgres://postgres:postgres@localhost:5432/chirpy`

Test your connection string by running `psql`, for example:

```bash
psql "postgres://wagslane:@localhost:5432/chirpy"
```

It should connect you to the `chirpy` database directly. If it's working, great. `exit` out of `psql` and save the connection string.

4. [ ] Run the up migration.

`cd` into the `sql/schema` directory and run:

```bash
goose postgres <connection_string> up

# example:
# goose postgres "postgres://wagslane:@localhost:5432/chirpy" up
```

Run your migration! Make sure it works by using `psql` with your connection string to find your newly created `users` table:

```bash
psql "<connection_string>"
\dt
```

5. [ ] Run the `down` migration to make sure it works (it should just drop the table).
6. [ ] When you're satisfied, run the up migration again to recreate the table.

# SQLC

[SQLC](https://sqlc.dev/) is an _amazing_ Go program that generates Go code from SQL queries. It's not exactly an [ORM](https://www.freecodecamp.org/news/what-is-an-orm-the-meaning-of-object-relational-mapping-database-tools/), but rather a tool that makes working with raw SQL easy and type-safe.

We will be using Goose to manage our database migrations (the schema). We'll be using SQLC to generate Go code that our application can use to interact with the database (run queries).

## Assignment

1. [ ] Install SQLC.

SQLC is just a command line tool, it's not a package that we need to import. I recommend [installing](https://docs.sqlc.dev/en/latest/overview/install.html) it using `go install`. Installing Go CLI tools with `go install` is easy and ensures compatibility with your Go environment.

```bash
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest
```

Then run `sqlc version` to make sure it's installed correctly.

2. [ ] Configure [SQLC](https://docs.sqlc.dev/en/latest/tutorials/getting-started-postgresql.html). You'll always run the `sqlc` command from the root of your project. Create a file called `sqlc.yaml` in the root of your project. Here is mine:

```yaml
version: "2"
sql:
  - schema: "sql/schema"
    queries: "sql/queries"
    engine: "postgresql"
    gen:
      go:
        out: "internal/database"
```

We're telling SQLC to look in the `sql/schema` directory for our schema structure (which is the same set of files that Goose uses, but `sqlc` automatically ignores "down" migrations), and in the `sql/queries` directory for queries. We're also telling it to generate Go code in the `internal/database` directory.

3. [ ] Write a query to create a user. Inside the `sql/queries` directory, create a file called `users.sql`. Here's the format:

```sql
-- name: CreateUser :one
INSERT INTO users (id, created_at, updated_at, email)
VALUES (
    ...
)
RETURNING *;
```

- We'll be using UUIDs for ID values, so you can use [`gen_random_uuid()`](https://www.postgresql.org/docs/current/functions-uuid.html) to generate a new UUID.
- The `created_at` and `updated_at` fields should be set to the current timestamp. In Postgres, you can use `NOW()` to get the current timestamp.
- The `email` should be passed in by our application. Use `$1` to represent the first parameter passed into the query. (in future queries, we'll use `$2`, `$3`, etc. for additional parameters)

The `:one` at the end of the query name tells SQLC that we expect to get back a single row (the created user).

Keep the [SQLC postgres docs](https://docs.sqlc.dev/en/latest/tutorials/getting-started-postgresql.html) handy, you'll probably need to refer to them again later.

4. [ ] Generate the Go code. Run `sqlc generate` from the root of your project. It should create a new package of go code in `internal/database`. You'll notice that the generated code relies on Google's [uuid](https://pkg.go.dev/github.com/google/uuid) package, so you'll need to add that to your module:

```bash
go get github.com/google/uuid
```

5. [ ] Import a PostgreSQL driver.

We need to add and import a [Postgres driver](https://github.com/lib/pq) so our program knows how to talk to the database. Install it in your module:

```bash
go get github.com/lib/pq
```

Add this import to the top of your `main.go` file:

```go
import _ "github.com/lib/pq"
```

This is one of my least favorite things working with SQL in Go currently. You have to import the driver, but you don't use it directly anywhere in your code. The underscore tells Go that you're importing it for its side effects, not because you need to use it.

6. [ ] Create a [`.env`](https://dotenvx.com/docs/env-file) file in the root of your project:

```
DB_URL="YOUR_CONNECTION_STRING_HERE"
```

Add it to your `.gitignore` file. It's _incredibly insecure_ to commit secret keys to a Git repo.

You would never use a plain text `.env` file in a production environment, but for local development of a personal project, you're fine.

Add a query parameter to the end of the connection string to disable SSL, e.g. `postgres://wagslane:@localhost:5432/chirpy?sslmode=disable`.

7. [ ] `go get github.com/joho/godotenv`, then call `godotenv.Load()` at the beginning of your `main()` function to load the `.env` file into your environment variables. Then you can use [`os.Getenv`](https://pkg.go.dev/os#Getenv) to get the `DB_URL` from the environment:

```go
dbURL := os.Getenv("DB_URL")
```

8. [ ] Next, [sql.Open()](https://pkg.go.dev/database/sql#Open) a connection to your database:

```go
db, err := sql.Open("postgres", dbURL)
```

Make sure all packages used are imported at the top.

Use your SQLC generated `database` package to create a new `*database.Queries`, and store it in your `apiConfig` struct so that handlers can access it:

```go
dbQueries := database.New(db)
```

# Database Review

It's very standard to use database software to store web server data on disk. Sometimes that database runs on the same host machine as your server (like we're doing on your local machine), but it's also common to have a separate database server that your server connects to over the network.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/tHmqS6O-1280x720.png)

That's why we use a connection URL: it can point to a local database or a remote one.

## Popular Databases

You don't _need_ to know about all of these, but you might be curious about some of the database technologies out there. Here are a few of the most popular ones:

- [PostgreSQL](https://www.postgresql.org/): A fantastic open-source SQL database.
- [MySQL](https://www.mysql.com/): Another open-source SQL database. Less fantastic IMO.
- [MongoDB](https://www.mongodb.com/): A popular open-source NoSQL document database.
- [Firebase](https://firebase.google.com/): A popular cloud-based NoSQL database service.
- [SQLite](https://www.sqlite.org/index.html): A popular embedded SQL database.

Feel free to browse [DB Engine](https://db-engines.com/en/ranking) if you want to dive deeper into the world of database technologies.

# Create User

We've written the SQL query, now it's time to write the API handler that will allow users to create a new user.

## The Context Package

The `context` package is a part of Go's standard library. It does several things, but the most important thing is that it handles timeouts. All of SQLC's database queries accept a [`context.Context`](https://pkg.go.dev/context#Context) as their first argument:

```go
user, err := cfg.db.CreateUser(r.Context(), params.Email)
```

By passing your handler's [`http.Request.Context()`](https://pkg.go.dev/net/http#Request.Context) to the query, the library will automatically cancel the database query if the HTTP request is canceled or times out.

The benefit is that it will save your server from getting bogged down by long-running queries!

## Assignment

1. [ ] Add a new endpoint to your server `POST /api/users` that allows users to be created. It accepts an `email` as JSON in the request body and returns the user's ID, email, and timestamps in the response body.

**Request**:

```json
{
  "email": "user@example.com"
}
```

**Response**:

`HTTP 201 Created`

```json
{
  "id": "50746277-23c6-4d85-a890-564c0044c2fb",
  "created_at": "2021-07-07T00:00:00Z",
  "updated_at": "2021-07-07T00:00:00Z",
  "email": "user@example.com"
}
```

2. [ ] Update the `POST /admin/reset` endpoint to delete all users in the database (but don't mess with the schema). You'll need a new SQLC query for this. Add a new value to your `.env` file called `PLATFORM` and set it equal to "dev". Read it into your `apiConfig`. If `PLATFORM` is not equal to "dev", this endpoint should return a `403 Forbidden`. This ensures that this extremely dangerous endpoint can only be accessed in a local development environment.

**Run and submit** the CLI tests.

## Tips

I created a `User` struct in my `main` package. When the `database` package returns a `database.User`, I map it to my `main` package's `User` struct before marshalling it to JSON so that I can control the JSON keys:

```go
type User struct {
	ID        uuid.UUID `json:"id"`
	CreatedAt time.Time `json:"created_at"`
	UpdatedAt time.Time `json:"updated_at"`
	Email     string    `json:"email"`
}
```

Alternatively, you can use the ['emit_json_tags' configuration option ](https://docs.sqlc.dev/en/stable/reference/config.html#go) to automatically include the JSON tags. However, in larger projects, this may be more restrictive than useful.

# Create Chirp

Our API needs to support standard CRUD operations for "chirps". A "chirp" is just a short message that a user can post to the API, like a tweet.

## Assignment

1. [ ] Add a `POST /api/chirps` handler. It accepts a JSON payload with a `body` field:

```json
{
  "body": "Hello, world!",
  "user_id": "123e4567-e89b-12d3-a456-426614174000"
}
```

Delete the `/api/validate_chirp` endpoint that we created before, but port all that logic into this one. Users should not be allowed to create invalid chirps!

2. [ ] If the chirp is valid, you should save it in the database with:
    - A new random `id`: A UUID
    - `created_at`: A non-null timestamp
    - `updated_at`: A non null timestamp
    - `body`: A non-null string
    - `user_id`: This should [reference](https://en.wikipedia.org/wiki/Foreign_key) the `id` of the user who created the chirp, and [`ON DELETE CASCADE`](https://www.postgresql.org/docs/9.2/ddl-constraints.html), which will cause a user's chirps to be deleted if the user is deleted.

You'll need a new up/down migration for this table.

As a general rule it's always a good idea to use `created_at` and `updated_at` timestamps for all your resources. It gives you a nice audit trail and makes it easier to debug issues.

3. [ ] If creating the record goes well, respond with a `201` status code and the full chirp resource:

```json
{
  "id": "94b7e44c-3604-42e3-bef7-ebfcc3efff8f",
  "created_at": "2021-01-01T00:00:00Z",
  "updated_at": "2021-01-01T00:00:00Z",
  "body": "Hello, world!",
  "user_id": "123e4567-e89b-12d3-a456-426614174000"
}
```

_Yes, this isn't secure because it means any user can create a chirp on behalf of any other user. We'll fix that in a future assignment._

# Collections and Singletons

We're building a _fairly_ [RESTful API](https://restfulapi.net/).

REST is a set of guidelines for how to build APIs. It's not a standard, but it's a set of conventions that many people follow. Not all back-end APIs are RESTful, but many are. As a back-end developer, you'll need to know how to build RESTful APIs.

## Collections and Singletons

In REST, it's conventional to name all of your endpoints after the resource that they represent and for the name to be plural. That's why we use `POST /api/chirps` to create a new chirp instead of `POST /api/chirp`.

To get a collection of resources it's conventional to use a `GET` request to the plural name of the resource. So we are going to use `GET /api/chirps` to get all of the chirps.

To get a _singleton_, or a _single instance_ of a resource, it's conventional to use a `GET` request to the plural name of the resource, followed by the `ID` of the resource. So we are going to use `GET /api/chirps/94b7e44c-3604-42e3-bef7-ebfcc3efff8f` to get the chirp with ID `94b7e44c-3604-42e3-bef7-ebfcc3efff8f`.

# Get All Chirps

We need a way to retrieve _all_ the chirps from the database. Later, we'll add sorting and filtering functionality, but you can think of this as a very basic version of an endpoint that might serve a timeline of chirps.

## Assignment

1. [ ] Add a new query that retrieves all chirps in ascending order by `created_at`.
2. [ ] Add a `GET /api/chirps` endpoint that returns all chirps in the database. It should return them in the same structure as the `POST /api/chirps` endpoint, but as an array. Use a `200` status code for success. Order them by `created_at` in ascending order.

```json
[
  {
    "id": "94b7e44c-3604-42e3-bef7-ebfcc3efff8f",
    "created_at": "2021-01-01T00:00:00Z",
    "updated_at": "2021-01-01T00:00:00Z",
    "body": "Yo fam this feast is lit ong",
    "user_id": "123e4567-e89b-12d3-a456-426614174000"
  },
  {
    "id": "f0f87ec2-a8b5-48cc-b66a-a85ce7c7b862",
    "created_at": "2022-01-01T00:00:00Z",
    "updated_at": "2023-01-01T00:00:00Z",
    "body": "What's good king?",
    "user_id": "123e4567-e89b-12d3-a456-426614174000"
  }
]
```

# Get Chirp

Now we need a way to lookup a single chirp by its ID. You might be thinking:

> "If I can get all of the chirps, why do I need a way to get just one?"

Imagine there are 10,000 chirps in the database - no, imagine 10,000,000,000! We'll obviously need to change our `GET /api/chirps` endpoint to only return a subset of chirps at a time.

However, our users will still need a way to view a single chirp - for example, maybe they have a link directly to it.

## Assignment

1. [ ] Add a `GET /api/chirps/{chirpID}` endpoint that returns a single chirp by its ID. The chirp ID will be passed in as a path parameter. For example:

```
GET /api/chirps/94b7e44c-3604-42e3-bef7-ebfcc3efff8f
```

You can get the string value of the path parameter like in Go with the [`http.Request.PathValue`](https://pkg.go.dev/net/http#Request.PathValue) method.

2. [ ] If the chirp is found, return it like so with a `200` code:

```json
{
  "id": "94b7e44c-3604-42e3-bef7-ebfcc3efff8f",
  "created_at": "2021-01-01T00:00:00Z",
  "updated_at": "2021-01-01T00:00:00Z",
  "body": "fr? no clowning?",
  "user_id": "123e4567-e89b-12d3-a456-426614174000"
}
```

3. [ ] Otherwise, return a `404`.

---

CH6: Authentication

# Authentication With Passwords

Authentication is the process of verifying _who_ a user is. If you don't have a secure authentication system, your back-end systems will be open to attack!

Imagine if I could make an HTTP request to the YouTube API and upload a video to _your_ channel. YouTube's authentication system prevents this from happening by verifying that I am who I say I am.

## Passwords

Passwords are a common way to authenticate users. You know how they work: When a user signs up for a new account, they choose a password. When they log in, they enter their password again. The server will then compare the password they entered with the password that was stored in the database.

There are 2 _really important_ things to consider when storing passwords:

1. **Storing passwords in plain text is awful.** If someone gets access to your database, they will be able to see all of your users' passwords. If you store passwords in plain text, you are giving away your users' passwords to anyone who gets access to your database.
2. **Password strength matters.** If you allow users to choose weak passwords, they will be more likely to reuse the same password on other websites. If someone gets access to your database, they will be able to log in to your users' other accounts.

We won't be writing code to validate password strength in this course, but you get the idea: you can enforce rules in your HTTP handlers to make sure passwords are of a certain length and complexity.

## Hashing

On the other hand, we _will_ be writing code to store passwords in a way that prevents them from being read by anyone who gets access to your database. This is called _hashing_. Hashing is a one-way function. It takes a string as input and produces a string as output. The output string is called a _hash_.

We'll cover how hashing works in-depth in a later course. For now, just know that hashing is a way to store passwords in a way that prevents them from being read by anyone who gets access to your database, but still allows us to _compare_ passwords when a user logs in.

## Assignment

1. [ ] Add and run a new migration that adds a non-null `TEXT` column to the `users` table called `hashed_password`. It should default to "unset" for existing users.

For the password hash, we will use [Argon2](https://en.wikipedia.org/wiki/Argon2). To help with this, use the library [argon2id](https://github.com/alexedwards/argon2id), which is a convenience wrapper around Argon2.

Download the library:

```bash
go get github.com/alexedwards/argon2id
```

2. [ ] Create an `internal/auth` package and expose two functions:
    - [ ] `func HashPassword(password string) (string, error)`: Hash the password using the `argon2id.CreateHash` function.
    - [ ] `func CheckPasswordHash(password, hash string) (bool, error)`: Use the `argon2id.ComparePasswordAndHash` function to compare the password that the user entered in the HTTP request with the password that is stored in the database.

I wrote a couple of simple [unit tests](https://go.dev/doc/tutorial/add-a-test) to ensure the package is working as expected.

3. [ ] Update the `POST /api/users` endpoint. The body parameters should now require a new `password` field:

```json
{
  "password": "04234",
  "email": "lane@example.com"
}
```

As long as your server uses HTTPS in production, it's safe to send raw passwords in HTTP requests, because the entire request is encrypted.

Use your internal package's `HashPassword` function to hash the password before storing it in the database. Do **NOT** return the hashed password in the response. Again, that would be a security risk.

4. [ ] Add a `POST /api/login` endpoint. This endpoint should allow a user to login. In a future exercise, this endpoint will be used to give the user a token that they can use to make authenticated requests. For now, let's just make sure password validation is working. It should accept this body:

```json
{
  "password": "04234",
  "email": "lane@example.com"
}
```

You'll need a new query to look up a user by their email address (you don't have access to an ID here). Once you have the user, check to see if their password matches the stored hash using your internal package. If either the user lookup or the password comparison errors, just return a `401 Unauthorized` response with the message "Incorrect email or password".

If the passwords match, return a `200 OK` response and a copy of the user resource (without the password of course):

```json
{
  "id": "f0f87ec2-a8b5-48cc-b66a-a85ce7c7b862",
  "created_at": "2021-07-07T00:00:00Z",
  "updated_at": "2021-07-07T00:00:00Z",
  "email": "lane@example.com"
}
```

# Password Review

It's a really bad idea for users to reuse the same passwords across sites. If someone figures out their password for one site, they can try it on other sites. If they get lucky, they can log in to and compromise many of their accounts.

Unfortunately, it's very common for users to reuse passwords. We can't _force_ users to not reuse passwords on the server side, but we can take steps to make it harder for them to reuse passwords. Namely, we can require that passwords are strong.

## Passwords Should Be Strong

The most important factor for the strength of a password is its _entropy_. [Entropy](https://www.boot.dev/blog/computer-science/what-is-entropy-in-cryptography/) is a measure of how many possible combinations of characters there are in a string. To put it simply:

- The longer the password the better
- Special characters and capitals should always be allowed
- Special characters and capitals aren't as important as length

![](https://imgs.xkcd.com/comics/password_strength.png)

- [xkcd: Password Strength](https://xkcd.com/936/)

## Passwords Should Never Be Stored in Plain Text

The most critical thing we can do to protect our users' passwords is to _never_ store them in plain text. We should use cryptographically strong key derivation functions (which are a special class of hash functions) to store passwords in a way that prevents them from being read by anyone who gets access to your database.

[Argon2id](https://en.wikipedia.org/wiki/Argon2) is a great choice. [SHA-256](https://www.boot.dev/blog/computer-science/how-sha-2-works-step-by-step-sha-256/) and [MD5](https://en.wikipedia.org/wiki/MD5) are not.

# Types of Authentication

Here are a few of the most common authentication methods you'll see in the wild:

1. Password + ID (username, email, etc.)
2. 3rd Party Authentication ("Sign in with Google", "Sign in with GitHub", etc)
3. Magic Links
4. API Keys

## 1. Password + ID

This is the most common type of authentication that requires a manual login from a user. When users use password managers, it's one of the more secure ways to authenticate users, unfortunately, many users don't, so it's not as secure as it could be.

That said, it's a valid choice.

## 2. 3rd Party Authentication

3rd party authentication is a way to authenticate users using a service like Google or GitHub. 3rd party auth is great for user experience because it allows users to use their existing accounts to log in to your app, lowering friction.

It's also nice because you don't need to worry about storing passwords yourself, meaning you can outsource the security of your users' passwords to a company that, _hopefully_, does a good job.

The only real drawbacks to 3rd party auth is that you're trusting a 3rd party and if your users don't have an account with that 3rd party, they won't be able to log in.

## 3. Magic Links

Magic links are a way to authenticate users without a password. It relies on the assumption that the user's email is something that they have unique access to.

The webserver sends a link to the user's email and encodes a unique token in that link. When the user clicks the link, the webserver can decode the token and use it to authenticate the user. Eg:

`https://example.com/login?token=...`

## 4. API Keys

API keys are a fantastic way to authenticate users and systems programmatically. An API Key is just a long, secure string that uniquely identifies a user or system, and that can't be guessed. Because they're intended to be used in code, they don't need to be memorized and, as such, can be much longer and double as an identifier. An API key might look something like this:

`bd_JDS543J3n5NMKspDXNRlowiqw523lKHK32K43kl`

# JWTs

There are several different ways to handle authentication. We'll use [JWTs](https://www.boot.dev/blog/backend/hmac-and-macs-in-jwts/) in this course. They're a popular choice for APIs that are consumed by web applications and mobile apps.

## What Is a JWT?

A JWT is a JSON Web Token. It's a cryptographically signed JSON object that contains information about the user. You'll learn about how the cryptography of JWTs work in our [Learn Cryptography](https://boot.dev/courses/learn-cryptography) course, for now, it's just important to know that once the token is created by the server, the data in the token can't be changed without the server knowing.

_When your server issues a JWT to Bob, Bob can use that token to make requests as Bob to your API. Bob won't be able to change the token to make requests as Alice._

## Assignment

The first building blocks you'll write are the functions for creating and validating JWTs, which will be used in the next lesson to authenticate users.

1. [ ] Add a `MakeJWT` function to your `auth` package:

```go
func MakeJWT(userID uuid.UUID, tokenSecret string, expiresIn time.Duration) (string, error)
```

Create and return a JWT using this [JWT library](https://github.com/golang-jwt/jwt), which you can import into your code by running:

```sh
go get -u github.com/golang-jwt/jwt/v5
```

2. [ ] Create a new token.
    1. [ ] Use [`jwt.NewWithClaims`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#NewWithClaims)
    2. [ ] Use [`jwt.SigningMethodHS256`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#SigningMethodHS256) as the signing method.
    3. [ ] Use [`jwt.RegisteredClaims`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#RegisteredClaims) as the claims.
        - [ ] Set the `Issuer` to "chirpy-access"
        - [ ] Set `IssuedAt` to the current time in UTC
        - [ ] Set `ExpiresAt` to the current time plus the expiration time (`expiresIn`)
        - [ ] Set the `Subject` to a stringified version of the user's `id`
    4. [ ] Use [`token.SignedString`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#Token.SignedString) to sign the token with the secret key. Refer to [here](https://golang-jwt.github.io/jwt/usage/signing_methods/#signing-methods-and-key-types) for an overview of the different signing methods and their respective key types.
3. [ ] Add a `ValidateJWT` function to your `auth` package:

```go
func ValidateJWT(tokenString, tokenSecret string) (uuid.UUID, error)
```

4. [ ] Use the [`jwt.ParseWithClaims`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#ParseWithClaims) function to validate the signature of the JWT and extract the claims into a [`*jwt.Token`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#Token) struct. The `keyFunc` callback must return the same key type (`[]byte`) used when the token was signed. An error will be returned if the token is invalid or has expired.

If all is well with the token, use the [`token.Claims`](https://pkg.go.dev/github.com/golang-jwt/jwt/v5#Claims) interface to get access to the user's `id` from the claims (which should be stored in the `Subject` field). Return the `id` as a `uuid.UUID`.

5. [ ] Add some more unit tests to the `auth` package. Make sure that you can create and validate JWTs, and that expired tokens are rejected and JWTs signed with the wrong secret are rejected.

# Authentication With JWTs

Let's take a closer look at how JWTs work in the authentication flow.

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/hFgop3U-480x720.png)

## Step 1: Login

It would be pretty annoying if you had to enter your username and password every time you wanted to make a request to an API. Instead, after a user enters a username and password, our server should respond with a _token_ (JWT) that's saved in the client's device.

The token remains valid until it expires, at which point the user will need to log in again.

## Step 2: Using the Token

When the user wants to make a request to the API, they send the token along with the request in the HTTP headers. The server can then verify that the token is valid, which means the user is who they say they are.

## Assignment

1. [ ] Add a `GetBearerToken` function to your `auth` package:

```go
func GetBearerToken(headers http.Header) (string, error)
```

Auth information will come into our server in the `Authorization` header. Its value will look like this:

```
Bearer TOKEN_STRING
```

This function should look for the `Authorization` header in the `headers` parameter and return the `TOKEN_STRING` if it exists (stripping off the `Bearer` prefix and whitespace). If the header doesn't exist, return an error. This is an easy one to write a unit test for, and I'd recommend doing so.

2. [ ] Create a _secret_ for your server and store it in your `.env` file. This is the secret used to sign and verify JWTs. By keeping it safe, no other servers will be able to create valid JWTs for your server. We will yet again use [environment variables](https://en.wikipedia.org/wiki/Environment_variable). You can generate a nice long random string on the command line like this:

```bash
openssl rand -base64 64
```

Secrets should **NOT** be stored in Git, just in case anyone malicious gains access to your repository.

3. [ ] Load the JWT secret from your `.env` file in your `main()` function and store it in your `apiConfig` struct.
4. [ ] Update your `POST /api/login` endpoint. It should accept a new, _optional_ `expires_in_seconds` field in the request body:

```json
{
  "password": "04234",
  "email": "lane@example.com",
  "expires_in_seconds": 2
}
```

If it's specified by the client, use it as the expiration time. If it's not specified, use a default expiration time of 1 hour. If the client specified a number over 1 hour, use 1 hour as the expiration time.

Once you have the token created with the new params, respond to the request with a `200` code and this body shape:

```json
{
  "id": "5a47789c-a617-444a-8a80-b50359247804",
  "created_at": "2021-07-01T00:00:00Z",
  "updated_at": "2021-07-01T00:00:00Z",
  "email": "lane@example.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```

5. [ ] Update the `POST /api/chirps` endpoint. It is not an authenticated endpoint yet. To post a chirp, a user needs to have a valid JWT. The JWT will determine which user is posting the chirp. Use your `GetBearerToken` and `ValidateJWT` functions. If the JWT is invalid, return a `401 Unauthorized` response.

# JWT Review

JWTs are cryptographically signed JSON objects that contain information about an authenticated user.

I've heard "JWT" pronounced as "jot", but I pronounce it "jay double yoo tee".

## JWTs Can't Be Changed

We'll talk about [MACs, HMACs](https://www.boot.dev/blog/backend/hmac-and-macs-in-jwts/), and digital signatures in a later course, which are the cryptographic concepts that power JWTs. For now, it's just important to know that once the token is created by a server, the data in the token can't be changed without the server being aware of it.

_When your server issues a JWT to Bob, Bob can use that token to make requests as Bob to your API. Bob won't be able to change the token to make requests as Alice._

## JWTs Are Not Encrypted

JWTs are not encrypted. Anyone who has the token can read the data (like the expiry and the user id) in the token. This is why you should never store sensitive information in a JWT. It's just a way to authenticate a user.

I like using [JWT.io](https://jwt.io/) to inspect JWTs. It is a great tool for playing around with them and learning how they work.

## JWT Lifecycle

![](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/hFgop3U-480x720.png)

Since a JWT (JSON Web Token) is a self-contained credential, any server that trusts that token will treat the bearer of the token as the user identified within it. This is why JWT-based authentication is often called "Bearer Authentication."

Think of a JWT like a physical key or a movie ticket. The theater usher doesn't necessarily care _who_ bought the ticket; they only care that the ticket is authentic and hasn't been tampered with. If you find a valid ticket on the ground, you can use it to enter the theater.

Because of this, there are a few critical security implications:

1. **Transport Security**: You must always use HTTPS. If the token is sent over an unencrypted connection, someone could "sniff" the network traffic and steal it.
2. **Storage**: On the client side (like a browser), tokens need to be stored securely to prevent malicious scripts (XSS attacks) from stealing them.
3. **No Sensitivity**: As mentioned in the lesson, never put sensitive data like passwords or social security numbers inside the token, because anyone who sees the token can decode the JSON and read it.
4. **Expiration**: JWTs should generally have a short lifespan. If a token is stolen, the "window of opportunity" for the attacker is limited until the token expires.

The server's primary defense isn't knowing _who_ is holding the token, but rather using the **signature** to ensure the token itself is legitimate and hasn't been modified since the server issued it.

JWT is **not** a one-way hash.

A one-way hash (like SHA-256) is designed to be impossible to reverse. You put data in, and you get a unique fingerprint out, but you can't turn that fingerprint back into the original data.

A JWT is actually **Base64URL encoded**. Encoding is very different from hashing or encrypting:

- **Encoding** is just a way of transforming data into a different format (usually to make it safe for URLs or headers). It is trivial to decode. Anyone can take a JWT, paste it into a decoder, and read the JSON data inside instantly.
- **The Signature** (the third part of the JWT) involves a hash, but it's used to verify the integrity of the data, not to hide it.

To visualize it:

1. **Header**: Base64 encoded (Publicly readable)
2. **Payload**: Base64 encoded (Publicly readable)
3. **Signature**: A hash of the Header + Payload + a Secret Key.

If you change a single letter in the **Payload**, the server will re-calculate the hash using its secret key. When the new hash doesn't match the **Signature** attached to the token, the server knows the token has been tampered with.

So while the data is "clear text" for anyone to read, it is "read-only" for everyone except the server that holds the secret key.

# Revoking JWTs

One of the main benefits of JWTs is that they're _stateless_. The server doesn't need to keep track of which users are logged in via JWT. The server just needs to issue a JWT to a user and the user can use that JWT to authenticate themselves. Statelessness is _fast and scalable_ because your server doesn't need to consult a database to see if a user is currently logged in.

However, that same benefit poses a potential problem. JWTs can't be revoked. If a user's JWT is stolen, there's no easy way to stop the JWT from being used. JWTs are just a signed string of text.

The JWTs we've been using so far are more specifically _access tokens_. Access tokens are used to authenticate a user to a server, and they provide _access_ to protected resources. Access tokens are:

- Stateless
- Short-lived (15m-24h)
- Irrevocable

They _must_ be short-lived because they can't be revoked. The shorter the lifespan, the more secure they are. Trouble is, this can create a poor user experience. We don't want users to have to log in every 15 minutes.

## A Solution: Refresh Tokens

Refresh tokens don't provide access to resources directly, but they can be used to get new access tokens. Refresh tokens are much longer lived, and importantly, they _can_ be revoked. They are:

- Stateful
- Long-lived (24h-60d)
- Revocable

Now we get the best of both worlds! Our endpoints and servers that provide access to protected resources can use access tokens, which are fast, stateless, simple, and scalable. On the other hand, refresh tokens are used to keep users logged in for longer periods of time, and they can be revoked if a user's access token is compromised.

# Refresh Tokens

To allow our users to stay logged in for longer periods, let's add refresh tokens to our authentication system. At the same time, we'll reduce the lifespan of our access tokens to improve security.

## Session Store

In our case, a refresh token will just be a random 256-bit string. It's a _token_, but not a _JSON Web Token_. It doesn't need to be a JWT because we'll store it in our database and associate it with a user server-side. No point in using stateless JWTs if we're going to store them in a database anyway.

To revoke a refresh token, we'll set a `revoked_at` timestamp in the database. If `revoked_at` is not null, the token is revoked and will be considered invalid.

## Assignment

1. [ ] Create a new database table with up/down migrations called `refresh_tokens`.
    - `token`: the primary key - it's just a string
    - `created_at`
    - `updated_at`
    - `user_id`: foreign key that deletes the row if the user is deleted
    - `expires_at`: the timestamp when the token expires
    - `revoked_at`: the timestamp when the token was revoked (null if not revoked)
2. [ ] Add a `func MakeRefreshToken() string` function to your `internal/auth` package. It should use the following to generate a random 256-bit (32-byte) hex-encoded string:
    - [rand.Read](https://pkg.go.dev/crypto/rand#Read) to generate 32 bytes (256 bits) of random data from the `crypto/rand` package (`math/rand`'s `Read` function is deprecated).
    - [hex.EncodeToString](https://pkg.go.dev/encoding/hex#EncodeToString) to convert the random data to a hex string
3. [ ] Update the `POST /api/login` endpoint to return a refresh token, as well as an access token:
    - [ ] Access tokens (JWTs) should expire after 1 hour. Expiration time is stored in the `exp` claim. You can remove the optional `expires_in_seconds` parameter from the endpoint.
    - [ ] Refresh tokens should expire after 60 days. Expiration time is stored in the database.
    - [ ] The `revoked_at` field should be null when the token is created.

```json
{
  "id": "5a47789c-a617-444a-8a80-b50359247804",
  "created_at": "2021-07-01T00:00:00Z",
  "updated_at": "2021-07-01T00:00:00Z",
  "email": "lane@example.com",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c",
  "refresh_token": "56aa826d22baab4b5ec2cea41a59ecbba03e542aedbb31d9b80326ac8ffcfa2a"
}
```

4. [ ] Create a `POST /api/refresh` endpoint. This new endpoint does _not_ accept a request body, but _does_ require a **refresh token** to be present in the headers, in the same `Authorization: Bearer <refresh-token>` format.

Look up the refresh token in the database. If it doesn't exist, or if it's expired or revoked, respond with a `401` status code. Otherwise, respond with a `200` code and this shape:

```json
{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"
}
```

The `token` field should be a newly created access token _for the given user_ that expires in 1 hour. I wrote a `GetUserFromRefreshToken` SQL query.

5. [ ] Create a new `POST /api/revoke` endpoint. This new endpoint does _not_ accept a request body, but _does_ require a **refresh token** to be present in the headers, in the same `Authorization: Bearer <refresh-token>` format.

Revoke the refresh token record in the database that matches the refresh token passed in the request header by setting the `revoked_at` to the current timestamp. Remember that any time you update a record, you should also be updating the `updated_at` timestamp.

Respond with a [`204` status code](https://www.rfc-editor.org/rfc/rfc9110.html#name-204-no-content). A 204 status means the request was successful but no body is returned.

# Cookies

HTTP [cookies](https://en.wikipedia.org/wiki/HTTP_cookie) are one of the most talked about, but least understood, aspects of the web.

When cookies are talked about in the news, they're usually implied to simply be privacy-stealing bad guys. While cookies can certainly invade your privacy, that's not what they _are_.

## What Is an HTTP Cookie?

A cookie is a small piece of data that a server sends to a client. The client then dutifully stores the cookie and sends it back to the server on subsequent requests.

Cookies can store any arbitrary data:

- A user's name or other tracking information
- A JWT (refresh and access tokens)
- Items in a shopping cart
- etc.

The server decides _what_ to put in a cookie, and the client's job is simply to store it and send it back.

## How Do Cookies Work?

Simply put, cookies work through HTTP headers.

Cookies are sent from the server to the client in the `Set-Cookie` header. Cookies are most popular for web (browser-based) applications because browsers _automatically_ send any cookies they have back to the server in the `Cookie` header.

## Why Aren't We Using Cookies?

Simply put, Chirpy's API is designed to be consumed by mobile apps and other servers. Cookies are primarily for browsers.

A good use-case for cookies is to serve as a more strict and secure transport layer for JWTs within the context of a browser-based application.

For example, when using [httpOnly cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies#block_access_to_your_cookies), you can ensure that 3rd party JavaScript that's being executed on your website can't access any cookies. That's a lot better than storing JWTs in the browser's local storage, where it's easily accessible to any JavaScript running on the page.

---

CH7: Authorization

# Authorization

While authentication is about verifying _who_ a user is, authorization is about verifying _what a user is allowed to do_.

For example, a hypothetical YouTuber `ThePrimeagen` should be allowed to edit and delete the videos on his account, and everyone should be allowed to view them. Another absolutely-not-real YouTuber `TEEJ` should be able to view `ThePrimeagen`'s videos, but not edit or delete them.

Authorization logic is just the code that enforces these kinds of rules.

## Assignment

We already have a bit of authorization built into Chirpy: authenticated users can only create chirps for themselves, not for others.

1. [ ] Add a `PUT /api/users` endpoint so that users can update their own (but not others') email and password. It requires:
    - An access token in the header
    - A new `password` and `email` in the request body
2. [ ] Hash the password, then update the hashed password and the email for the authenticated user in the database. Respond with a `200` if everything is successful and the newly updated `User` resource (omitting the password of course).
3. [ ] If the access token is malformed or missing, respond with a `401` status code.

#   
Authentication vs. Authorization

As we covered briefly, authentication and authorization are two different things.

## Authentication

Verify _who_ a user is, typically by asking for a password, api key, or other credentials.

## Authorization

Only allow a verified user to perform actions that _they_ are allowed to perform. Sometimes it's based on exactly who they are, but often it's based on a role, like "admin" or "owner".

# Delete Chirp

Oh no... the Chirpy CEO is chirping again. He's about to get the entire company cancelled. Let's add delete functionality!

## Assignment

1. [ ] Add a new `DELETE /api/chirps/{chirpID}` route to your server that deletes a chirp from the database by its `id`.
    - [ ] This is an authenticated endpoint, so be sure to check the token in the header. Only allow the deletion of a chirp if the user is the author of the chirp.
    - [ ] If they are not, return a `403` status code.
2. [ ] If the chirp is deleted successfully, return a `204` status code.
3. [ ] If the chirp is not found, return a `404` status code.

---

CH8: Webhooks

# Webhooks

Webhooks sound like a scary advanced concept, but they're quite simple.

A webhook is just an event that's sent to your server by an external service when something happens.

For example, here at Boot.dev we use Stripe as a third-party payment processor. When a student makes a payment, Stripe sends a webhook to the Boot.dev servers so that we can unlock the student's membership.

1. Student makes a payment to stripe
2. Stripe processes the payment
3. If the payment is successful, Stripe sends an `HTTP POST` request to `https://api.boot.dev/stripe/webhook` (that's not the real URL, but you get the idea)

That's it! The only real difference between a webhook and a typical `HTTP` request is that the system making the request is an automated system, not a human loading a webpage or web app. As such, webhook handlers must be [idempotent](https://en.wikipedia.org/wiki/Idempotence) because the system on the other side may retry the request multiple times.

## Idempo... What?

Idempotent, or "idempotence", is a fancy word that means "the same result no matter how many times you do it". For example, your typical `POST /api/chirps` (create a chirp) endpoint will _not_ be idempotent. If you send the same request twice, you'll end up with two chirps with the same information but different IDs.

Webhooks, on the other hand, should be idempotent, and it's typically easy to build them this way because the client sends some kind of "event" and usually provides its own unique ID.

## Assignment

We recently rolled out a new feature called "Chirpy Red". It's a membership program, and members of "Chirpy Red" get pretty incredible features: like the ability to edit chirps after posting them. But that's beside the point...

Chirpy uses "Polka" as its payment provider. They send us webhooks whenever a user subscribes to Chirpy Red. We need to mark users as Chirpy Red members when we receive these webhooks.

1. [ ] Add a migration to the `users` table to include a new column called `is_chirpy_red`. This column should be a boolean, and it should default to `false`.
2. [ ] Add a database query that upgrades a user to chirpy red based on their ID.
3. [ ] Add a `POST /api/polka/webhooks` endpoint. It should accept a request of this shape:

```json
{
  "event": "user.upgraded",
  "data": {
    "user_id": "3311741c-680c-4546-99f3-fc9efac2036c"
  }
}
```

- [ ] If the `event` is anything _other_ than `user.upgraded`, the endpoint should immediately respond with a `204` status code - we don't care about any other events.
- [ ] If the `event` _is_ `user.upgraded`, then it should update the user in the database, and mark that they are a Chirpy Red member.
- [ ] If the user is upgraded successfully, the endpoint should respond with a `204` status code and an empty response body. If the user can't be found, the endpoint should respond with a `404` status code.

_Polka uses the response code to know whether or not the webhook was received successfully. If the response code is anything other than `2XX`, they'll retry the request._

4. [ ] Update all endpoints that return user resources to include the `is_chirpy_red` field.

# Webhooks Review

A webhook is just an event that's sent to your server by an external service. There are just a couple of things to keep in mind when building a webhook handler:

- The third-party system will probably retry requests multiple times, so your handler should be [idempotent](https://en.wikipedia.org/wiki/Idempotence).
- Be extra careful to never "acknowledge" a webhook request unless you processed it successfully. By sending a `2XX` code, you're telling the third-party system that you processed the request successfully, and they'll stop retrying it.
- When you're writing a server, you typically get to define the API. However, when you're integrating a webhook from a service like Stripe, you'll probably need to adhere to their API: they'll tell you what shape the events will be sent in.

## Are Webhooks and Websockets the Same Thing?

Nope! A websocket is a persistent connection between a client and a server. Websockets are typically used for real-time communication, like chat apps. Webhooks are a one-way communication from a third-party service to your server.

We'll talk about websockets in a future course.

# API Keys

You may have noticed that there is an issue with our webhook handler: it's not secure!

Anyone can send a request to our webhook handler, and we'll process it. That means that if Chirpy users figured out our API documentation, they could simply upgrade their account without paying!

## Assignment

Luckily, Polka has a solution for this: API keys. Polka provided us with an API key, and if a request to our webhook handler doesn't use that API key, we should reject the request. This ensures that only Polka can tell us to upgrade a user's account.

Your Polka key: `f271c81ff7084ee5b99a5091b42d486e`

1. [ ] Add a new secret value to your `.env` file called `POLKA_KEY`. This is the api key that polka will send so that we know it's them (and not someone else trying to get free Chirpy red). Load it into your server and store it in your `apiConfig`.
2. [ ] Add a `func GetAPIKey(headers http.Header) (string, error)` to your `auth` package. It should extract the api key from the `Authorization` header, which is expected to be in this format:

```
Authorization: ApiKey THE_KEY_HERE
```

You'll need to strip out the `ApiKey` part and the whitespace and return just the key.

3. [ ] Update the `POST /api/polka/webhooks` endpoint. It should ensure that the API key in the header matches the one stored in the `.env` file. If it doesn't, the endpoint should respond with a `401` status code.

---
CH9: Documentation

# Documentation

When you're designing a server-side API, no one is going to know how to interact with it unless you tell them. Are you going to force the front-end developers, mobile developers, or other back-end service teams to sift through your code and reverse engineer your API?

Of course not! You're a good person. You're going to write documentation.

## First Be Obvious, Then Document It Anyway

We've talked a lot about how your REST API should follow conventions as much as possible. That said, the conventions _are not enough_. You still need to document your endpoints. Without documentation, no one will know:

- Which resources are available
- What the path to the endpoints are
- Which HTTP methods are supported for each resource
- What the shape of the data is for each resource
- etc.

## Assignment

One type of endpoint that's nearly impossible to interact with without documentation is a plural `GET` endpoint, that is, an endpoint that returns a list of resources. They often have different sorting, filtering, and [pagination](https://developer.squareup.com/docs/build-basics/common-api-patterns/pagination) features.

1. [ ] Update the `GET /api/chirps` endpoint. It should accept an _optional_ query parameter called `author_id`.
    - [ ] If the `author_id` query parameter is provided, the endpoint should return only the chirps for that author.
    - [ ] If the `author_id` query parameter is not provided, the endpoint should return all chirps as it did before.

For example:

`GET http://localhost:8080/api/chirps?author_id=1`

_Continue sorting the chirps by `created_at` in ascending order._

Be sure to filter by author ID at the database level, not in memory! That will be more efficient on large datasets.

**Run and submit** the CLI tests.

## Tips

The [http.Request](https://pkg.go.dev/net/http#Request) struct has a way to grab the query parameters from the URL:

```go
s := r.URL.Query().Get("author_id")
// s is a string that contains the value of the author_id query parameter
// if it exists, or an empty string if it doesn't
```

# Documentation

As far as creating documentation goes, there are 2 main approaches:

1. Manually write documentation
2. Use a tool to generate documentation

Obviously, the first approach is easier to get going with if you have a small API, but as the system grows, it can be really hard to keep the documentation up to date.

**Incorrect documentation is worse than no documentation.**

At least when there is _no_ documentation, your clients will reach out and ask for clarification. When the documentation is incorrect, it can lead to a lot of wasted time and frustration.

## Manually Writing Documentation

When I've worked on smaller teams, we've generally opted to write our documentation in [Markdown files](https://www.markdownguide.org/) and host them on GitHub. This is a great way to get started because Markdown is a simple format that is easy to write and easy to read.

## Automated Documentation Generation

I've also written and consumed APIs that have used:

- [Swagger](https://swagger.io/)
- [GraphQL](https://graphql.org/) (not RESTful, but still a networking API)
- [Godoc](https://go.dev/blog/godoc) (which only works for REST APIs if you provide an SDK)
- [Postman](https://learning.postman.com/docs/publishing-your-api/documenting-your-api/) (only useful if your team all uses Postman as their HTTP client)

And with LLMs, there are also now trivial ways to use AI to generate simple markdown documentation directly from your backend code.

## Okay, but What Should I Do for Now?

I recommend personally writing documentation for your projects in Markdown files and storing them alongside the rest of your code in Git. Your project's `README.md` file is a great place to start, but it's also common for the `README.md` file to link to a `/docs` folder that contains more detailed documentation. The benefits are:

- It's easy to get started (no need to install any additional tools)
- The documentation lives alongside your code, so it's easy to keep it up to date
- You'll learn Markdown, which is a great skill to have
- GitHub/GitLab will render your Markdown files for you, so your docs will look great

# Sorting Chirps

A common feature in APIs is the ability to sort the response by a field. We don't want to add additional endpoints for every possible sort order, so we'll use a query parameter instead.

## Assignment

Update the `GET /api/chirps` endpoint. It should accept an _optional_ query parameter called `sort`. It can have 2 possible values:

- `asc` - Sort the chirps in the response by `created_at` in ascending order
- `desc` - Sort the chirps in the response by `created_at` in descending order

`asc` is the default if no `sort` query parameter is provided.

Keep it simple! You can just sort the chirps in-memory using [`sort.Slice`](https://pkg.go.dev/sort#Slice).

**Run and submit** the CLI tests.

## Examples of Valid URLs

- `GET http://localhost:8080/api/chirps?sort=asc`
- `GET http://localhost:8080/api/chirps?sort=desc`
- `GET http://localhost:8080/api/chirps`

# Adding a README

You're done building Chirpy! Great work!

I want to take a moment to cover how you should think about your public GitHub profile, and especially how it can help you in your job search.

We have a more [in-depth course](https://www.boot.dev/courses/learn-job-search) that includes building a professional GitHub profile, but for now, I want to cover some basics.

## Do I Have to Put This on GitHub?

You _really should_. It doesn't necessarily _need_ to be publicly visible, but it's good to keep copies of _all_ of your code for future reference.

## Projects Are Your Professional Portfolio

When you start looking for jobs, you're going to want a couple of _great_ projects on your GitHub or GitLab profile that show off your skills.

_This probably isn't one of those because you built it using a guide._

That said, it may be wise to _treat_ this project like one and get your feet wet with the process of presenting a project to the world.

## How to Present This Project

When someone navigates to your project's link, the _first_ thing they'll see is the `README.md` file. You should quickly and concisely explain:

- What your project does
- Why someone should care
- How to install and run your project

Take a look at one of my portfolio projects as an example: [go-rabbitmq](https://github.com/wagslane/go-rabbitmq).

Good luck!

