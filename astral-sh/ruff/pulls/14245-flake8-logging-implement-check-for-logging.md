```yaml
number: 14245
title: "[`flake8-logging`] Implement check for `logging.exception()`  outside of exception handler (`LOG004`)"
type: pull_request
state: closed
author: JCWasmx86
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-11-10T16:23:07Z
updated_at: 2025-06-03T19:32:47Z
url: https://github.com/astral-sh/ruff/pull/14245
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-logging`] Implement check for `logging.exception()`  outside of exception handler (`LOG004`)

---

_Pull request opened by @JCWasmx86 on 2024-11-10 16:23_

Relates to #7248


## Summary

This implements partially LOG004 of `flake8-logging`:
```python
logging.exception('') # Should be replaced by logging.error
try:
  logging.exception('') # Should be replaced by logging.error
except:
  logging.exception('') # That's fine
  def handle():
    logging.exception('') # Should be replaced, as the function can be called from everywhere
```
What's currently not implemented:
```
logger = logging.getLogger("foo")
logger.exception("abc")
```
That's because I'm unsure what's the best way to implement it:
- Like the plugin is doing: Walk the ast and find all identifiers that look like `$ID = logging.getLogger(...)` or `$ID = logging.Logger(...)`
- (Not sure how possible it is with current ruff): Check whether the variable has the type `Logger`
- Heuristics (Variable named like LOG/LOGGER/loggger/etc. and called on a method called `exception`)

The docs are nearly copied from the flake8-logging readme (Not sure, if that's allowed/how to properly attribute)
## Test Plan

With a manual test file (Parts of it included here), inspired by the tests in the flake8-logging repo.


---

_Comment by @github-actions[bot] on 2024-11-10 16:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/base.py#L265'>zerver/worker/base.py:265:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/base.py#L267'>zerver/worker/base.py:267:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/email_senders.py#L39'>zerver/worker/email_senders.py:39:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG004 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/base.py#L265'>zerver/worker/base.py:265:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/base.py#L267'>zerver/worker/base.py:267:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
+ <a href='https://github.com/zulip/zulip/blob/efd7c06e30466cda7a9445838a0581eca0344ef8/zerver/worker/email_senders.py#L39'>zerver/worker/email_senders.py:39:17:</a> LOG004 [*] Use of `logging.exception` outside exception handler
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG004 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @InSyncWithFoo on 2024-11-13 00:46_

Regarding the second example, you can check the qualified name against [`lint.logger-objects`](https://docs.astral.sh/ruff/settings/#lint_logger-objects).

---

_Comment by @ntBre on 2025-06-03 19:32_

I think this can be closed since LOG004 was added in https://github.com/astral-sh/ruff/pull/15799. Thanks for your work on this!

---

_Closed by @ntBre on 2025-06-03 19:32_

---
