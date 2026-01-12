```yaml
number: 10228
title: E302 and E305 cause the linter and formatter to make conflicting changes in notebooks
type: issue
state: closed
author: vancromy
labels:
  - rule
  - notebook
assignees: []
created_at: 2024-03-04T10:34:54Z
updated_at: 2024-03-25T11:19:32Z
url: https://github.com/astral-sh/ruff/issues/10228
synced_at: 2026-01-12T15:54:50Z
```

# E302 and E305 cause the linter and formatter to make conflicting changes in notebooks

---

_@vancromy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The new rules introduced in ruff v0.2.2 via #9266 are causing the ruff linter to make conflicting changes in notebook cells that the formatter is then trying to fix.

Here is an example notebook which reproduces the bug:
![image](https://github.com/astral-sh/ruff/assets/59556820/cc5c3db0-89f1-4dd4-a89d-507d585ebc9e)

ruff config:
```
[tool.ruff]
extend-include = ["*.ipynb"]

[tool.ruff.lint]
select = [
    "E",
]
```

If you run the commands in this succession and look at the diffs you will notice that the linter adds two lines at the end of:
1. Cell 3 (the one just printing out the variable `some_computation`). This is rule `E302` in action
2. Cell 4 (after the function definition). This is rule `E305` in action.

The commands I ran:
1. `ruff check --force-exclude --preview --fix test.ipynb`
2. `ruff format --force-exclude --preview test.ipynb`

(The `--force-exclude` is because I discovered this bug through pre-commit and I was trying to mimic the pre-commit behaviour)

This behaviour happens from v0.2.2 upwards (I've also tested v0.3.0) as long as the preview flag is on.

Let me know if the MRE needs more explanation.

---

_Label `rule` added by @MichaReiser on 2024-03-04 11:41_

---

_Label `notebook` added by @MichaReiser on 2024-03-04 11:41_

---

_Comment by @MichaReiser on 2024-03-04 11:41_

Thanks for the excellent write up. Agree, that is a bug in `E302` and `E305`. It should not enforce blank lines at the end of a cell. 

---

_Comment by @hoel-bagard on 2024-03-04 15:01_

I can try to fix it if no one has started working on it yet.

---

_Comment by @9128305 on 2024-03-06 08:45_

I have the same conflicts in .pyi files like this:
```python
class C:
    def foo(self) -> None: ...

x: C
```
This triggers E305. E301, E302 also make conflicts between ruff check and ruff format

---

_Comment by @MichaReiser on 2024-03-06 09:03_

@9128305 this should be fixed in the next release. See https://github.com/astral-sh/ruff/pull/10098

---

_Comment by @hoel-bagard on 2024-03-06 13:37_

@MichaReiser You are only talking about the comment above, right ?

---

_Comment by @MichaReiser on 2024-03-06 13:54_

@hoel-bagard Yes, my comment was specific about compatibility within typing stub file (pyi). The incompatibility documented in this issue isn't solved.

@dhruvmanila any recommendation how to fix this or is this something you could take on?

---

_Comment by @dhruvmanila on 2024-03-06 16:34_

We do have custom handling for rules in Notebooks (`B018`, `B015`), so I think this should be possible.

There's a [helper function](https://github.com/astral-sh/ruff/blob/cbd927f3463e1de4b664103b2cc70d5e67486e0e/crates/ruff_linter/src/rules/flake8_bugbear/helpers.rs) which checks if it's the last expression in the cell. We could either use the same function or provide a similar function depending on what the needs are here and check if we're at the cell boundary. If so, ignore checking for blank lines.

In the linked function, the tokenizer would start at the _end_ of the expression. But for `E302`, we will need to start from the cell boundary and end at the statement start position. This will require a new function.

I'm not sure how exactly to get the range of the entire function / class as these rules are token-based and not AST-based. For an AST node we can directly get the end of the function / class node. It might be that I would need to look into it in detail to understand that.

@MichaReiser I'm not sure if I want to look into it right now unless it's a priority.

---

_Comment by @MichaReiser on 2024-03-06 17:11_

Thanks @dhruvmanila. @hoel-bagard would you be interested to look into this issue? I'm sure @dhruvmanila would reply to any questions you have

---

_Comment by @hoel-bagard on 2024-03-07 00:18_

@MichaReiser I would be interested to look into it yes. I'll start by looking at the references given by @dhruvmanila, and ask if I get stuck on something.

---

_Assigned to @hoel-bagard by @MichaReiser on 2024-03-07 06:14_

---

_Closed by @dhruvmanila on 2024-03-25 11:19_

---
