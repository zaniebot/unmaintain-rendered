---
number: 8642
title: "Rule `E203` conflicts with formatter"
type: issue
state: closed
author: ofek
labels:
  - documentation
assignees: []
created_at: 2023-11-13T04:19:18Z
updated_at: 2025-01-27T10:53:43Z
url: https://github.com/astral-sh/ruff/issues/8642
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule `E203` conflicts with formatter

---

_Issue opened by @ofek on 2023-11-13 04:19_

Running the formatter on

```python
release_lines = history_file_lines[history_file_lines.index('## Unreleased') + 1: -1]
```

produces

```python
release_lines = history_file_lines[history_file_lines.index('## Unreleased') + 1 : -1]
```

which then fails the rule

```
scripts\utils.py:22:85: E203 Whitespace before ':'
```

Is the fix simply to document that here? https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules

---

_Comment by @charliermarsh on 2023-11-13 04:30_

I will add it to that list, though there's an open issue to fix the rule to allow this: https://github.com/astral-sh/ruff/issues/7259.

---

_Label `documentation` added by @charliermarsh on 2023-11-13 04:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-13 05:00_

---

_Referenced in [astral-sh/ruff#8654](../../astral-sh/ruff/pulls/8654.md) on 2023-11-13 15:50_

---

_Comment by @charliermarsh on 2023-11-13 16:42_

I decided to just fix it instead.

---

_Comment by @ofek on 2023-11-13 17:12_

Oh nice that's much better, thanks!

---

_Closed by @charliermarsh on 2023-11-13 17:16_

---

_Referenced in [yaegassy/coc-ruff#22](../../yaegassy/coc-ruff/issues/22.md) on 2024-02-09 21:39_

---

_Comment by @JohannesBuchner on 2025-01-27 09:27_

I am still getting this unwanted behaviour with ruff-0.9.3:

```
$ cat > foo.py
lanes[laneid + 1:]
$ ruff format foo.py
1 file reformatted
$ cat foo.py
lanes[laneid + 1 :]
```

---

_Comment by @dhruvmanila on 2025-01-27 10:43_

@JohannesBuchner That's an expected behavior. What the original issue was pointing out is that `E203` rule is _removing_ the whitespace before the `:` operator while the formatter adds it - causing the conflicting behavior between the linter and the formatter.

---

_Comment by @JohannesBuchner on 2025-01-27 10:53_

@dhruvmanila Thanks for the quick response. flake8 gives the error "E203 whitespace before ':'" with the reformatted output. I see from https://github.com/psf/black/issues/315 that they claim that this is an issue of pep8. There is help at https://ichard26-testblackdocs.readthedocs.io/en/refactor_docs/compatible_configs.html for configuring flake8. Thanks.

---

_Referenced in [archlinux/archinstall#3459](../../archlinux/archinstall/pulls/3459.md) on 2025-05-14 09:17_

---
