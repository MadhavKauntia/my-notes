# [Practical Go Foundations](https://www.ardanlabs.com/training/ultimate-go/foundations/)

- Functions which are exported start with a capital letter, other internal functions start with a lower case letter.
- Go code is formatted using go fmt.
- Go executable files (generated using `go build`) are comparatively larger than executable files of some other languages (eg - C) because the executable contains Go runtime, garbage collector. Also, it is a statically linked executable, which means that it does not depend on any shared libraries. If it is moved to another machine with different libraries, it will still run.
- Go uses byte and rune to represent character values. The byte data type represents ASCII characters while the rune data type represents a more broader set of Unicode characters that are encoded in UTF-8 format.
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
- On printing with a '#v' (eg - `fmt.Printf("%#v", r)`), the datatype of the variable is printed as well.
- `defer` can be used to perform an action when a function exits. For example, when we are opening a file, we can defer closing the stream. This will not execute on the line it is called, but instead will execute only when the function exits.
- defers are called in LIFO order.
- Slices have a property called `cap` which specifies the capacity of the slice. This property is helpful while appending a value to the slice. It helps Go determine if there is enough capacity or if space needs to be reallocated.