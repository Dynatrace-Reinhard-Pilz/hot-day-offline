## Reporting Metrics
Apart from getting deeper insight into what's going on within an application on a transactional level, OpenTelemetry also covers the ability to gather metrics.
In this lesson we learn how to enable the OpenTelemetry Metric Exporter for Dynatrace.

### Prerequisites
- You have completed the previous task `Creating Instrumentation Libraries`

### Step 1: Initialize the Dynatrace Metrics Provider for OpenTelemetry
- Delete Line 18 and Line 20 in `main.go`. This way `initMetricsProvider()` will get called upon process start
- Function `main` in `main.go` should now look like this:
  ```go
    func main() {
        if err := initGlobalTracer(nil); err != nil {
            panic(err)
        }
        initMetricsProvider()
        http.HandleFunc("/fib", FibServer)
        http.HandleFunc("/favicon.ico", faviconHandler)
        http.ListenAndServe(":28080", nil)
    }
  ```
- Press `Ctrl-S` in order to save the changes you've made in `main.go`
- Line 21 within `metrics.go` requires the endpoint URL for metric ingest of your Dynatrace environment to be specified
    - You can find the host name of your Dynatrace environment in the address line of your browser when navigating within the Dynatrace WebUI
    - Example: `https://abc12345.sprint.dynatracelabs.com/api/v2/metrics/ingest`
- Line 18 within `metrics.go` requires you to specify an API Token. It is required for authentication.
    - In the Dynatrace WebUI navigate to `Settings` > `Integration` > `Dynatrace API`
    - Generate a new API Token here
    - The access scope (= permissions) needs to include `Ingest metrics`
    - The name of the API Token can get chosen freely
    - You can now copy the API Token to your clipboard and paste into the String on Line 18 of `metrics.go`
    - ![API Token](../../../assets/images/api_token.png)
- Shut down `fib.exe` using `Ctrl-C` within your Terminal if you haven't done so already.
- Compile the program using the command line `go build`. This produces a file name `fib.exe` within the current directory.
- Launch `fib.exe` via your current Terminal
- In your Browser or via `curl` create a series of requests like these. Feel free to repeat some of these requests a couple of times
  - `http://localhost:28080/fib?n=1`
  - `http://localhost:28080/fib?n=2`
  - `http://localhost:28080/fib?n=3`
  - `http://localhost:28080/fib?n=4`
  - `http://localhost:28080/fib?n=5`
  - `http://localhost:28080/fib?n=6`
  - `http://localhost:28080/fib?n=7`
  - `http://localhost:28080/fib?n=8`
  - `http://localhost:28080/fib?n=9`
  - `http://localhost:28080/fib?n=10`
  - `http://localhost:28080/fib?n=11`
- Within the Dynatrace WebUI select `Create custom chart` in the left hand side menu
- On top of your screen you're getting the option to `Analyze multidimensional metrics from Prometheus, StatsD and others channels right on your dashboards`. Click on `Try it out` in order to launch the `Data explorer`.
- ![Custom Chart](../../../assets/images/custom_chart.png)
- Click on the Dropdown box `Filter metrics by` and type in `otel`. You should now be able to select the metric `otel.fibonacci.iterations`.
- Click on the input box `Split by`. You should be able to select `input` here.
- For this specific kind of metric it makes sense to select `Pie` as the Visualization on the right hand side menu
- ![Data Explorer](../../../assets/images/data_explorer.png)
- Click on `Run query`

### You've arrived
- You have successfully reported a metric via OpenTelemetry and charted it in Dynatrace
- Like with additional Spans you can expect third party libraries to already have chosen a set of metrics for you, that are worth reporting.
- The only job left for you is to initialize the Dynatrace Exporter.
