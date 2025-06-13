**Tags:**
#programming #programming-note #programming-concept #go #testing
**Reference**
- [TableDrivenTest](https://go.dev/wiki/TableDrivenTests)
- [Testing in Go with table driven test and Testify](https://dev.to/zpeters/testing-in-go-with-table-drive-tests-and-testify-kd4)
___

- In Table-driven Test, Each table entry is a complete test case with inputs and expected results, and sometimes with additional information such as a test name to make the test output easily readable. 
- Given a table of test cases, the actual test simply iterates through all table entries and for each entry performs the necessary tests. The test code is written once and amortized over all table entries, so it makes sense to write a careful test with good error messages.
- Table driven testing is not a tool, package or anything else, it’s just a way and perspective to write cleaner tests.

# Example

`mylib.go`
```go
package mylib

import "fmt"    

func MyFunction(in string) (out string, err error) {
    out = fmt.Sprintf("Hello: %s", in)
    return out, nil
}
```

`mylib_test.go` **(without table-driven test)**
```go
package mylib

import "testing"

func TestMyFunction(t *testing.T) {
    out, err := MyFunction("Bob")
    if err != nil {
        t.Errorf("Should not get an error")
    }
    if out != "Hello: Bob" {
            t.Errorf("Should have gotten expected output")
    }

}
```

`mylib_test.go` **(with table-driven test + testify)**
```go
package mylib

import (
    "testing"
    "github.com/stretchr/testify/require"
)

func TestMyFunction(t *testing.T) {
    var myTests = []struct {
        inputName string
        expectedOutput string
        expectedError error
    }{
        {inputName: "Bob",  expectedOutput: "Hello: Bob", expectedError: nil},
        {inputName: "Jian", expectedOutput: "Hello: Jian", expectedError: nil},
        {inputName: "지안",   expectedOutput: "Hello: 지안", expectedError: nil},
    }

    for _, tt := range myTests {
        actualOutput, actualError := MyFunction(tt.inputName)
        //if actualError != tt.expectedError {
        //  t.Errorf("Should not get an error")
        //}
        require.Equal(t, tt.expectedError, actualError, "optional message here")
        //if actualOutput != tt.expectedOutput {
        //  t.Errorf("Should have gotten expected output")
        //}
        require.Equal(t, tt.expectedOutput, actualOutput)
}
```

# Guides

```go
package mylib

import "testing"

func TestMyFunction(t *testing.T) {
    // myTests is a slice of structs
    // The struct holds our inputs and expected outputs
    // There is nothing special about the naming of the fields
    // it is just my own convention
    var myTests = []struct {
        inputName string
        expectedOutput string
        expectedError error
    }{
        // Now we define the test scenario/struct this form we are 
        // using is just a shorthand of defining the struct and initialising its
        // values at the same time. 
        // This is really just an elaborate form of:
        // var myString string = "My String"
        {inputName: "Bob",  expectedOutput: "Hello: Bob", expectedError: nil},
    }


    // Now that we have created our "test case" we need to actually test it
    // _ is the index give by range, we don't need this
    // tt is just a placeholder, it can be called anything.  I just 'tt' by convention
    // because it is easy to type!
    for _, tt := range myTests {
        // First lets get our output.  I like "actual" or sometimes "got" to distinguish it from
        // the "expected" output we'll compare it to
        // tt.inputName is the inputName field of whatever loop of
        // the testing struct we are on.  In this case "Bob"
        actualOutput, actualError := MyFunction(tt.inputName)
        // Now, much like we did before lets test our outputs and make sure
        // they match our expectations

        // Make sure the actual error matches our expectation, which is *nil*
        if actualError != tt.expectedError {
            t.Errorf("Should not get an error")
        }
        // Make sure our output matches
        if actualOutput != tt.expectedOutput {
            t.Errorf("Should have gotten expected output")
        }
}
```