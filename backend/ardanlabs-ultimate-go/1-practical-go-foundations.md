# [Practical Go Foundations](https://www.ardanlabs.com/training/ultimate-go/foundations/)

## Introduction
- Functions which are exported start with a capital letter, other internal functions start with a lower case letter.
- Go code is formatted using go fmt.
- Go executable files (generated using `go build`) are comparatively larger than executable files of some other languages (eg - C) because the executable contains Go runtime, garbage collector. Also, it is a statically linked executable, which means that it does not depend on any shared libraries. If it is moved to another machine with different libraries, it will still run.
- Go uses byte and rune to represent character values. The byte data type represents ASCII characters while the rune data type represents a more broader set of Unicode characters that are encoded in UTF-8 format.
- On printing with a '#v' (eg - `fmt.Printf("%#v", r)`), the datatype of the variable is printed as well.
- Type Assertion in Go:
```go
i := "hi"

s := i.(string) // will not panic
fmt.Println("s:", s) // s: hi

n, ok := i.(int) // will panic
if ok {
	fmt.Println("n:", n)
} else {
	fmt.Println("not an int") // this will get printed
}
```

## Calling REST APIs
- | JSON | Go |
   | ------ | --- |
   | true/false | true/false |
   | string | string |
   | null | null |
   | number | float64, float32, int8, int16, int32, int64, int, uint, ... |
   | array | []any ([]interface{}) |
   | object | map[string]any, struct |	

- Encoding/JSON API
    JSON : io.Reader -> Go: json.Decoder
    JSON -> []byte -> Go: json.Unmarshal
    Go -> io.Writer -> JSON: json.Encoder
    Go -> []byte -> JSON: json.Marshal

## Working with Files
- `defer` can be used to perform an action when a function exits. For example, when we are opening a file, we can defer closing the stream. This will not execute on the line it is called, but instead will execute only when the function exits.
- defers are called in LIFO order.

## Slices
- Slices have a property called `cap` which specifies the capacity of the slice. This property is helpful while appending a value to the slice. It helps Go determine if there is enough capacity or if space needs to be reallocated.
- To create a new slice with specified length, we can use `make([]int, len)`. This can be used to create a variable of any type.
- Slices can be divided using `slice[i1:i2]`. This returns a reference to the `slice` variable from indices `i1` to `i2-1`.

## Structs
- Suppose a function returns a reference to a struct/variable. In this case, the variable will outlive  the function. Normally, the variables in a function are stored in the stack, but in this case, the Go compiler will perform *escape analysis* and will allocate that variable on the heap.
- If you want to mutate a struct, use pointer receiver.
```go

i := Item{X = 0, Y = 0}
i.Move(100, 200)

// this will not work
func (i Item) Move(x, y int) {
	i.X = x
	i.Y = y
}

// this will work
func (i *Item) Move(x, y int) {
	i.X = x
	i.Y = y
}
```
- If there is a struct `a`  inside another struct `b`, properties of `a` can be accessed directly from `b` unless there is a duplicate variable, in which case overshadowing will occur.
- When you implement an interface in Go, you don't have to declare it. You just declare a function and Go automatically detects that it is an overriden method.
- Go's version of enum:
```go
type Key byte

const(
	JADE Key = iota + 1
	COPPER
	CRYSTAL
)

// this function overrides the String() function in fmt.Stringer
// this is Go's equivalent of Java's toString()
func (k Key) String() string {
	switch k {
		case Jade: return "jade"
		case Copper: return "copper"
		case Crystal: return "crystal"
	}
	return fmt.Sprintf("<Key %d>", k)
}

func main() {
	k := Jade
	fmt.Println("k:", k) // k: jade
	fmt.Println("key:", Key(17)) // key: <Key 17>
}
```

## Panics
- We can set named return values in Go. This makes return values accessible locally within the function.
- A function to perform division and recover from panics in case the denominator is zero.
```go
func safeDivision(a, b int) (q int, err error) {
	// catching panics in Go
	defer func() {
		if e := recover(); e != nil {
			log.Println("ERROR:", e)
			err = fmt.Errorf("%v", e)
		}
	}()
	return a / b, nil
}

func main() {
	fmt.Prinlln(safeDivision(1, 0)) // 0 runtime error: integer divide by zero
	fmt.Println(safeDivision(7, 2)) // 3 <nil>
}
```

## Distributing Work
- **Concurrency** - Concurrency is dealing with multiple things at the same time. For example, when you're cooking, you may be checking the oven and the stove too. You're a single person, taking care of multiple things at once.
- **Parallelism** - Parallelism is actually doing multiple things at the same time. For example, you're cooking and your wife is chopping the veggies.
- Refer [this](https://www.ardanlabs.com/blog/2018/08/scheduling-in-go-part2.html) article to understand how scheduling works in Go.
- `go <function>` runs the function as a goroutine.

```go
/* Go Runtime does not wait for goroutines */

// Output:
//  main
func main1() {
	go fmt.Println("goroutine")
	fmt.Println("main")
}

// Output: 
//	main
//  goroutine
func main2() {
	go fmt.Println("goroutine")
	fmt.Println("main")

	time.Sleep(10 * time.Millisecond)
}
```
