```yaml
number: 11070
title: "[ruff] fix async comprehension false positive (RUF029)"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-ruf029-async-comprehension
created_at: 2024-04-21T12:04:56Z
updated_at: 2024-04-21T12:30:52Z
url: https://github.com/astral-sh/ruff/pull/11070
synced_at: 2026-01-10T22:37:01Z
```

# [ruff] fix async comprehension false positive (RUF029)

---

_Pull request opened by @JonathanPlasse on 2024-04-21 12:04_

## Summary

- Fix #11043 

## Test Plan

Added the false positive code in the test fixture.


---

_Comment by @charliermarsh on 2024-04-21 12:16_

Hey @JonathanPlasse, great to see you on the repo!

---

_@charliermarsh approved on 2024-04-21 12:16_

---

_Label `bug` added by @charliermarsh on 2024-04-21 12:16_

---

_Comment by @github-actions[bot] on 2024-04-21 12:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -5 violations, +0 -0 fixes in 2 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/5e2ced2aa08a85ff555eb79bf70119bfa077e846/tests/test_utils.py#L706'>tests/test_utils.py:706:11:</a> RUF029 Function `test_as_chunks` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/evaluation/test_marker_tracker_loader.py#L137'>tests/core/evaluation/test_marker_tracker_loader.py:137:11:</a> RUF029 Function `test_warn_count_exceeds_store` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_exporter.py#L111'>tests/core/test_exporter.py:111:11:</a> RUF029 Function `test_fetch_events_within_time_range_tracker_does_not_err` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_exporter.py#L123'>tests/core/test_exporter.py:123:11:</a> RUF029 Function `test_fetch_events_within_time_range_tracker_contains_no_events` is declared `async`, but doesn't `await` or use `async` features.
- <a href='https://github.com/RasaHQ/rasa/blob/7807b19ad5fffab73ca1a04dc710f812115a9288/tests/core/test_exporter.py#L72'>tests/core/test_exporter.py:72:11:</a> RUF029 Function `test_fetch_events_within_time_range` is declared `async`, but doesn't `await` or use `async` features.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF029 | 5 | 0 | 5 | 0 | 0 |

</p>
</details>




---

_Merged by @charliermarsh on 2024-04-21 12:17_

---

_Closed by @charliermarsh on 2024-04-21 12:17_

---

_Branch deleted on 2024-04-21 12:25_

---

_Comment by @JonathanPlasse on 2024-04-21 12:30_

Thanks

---
