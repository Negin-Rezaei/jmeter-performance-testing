# JSONPlaceholder API – Performance Test (Apache JMeter)

A small load-test project built with **Apache JMeter 5.6.3** against the public
[JSONPlaceholder](https://jsonplaceholder.typicode.com) REST API. It exercises two
endpoints under concurrent load and validates both correctness and response time.

## Scenario

| Step | Request          | Assertions                                        |
|------|------------------|--------------------------------------------------|
| 1    | `GET /posts/1`   | HTTP 200 · JSON body `$.id == 1` · response < SLA |
| 2    | `POST /posts`    | HTTP 201 · response < SLA                         |

Each virtual user runs the two requests in sequence with a randomized think time
(0.5–1.5 s) between iterations.

## Test plan structure

```
Test Plan
├── HTTP Request Defaults        (host, protocol, timeouts)
├── HTTP Header Manager          (Content-Type / Accept: application/json)
└── Thread Group "API Users"
    ├── GET /posts/1
    │   ├── Response Assertion    – 200
    │   ├── JSON Path Assertion   – $.id = 1
    │   └── Duration Assertion    – < sla_ms
    ├── POST /posts
    │   ├── Response Assertion    – 201
    │   └── Duration Assertion    – < sla_ms
    ├── Uniform Random Timer      – think time
    └── Summary Report
```

## Configurable load (JMeter properties)

| Property     | Default | Meaning                                  |
|--------------|---------|------------------------------------------|
| `threads`    | 10      | Concurrent virtual users                 |
| `rampup`     | 5       | Ramp-up period (seconds)                 |
| `loops`      | 1       | Iterations per user                      |
| `scheduler`  | false   | Run for a fixed duration instead of loops|
| `duration`   | 60      | Duration in seconds (when `scheduler=true`) |
| `sla_ms`     | 2000    | Max acceptable response time (ms)        |
| `base_url`   | jsonplaceholder.typicode.com | Target host             |

## How to run (headless / CI mode)

```bash
# basic run
jmeter -n -t api-load-test.jmx -l results/results.jtl -e -o results/report

# custom load
jmeter -n -t api-load-test.jmx \
  -Jthreads=50 -Jrampup=20 -Jloops=10 -Jsla_ms=1500 \
  -l results/results.jtl -e -o results/report
```

Open `results/report/index.html` for the HTML dashboard.

> GUI mode (`jmeter -t api-load-test.jmx`) is for editing/debugging only – always
> run load tests in non-GUI mode.

## Requirements

- Apache JMeter 5.6.3+
- Java 8+ (Java 17 recommended)

## Notes

- JSONPlaceholder is a mock API: `POST /posts` returns `201` but does not
  actually persist data. The test is a demonstration of methodology, not a
  benchmark of a real backend.
- `results/` is git-ignored; commit a curated report separately if you want to
  share numbers.
