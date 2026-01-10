---
number: 19603
title: "Whitespace before classes not inserted when used with \"modificationsIfAvailable\" in vscode"
type: issue
state: open
author: maltevesper
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-07-28T15:01:33Z
updated_at: 2025-08-28T14:22:55Z
url: https://github.com/astral-sh/ruff/issues/19603
synced_at: 2026-01-10T01:23:00Z
---

# Whitespace before classes not inserted when used with "modificationsIfAvailable" in vscode

---

_Issue opened by @maltevesper on 2025-07-28 15:01_

### Summary

I am not sure if this is a vscode plugin or ruff issue, that said:

When using the `"editor.formatOnSaveMode": "modificationsIfAvailable"` setting, Ruff does not add blank lines around newly added classes. This results in the CI checking the entire file finding new issues in the file.

Example:
```
def oldCode():
    ...
class InsertedClass:    # New
    pass                        # New
def moreOldCode():
    ...
```

After formating I get the formated class back as (formatting inside the class I added works as expected):
```
class InsertedClass:    # New
    pass                        # New
```

I would like the language server to return the new class with surrounding needed whitespace if that is possible, i.e.:
```
                                  # ruff add whitespace before
                                  # ruff add whitespace before
class InsertedClass:    # New
    pass                        # New
                                  # ruff add whitespace after
                                  # ruff add whitespace after
```

I have not dug into the language server protocol and I am not sure if this is feasible (i.e. does the language server get enough context (i.e. code outside the edited area), when `modificationsIfAvailable` is set?

### Version

_No response_

---

_Label `question` added by @ntBre on 2025-07-28 16:55_

---

_Comment by @ntBre on 2025-07-28 17:08_

Ah, that's interesting and does seem like a nice feature. I'm not sure if ruff gets a large enough range to make it feasible either, as you said. Maybe @MichaReiser or @dhruvmanila would know more.

Here are a few related links I found, including support for `modificationsIfAvailable` in ruff-lsp:
- https://code.visualstudio.com/updates/v1_49#_only-format-modified-text
- https://github.com/astral-sh/ruff-vscode/issues/319
- https://github.com/astral-sh/ruff-lsp/pull/373

When reproducing this locally, I also see whitespace added _after_ the new class but not before. Maybe that's something with my local setup, though.

Out of curiosity, what's the benefit of using `modificationsIfAvailable` in this situation, if you're formatting the whole file in CI anyway?

---

_Comment by @MichaReiser on 2025-07-29 06:49_

I can reproduce this in the CLI using:


```
uvx ruff@latest format --range 3:1-5:1 range_formatting.py
```

The main idea of `modificationsIfAvailable` is that Ruff leaves unchanged lines alone. Now, Ruff doesn't know if the class was added or removed. It only knows that there's a class and the class is in the range that should be formatted. If it's a new class, it should add the empty lines. If it's an existing class, things are less clear. 

Given that Ruff can't tell if the class is new or not, I'm still leaning towards adding the blank lines if the range covers a class (or function, etc.).

The logic for this is in [`format_range`](https://github.com/astral-sh/ruff/blob/c9dff5c7d5bcab2a83f1c3b4a1a80af230ece541/crates/ruff_python_formatter/src/range.rs#L51). It's fairly involved because we first:

* Try to find the smallest possible range the formatter can format and that covers the range provided by the user
* After formatting, identify the range again and extract the relevant formatted code. This uses source maps to map the original offsets to the new offsets. 

I'm not entirely sure where I would implement the fix. The narrowing seems to be working as expected, it only selects the class. But we then don't include the empty lines when extracting the formatted code. This may require changing [`Printed.slice_range`](https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_formatter/src/lib.rs#L451) or emitting more source map markers in `FormatSuite` (etc)

This test case seems related https://github.com/astral-sh/ruff/blob/4f7fb566f0c7f5ec9fb336373eae3e144e505d4e/crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/regressions.py#L77-L85

---

_Label `bug` added by @MichaReiser on 2025-07-29 06:50_

---

_Label `formatter` added by @MichaReiser on 2025-07-29 06:50_

---

_Label `question` removed by @MichaReiser on 2025-07-29 06:50_

---

_Comment by @maltevesper on 2025-08-28 14:22_

> Out of curiosity, what's the benefit of using `modificationsIfAvailable` in this situation, if you're formatting the whole file in CI anyway?

@ntBre : I think I still owe you an answer: I set my editor to format only the code I changed, to cause less issues with projects not using a formatter, and noticed when a project which uses a pipeline to check formatting complained.
In hindsight I think this issue is minor. It might help with easing in teams which are reluctant on enforced formatting (introducing it for new code first), but as long as CI and formatting are consistent it should be fine. Obviously one would have to write a CI pipeline to only format the changed code for the last usecase to work. I assume this usecase would work with the current implementation as the reformatting should be consistent between duing it in bulk in the ci or running this from the editor.


---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---
