---
number: 7847
title: B023 is emitted when closure is defined inside a loop and uses a variable from the same loop
type: issue
state: open
author: Guilherme-Vasconcelos
labels:
  - bug
assignees: []
created_at: 2023-10-07T15:07:10Z
updated_at: 2025-08-31T07:19:27Z
url: https://github.com/astral-sh/ruff/issues/7847
synced_at: 2026-01-07T13:12:15-06:00
---

# B023 is emitted when closure is defined inside a loop and uses a variable from the same loop

---

_Issue opened by @Guilherme-Vasconcelos on 2023-10-07 15:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Code snippet that reproduces the bug

The following snippet emits B023 (function-uses-loop-variable):

```python
def make_squared_list(size: int) -> list[int]:
    result = []

    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            # Function definition does not bind loop variable `square` Ruff[B023](https://docs.astral.sh/ruff/rules/function-uses-loop- 
            # variable)
            # (variable) square: int
            result.append(square)

        _append_to_result()

    return result
```

I have found out that adding a `nonlocal <variable>` declaration followed by an assignment to itself makes the error go away, i.e. change the closure `_append_to_result` to this:

```python
def _append_to_result() -> None:
    # No longer emits any warnings.
    nonlocal square
    square = square
    result.append(square)
```

## Command invoked

```sh
$ poetry run ruff ./ruff_bug.py
```

## Current ruff settings

Relevant sections from `pyproject.toml`:

```toml
[tool.ruff]
select = [
    "F",
    "E",
    "W",
    "I",
    "N",
    "UP",
    "S",
    "BLE",
    "B",
    "A",
    "C4",
    "SIM",
    "PERF",
    "RUF"
]
line-length = 88
target-version = "py311"
ignore = [
    "S101",
    "B011",
]

[tool.ruff.pycodestyle]
ignore-overlong-task-comments = true
```

## Current ruff version

```sh
$ poetry run ruff --version
ruff 0.0.291
```

---

_Comment by @zanieb on 2023-10-07 15:13_

Thanks for the well written issue :) this does look like a bug, even if I move return the function and run it in a new scope the variable is indeed bound

```python
def make_squared_list(size: int) -> list[int]:
    result = []

    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            # Function definition does not bind loop variable `square` Ruff[B023](https://docs.astral.sh/ruff/rules/function-uses-loop- 
            # variable)
            # (variable) square: int
            print(square)
            result.append(square)

        _append_to_result()

    return _append_to_result

make_squared_list(3)()
```
```
0
1
4
4
```

---

_Label `bug` added by @zanieb on 2023-10-07 15:13_

---

_Comment by @charliermarsh on 2023-10-08 14:28_

(Confirmed that flake8-bugbear also flags these cases. So it's not a bug in our re-implementation. We should still fix if we can.)

---

_Comment by @charliermarsh on 2023-10-08 14:35_

Hmm, I looked at some bugbear issues, and it does seem like this is valid to flag in some cases.

For example, this prints `[1, 1]` instead of `[0, 1]`. Note that the function defined in the loop is called _after_ the loop, rather than within each iteration:

```python
def make_squared_list(size: int) -> list[int]:
    result = []

    functions = []
    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            # Function definition does not bind loop variable `square` Ruff[B023](https://docs.astral.sh/ruff/rules/function-uses-loop- 
            # variable)
            # (variable) square: int
            result.append(square)

        functions.append(_append_to_result)

    for function in functions:
        function()

    return result


print(make_squared_list(2))
```

See https://github.com/PyCQA/flake8-bugbear/issues/402.

---

_Referenced in [canonical/operator#1114](../../canonical/operator/pulls/1114.md) on 2024-02-15 00:26_

---

_Comment by @beauxq on 2024-08-29 13:01_

I think this false positive could be eliminated without risking any false negatives.

We should be able to detect whether the only thing that is done with the function is calling it within the same loop.
If that is the case, it shouldn't be reported.
If anything else is done with the function besides calling it in the same loop, the error should be reported.

---

```python
    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            result.append(square)

        _append_to_result()
    return result
```
Nothing is done besides calling it in the same loop, so it should not be reported.

---

```python3
    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            result.append(square)

        _append_to_result()
    return _append_to_result
```
Something is done besides calling it in the same loop (returning it), so it should be reported.

---

```python3
    for i in range(size):
        square = i**2

        def _append_to_result() -> None:
            result.append(square)

        functions.append(_append_to_result)
```
Something is done besides calling it in the same loop (passing it to another function), so it should be reported.

---

In case someone wonders why someone would want to define a function only to call it within the same loop, the biggest reason I do it is code organization.
- It organizes a section of code together and gives it a name.
- It creates a new scope so temp variables don't pollute the name space.
- It is collapsible in a code editor.

In one place I ran into this false positive, there were 5 variables captured by the function.
Having to bind all of them would make a bunch of noise that would make it harder to read.
`noqa` comments on all the usages would also make it harder to read. And I don't want to risk a false negative somewhere else by ignoring the whole file. Ignoring the rule also brings the risk that someone else could come later and change it to use the function outside the loop, and the problem wouldn't be caught.

---

_Comment by @pankajp on 2024-09-10 15:47_

I think @beauxq is right and ruff should be able to detect the first case where the only thing being done with the function is calling it within the same loop.
I just had to disable B023 checks in my config as we have numerous such uses

---

_Referenced in [astral-sh/ruff#15716](../../astral-sh/ruff/issues/15716.md) on 2025-01-24 13:55_

---

_Referenced in [astral-sh/ruff#16046](../../astral-sh/ruff/issues/16046.md) on 2025-02-08 22:54_

---

_Referenced in [llamastack/llama-stack#1184](../../llamastack/llama-stack/pulls/1184.md) on 2025-02-27 16:55_

---

_Comment by @sergey-protserov-uhn on 2025-08-30 14:48_

Just encountered this with Ruff 0.12.1 on the following code:
```
for i in range(1, 6 + 1):
...
    label_mapping = {k: v for v, k in enumerate(labels_)}
    label_mapping_f = np.vectorize(lambda x: label_mapping[x])  # triggered for label_mapping in this line
    labels = label_mapping_f(labels)
```

---

_Comment by @beauxq on 2025-08-30 15:40_

> ```
> for i in range(1, 6 + 1):
> ...
>     label_mapping = {k: v for v, k in enumerate(labels_)}
>     label_mapping_f = np.vectorize(lambda x: label_mapping[x])  # triggered for label_mapping in this line
>     labels = label_mapping_f(labels)
> ```

I don't think that one is really safe for the linter to ignore, because it doesn't know whether `np.vectorize` is going to call it immediately, or save a reference to it to call later.

---

_Comment by @sergey-protserov-uhn on 2025-08-31 07:19_

I see, that makes perfect sense, thank you for your answer!

---
