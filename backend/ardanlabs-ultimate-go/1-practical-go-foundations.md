# [Practical Go Foundations](https://www.ardanlabs.com/training/ultimate-go/foundations/)

- Functions which are exported start with a capital letter, other internal functions start with a lower case letter.
- Go code is formatted using go fmt.
- Go executable files (generated using `go build`) are comparatively larger than executable files of some other languages (eg - C) because the executable contains Go runtime, garbage collector. Also, it is a statically linked executable, which means that it does not depend on any shared libraries. If it is moved to another machine with different libraries, it will still run.
