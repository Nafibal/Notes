**Tags:**
#programming #programming-note #go #go-package 
**Reference:**
- [Golang Testify: require vs assert](https://medium.com/freethreads/golang-testify-require-vs-assert-b3bbfb4e0b8f)

___

Key Differences Summarized:

| Feature          | Assert                                 | Require                                        |
| ---------------- | -------------------------------------- | ---------------------------------------------- |
| Failure Handling | Continues test execution after failure | Stops test execution immediately after failure |
| Purpose          | Check internal logic & invariants      | Validate inputs & preconditions                |
| Use Case         | Multiple checks, collect all failures  | Critical conditions, fail-fast approach        |

When to Use Which:
- Use `assert` when you want to see all failures in a test run, even if some assertions fail.
- Use `require` when a failure means the test cannot continue meaningfully, and you want to stop execution immediately.