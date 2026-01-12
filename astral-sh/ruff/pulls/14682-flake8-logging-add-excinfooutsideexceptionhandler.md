```yaml
number: 14682
title: "[flake8-logging] Add `ExcInfoOutsideExceptionHandler` (LOG014)"
type: pull_request
state: closed
author: snowdrop4
labels:
  - rule
  - preview
assignees: []
base: main
head: AVK/ExcInfoOutsideExceptionHandler
created_at: 2024-11-29T16:57:00Z
updated_at: 2025-02-04T10:41:20Z
url: https://github.com/astral-sh/ruff/pull/14682
synced_at: 2026-01-12T15:55:48Z
```

# [flake8-logging] Add `ExcInfoOutsideExceptionHandler` (LOG014)

---

_@snowdrop4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add `ExcInfoOutsideExceptionHandler` (`LOG014`) from the `flake8-logging` plugin (parent issue #7248).

This rule detects the `exc_info` kwarg being set to `True` outside of an exception handling block, which results in unwanted logging output.

I did some mild refactoring by adding a `helpers.rs` in the crate to reduce duplication.

## Test Plan

Lots of fixtures.


---

_Renamed from "New Rule: LOG014 from flake8-logging" to "[flake8-logging] Add LOG014" by @snowdrop4 on 2024-11-29 17:05_

---

_Renamed from "[flake8-logging] Add LOG014" to "[flake8-logging] Add `ExcInfoOutsideExceptionHandler` (LOG014)" by @snowdrop4 on 2024-11-29 17:06_

---

_Comment by @github-actions[bot] on 2024-11-29 17:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/35000c91ea7b2ce0851a8af117fdfc3683ec07bb/task_sdk/src/airflow/sdk/execution_time/supervisor.py#L527'>task_sdk/src/airflow/sdk/execution_time/supervisor.py:527:9:</a> LOG014 Use of `exc_info` outside an exception handler
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+12 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/commands/report/base.py#L105'>superset/commands/report/base.py:105:13:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/db_engine_specs/hive.py#L587'>superset/db_engine_specs/hive.py:587:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/sql_lab.py#L136'>superset/sql_lab.py:136:5:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/sql_lab.py#L140'>superset/sql_lab.py:140:5:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/tasks/cache.py#L270'>superset/tasks/cache.py:270:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/utils/core.py#L572'>superset/utils/core.py:572:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L140'>superset/views/error_handling.py:140:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L145'>superset/views/error_handling.py:145:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L151'>superset/views/error_handling.py:151:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L160'>superset/views/error_handling.py:160:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L187'>superset/views/error_handling.py:187:9:</a> LOG014 Use of `exc_info` outside an exception handler
+ <a href='https://github.com/apache/superset/blob/97dde8c4855641de38f01218d0a4bb5460e3f1b2/superset/views/error_handling.py#L209'>superset/views/error_handling.py:209:9:</a> LOG014 Use of `exc_info` outside an exception handler
</pre>

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/901667b8e2dc55caee1ccb7a02d22446131c1529/rotkehlchen/api/server.py#L431'>rotkehlchen/api/server.py:431:9:</a> LOG014 Use of `exc_info` outside an exception handler
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/lib/push_notifications.py#L1330'>zerver/lib/push_notifications.py:1330:17:</a> LOG014 Use of `exc_info` outside an exception handler
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| LOG014 | 15 | 15 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @AlexWaygood on 2024-11-29 17:13_

---

_Label `preview` added by @AlexWaygood on 2024-11-29 17:13_

---

_Review comment by @snowdrop4 on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-11-29 17:15_

The ecosystem checks are turning up a few instances just like this.

Technically in this case, if we are able to tell that `banana()` is *only* ever called from inside an exception handling block, then we could mark this as okay. But I don't think it's possible to check all calling sites?

---

_@snowdrop4 reviewed on 2024-11-29 17:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-11-29 17:32_

It's possible, but you'd need to move the rule to `crates/ruff_linter/src/checkers/ast/analyze/bindings.rs` rather than `crates/ruff_linter/src/checkers/ast/analyze/expression.rs`. The difference is that the rules executed from `bindings.rs` are run after the entire semantic model has been built, so they have more information available to them in some ways: they're able to iterate through all the references to a binding as well as looking at the original binding itself. You can see an example of how to do it in a PR I merged this morning: https://github.com/astral-sh/ruff/pull/14661.

---

_@AlexWaygood reviewed on 2024-11-29 17:32_

---

_Review comment by @snowdrop4 on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-11-29 18:50_

Ahh. I'll revise it to include that check then. Thanks.

---

_@snowdrop4 reviewed on 2024-11-29 18:50_

---

_@AlexWaygood reviewed on 2024-11-29 18:51_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-11-29 18:51_

no problem, thanks for the PR!

---

_Review comment by @snowdrop4 on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-12-02 14:29_

Forgive me if I'm misunderstanding, but I don't think moving it to `bindings.rs` works?

An expression like `logging.info("foo", exc_info=True)` isn't a binding, so if I move my lint function call to `bindings.rs`, then the only situation the lint could possibly apply would be `x = logging.info("foo", exc_info=True)` or `(x := logging.info("foo", exc_info=True))`, which no one writes.

Is it possible to make a new version of `expression.rs` that's called after the entire semantic model has been built? I don't think there's any way around calling the lint function on expressions, since the lint is designed to find bad function calls.

---

_@snowdrop4 reviewed on 2024-12-02 14:29_

---

_@AlexWaygood reviewed on 2024-12-02 14:35_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_logging/LOG014.py`:154 on 2024-12-02 14:35_

> An expression like `logging.info("foo", exc_info=True)` isn't a binding, so if I move my lint function call to `bindings.rs`, then the only situation the lint could possibly apply would be `x = logging.info("foo", exc_info=True)` or `(x := logging.info("foo", exc_info=True))`, which no one writes.

ah, you're quite right.

I suppose what would be best would be to collect all `logging.(...)` calls into a `Vec` stored on [this struct](https://github.com/astral-sh/ruff/blob/6dfe125f44352f5fdff19f6d47fc33ac7a6e86ad/crates/ruff_linter/src/checkers/ast/deferred.rs#L4-L15) as we traverse the AST in https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/checkers/ast/mod.rs. Then after the AST has been fully visited and the `SemanticModel` has been fully constructed, we can call this rule on each `logging(...)` call we stored in the Vec.

---

_Review requested from @dylwil3 by @dylwil3 on 2024-12-04 18:14_

---

_Comment by @dylwil3 on 2024-12-05 19:28_

@snowdrop4 Thanks for working on this!

Would you, by any chance, be able to walk me through (at least some of) the ecosystem results and help me understand why/whether they are true positives?

I'm a little worried (probably unnecessarily) that a library may define a function in a publicly accessible namespace

```python
def foo():
    # <-- function logic here --- >
    
    # exc_info outside of except
    logging.exception(..., exc_info=True)
```

and then any of the following (or a mixture) may occur:

1. `foo` is imported in a different file and called inside an except block.
2. `foo` is called from the body of `bar` which is called from the body of `baz`... etc. which is then called inside an except block.
3. `foo` has a decorator which secretly inserts a try/except.
4. `foo` is not called in an except block in the whole library, but it is meant to be consumed by users within an except block.

I don't think I know enough about the use-cases for specifying `exc_info=True` outside of an except block to tell whether people are really doing it by accident or intentionally for something like 1-4.

I don't think there's a problem with performing this lint for functions defined inside another function scope, since they cannot be used elsewhere, but for the above case I'm less sure.

Apologies if I'm making a mountain out of a molehill here - you probably know more than me about Python logging practices and can tell me I'm off base here!


---

_Comment by @dylwil3 on 2024-12-17 16:20_

@snowdrop4 Sorry again for the added complexity here. What do you think of the following conservative approach to this rule?

1. If the violation is in the module-level scope, emit a lint.
2. If the violation is in a _nested_ function, emit a lint
3. If the violation is in a function _and_ the function is used somewhere in the file _not_ inside an except-handler, emit a lint
4. otherwise, don't emit a lint

As I said in the previous comment, I'm happy to be overruled here if you think the current ecosystem check and scope of the rule seems reasonable given how Python developers use this logging option generally.

Thanks for your patience and your contribution!

---

_Comment by @MichaReiser on 2025-02-04 09:52_

I'll close this because LOG014 was added in https://github.com/astral-sh/ruff/pull/15799

Thanks again for giving this rule a try! 

---

_Closed by @MichaReiser on 2025-02-04 09:52_

---

_Comment by @snowdrop4 on 2025-02-04 10:39_

Ah, sorry, I've been so busy, I wasn't able to revisit.

---

_Comment by @AlexWaygood on 2025-02-04 10:41_

No apology needed @snowdrop4. Thanks again for the PR!

---
