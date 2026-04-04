**Tags:**
#programming #package #go #go-package
**Reference:**
[Github](https://github.com/stretchr/testify) | [Docs](https://pkg.go.dev/github.com/stretchr/testify)

___

Testify is a testing package for golang. It has 3 main features: Assertion, Mocking, and Testing suite interfaces and function.
# Package Assertion
- Package assert provides a set of comprehensive testing tools for use with the normal Go testing system.
- Every assert func takes the `testing.T` object as the first argument. This is how it writes the errors out through the normal `go test` capabilities.
- Every assertion function also takes an optional string message as the final argument, allowing custom error messages to be appended to the message the assertion method outputs.
- Every assert func returns a bool indicating whether the assertion was successful or not, this is useful for if you want to go on making further assertions under certain conditions.
## Example
```go
package yours

import (
	"testing"

	"github.com/stretchr/testify/assert"
)

func TestSomething(t *testing.T) {
	// assert equality
	assert.Equal(t, 123, 123, "they should be equal")

	// assert inequality
	assert.NotEqual(t, 123, 456, "they should not be equal")

	// assert for nil (good for errors)
	assert.Nil(t, object)

	// assert for not nil (good when you expect something)
	if assert.NotNil(t, object) {
		// now we know that object isn't nil, we are safe to make
		// further assertions without causing any errors
		assert.Equal(t, "Something", object.Value)
	}
}
```
# Package Require
The require package provides same global functions as the assert package, but instead of returning a boolean result they terminate current test. These functions must be called from the goroutine running the test or benchmark function, not from other goroutines created during the test. Otherwise race conditions may occur.

See [`t.FailNow`](https://pkg.go.dev/testing#T.FailNow) for details.
# Package Mock
- The mock package provides a mechanism for easily writing mock objects that can be used in place of real objects when writing test code.
- Package mock provides a system by which it is possible to mock your objects and verify calls are happening as expected.
- You can use the [mockery tool](https://vektra.github.io/mockery/latest/) to autogenerate the mock code against an interface as well, making using mocks much quicker.
## example
```go
package yours

import (
	"testing"

	"github.com/stretchr/testify/mock"
)

/*
  Test objects
*/

// MyMockedObject is a mocked object that implements an interface
// that describes an object that the code I am testing relies on.
type MyMockedObject struct {
	mock.Mock
}

// DoSomething is a method on MyMockedObject that implements some interface
// and just records the activity, and returns what the Mock object tells it to.
//
// In the real object, this method would do something useful, but since this
// is a mocked object - we're just going to stub it out.
//
// NOTE: This method is not being tested here, code that uses this object is.
func (m *MyMockedObject) DoSomething(number int) (bool, error) {
	args := m.Called(number)
	return args.Bool(0), args.Error(1)
}

/*
  Actual test functions
*/

// TestSomething is an example of how to use our test object to
// make assertions about some target code we are testing.
func TestSomething(t *testing.T) {
	// create an instance of our test object
	testObj := new(MyMockedObject)

	// set up expectations
	testObj.On("DoSomething", 123).Return(true, nil)

	// call the code we are testing
	targetFuncThatDoesSomethingWithObj(testObj)

	// assert that the expectations were met
	testObj.AssertExpectations(t)
}

// TestSomethingWithPlaceholder is a second example of how to use our test object to
// make assertions about some target code we are testing.
// This time using a placeholder. Placeholders might be used when the
// data being passed in is normally dynamically generated and cannot be
// predicted beforehand (eg. containing hashes that are time sensitive)
func TestSomethingWithPlaceholder(t *testing.T) {
	// create an instance of our test object
	testObj := new(MyMockedObject)

	// set up expectations with a placeholder in the argument list
	testObj.On("DoSomething", mock.Anything).Return(true, nil)

	// call the code we are testing
	targetFuncThatDoesSomethingWithObj(testObj)

	// assert that the expectations were met
	testObj.AssertExpectations(t)

}

// TestSomethingElse2 is a third example that shows how you can use
// the Unset method to cleanup handlers and then add new ones.
func TestSomethingElse2(t *testing.T) {
	// create an instance of our test object
	testObj := new(MyMockedObject)

	// set up expectations with a placeholder in the argument list
	mockCall := testObj.On("DoSomething", mock.Anything).Return(true, nil)

	// call the code we are testing
	targetFuncThatDoesSomethingWithObj(testObj)

	// assert that the expectations were met
	testObj.AssertExpectations(t)

	// remove the handler now so we can add another one that takes precedence
	mockCall.Unset()

	// return false now instead of true
	testObj.On("DoSomething", mock.Anything).Return(false, nil)

	testObj.AssertExpectations(t)
}
```
