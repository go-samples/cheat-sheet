#RegExp
```go
func main() {
	r, _ := regexp.Compile("^[a-z|A-Z].+?\\d$")

	// string representation
	fmt.Println(r.String()) // ^[a-z|A-Z].+?\\d$

	// test match
	fmt.Println(r.MatchString("foobar"))  // false
	fmt.Println(r.MatchString("foobar0")) // true

	// replace matches
	r, _ = regexp.Compile("[b|B]a[z|r|t]")
	fmt.Println(r.ReplaceAllString("bar baz", "foo"))		// foo foo
	fmt.Println(r.FindAllString("Mr Bar has a baz and bat", 3))	// [Bar baz bar]

	// return a slice if substring between the expression matches
	// The `3` determines the number of substrings
	s := regexp.MustCompile("-").Split("bar-baz-bat", 3)
	fmt.Println(s)
}
```
