# Learning Log

## 2026-09-19 — Week 4 (Sep 13–19): service failure counts

Week numbering follows the existing study-material folder, not completion of
four project stages. This is the first packaged project entry.

- **Learned:** CTE scope, LEFT JOIN row multiplication, grouped counts, and why
  counting a matched column preserves a zero for an unmatched service.
- **Built:** the service failure query and corrected explanation in the completed
  Python/SQLite exercise. The SQL was extracted and the project organized with
  Codex assistance; that packaging is not an additional personal skill claim.
- **Verified:** all 7 automated tests passed on 2026-09-19 using
  `python3 -B -m unittest discover -s tests -v`. The standalone command
  `sqlite3 :memory: < queries/service_failure_counts.sql` also ran successfully,
  returning 3 service rows and 3 total failures, matching the source count.
  Four of the seven tests are additional Codex packaging QA, not original
  submitted exercise checks.
- **Evidence:** `queries/service_failure_counts.sql` and
  `tests/test_service_failure_counts.py`.
- **What I Can Explain Now:** why the direct join returns two search rows, why
  grouping produces a single service summary, and why COUNT of the matched
  column gives zero for the unmatched row. Personal participation and ability
  to explain the submitted exercise were confirmed during review.
- **Next Milestone:** compare a preaggregated version against the same sample,
  explain its join grain, and test it. Not yet completed.
