```yaml
number: 12213
title: "[`flake8-bandit`] fix S113 false positive for httpx without `timeout` argument"
type: pull_request
state: merged
author: trim21
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-s113-httpx
created_at: 2024-07-06T04:05:36Z
updated_at: 2024-07-06T19:56:16Z
url: https://github.com/astral-sh/ruff/pull/12213
synced_at: 2026-01-12T15:55:40Z
```

# [`flake8-bandit`] fix S113 false positive for httpx without `timeout` argument

---

_@trim21_


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

snap updated


---

_Comment by @mkniewallner on 2024-07-06 16:21_

We should also probably update [the fixture](https://github.com/astral-sh/ruff/blob/9d617272892bf3f3cd7777c350208c2eccc7e88e/crates/ruff_linter/resources/test/fixtures/flake8_bandit/S113.py#L44-L71) to move the calls that don't pass `timeout` to the "OK" section.

---

_Comment by @trim21 on 2024-07-06 17:14_

> We should also probably update [the fixture](https://github.com/astral-sh/ruff/blob/9d617272892bf3f3cd7777c350208c2eccc7e88e/crates/ruff_linter/resources/test/fixtures/flake8_bandit/S113.py#L44-L71) to move the calls that don't pass `timeout` to the "OK" section.

make sense

---

_Label `bug` added by @charliermarsh on 2024-07-06 19:08_

---

_Renamed from "[flake8-bandit] fix S113 false positive for httpx without `timeout` argument" to "[`flake8-bandit`] fix S113 false positive for httpx without `timeout` argument" by @charliermarsh on 2024-07-06 19:08_

---

_Comment by @charliermarsh on 2024-07-06 19:08_

Thank you! Sorry that we missed this initially.

---

_Merged by @charliermarsh on 2024-07-06 19:08_

---

_Closed by @charliermarsh on 2024-07-06 19:08_

---

_Branch deleted on 2024-07-06 19:56_

---
