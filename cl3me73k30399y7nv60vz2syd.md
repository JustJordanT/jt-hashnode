## Day 2 - Packages, Variables, and Functions Oh My!

# Day 2 - Packages, Variables, and Functions Oh My!

First things first; we need to make sure that we get our GO environment set up. We will need to make sure that we have GO [installed](https://go.dev/doc/install) and make sure that we are using an IDE of our choice as we spoke about in [Day-1](https://blog.justjordant.com/day-1-get-going-with-go); Okay, put your comfy shoes cause this one might be a stretch.

## What are Packages, Variables, and Functions?

### Packages.

The purpose of a package is to design and maintain numerous programs by grouping related features together into single units so that they can be easy to maintain and understand and independent of the other package programs

Go was designed to encourage good software engineering practices. One of the guiding principles of high-quality software is the DRY principle - **D**on’t **R**epeat **Y**ourself, In turn, you should never write the same code twice. You should reuse and build upon existing code as much as possible; Packages can help us do this.

### Variables.
Variables are the names you give to computer memory locations that are used to store values in a computer program. We can think of this just like a container where we store certain data that we wish to retrieve at a later time.

### Functions.
A function inputs the software system, its behavior, and outputs. It can be a calculation, data manipulation, business process, user interaction, or specific functionality which we can define to complete a task or tasks that we set out to do.

### Types.
Types can be viewed as a schema for our values, and these values can be viewed as type instances. i.e `bool`, `int`  and `strings`

## How can we use Packages, Variables, and Functions?

### Packages
All GO programs are made up of packages; programs start in the `main` package.

```GO
package main

import (
	"fmt"
	"math/rand"
)

func main() {
	fmt.Println("I ran this many mile", rand.Intn(10))
}
```

We can see in the above program, we are importing `fmt` and `math/rand`. By convention, the package name is the same as the last element of the importing path. An example being for our `math/rand` package, which consists of files that start with `package rand`

#### Imports
We are able to `import` right after our `package` statement. We add packages one at a time like the following. 

```go
import "fmt"
```

Or we can import multiple packages: 
```GO
import (
	"fmt"
	"math"
	"bufio"
)
```

#### Exported names
In the world of Go naming is imported, if a name is exported this will start with a Capital letter. 

```go
package main

import (
	"fmt"
	"math"
)

func main() {
	fmt.Println(math.Sqrt2)
}

```

Exported names will come from the `package`;  so looking at `math.Sqrt2`  the exported name would be `Sqrt2`. When we import packed we are able to import the exported names only the un-exported names will not be available to us from outside the package. 


### Variables
The var statement can declare a list of variables or one value by itself. Name variables can be hard; make sure it is meaningful.

![](https://i.imgur.com/Sij05LV.jpg)


```go
var isUser, isLoggedIn, isAdmin bool

var ptoDays int 
```

A var statement can be set at the `function` level or at the `package` level:

```go
package main

import "fmt"

var isUser, isLoggedIn, isAdmin bool

func main() {
	var ptoDays int 
	fmt.Println(ptoDays, isUser, isLoggedIn, isAdmin)
}
```

#### Initializers
Variables can also be initialized too; when we initialize our variables we can leave out the type since we are giving the type with the data we are putting in. 

```go
package main

import "fmt"

var isUser, isLoggedIn, isAdmin = true, false, false

func main() {
	var ptoDays = 0
	fmt.Println(ptoDays, isUser, isLoggedIn, isAdmin)
}
```

#### Short assignments
We also have the option to use short assignments from within a `func`.

```go
i := 20
```

Outside the function, we only have access to `var`, `func` so the `:=` would not work from the `package` level.

### Functions
![](https://i.imgur.com/A0As15U.png)

With functions, we can take 0 or more arguments; 
```go
package main

import "fmt"

func times(x int, y int) int {
	return x * y
}

func main() {
	fmt.Println(times(100, 2))
}
```

Here we made a multiplication `function` that takes 2 args with the type int. Take note that we are adding the type after `variable` name; If you have experience with Java or C# making functions/methods in there we call the type before not after.

Another note to add is that we are able to omit the type from all variables in our function except the last. 

```go
package main

import "fmt"

func times(x, y int) int {
	return x * y
}

func main() {
	fmt.Println(times(100, 2))
}
```

#### Returning Multiple Results

![](https://i.imgur.com/20pd52D.jpg)

Our `functions` can return more than one result if need be; take a look at the following example. 
```go
package main

import "fmt"

func swap(x, y, j string) (string, string, string) {
	return j, y, x
}

func main() {
	a, b, c := swap("hello", "world", "planet")
	fmt.Println(a, b, c)
}
```

### Types
Here are some basic types in Go.
```go
uint8       the set of all unsigned  8-bit integers (0 to 255)
uint16      the set of all unsigned 16-bit integers (0 to 65535)
uint32      the set of all unsigned 32-bit integers (0 to 4294967295)
uint64      the set of all unsigned 64-bit integers (0 to 18446744073709551615)

int8        the set of all signed  8-bit integers (-128 to 127)
int16       the set of all signed 16-bit integers (-32768 to 32767)
int32       the set of all signed 32-bit integers (-2147483648 to 2147483647)
int64       the set of all signed 64-bit integers (-9223372036854775808 to 9223372036854775807)

float32     the set of all IEEE-754 32-bit floating-point numbers
float64     the set of all IEEE-754 64-bit floating-point numbers

complex64   the set of all complex numbers with float32 real and imaginary parts
complex128  the set of all complex numbers with float64 real and imaginary parts

byte        alias for uint8
rune        alias for int32
```

For more on [types](https://go.dev/ref/spec#Types), I would always reference the Go documentation.

## Why: Packages, Variables, and Functions?

Using all of these together can allow us to make our software manageable and scaleable for others to use, as well as allow us to stick to good principles like [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) and [SOLID](https://en.wikipedia.org/wiki/SOLID)


### Resources

[SOLID: The First 5 Principles of Object-Oriented Design](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)
[go101](https://go101.org/)
[Curated list of Go design patterns, recipes, and idioms](https://github.com/tmrts/go-patterns)
[awesome-go](https://github.com/avelino/awesome-go)
[GoBooks](https://github.com/dariubs/GoBooks)
[Design Patterns / Refactoring](https://refactoring.guru/)