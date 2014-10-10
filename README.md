#Go Cheat Sheet
>Useful cheat sheet for Go programming language(syntax, features, examples and more).

##Credits
Most of the examples taken from [A Tour of Go](http://tour.golang.org/) and [The Golang spec](https://golang.org/ref/spec)

##Table of contents:
- [Structs](#structs)
- [Arrays](#arrays)
- [Slices](#slices)
- [For statements](#for-statements)
- [Functions](#functions)
- [Range](#range)
- [Maps](#maps)
- [Go Builtin](#go-builtin)
- [Goroutines](#goroutines)
- [Channels](#channels)


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
