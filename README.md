# SQL Practice: Service Failure Counts

## Problem

Count failed requests for each service while retaining services with no failures.
This learning exercise shows why joining a service to several matching logs can
produce repeated rows, and how aggregation produces one summary row per service.

## Data

- Source: synthetic data embedded in [the SQL script](queries/service_failure_counts.sql).
- Scope: 3 services and 5 request logs; failure means `status >= 400` in this exercise.
- Tables: `services(id, name)` and `logs(id, service_id, status)`.
- Limitations: tiny sample, assumed unique service IDs, no real users or traffic.
  Unknown status values do not satisfy the failure filter. Orphan logs and
  duplicate service IDs are not validated or repaired.

## What I Built

A CTE and LEFT JOIN query with GROUP BY and COUNT, plus an explanation and
counterexample for a repeated-row bug. The completed exercise was packaged into
a standalone SQL file and an automated test runner with Codex assistance.
The original query logic is retained. Extra packaging checks are distinguished
from the original three checks in the test file.

## Methods

Filter failure logs in a CTE, keep `services` on the left, and group by service
ID and name. Count the matched `failures.service_id`, not `COUNT(*)`, so an
unmatched service gets zero. This implementation aggregates **after** joining;
it does not implement preaggregation or window functions.

## Validation

Run [the tests](tests/test_service_failure_counts.py) with the command below.
The original exercise covers a normal count, a two-match join counterexample,
and a zero-failure service. Packaging QA also checks unique output grain,
source-total reconciliation, a service without logs, and a NULL status.
These checks cover the stated examples, not every possible input.

## Results

| Service | Failure count |
| --- | ---: |
| login | 1 |
| search | 2 |
| profile | 0 |

The direct join returns two `search` rows. The corrected query returns three
service rows and three total failures. This is a local learning result, not a
production metric or business impact claim.

## Repository Structure

- `queries/service_failure_counts.sql`: sample tables, query, and validation queries.
- `tests/test_service_failure_counts.py`: reproducible checks using SQLite.
- `LEARNING_LOG.md`: verified learning evidence and next milestone.

## How to Reproduce

Requires Python 3 with its standard-library `sqlite3` module. No packages,
network access, credentials, or external data are required.
From this project directory:

```sh
python3 -B -m unittest discover -s tests -v
```

The runner executes the SQL file in a fresh in-memory SQLite database for each
test. Optionally, with the SQLite CLI installed, print the result and validation
tables directly:

```sh
sqlite3 :memory: < queries/service_failure_counts.sql
```

## Limitations and Next Steps

The parent folder reserves a future product-analytics project name, but this
version contains only the service-count exercise. Funnel, cohort, retention,
window functions, production performance, and website publication are not
completed here. A future exercise can compare this query with preaggregation
using independently explained tests. No progress is inferred from that plan.
