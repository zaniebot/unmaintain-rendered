```yaml
number: 5751
title: "Remove `B904`'s lowercase exemption"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/b904
created_at: 2023-07-13T23:25:42Z
updated_at: 2023-07-14T15:46:23Z
url: https://github.com/astral-sh/ruff/pull/5751
synced_at: 2026-01-12T15:55:19Z
```

# Remove `B904`'s lowercase exemption

---

_@charliermarsh_

## Summary

It looks like bugbear, [from the start](https://github.com/PyCQA/flake8-bugbear/pull/181#issuecomment-904314876), has had an exemption here to exempt `raise lower_case_var`. I looked at Hypothesis and Trio, which are mentioned in that issue, and Hypothesis has exactly one case of this, and Trio has none, so IMO it doesn't seem worth special-casing.

Closes https://github.com/astral-sh/ruff/issues/5664.


---

_Review requested from @zanieb by @charliermarsh on 2023-07-13 23:25_

---

_Comment by @github-actions[bot] on 2023-07-13 23:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -0, 0 error(s))

<details><summary>bokeh (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/4ad3732a2753ac62dd3668ba8a044d11b88d86ba/tests/support/util/filesystem.py#L74'>tests/support/util/filesystem.py:74:17:</a> B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
</pre>

</p>
</details>
<details><summary>zulip (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b29ec4d62ef38a224467659c4f729ac9909ab69b/zerver/management/commands/register_server.py#L127'>zerver/management/commands/register_server.py:127:17:</a> B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| B904 | 2 | 2 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.02ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.01   1867.3±3.31µs     8.9 MB/sec    1.00   1857.8±2.45µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    208.1±0.51µs    14.2 MB/sec    1.00    208.6±0.41µs    14.1 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.6±0.02ms     3.0 MB/sec    1.00     13.6±0.03ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.8 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.03    456.8±3.20µs     6.5 MB/sec    1.00    445.6±0.69µs     6.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.01ms     4.2 MB/sec    1.00      6.1±0.01ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.7±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1488.2±1.49µs    11.2 MB/sec    1.00   1465.5±2.66µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.03    170.9±0.24µs    17.3 MB/sec    1.00    165.7±0.17µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     12.8±0.34ms     3.2 MB/sec    1.03     13.2±0.25ms     3.1 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.5±0.08ms     6.7 MB/sec    1.04      2.6±0.07ms     6.5 MB/sec
formatter/numpy/globals.py                 1.00   295.5±26.39µs    10.0 MB/sec    1.02   301.3±24.30µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.4±0.13ms     4.7 MB/sec    1.04      5.6±0.16ms     4.5 MB/sec
linter/all-rules/large/dataset.py          1.04     19.3±0.44ms     2.1 MB/sec    1.00     18.6±0.35ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.14ms     3.5 MB/sec    1.01      4.8±0.11ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.03   609.0±24.06µs     4.8 MB/sec    1.00   594.1±18.71µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.6±0.18ms     3.0 MB/sec    1.00      8.6±0.23ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01      9.7±0.23ms     4.2 MB/sec    1.00      9.5±0.19ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.04ms     8.1 MB/sec    1.02      2.1±0.05ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    240.4±6.59µs    12.3 MB/sec    1.02    244.1±7.43µs    12.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.13ms     5.9 MB/sec    1.02      4.4±0.08ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb approved on 2023-07-13 23:39_

Yeah this exclusion doesn't make sense to me? Even if I wanted to use `with_traceback` you'd need `from None` to hide the other tracebacks? e.g.

```python
traceback_exc = None
try:
    try:
        raise ValueError()
    except Exception as inner:
        traceback_exc = inner
        raise TypeError() from inner
except Exception:
    raise RuntimeError().with_traceback(traceback_exc.__traceback__) from None
```

This change does create a new common case though, when an exception is reraised

```python
try:
    raise ValueError()
except ValueError as exc:
    raise exc
```

Throws

```
example.py:4:5: B904 Within an `except` clause, raise exceptions with `raise ... from err` or `raise ... from None` to distinguish them from errors in exception handling
```

but should suggest using just `raise` to avoid adding an extra traceback frame e.g.

```
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 4, in <module>
    raise exc
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 2, in <module>
    raise ValueError()
ValueError
```
vs

```python
try:
    raise ValueError()
except ValueError:
    raise
```
```
Traceback (most recent call last):
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 2, in <module>
    raise ValueError()
ValueError
```

Edit: Charlie pointed out that this is already covered by `TRY201` so it probably ought to be excluded entirely here?

---

_Comment by @charliermarsh on 2023-07-13 23:39_

Ah interesting, I think these are now false positives whereby in most cases, they're just re-raising the exception. Need to fix.

---

_Comment by @charliermarsh on 2023-07-14 13:45_

I'm gonna change the rule to allow those "verbose re-raises", since we have TRY201 to flag them.

---

_Comment by @charliermarsh on 2023-07-14 13:52_

(Gonna see how ecosystem CI checks shake out.)

---

_Comment by @charliermarsh on 2023-07-14 13:54_

I'm worried this may actually be a dupe of `TRY200` now, but I guess this is still fine to fix.

---

_@zanieb reviewed on 2023-07-14 15:12_

I still find this message confusing for `except e: raise e` — both suggestions `raise e from None` and `raise e from e` are less correct than `raise` or `raise e`.

---

_Comment by @charliermarsh on 2023-07-14 15:15_

Hmm, we no longer flag `except e: raise e` though.

---

_Comment by @charliermarsh on 2023-07-14 15:17_

Ah, ok, so we are flagging it for the nested exception in Zulip.

---

_Comment by @zanieb on 2023-07-14 15:33_

@charliermarsh Ah I see that's nested — that may be reasonable actually?


As written look like a violation:

```python
try:
    try:
        raise RuntimeError("one") # Raise for traceback
    except Exception as e1:
        raise RuntimeError("two") from e1
except RuntimeError as e2:
    try:
        raise RuntimeError("three")
    except RuntimeError as e3:
        raise e2
```
```python
Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 3, in <module>
    raise RuntimeError("one") # Raise for traceback
    ^^^^^^^^^^^^^^^^^^^^^^^^^
RuntimeError: one

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 10, in <module>
    raise e2
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 5, in <module>
    raise RuntimeError("two") from e1
RuntimeError: two
```

Suggestions look reasonable:

```python
try:
    try:
        raise RuntimeError("one") # Raise for traceback
    except Exception as e1:
        raise RuntimeError("two") from e1
except RuntimeError as e2:
    try:
        raise RuntimeError("three")
    except RuntimeError as e3:
        raise e2 from e2
```
```python
Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 10, in <module>
    raise e2 from e2
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 5, in <module>
    raise RuntimeError("two") from e1
RuntimeError: two
```

```python
try:
    try:
        raise RuntimeError("one") # Raise for traceback
    except Exception as e1:
        raise RuntimeError("two") from e1
except RuntimeError as e2:
    try:
        raise RuntimeError("three")
    except RuntimeError as e3:
        raise e2 from None
```
```python
Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 10, in <module>
    raise e2 from None
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 5, in <module>
    raise RuntimeError("two") from e1
RuntimeError: two
```

```python
try:
    try:
        raise RuntimeError("one") # Raise for traceback
    except Exception as e1:
        raise RuntimeError("two") from e1
except RuntimeError as e2:
    try:
        raise RuntimeError("three")
    except RuntimeError as e3:
        raise e2 from e3
```
```python
Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 8, in <module>
    raise RuntimeError("three")
RuntimeError: three

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 10, in <module>
    raise e2 from e3
  File "/Users/mz/eng/src/RustPython/Parser/example.py", line 5, in <module>
    raise RuntimeError("two") from e1
RuntimeError: two
```

---

_Merged by @charliermarsh on 2023-07-14 15:46_

---

_Closed by @charliermarsh on 2023-07-14 15:46_

---

_Branch deleted on 2023-07-14 15:46_

---
