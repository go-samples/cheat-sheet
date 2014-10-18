# text/template
**pipeline:**  
the value of the pipeline is copied to the output
```go
import(
  	"text/template"
	"os"
)

type Inventory struct {
	Material string
	Count    uint
}

func main() {
	sweaters := Inventory{ "wool", 17 }
	tmpl, err := template.New("test").Parse("{{.Count}} items are made of {{.Material}}")
	if err != nil {
		panic(err)
	}

	err = tmpl.Execute(os.Stdout, sweaters)
	if err != nil {
		panic(err)
	}
}
```
**comment:**
```go
func main() {
	sweaters := Inventory{ "wool", 17 }
	tmpl, err := template.New("test").Parse("{{.Count}} items are made of {{.Material}}" +
		 "{{/* this is a comment */}}...")
	if err != nil {
		panic(err)
	}

	err = tmpl.Execute(os.Stdout, sweaters)
	if err != nil {
		panic(err)
	}
}
```
**condition:**
```go
func main() {
	i, _ := strconv.Atoi(os.Args[1])
	sweaters := Inventory{ "wool", i }
	tmpl, err := template.New("test").Parse("{{if .Count}}{{ .Count }} sweaters " +
			"{{else}} there's no sweaters in the inventory{{end}}")
	if err != nil {
		panic(err)
	}

	err = tmpl.Execute(os.Stdout, sweaters)
	if err != nil {
		panic(err)
	}
}
```
