#Go Cheat Sheet
>Useful cheat sheet for Go programming language(syntax, features, examples and more).

##Credits
Most of the examples taken from [A Tour of Go](http://tour.golang.org/), [The Golang spec](https://golang.org/ref/spec) and Google I/O talks.

##Table of contents:
- [Structs](#structs)
- [Arrays](#arrays)
- [Slices](#slices)
- [Constants](#constants)
- [For statements](#for-statements)
- [Functions](#functions)
- [Range](#range)
- [Maps](#maps)
- [Packages](#packages)
- [Go Builtin](#go-builtin)
- [Goroutines](#goroutines)
- [Channels](#channels)
- [Buffered Channels](#buffered-channels)
- [Select](#select)
- [Interfaces](#interfaces)
- [Reflection](#reflection)
- [Formatting](#formatting)


##Structs
**Defining and accessing structs:**
```go
type Man struct {
	name	string
	age		int
}

func main() {

	man1 := Man{"Ariel", 26}
	man2 := Man {
		age: 24,
		name: "Ben",
	}
	var man3 Man
	man3.name = "Dan"
	man3.age = 25

	//accessing structs
	for _, man := range []Man {man1, man2, man3} {
			println(man.name)
	}
}
```
**Methods for structs:**
```go
func (man Man) sayHello() {
	println("Hello buddy, my name is: ", man.name)
}

func main() {
	ariel := Man{ "Ariel", 26 }
	ariel.sayHello()
}
```
**Anonymous structs:**  
```go
func main() {
	// Cheaper and safer than using map[string]interface{}.
	list := struct {
		group	string
		users	[]string
	}{
		"Students",
		[]string{"Tom", "Joran", "Roger", "Cati"},
	}
	fmt.Println(list)
}
```

#Arrays
**Defining and accessing arrays:**
```go
func main() {
	//declare
	var arr [1]string
	//set
	arr[0] = "Hello World!"
	//read
	println(arr[0]) //Hello World!
	
	//declare and initialize
	arr1 := [2]int{1, 2}
	//the compiler count capacity for you
	arr2 := [...]int{3, 4} 
}
```
#Slices
**Defining and accessing slices:**
```go
func main() {
	str := []string {"Hello"}

	println(str[0]) // Hello
}
```
**Increase capacity of slice:**
```go
func main() {
	str := []string {"Hello"}
	str = append(str, "World!")

	println(str[0], str[1]) //Hello World!
}

```
**Slicing slices:** <br/>
Slices can be re-sliced, creating a new slice value that points to the same array.<br/>
**Usage:** `slice[low:high]`

```go
import "fmt"

func main() {
	arr := []int{2,4,6,8,10,12,14}
	
	fmt.Println(arr[1:4]) // slice from index 1 to 3, Result: [4 6 8]
	fmt.Println(arr[:4]) // missing low index implies 0, Result: [2 4 6 8]
	fmt.Println(arr[4:]) // missing high index implies len(a), Result: [10 12 14]
}

```
**Making slices:**<br/>
**Usage:** `make(T, n)` or `make(T, n, m)`
```go
func printSlice(x []int) {
	fmt.Printf("%v, length=%d, capacity=%d\n",
		x, len(x), cap(x))
}

func main() {

	a := make([]int, 5)
	printSlice(a) //[0 0 0 0 0], length=5, capacity=5

	b := make([]int, 0, 5)
	printSlice(b) //[], length=0, capacity=5

	//We reslice the slice to add 5 to its original length
	//We can do this because cap(slice) == 5
	b = b[:cap(b)]
	for i := 0; i < cap(b); i++ {
		b[i] = i
	}
	printSlice(b) //[0 1 2 3 4], length=5, capacity=5

	c := b[:2]
	printSlice(c) //[0 1], length=2, capacity=5

	d := c[2:5]
	printSlice(d) //[2 3 4], length=3, capacity=3

}
```
#Constants
constants can be character, string, boolean, or numeric values.  
**Note:** constants cannot be declared using the `:=` syntax.
```go
type ByteSize float64

const (
        _ = iota // Ignore first value(i.e: 0)
        KB ByteSize = 1 <<(10 * iota)
        MB
        GB
        TB
        PB
)

func main() {
        fmt.Println(KB, MB, GB, TB, PB) // 1024 1.048576e+06 1.0737418....
}
```
#For statements<br/>
Go has only one looping construct, the for loop. (the `{ }` are required!)
```go
func main() {
	for i := 0; i < 10; i++ {
		println(i)
	}
	
	//You can leave the pre and post statements empty.
	sum := 1
	for ; sum < 1000; {
		sum += sum
	}
	fmt.Println(sum)
}
```
**While statement:**<br/>
`for` is Go's `while`
```go
func main() {
	sum := 1
	for sum < 1000 {
		sum += sum
	}
	fmt.Println(sum)
}
```
**Forever:**
```go
func main() {
    for {
    }
}
```
#Functions
**Declaration:**
```go
func printTwice(str string) {
	println(str, str)
}

func main() {
	printTwice("foo")
}
```
**Returing value:**
```go
func add(x int, y int) int {
	return x + y
}

func main() {
	println(add(10, 20))
}
```
**Returing multiple values:**
```go
import "strings"

func trim(str string, word string) (res string, i int){
	//splits the string
	fields := strings.Fields(str)

	for _, e := range fields {
		if(e == word) {
			i++
			continue
		}
		res += e + " "
	}
	return res, i
}

func main() {
	//calling a function that return multiple values
	newStr, len:= trim("foo bar baz foo", "foo")
	println("trimmed string:", newStr)
	println("times of showing:", len)
}
```
#Range
The range form of the for loop iterates over a slice or map.
```go
func main() {
	var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
	
    	for i, v := range pow {
		fmt.Printf("2**%d = %d\n", i, v)
    	}
    
    	//if you only want the index, drop the "v" entirely.
    	for i := range pow {
        	fmt.Printf(i)
    	}
    
    	//or, you can skip the index or value by assigning to _
    	for _, value := range pow {
        	fmt.Printf("%d\n", value)
    	}
}
```
#Maps
Maps must be created with make (not new) before use
```go
func main() {
	foo := make(map[string]int)
	foo["a"] = 1
	foo["b"] = 2
	foo["c"] = 3

	println(foo["a"], foo["b"], foo["c"]) //1 2 3
}
```
**Map literals:**<br/>
Map literals are like struct literals, but the keys are required
```go
type Human struct {
	name 	string;
	gender 	string;
	age	 	int;
}

func main() {
	group := map[int8]Human {
		0: Human {
			"Ariel M.", "Man", 26,
		},
		//you can omit the type from the element literal
		1: {
			"Dan J.", "Man", 45,
		},
	}

	fmt.Println(group[0], group[1]) //{Ariel M. Man 26} {Dan J. Man 45}
}
```
#Packages
**import** keyword:
```go
import "fmt"
import "math"

//or use:
import (
	"fmt"
	"math"
)
```
define local name("f") to "fmt" package:
```go
import f "fmt"

func main() {
	f.Println(`Hello World!`)
}
```
If an explicit period (.) appears instead of a name, all the package's exported identifiers will be declared in the current file's file block and can be accessed without a qualifier.  
```go
import . "fmt"

func main() {
	Println(`Hello World!`)
}

```
#Go Builtin  
**new:** The expression `new(T)` allocates a zeroed T value and returns a pointer to it.
```go
type Human struct {
	name 	string;
	gender 	string;
	age	 	int;
}

func main() {
	me := new(Human)
	fmt.Println(*me)//{  0}

	me.name, me.gender, me.age = "Ariel", "Male", 26
	fmt.Println(*me)//{Ariel Male 26}
}
```
#Goroutines
**What is goroutine?**  
It's an independently executing function, lunched by go statement.  
It has it's own call stack, which grows and shrinks as required.  
It's very cheap, It's practical to have thousands, even hundreds of thousands of goroutines.  

It's not thread.  
There might be only one thread in a program with thousands goroutines.  

Instead, goroutines are multiplexed dynamically onto threads as needed to keep all goroutines running.  
But if you think of it as a very cheap thread, you won't be far off.  
<i>**Credits:** Rob Pike presentation</i>  
```go
import(
    t "time"
    f "fmt"
)

func say(s string) {
    for i := 0; i < 5; i++ {
        t.Sleep(100 * t.Millisecond)
        f.Println(s)
    }
}

func main() {
    // The `go` statement runs a function as usual, but doesn't make the caller wait.
    // This functionality is analogous to the & on the end of the shell command
    go say("world")
    say("hello")
}
```
#Channels
A channel in Go provides a connection between two goroutines, allowing them to communicate.  
By default, sends and receives block until the other side is ready.
```go
//Declaring and initializing
var c1 chan int
c1 = make(chan int)
//or
c2 := make(chan int)
	
//Sending on a channel
c1 <- 1
c2 <- 2
	
//Receiving from a channel
//The arrow(<-) indicates the direction of data flow
res1, res2 := <-ch, <-ch
```
**Example:**
```go
func seq(num int, c chan int) {
	x, y := 0, 1
	for num != 0 {
		x, y = y, x + y
		num--
	}
	c <-x
}

func main() {
	//channels must be created before use
	ch := make(chan int)
	go seq(50, ch)
	go seq(20, ch)

	res1, res2 := <-ch, <-ch
	println(res1, res2) //12586269025 6765
}
```

**Ping pong table example:**  
<i>**Note:** this example was presented at Google I/O 2013 by Sameer Ajmani</i>
```
type Ball struct {
	hits	int
}

func player(name string, table chan *Ball) {
	for {
		ball := <-table
		ball.hits++
		fmt.Println(name, ball.hits)
		time.Sleep(100 * time.Millisecond)
		table <- ball
	}
}

func main() {
	table := make(chan *Ball)
	go player("ping", table)
	go player("ping", table)

	table <- new(Ball) // Game on; toss the ball
	time.Sleep(time.Second)
	<-table // Game over; grab the ball
}
```

**Rob Pike example:**
```go
// When the main function executes <-c, it will wait for a value to be sent.
// Similarly, when the boring function executes c <- value, it waits for a receiver to be ready.
// A sender and receiver must both be ready to play their part in the communication. 
// Otherwise we wait until they are.
// Thus channels both communicate and synchronize.

func boring(msg string, c chan string) {
	for i := 0; ; i++ {
		c <- fmt.Sprintf("%s %d", msg, i) //Expression to be sent can be any suitable value.
		time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
	}
}

func main() {
	c := make(chan string)
	go boring("boring!", c)

	for i:= 0; i < 5; i++ {
		fmt.Printf("You say: %q\n", <-c) //Receive expression is just a value
	}
	fmt.Println("You're boring! I'm leaving...")
}
```
**Generator:** function that returns a channel.  
Channels are first-class values, just like strings and integers.  
```go
func boring(msg string) <-chan string { // Returns receive-only channel of strings.
	c := make(chan string)
	go func() { // We launch the goroutine from inside the function
		for i := 0; ; i++ {
			c <- fmt.Sprintf("%s %d", msg, i) //Expression to be sent can be any suitable value.
			time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond)
		}
	}()
	return c // Return the channel to the caller
}

func main() {
	joe, ann := boring("Joe"), boring("Ann")
	for i:= 0; i < 5; i++ {
		fmt.Printf("Joe say: %q\n", <-joe)
		fmt.Printf("Ann say: %q\n", <-ann)
	}
	fmt.Println("You're both boring; I'm leaving...")
}
```
**Multiplexing:**
These programs make Joe and Ann count in lockstep.  
We can instead use a fan-in function to let whosoever is ready talk.  
```go
func fanIn(input1, input2 <-chan string) <-chan string {
	c := make(chan string)
	go func () { for { c <- <-input1 } }()
	go func () { for { c <- <-input2 } }()
	return c
}

func main() {
	// We actually decoupled the execution
	c := fanIn(boring("Joe"), boring("Ann"))
	for i:= 0; i < 20; i++ {
		fmt.Println(<-c)
	}
	fmt.Println("You're both boring; I'm leaving...")
}
```
Lets Rewrite our fanIn function using select, (only one goroutine is needed).  
see: [Select statement](#select)
```go
func fanIn(input1, input2 <-chan string) <-chan string {
	c := make(chan string)
	go func() {
		for {
			select {
			case s := <-input1: c <- s
			case s := <-input2: c <- s
			}
		}
	}()
	return c
}
```
`range` and `close`:  
A sender can close a channel to indicate that no more values will be sent.  
Receivers can test whether a channel has been closed by assigning a second parameter to the receive expression.  
**Usage:**
```go
// ok will be false if there are no more values to receive 
// and the channel is closed.
v, ok := <-ch
```
```go
func fibonacci(n int, c chan int) {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        c <- x
        x, y = y, x+y
    }
    close(c)
}

func main() {
    c := make(chan int, 10)
    go fibonacci(cap(c), c)
    //the loop receives values from the channel repeatedly until it is closed.
    for i := range c {
        fmt.Println(i)
    }
}
```
**Note:**
* Only the sender should close a channel.
* You don't usually need to close the channels. closing is only necessary when the receiver must be told there are no more values coming, e.g: range loop.

#Buffered Channels
Go channels can be also be created with a buffer.  
Buffering removes synchronization.  
Buffering makes them more like Erlang's mailboxes.  

Buffered channels can be important for some problems, but they are more subtle to reason about.  
Sends to a buffered channel block only when the buffer is full. Receives block when the buffer is empty.  
**Usage:** `ch := make(chan int, length)`  
```go
func main() {
    c := make(chan int, 2)
    c <- 1
    c <- 2
    fmt.Println(<-c)
    fmt.Println(<-c)
}
```
#Select
A control structure unique to concurrency.  
The reason channels and goriutines are built in into the language.  

The select statement provides another way to handle multiple channels.  
It's like a switch statement, but each case is a communication.  
* All channels are evaluated.  
* Selection blocks until one communication can proceed, which then does.  
* If multiple can proceed, select choose pseudo-randomly.  
* A default clause, if present, executes immediately if no channel is ready.  
```go
func main() {
	c1, c2, c3 := boring("foo"), boring("bar"), boring("baz")
	time.Sleep(time.Duration(rand.Intn(1e3)) * time.Millisecond) // Add timeout
	select {
	case v1 := <-c1:
		fmt.Printf("Received %v from c1\n", v1)
	case v2 := <-c2:
		fmt.Printf("Received %v from c2\n", v2)
	case v3 := <-c3:
		fmt.Printf("Received %v from c3\n", v3)
	default:
		fmt.Printf("No one was ready to communicate\n")
	}
}
```
#Interfaces
An interface type is defined by a set of methods.  
A value of interface type can hold any value that implements those methods.  
```go
type Person struct {
	Name string
	Age  int
}

func (p Person) String() string {
	return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
}

func main() {
	a := Person{"Arthur Dent", 42}
	z := Person{"Zaphod Beeblebrox", 9001}
	
	// Stringer is implemented by any value that has a String method,
	// which defines the `native` format for that value.
	// The String method is used to print values passed as an operand 
	// to any format that accepts a string or to an unformatted printer such as Print.
	fmt.Println(a, z)
	// Print: `Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)`
}
```
**Polymorphism in Go:**
```go
// Create Shaper interface that has a single function
// `Area` that returns an int.
type Shaper interface {
	Area() int
}

type Rectangle struct {
	length, width int
}

type Square struct {
	side int
}

// Rectangle and Square have the function `Area` with the same
// signature of Shaper.
// Therefore, Rectangle and Sqaure implements the interface Shaper.
func (r Rectangle) Area() int {
	return r.length * r.width
}

func (s Square) Area() int {
	return s.side * s.side
}

func main() {
	r := Rectangle{length: 15, width: 15}
	s := Square{side: 10}
	// Create slice of shapes
	shapes := [...]Shaper{r, s}
	// Iterate over the slice and print the area of each shape
	for _, shape := range shapes {
		fmt.Println(`Area of shape: `, shape.Area())
	}
}
```
#Formatting
**Indentation:** Use tabs (width = 8) for indentation and blanks for alignment. Use spaces only if you must.  
**Line length:** Go has no line length limit. Don't worry about overflowing a punched card. If a line feels too long, wrap it and indent with an extra tab.  
