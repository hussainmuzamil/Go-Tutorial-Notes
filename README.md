
# Golang Notes

## Why Go?
- Build Time
- Fast Startup
- Performance And Efficiency
- Concurrency Model
- Static Typing and Compilation
- Running on Scaled and Distributed Applications

## Packages
- Programs start running in package `main`.
- By convention, the package name is the same as the last element of the import path. For instance, the `fmt` package comprises files that begin with the statement `package fmt`.
- **Exported Names**: In Go, a name is exported if it begins with a capital letter. When importing a package, you can refer only to its exported names. Any "unexported" names are not accessible from outside the package.

## Functions
- When two or more consecutive named function parameters share a type, you can omit the type from all but the last.
- Go's return values may be named. If so, they are treated as variables defined at the top of the function.
- A return statement without arguments returns the named return values. This is known as a "naked" return.
- Naked return statements should be used only in short functions, as they can harm readability in longer functions.

## Variables
- Inside a function, the `:=` short assignment statement can be used in place of a `var` declaration with an implicit type.
- Outside a function, every statement begins with a keyword (`var`, `func`, and so on).
- Constants cannot be declared with `:=` syntax.

## Strings
- The length of the string is the number of bytes required to represent the string, not the number of characters.  
  For example:
  ```go
  s := "elite"  // length = 5
  s := "èlite"  // length = 6
  ```
- Strings are immutable and can share the underlying storage.  
  Example:
  ```go
  s := "hello, world"
  hello := s[:5]
  world := s[7:]
  ```
  Here, `hello` and `world` are substrings of `s` and share its memory.
- Strings overload the addition operator (`+` and `+=`).
- Strings are passed by reference, so they aren't copied.
- Example of creating a new string:
  ```go
  s = strings.ToUpper(s) 
  ```
  This creates a new string with uppercase letters, and `s` points to it. The garbage collector will remove the old string if it’s no longer used.

## Arrays, Slices & Maps

### Arrays
- Arrays are typed by size, which is fixed at compile time.
- Arrays are comparable.
- Arrays are passed by value, so elements are copied.
- Assigning arrays of different sizes results in a type mismatch.

### Slices
- Slices have variable length, backed by some array.
- Slices are passed by reference, so no copying occurs, and updates are allowed.
- Slices are not comparable.

#### Sample Program
```go
package main
import "fmt"

func main() {
	fmt.Println("slices")
	var nums = make([]int, 2) // taking 3 args: type, size, capacity
	nums = append(nums, 1)
	nums = append(nums, 2)
	nums = append(nums, 3)
	nums = append(nums, 4)
	fmt.Println(nums)
	fmt.Println(len(nums))
	fmt.Println(cap(nums))

	var nums2 = make([]int, len(nums))
	copy(nums2, nums)
	fmt.Println(nums2)
	nums2 = append(nums2, 8, 9, 10, 11, 12, 13, 14)
	fmt.Println(len(nums2))
	fmt.Println(cap(nums2))

	var sliceOfSlice = nums[:3]
	fmt.Println(sliceOfSlice)
}
```

### Maps
- You can read from a nil map (returns the default value of the type of value), but inserting will cause panic.

#### Example
```go
var m map[string]int // nil, no storage
p := make(map[string]int) // non-nil but empty
a := p["the"]  // returns 0
b := m["the"]  // same thing
m["and"] = 1    // PANIC - nil map
m = p
m["and"]++      // OK, same map as p now
c := p["and"]   // returns 1
```

#### Sample Program
```go
package main
import "fmt"

func main() {
	m := make(map[string]string)

	m["name"] = "jack"
	m["age"] = "18"
	m["qualification"] = "bachelors"
	fmt.Println(m["name"])
	fmt.Println(m["age"])
	fmt.Println(m["qualification"])

	// Uncomment the lines below to test deletion and clearing.
	// delete(m, "age")
	// fmt.Println(m)
	// clear(m)
	// fmt.Println(m)
}
```

- Maps are passed by reference, so no copying occurs, and updates are allowed.
- The type used for the key must have `==` and `!=` defined (not slices, maps, or functions).

### Additional Notes
- Many built-ins are safe: `len`, `cap`, `range`.
