#Go Cheat Sheet
>Useful cheat sheet for Go programming language(syntax, features, examples and more).

##Credits
Most of the examples taken from [A Tour of Go](http://tour.golang.org/) and [The Golang spec](https://golang.org/ref/spec)

##Table of contents:
- [Structs](#structs)
- [Arrays](#arrays)
- [Slices](#slices)
- [Functions](#functions)


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
