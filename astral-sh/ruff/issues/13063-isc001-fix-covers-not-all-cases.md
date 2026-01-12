```yaml
number: 13063
title: ISC001 fix covers not all cases
type: issue
state: open
author: serjflint
labels:
  - fixes
assignees: []
created_at: 2024-08-22T19:37:29Z
updated_at: 2024-08-22T20:23:33Z
url: https://github.com/astral-sh/ruff/issues/13063
synced_at: 2026-01-12T15:54:52Z
```

# ISC001 fix covers not all cases

---

_@serjflint_

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

ruff 0.5.3
ruff check --fix

Hi! I am trying to fix all ISC errors after switching from 80 line-length to 120.
I still have many cases which are flagged by ISC001 but aren't auto-fixed. 

One such case is where only one side is an f-string:
`'Argument "num" has incorrect type. ' f'Expected {num}.'`
`f'Request to {TVM_SERVICE_NAME} failed. ' 'Unexpected error: ' + error_msg`
`f'oneOf schema variants {nullable_variants} are nullable ' 'at the same time, this create ambiguities'`

Another one was with different quote-styles (because some string are double-quoted even with `quote-style = "single"`), but I worked around it by formatting everything to double-quotes and back.

---

_Label `fixes` added by @AlexWaygood on 2024-08-22 19:50_

---

_Comment by @dylwil3 on 2024-08-22 19:58_

The current implementation of this rule requires that both the leading and trailing quotes of each string agree before concatenating. That's actually causing both issues, because the leading quote for `f"{num}"` comes out as `f"` not `"`.

https://github.com/astral-sh/ruff/blob/2edd32aa31446c81c245beb1230de26220a7d7e8/crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs#L159-L173

From my point of view, the f-string miss is a bug but the disagreement in quote-styles is maybe not (and fixable, as you say, by running a formatter first). Maybe these can be concatenated with correct quotes depending on a user-defined setting? Or, if the linter is allowed to look at formatter settings, the rule could default to concatenating `"a" 'b'` to `"ab"` if the formatter settings prefer double-quotes, for example.

But in any event, I defer to the maintainers.

---
