```yaml
number: 20657
title: "[`ruff`] Add support for additional eager conversion patterns (`RUF065`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-20583
created_at: 2025-10-01T00:25:50Z
updated_at: 2025-10-29T21:54:02Z
url: https://github.com/astral-sh/ruff/pull/20657
synced_at: 2026-01-12T15:57:07Z
```

# [`ruff`] Add support for additional eager conversion patterns (`RUF065`)

---

_@danparizher_

## Summary

Fixes #20583


---

_Comment by @github-actions[bot] on 2025-10-01 00:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+9 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+8 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L2427'>libs/core/langchain_core/callbacks/manager.py:2427:25:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L289'>libs/core/langchain_core/callbacks/manager.py:289:25:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L296'>libs/core/langchain_core/callbacks/manager.py:296:21:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L337'>libs/core/langchain_core/callbacks/manager.py:337:71:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L349'>libs/core/langchain_core/callbacks/manager.py:349:67:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L391'>libs/core/langchain_core/callbacks/manager.py:391:17:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/core/langchain_core/callbacks/manager.py#L398'>libs/core/langchain_core/callbacks/manager.py:398:13:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
+ <a href='https://github.com/langchain-ai/langchain/blob/d2189367630d9d35b2048dcfc017803ef4635881/libs/langchain/langchain_classic/smith/evaluation/runner_utils.py#L1151'>libs/langchain/langchain_classic/smith/evaluation/runner_utils.py:1151:61:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-poetry/poetry">python-poetry/poetry</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python-poetry/poetry/blob/500a313d68637cbd317171c567280a10eaabae3c/src/poetry/inspection/lazy_wheel.py#L372'>src/poetry/inspection/lazy_wheel.py:372:57:</a> RUF065 Unnecessary `repr()` conversion when formatting with `%s`. Use `%r` instead of `%s`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF065 | 9 | 9 | 0 | 0 | 0 |

</p>
</details>




---

_Label `rule` added by @ntBre on 2025-10-15 20:33_

---

_Label `preview` added by @ntBre on 2025-10-15 20:33_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF065.py`:61 on 2025-10-15 20:55_

I'm not sure if we should be handling bytes. I might be doing something wrong, but these don't appear to work on Python 3.13:

```pycon
>>> logging.info("Bytes: %b", bytes("Hello", "utf-8"))
--- Logging error ---
Traceback (most recent call last):
  File "/usr/lib/python3.13/logging/__init__.py", line 1151, in emit
    msg = self.format(record)
  File "/usr/lib/python3.13/logging/__init__.py", line 999, in format
    return fmt.format(record)
           ~~~~~~~~~~^^^^^^^^
  File "/usr/lib/python3.13/logging/__init__.py", line 712, in format
    record.message = record.getMessage()
                     ~~~~~~~~~~~~~~~~~^^
  File "/usr/lib/python3.13/logging/__init__.py", line 400, in getMessage
    msg = msg % self.args
          ~~~~^~~~~~~~~~~
ValueError: unsupported format character 'b' (0x62) at index 8
```

This happens even if I make the message a byte string, which otherwise works with `%` formatting:

```pycon
>>> logging.info(b"Bytes: %b", bytes("Hello", "utf-8"))
--- Logging error ---
Traceback (most recent call last):
# ...
>>> b"Bytes: %b" % bytes("Hello", "utf-8")
b'Bytes: Hello'
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:76 on 2025-10-15 20:56_

Could we break these lines up? We can't insert actual newlines since the message needs to remain on a single line, but we can do something like:


```suggestion
            (FormatConversion::Str, Some("oct")) => "Unnecessary `oct()` conversion when formatting with `%s`. \
            Use `%#o` instead of `%s`".to_string(),
```

Alternatively, I think the `Use ...` parts would fit nicely in the `fix_title` function, which I think we can include even if we don't attach a fix.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/logging_eager_conversion.rs`:65 on 2025-10-15 20:59_

Does `function_name` need to be a `String`? It looks like it's only ever set to a `&'static str`.

I also think it might make sense to define our own, separate enum here instead of reusing `FormatConversion`, especially if we don't support `Bytes`, as in my other comment.

---

_@ntBre reviewed on 2025-10-15 21:02_

---

_Review requested from @ntBre by @danparizher on 2025-10-15 23:53_

---

_@ntBre approved on 2025-10-29 21:39_

Thanks!

(Just closing and reopening to make sure CI still passes)

---

_Closed by @ntBre on 2025-10-29 21:40_

---

_Reopened by @ntBre on 2025-10-29 21:40_

---

_Merged by @ntBre on 2025-10-29 21:45_

---

_Closed by @ntBre on 2025-10-29 21:45_

---

_Branch deleted on 2025-10-29 21:54_

---
