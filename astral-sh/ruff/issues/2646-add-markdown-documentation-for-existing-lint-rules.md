```yaml
number: 2646
title: Add Markdown documentation for existing lint rules
type: issue
state: closed
author: charliermarsh
labels:
  - documentation
assignees: []
created_at: 2023-02-07T23:56:08Z
updated_at: 2023-10-02T01:38:58Z
url: https://github.com/astral-sh/ruff/issues/2646
synced_at: 2026-01-12T15:54:43Z
```

# Add Markdown documentation for existing lint rules

---

_@charliermarsh_

If we write documentation on the `#[violation]` macro:

```rust
/// ### What it does
/// Checks for `self.assertRaises(Exception)`.
///
/// ## Why is this bad?
/// `assertRaises(Exception)` can lead to your test passing even if the
/// code being tested is never executed due to a typo.
///
/// Either assert for a more specific exception (builtin or custom), use
/// `assertRaisesRegex` or the context manager form of `assertRaises`.
#[violation]
pub struct NoAssertRaisesException;
```

This explanation shows up with `ruff rule B017`, and in the docs:

- https://github.com/charliermarsh/ruff/blob/main/docs/rules/assert-raises-exception.md
- https://beta.ruff.rs/docs/rules/assert-raises-exception/

We'll track progress here:

- [x] `airflow`
- [x] `eradicate`
- [x] `flake8-2020`
- [x] `flake8-annotations`
- [x] `flake8-async`
- [x] `flake8-bandit`
- [x] `flake8-blind-except`
- [x] `flake8-boolean-trap`
- [x] `flake8-bugbear`
- [x] `flake8-builtins`
- [x] `flake8-commas`
- [x] `flake8-comprehensions`
- [x] `flake8-copyright`
- [x] `flake8-datetimez`
- [x] `flake8-debugger`
- [x] `flake8-django`
- [x] `flake8-errmsg`
- [x] `flake8-executable`
- [x] `flake8-fixme`
- [x] `flake8-future-annotations`
- [x] `flake8-gettext`
- [x] `flake8-implicit-str-concat`
- [x] `flake8-import-conventions`
- [x] `flake8-logging-format`
- [x] `flake8-no-pep420`
- [x] `flake8-pie`
- [x] `flake8-print`
- [x] `flake8-pyi`
- [x] `flake8-pytest-style`
- [x] `flake8-quotes`
- [x] `flake8-raise`
- [x] `flake8-return`
- [x] `flake8-self`
- [x] `flake8-simplify`
- [x] `flake8-slots`
- [x] `flake8-tidy-imports`
- [x] `flake8-todos`
- [x] `flake8-type-checking`
- [x] `flake8-unused-arguments`
- [x] `flake8-use-pathlib`
- [x] `flynt`
- [x] `isort`
- [x] `mccabe`
- [x] `numpy`
- [x] `pandas-vet`
- [x] `pep8-naming`
- [x] `perflint`
- [x] `pycodestyle`
- [x] `pydocstyle`
- [x] `pyflakes`
- [x] `pygrep-hooks`
- [x] `pylint`
- [x] `pyupgrade`
- [x] `ruff`
- [x] `tryceratops`


---

_Label `documentation` added by @charliermarsh on 2023-02-07 23:56_

---

_Comment by @spaceone on 2023-02-08 17:34_

That's very nice!
In the README the rule is linked twice. Once for the code and once fore the human-readable-name. Isn't one link at the humann-readable-name enough? This would enable to double-click/select the rule name into copy&paste buffer instead of opening the link. 

---

_Comment by @charliermarsh on 2023-02-08 17:36_

We could link it once -- I don't feel strongly!

---

_Comment by @charliermarsh on 2023-02-10 01:50_

(Making a concerted effort to add these for all new rules.)

---

_Comment by @thatlittleboy on 2023-02-19 04:16_

Hi, I'm looking to do my part in contributing some docs~

**Quick question**: I noticed that in #2796  for example, there is a change to `bare_except.rs` and the OP added `docs/rules/bare_except.md`. Do I have to write the markdown file myself or is it a generated file (since it's largely replicated from the text in `define_violation!` macro)?

---

_Comment by @charliermarsh on 2023-02-19 04:22_

Thanks! Everything is generated from the rustdoc in the macro, so no need to touch any Markdown. (We actually don’t even check in the generated Markdown anymore, so it won’t appear as part of any PRs — only need to write the rustdoc part, and the rest is taken care of for you when we generate the docs :))

---

_Comment by @JonathanPlasse on 2023-03-08 17:05_

`try.ceratops` already has documented its rules in its [documentation](https://github.com/guilatrova/tryceratops/tree/main/docs/violations).

---

_Comment by @charliermarsh on 2023-03-08 18:44_

👍 I'd prefer not to copy over their documentation though, would prefer to write our own. But we can look to theirs for guidance.

---

_Comment by @charliermarsh on 2023-06-15 00:15_

Amazingly, thanks in large part to @tjkuson, we're now at 381 rules with documentation out of 634 total (> 60% coverage).

---

_Comment by @charliermarsh on 2023-06-17 16:58_

@tjkuson - FYI I shouted you out on Twitter :)

https://twitter.com/charliermarsh/status/1669139322077847553

---

_Comment by @tjkuson on 2023-06-17 17:21_

Aww, thanks! I think static code analysers are an under-appreciated learning resource, so am just happy to help out a bit.

---

_Comment by @harupy on 2023-07-23 02:08_

~I'll take `flake8-raise`.~

`flake8-raise` is already done :)

---

_Comment by @harupy on 2023-07-23 03:02_

I'll take `flake8-pytest-style`.

- [x] PT001
- [x] PT002
- [x] PT003
- [x] PT004
- [x] PT005
- [x] PT006
- [x] PT007
- [x] PT008
- [x] PT009
- [x] PT010
- [x] PT011
- [x] PT012
- [x] PT013
- ~PT014~
- [x] PT015
- [x] PT016
- [x] PT017
- [x] PT018
- [x] PT019
- [x] PT020
- [x] PT021
- [x] PT022
- [x] PT023
- [x] PT024
- [x] PT025
- [x] PT026

---

_Comment by @klistwan on 2023-08-01 06:01_

I'll take `flake8-datetimez`:

- [x] DTZ001
- [x] DTZ002
- [x] DTZ003
- [x] DTZ004
- [x] DTZ005
- [x] DTZ006
- [x] DTZ007
- [x] DTZ011
- [x] DTZ012


---

_Comment by @harupy on 2023-08-15 02:17_

`flake8-pytest-style` is complete!

---

_Comment by @charliermarsh on 2023-08-15 03:18_

It looks like we're down to 31 rules that lack documentation:

- [x] `call-with-shell-equals-true`
- [x] `start-process-with-a-shell`
- [x] `start-process-with-no-shell`
- [x] `call-datetime-strptime-without-zone`
- [x] `call-date-today`
- [x] `call-date-fromtimestamp`
- [x] `pass-statement-stub-body`
- [x] `non-empty-stub-body`
- [x] `typed-argument-default-in-stub`
- [x] `pass-in-class-body`
- [x] `argument-default-in-stub`
- [x] `assignment-default-in-stub`
- [x] `duplicate-union-member`
- [x] `quoted-annotation-in-stub`
- [x] `docstring-in-stub`
- [x] `unassigned-special-variable-in-stub`
- [x] `snake-case-type-alias`
- [x] `t-suffixed-type-alias`
- [x] `future-annotations-in-stub`
- [x] `stub-body-multiple-statements`
- [x] `no-return-argument-annotation-in-stub`
- [x] `unannotated-assignment-in-stub`
- [x] `string-or-bytes-too-long`
- [x] `numeric-literal-too-long`
- [x] `pytest-incorrect-fixture-name-underscore`
- [x] `missing-whitespace`
- [x] `unexpected-spaces-around-keyword-parameter-equals`
- [x] `missing-whitespace-around-parameter-equals`
- [x] `missing-whitespace-after-keyword`
- [x] `io-error`
- [x] `syntax-error`


---

_Closed by @charliermarsh on 2023-10-02 00:56_

---
