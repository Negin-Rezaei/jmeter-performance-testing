# JSONPlaceholder API – Performance Test (Apache JMeter)

A small **practice project** for getting familiar with Apache JMeter. It runs a
simple load test against the public [JSONPlaceholder](https://jsonplaceholder.typicode.com)
REST API and checks that each request returns the expected status code.

> Scope: this is a learning exercise, not a full performance-engineering suite.

## What it does

| Step | Request        | Check                     |
|------|----------------|---------------------------|
| 1    | `GET /posts/1` | HTTP status is `200`      |
| 2    | `POST /posts`  | HTTP status is `201`      |

**Load:** 10 virtual users, ramped up over 5 seconds, each running the two
requests 3 times, with a random think time (0.5–1.5 s) between iterations.

## Test plan structure

```
Test Plan
├── HTTP Request Defaults     (host + protocol)
├── HTTP Header Manager       (Content-Type: application/json)
└── Thread Group "API Users"  (10 users · ramp-up 5s · 3 loops)
    ├── GET /posts/1
    │   └── Response Assertion – 200
    ├── POST /posts
    │   └── Response Assertion – 201
    ├── Think Time (Uniform Random Timer)
    └── Summary Report
```

## How to run

**GUI (for editing / debugging):**

```
jmeter -t api-load-test.jmx
```

Add a *View Results Tree* listener while debugging to inspect individual
requests and responses.

**Headless (the right way to run a load test):**

```
jmeter -n -t api-load-test.jmx -l results/results.jtl -e -o results/report
```

Then open `results/report/index.html` for the HTML dashboard.

## Sample results

A sample run and its numbers are in [docs/RESULTS.md](docs/RESULTS.md).

## Requirements

- Apache JMeter 5.6.3+
- Java 8+ (Java 17 recommended)

## Notes

- JSONPlaceholder is a mock API – `POST /posts` returns `201` but does not
  actually store anything.
- `results/` is git-ignored.
