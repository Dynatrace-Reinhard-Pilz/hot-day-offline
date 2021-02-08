## Creating Instrumentation Libraries
With LESSION 02 we actually overshot.
First of all, we had to modify the existing source code of the Fibonacci Calculator. This is rarely possible for third party libraries. But even then you'd rather prefer to use the original libraries instead of patching them.
Second, our solution traces ALL the invocations of the `Calc` function. Trace sizes easily explode given a sufficiently high input value.

### Prerequisites
- You have completed the previous task `Instrumentation Libraries`
- Shut down `fib.exe` using `Ctrl-C` within your Terminal if you haven't done so already.

### Step 1: Restore the original Fibonacci Calculator and create a Wrapper
- Delete Lines 12 - 48 in `fibonacci.go`. We are getting rid of our patched version of of the Fibonacci Calculator
- Delete Lines 14 and 56 in `fibonacci.go`. What we're getting is the original source code of the Fibonacci Calculator - and a bit more on top
- Press `Ctrl-S` in order to save the changes you've made in `fibonacci.go`
- The changes we've made to `fibonacci.go` again are creating errors within `main.go`.
- Delete Line 45 and Line 65 in `main.go`. Between these two lines is yet another version of the function `FibServer`
- Delete Lines 26-43 in `main.go`. We're getting rid of the old version of `FibServer`
- Press `Ctrl-S` in order to save the changes you've made in `main.go`
- Shut down `fib.exe` using `Ctrl-C` within your Terminal if you haven't done so already.
- Compile the program using the command line `go build'`. This produces a file name `fib.exe` within the current directory.
- Launch `fib.exe` via your current Terminal
- Use either your Browser or `curl http://localhost:28080/fib?n=3` within a new Terminal to access your new HTTP Service

### You've arrived
- The PurePath you just created does no longer report a Span for every single invocation of the function `Calc`. Most importantly the code of the original Fibonacci Calculator remains untouched. You've managed to create an instrumentation library that performs the necessary work.
  ![OTelPurepathInstLib](../../../assets/images/OTelPurepathInstLib.png)
