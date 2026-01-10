```yaml
number: 4190
title: RUF001 reports false positive for non-latin languages
type: issue
state: closed
author: berzi
labels: []
assignees: []
created_at: 2023-05-02T13:50:17Z
updated_at: 2023-05-02T14:00:48Z
url: https://github.com/astral-sh/ruff/issues/4190
synced_at: 2026-01-10T11:09:47Z
```

# RUF001 reports false positive for non-latin languages

---

_Issue opened by @berzi on 2023-05-02 13:50_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The RUF001 rule will detect individual unicode non-latin-alphabet characters as ambiguous even when they are part of a word or string which is entirely comprised of non-latin-alphabet characters. This results in lots of false positives in strings in other languages, and autofix (if enabled with "ALL", for example) will destructively change these strings.

The rule should be improved to be a bit more nuanced, or else it will have to be disabled on any codebase which contains other alphabets (I'm not adding noqa comments on each line where Ruff thinks г should be r).

Examples:
```py
# Afghanistan's name in Cyrillic
"Афганистан"  # RUF001 String contains ambiguous unicode character `г` (did you mean `r`?)
# Most other letters are also signalled, since they look similar to latin letters.

# And in Arabic:
"أفغانستان" # RUF001 String contains ambiguous unicode character `ا` (did you mean `l`?)

# Zimbabwe in Armenian:
"Զիմբաբվե"  # RUF001 String contains ambiguous unicode character `ա` (did you mean `w`?)
```

My current version of Ruff is 0.0.263.

---

_Comment by @MichaReiser on 2023-05-02 14:00_

Thank @berzi for reporting this issue. We're aware of the issue but aren't sure yet how to solve it best. 

One feature that we consider introducing is distinguishing between fixes that are safe to apply and fixes that may be incorrect because they change the semantics of the program (see #4181). I'm not sure if that would help in your situation. 

We're also considering demoting the `ALL` selector because it leads to confusion that this is our recommended rule set where it is not. It's okay for you to disable individual rules. I would even encourage you do hand pick the rules that you care about. We're considering improving this experience in the future by categorizing rules by their severity: e.g. all rules that detect errors that cause your program to crash, rules that detect code that is likely wrong, security issues, and rules that are very opinionated and you should only opt-in if you agree with the rule's choice. 

I'll go ahead and close this issue because we're already tracking this in #3947

---

_Closed by @MichaReiser on 2023-05-02 14:00_

---
