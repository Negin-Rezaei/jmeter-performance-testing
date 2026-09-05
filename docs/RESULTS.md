# Sample run results

Configuration: 10 users · ramp-up 5 s · 3 loops → 60 requests total
Target: `https://jsonplaceholder.typicode.com`

| Label        | Samples | Avg (ms) | Min | Max  | Error % | Throughput (/s) |
|--------------|---------|----------|-----|------|---------|-----------------|
| GET /posts/1 | 30      | 625      | 506 | 888  | 0.00%   | 1.94            |
| POST /posts  | 30      | 994      | 610 | 8656 | 0.00%   | 1.94            |
| **TOTAL**    | 60      | 809      | 506 | 8656 | 0.00%   | 3.49            |

Raw export: [summary.csv](summary.csv)

## Notes

- No errors – all status-code assertions passed.
- `POST /posts` shows a high max (8.6 s) with a large standard deviation: a few
  slow outliers against the public mock API, not a steady-state problem.
- These numbers reflect a public shared API over the internet, not a controlled
  test environment.

## Screenshots

### Summary Report

The full test plan tree is visible in the left panel.

![Summary Report](screenshots/summary-report.png)

### View Results Tree

Every sample passed its status-code assertion (green icons).

![View Results Tree](screenshots/results-tree.png)
