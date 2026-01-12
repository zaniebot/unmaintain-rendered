```yaml
number: 12212
title: "Revert \"[`flake8-bandit`] Detect `httpx` for `S113` (#12174)\""
type: pull_request
state: closed
author: trim21
labels: []
assignees: []
base: main
head: revert-httpx-S113
created_at: 2024-07-06T03:02:07Z
updated_at: 2024-07-06T04:05:39Z
url: https://github.com/astral-sh/ruff/pull/12212
synced_at: 2026-01-12T15:55:40Z
```

# Revert "[`flake8-bandit`] Detect `httpx` for `S113` (#12174)"

---

_@trim21_

This reverts PR `feat(rules): detect httpx for S113` #12174


## Summary

S113 exists because `requests` doesn't have a default timeout, so request without timeout may hang indefinitely

> B113: Test for missing requests timeout
This plugin test checks for requests or httpx calls without a timeout specified.
>
> Nearly all production code should use this parameter in nearly all requests, **Failure to do so can cause your program to hang indefinitely.**


But httpx has default timeout 5s, so S113 for httpx request without `timeout` argument is a false positive, only valid case would be `timeout=None`.

https://www.python-httpx.org/advanced/timeouts/

> HTTPX is careful to enforce timeouts everywhere by default.
>
> The default behavior is to raise a TimeoutException after 5 seconds of network inactivity.


## Test Plan

No need, just revert


---

_Closed by @trim21 on 2024-07-06 04:05_

---
