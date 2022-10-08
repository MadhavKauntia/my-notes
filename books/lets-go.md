# [Let's Go by Alex Edwards](https://lets-go.alexedwards.net/)

## Foundations

### Project Setup and Enabling Modules

- To let Go know that we want to use the new [modules functionality](https://github.com/golang/go/wiki/Modules), we first need to decide and set the _module path_ for our project.
- The common convention is to namespace your module paths by basing them on a URL you own - madhavkauntia.com/snippetbox

```bash
$ cd $HOME/code/snippetbox
$ go mod init madhavkauntia.com/snippetbox
```

- If you’re creating a package or application which can be downloaded and used by other people and programs, then it’s good practice for your module path to equal the location that the code can be downloaded from.

### Web Application Basics

The three absolute essentials for a web application are:

- **Handler** - they are like controllers. They're responsible for executing your application logic and for writing HTTP response headers and bodies.

- **Router** - this stores the mapping between URL patterns and the corresponding handlers.

- **Web Server** - Go can establish a web server and listen for incoming requests as part of the application itself. External third-party server like Nginx or Apache is not required.

```go
package main

import (
	"log"
	"net/http"
)

// define a home handler function which writes a byte slice containing "Hello from Snippetbox" as the response body
func home(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("Hello from Snippetbox"))
}

func main() {
	// use the http.NewServeMux() function to initialize a new servemux, then register the home function as the handler for the "/" URL pattern
	mux := http.NewServeMux()
	mux.HandleFunc("/", home)

	// use the http.ListenAndServe() function to start a new web server. We pass in two parameters: the TCP network address to listen on (in this case ":4000") and the servemux we just created. If http.ListenAndServe() returns an error we use the log.Fatal() function to log the error message and exit
	log.Println("Starting server on :4000")
	err := http.ListenAndServe(":4000", mux)
	log.Fatal(err)
}
```

- The TCP address you pass to `http.ListenAndServe()` should be in the format `host:port`. If the host is omitted, then the server will listen to all your computer's available network interfaces.
- In other Go projects or documentation you might sometimes see network addresses written using named ports like ":http" or ":http-alt" instead of a number. If you use a named port then Go will attempt to look up the relevant port number from your /etc/services file when starting the server, or will return an error if a match can’t be found.

### Routing Requests

- Go's servermux supports two different types of URL patterns: _fixed paths_ and _subtree paths_. Fixed paths _don't_ end with a trailing slash whereas subtree paths _do_ end with a trailing slash.
- Fixed path patterns are only matched when the request URL path exactly matches the fixed path.
- It is possible to use the `DefaultServeMux` by directly using `http.Handle()` or `http.HandleFunc()`. However, this is not recommended because the `DefaultServeMux` is a global variable and any package can access it and register a route.
