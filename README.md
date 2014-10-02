#Go Cheat Sheet
>Useful cheat sheet for Go programming language(syntax, features, examples and more).

##Table of contents:
- [Structs](#structs)
- [Functions](#functions)


##Structs
**Defining and accessing variables:**
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

func contains(str string, word string) (res string, i int){
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
	newStr, len:= contains("foo bar baz foo", "foo")
	println("trimmed string:", newStr)
	println("times of showing:", len)
}
```
