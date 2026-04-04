**Tags:**
#programming #programming-note #go #error-handling 
**Docs:**
 [Panic](https://go.dev/doc/effective_go#panic) | [Os.Exit](https://pkg.go.dev/os#Exit)
___
In Go, both `panic` and `os.Exit` are used to handle situations where a program should stop executing, but they do so in very different ways and are used in different contexts.

---

## 🔥 `panic`

`panic` is used when something **unexpected or unrecoverable** happens during execution. When a `panic` occurs:

- The current function stops executing.
    
- Deferred functions are executed **in reverse order**.
    
- The program unwinds the stack and crashes with a message unless recovered.
    

### ✅ Use `panic` when:

- You're handling a **programming error** (e.g., nil pointer dereference, invalid state).
    
- You're writing a **library** and want the application to be aware something went very wrong.
    
- You want deferred functions to **run before termination** (e.g., cleanups, logs).
    

### 🔍 Example:

```go
package main

import "fmt"

func main() {
    defer fmt.Println("Deferred function runs")

    fmt.Println("About to panic")
    panic("Something went wrong") // Will trigger deferred functions
    fmt.Println("This will not run")
}
```

**Output:**

```
About to panic
Deferred function runs
panic: Something went wrong
```

---

## 🚪 `os.Exit`

`os.Exit(code)` **immediately terminates** the program with the given status code.

- `0` means success.
    
- Any other value means failure.
    
- **No deferred functions are run**!
    

### ✅ Use `os.Exit` when:

- You're in the `main()` function or a CLI tool.
    
- You want to **cleanly exit** with a status code, e.g., after flag parsing or validation fails.
    
- You don't care about running deferred functions (or you've done all cleanup manually).
    

### 🔍 Example:

```go
package main

import (
    "fmt"
    "os"
)

func main() {
    defer fmt.Println("Deferred function (will NOT run)")

    fmt.Println("Exiting with os.Exit")
    os.Exit(1)
    fmt.Println("This will not run")
}
```

**Output:**

```
Exiting with os.Exit
```

---

## 🧠 Summary

|Feature|`panic`|`os.Exit`|
|---|---|---|
|Runs defers|✅ Yes|❌ No|
|Stack trace|✅ Yes (if uncaught)|❌ No|
|Exit code|❌ No control (defaults to 2)|✅ Customizable (e.g., `os.Exit(1)`)|
|Usage scenario|Programming errors, unexpected logic|CLI exit, manual validations|

---

## 🛠️ When to Use Which?

- Use `panic`:
    
    - In libraries when the caller must be aware of a critical failure.
        
    - When something impossible has happened and the program cannot continue meaningfully.
        
- Use `os.Exit`:
    
    - In CLI apps or scripts where you want to exit with a specific code.
        
    - When you're done and don’t need deferred cleanup.
        

Let me know if you want examples involving `recover` as well!