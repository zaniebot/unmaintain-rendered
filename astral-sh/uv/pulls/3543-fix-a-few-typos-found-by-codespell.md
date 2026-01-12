```yaml
number: 3543
title: Fix a few typos found by codespell
type: pull_request
state: merged
author: DimitriPapadopoulos
labels: []
assignees: []
merged: true
base: main
head: codespell
created_at: 2024-05-13T11:45:58Z
updated_at: 2024-05-13T11:58:33Z
url: https://github.com/astral-sh/uv/pull/3543
synced_at: 2026-01-12T16:05:42Z
```

# Fix a few typos found by codespell

---

_@DimitriPapadopoulos_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Just fix typos.

While `alpha-numeric` is not really a misspelling:
- it is missing from mainstream curated dictionaries, all of them suggest `alphanumeric`;
- it is less used than `alphanumeric` (more than ⨉10 less) according to the Google [Ngram Viewer](https://books.google.com/ngrams/graph?content=alpha-numeric%2Calphanumeric&year_start=1900&year_end=2019&corpus=en-2019);
- it is [missing from SCOWL](http://app.aspell.net/lookup?dict=en_US-large;words=alpha-numeric).

## Test Plan

CI jobs.

---

_@konstin approved on 2024-05-13 11:47_

---

_Merged by @konstin on 2024-05-13 11:55_

---

_Closed by @konstin on 2024-05-13 11:55_

---

_Branch deleted on 2024-05-13 11:58_

---
