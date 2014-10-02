#Go Cheat Sheet
>Useful cheat sheet for Go programming language(syntax, features, examples and more).

##Table of contents:
- [Structs](#structs)


##Structs
**Defining and accessing variables:**
```go
package main

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
