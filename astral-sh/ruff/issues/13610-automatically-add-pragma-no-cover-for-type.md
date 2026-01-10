---
number: 13610
title: "Automatically add `pragma: no cover` for `TYPE_CHECKING` blocks"
type: issue
state: closed
author: MartinJepsen
labels:
  - rule
assignees: []
created_at: 2024-10-03T08:44:28Z
updated_at: 2025-07-18T07:09:53Z
url: https://github.com/astral-sh/ruff/issues/13610
synced_at: 2026-01-10T01:22:53Z
---

# Automatically add `pragma: no cover` for `TYPE_CHECKING` blocks

---

_Issue opened by @MartinJepsen on 2024-10-03 08:44_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


# Searches tried:

* pragma
* pragma no cover

Possibly related to #11941

# Feature request

When ruff automatically puts type-checking only imports into an `if TYPE_CHECKING:` block (`TCH001`, `TCH002`, `TCH003`), it would be great to allow the option of appending a `  # pragma: no cover` after the condition, so

```python
# current
if TYPE_CHECKING:
    ...

# with this feature
if TYPE_CHECKING:  # pragma: no cover
    ...
```

## Reasoning

`TYPE_CHECKING` is never `True` at runtime, and thus, code will never enter this block at runtime. Therefore, it makes sense to exclude the block from coverage measurement.

---

_Comment by @MartinJepsen on 2024-10-03 08:45_

I would like to give this issue a shot, as I feel like it's quite simple. I have never worked in this repo (or any open source for that matter) before, so some guidance would be welcome, assuming this feature becomes approved for implementation.

---

_Comment by @AlexWaygood on 2024-10-03 10:10_

Hmm, I wonder if this is the best way of solving this issue, though? I generally use the `tool.coverage.report.exclude_lines` setting in my `pyproject.toml` file to globally exclude all `if TYPE_CHECKING` blocks from coverage reports, so that I don't need to add `pragma: no cover` comments to all of them. E.g. see <https://github.com/AlexWaygood/typeshed-stats/blob/07949a0b06083d21b5f4a60cf1beb2a9dbe538cf/pyproject.toml#L198-L200> for an example :-)

---

_Label `rule` added by @AlexWaygood on 2024-10-03 10:10_

---

_Comment by @MartinJepsen on 2024-10-03 11:09_

@AlexWaygood I wasn't aware of that solution! I will start using that instead. I don't know if that would render the issue solved then? Curious what others think :-)

---

_Comment by @AlexWaygood on 2024-10-03 11:13_

I think using `coverage.py` configuration options is probably the cleaner solution here, and is reasonably commonly used. I'll close this for now on that basis, but I'm happy to reopen if there's an upswell of support for this issue from other users :-)

---

_Closed by @AlexWaygood on 2024-10-03 11:13_

---

_Comment by @MartinJepsen on 2024-10-03 11:14_

Sounds good, thanks for the help

---

_Comment by @KSmanis on 2025-07-18 07:09_

Correct me if I'm wrong, but ruff does proper AST parsing, while coverage.py relies on regexes, which can obviously lead to various false positives/negatives. I think it would be really neat if ruff could identify various common scenarios (`TYPE_CHECKING` blocks, abstract methods, protocols, typing overloads, etc., as outlined at https://coverage.readthedocs.io/en/latest/excluding.html#advanced-exclusion) and auto-apply the relevant `pragma` directives

---
