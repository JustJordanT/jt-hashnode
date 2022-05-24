## Day - 1 Get Going With GO.

# Day 1 - Get Going With GO!
Before we start, I would like to go over some **prerequisites** that we should know before learning GO.

### Prerequisites
-   **[Some programming experience](https://youtu.be/zOjov-2OZ0E).** The code here is pretty simple, but it helps to know something about functions.
- **[APIs](https://youtu.be/GZvSYJDk-us)**. On some days we will be working on making our own APIs, so this would be good to have some experience with using them.
-   **A tool to edit your code.** Any text editor you have will work fine. Most text editors have good support for Go. The most popular are [VSCode](https://code.visualstudio.com/) (free), [GoLand](https://www.jetbrains.com/go/) (paid) - (Free for students), and Vim (free).
-   **A command terminal.** Go works well using any terminal on Linux and Mac, and on PowerShell or cmd on Windows.


## What Is GO?
Go is a programming language that was made by Google in 2007 to improve [Programming productivity](https://en.wikipedia.org/wiki/Programming_productivity) The creators of Go wanted to address criticism of other languages in use at [Google](https://en.wikipedia.org/wiki/Google "Google"), but keep their useful characteristics.

-   [Static typing](https://en.wikipedia.org/wiki/Static_typing "Static typing") and [run-time](https://en.wikipedia.org/wiki/Run_time_(program_lifecycle_phase) "Run time (program lifecycle phase)") efficiency (like [C](https://en.wikipedia.org/wiki/C_(programming_language) "C (programming language)"))
-   Readability and usability (like [Python](https://en.wikipedia.org/wiki/Python_(programming_language) "Python (programming language)") or [JavaScript](https://en.wikipedia.org/wiki/JavaScript "JavaScript"))
-   High-performance [networking](https://en.wikipedia.org/wiki/Computer_network "Computer network") and [multiprocessing](https://en.wikipedia.org/wiki/Multiprocessing "Multiprocessing")

## How can GO help us?
Build fast, reliable, and efficient software at scale this is what we can achieve with using GO. 

### Minimalist

If you’ve played with the basics of Go, you will notice that the language is not trying to be overly complex or impressive. It’s just enough to get the job done.

Example HTTP server.
```GO
package main

import (
    "fmt"
    "net/http"
)

func hello(w http.ResponseWriter, req *http.Request) {

    fmt.Fprintf(w, "hello\n")
}

func headers(w http.ResponseWriter, req *http.Request) {

    for name, headers := range req.Header {
        for _, h := range headers {
            fmt.Fprintf(w, "%v: %v\n", name, h)
        }
    }
}

func main() {

    http.HandleFunc("/hello", hello)
    http.HandleFunc("/headers", headers)

    http.ListenAndServe(":8080", nil)
}
```

Output: 
```SH
$ go run http-servers.go 

$ curl localhost:8080/hello
hello
```


### Formating

There is one built-in [formatting engine](https://go.dev/blog/gofmt), no need to use things like `prettier.js` and there’s no need to re-invent the wheel. [Gofmt](https://go.dev/cmd/gofmt/) is a tool that automatically formats Go source code.

### Low boilerplate code

Go requires very little boilerplate code to create substantial applications, unlike [Rust](https://www.rust-lang.org/) which requires far more boilerplate code when matching equivalent capabilities. By now you have seen that you can write meaningful code with fewer lines using GO compared to other languages. 

Example Hello World in GO. 
```GO
package main

import "fmt"

func main() {
    fmt.Println("hello world")
}
```

## Why should we be using GO?
Even though Go is different from other object-oriented languages, it is still the same. Go provides you high performance like C/C++, super-efficient concurrency handling like Java and fun to code like Python/Perl. 

### Resources

- [Go (Programming Language)](<https://en.wikipedia.org/wiki/Go_(programming_language)>)
[How We Went from 30 Servers to 2: Go](https://blog.iron.io/how-we-went-from-30-servers-to-2/)
[Why we switched from Python to Go](https://getstream.io/blog/switched-python-go/)
[Go by Example](https://gobyexample.com/)