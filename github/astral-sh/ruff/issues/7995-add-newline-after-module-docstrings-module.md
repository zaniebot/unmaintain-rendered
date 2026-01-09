---
number: 7995
title: "Add newline after module docstrings (`module_docstring_newlines`)"
type: issue
state: closed
author: DanielNoord
labels:
  - formatter
  - preview
assignees: []
created_at: 2023-10-16T20:20:14Z
updated_at: 2023-11-29T00:34:03Z
url: https://github.com/astral-sh/ruff/issues/7995
synced_at: 2026-01-07T13:12:15-06:00
---

# Add newline after module docstrings (`module_docstring_newlines`)

---

_Issue opened by @DanielNoord on 2023-10-16 20:20_

```python
"""Test docstring"""
a = 1
```

On the playground this does not currently get a newline added with the `format` option turned on.

Recently a PR of mine got merged to `black` to include a newline after the module docstring:
https://github.com/psf/black/pull/3932
_Most credits should go to others though for that PR as I mostly copied and rebased code._

It's still in the preview style but got generally favourable responses when initially proposed a couple of year ago. Since nobody has really objected to it during its previous iterations I expect it to be part of the default style some day.

Would `ruff` be open to to include this change to `ruff format` as well?

_EDIT: Oh, and congrats on the `0.1.0` release 🎉_

---

_Comment by @charliermarsh on 2023-10-16 20:45_

Cool, so this is part of Black's preview style? If so, we'll likely add it under our own `--preview` flag soon.

(And thank you!)


---

_Label `formatter` added by @charliermarsh on 2023-10-16 20:45_

---

_Added to milestone `Formatter: Stable` by @charliermarsh on 2023-10-16 20:45_

---

_Comment by @charliermarsh on 2023-10-16 20:45_

@konstin - Perhaps another good / easy one for preview.

---

_Label `preview` added by @MichaReiser on 2023-10-17 00:42_

---

_Referenced in [astral-sh/ruff#6935](../../astral-sh/ruff/issues/6935.md) on 2023-10-17 00:43_

---

_Comment by @DanielNoord on 2023-10-17 06:48_

@charliermarsh Sorry should have been clearer. This is currently only available on `black` `main` as `preview`. (I run from `master` locally). You can see in the linked PR that it actually already applies to `blacks` own codebase. There hasn't been a release yet, I can ask them when they plan on doing so but I don't want to bother the maintainers too much.

---

_Comment by @konstin on 2023-10-18 13:09_

I added a test case in #8044, it's not trivial to fix though

---

_Comment by @MichaReiser on 2023-10-18 22:05_

> I added a test case in #8044, it's not trivial to fix though

We could try to format it as part of the module if the first statement in the suite is a doc comment without any leading comments? Not sure how to handle trailing suppression comments though.

---

_Assigned to @konstin by @konstin on 2023-10-20 10:45_

---

_Referenced in [astral-sh/ruff#8283](../../astral-sh/ruff/pulls/8283.md) on 2023-10-27 12:58_

---

_Closed by @charliermarsh on 2023-10-28 01:16_

---

_Referenced in [psf/black#4042](../../psf/black/issues/4042.md) on 2023-11-23 09:46_

---

_Renamed from "Add newline after module docstrings" to "Add newline after module docstrings (`module_docstring_newlines`)" by @MichaReiser on 2023-11-29 00:34_

---

_Referenced in [astral-sh/ruff#8678](../../astral-sh/ruff/issues/8678.md) on 2023-11-29 00:34_

---

_Referenced in [astral-sh/ruff#9639](../../astral-sh/ruff/pulls/9639.md) on 2024-02-12 17:28_

---

_Referenced in [psf/black#4175](../../psf/black/issues/4175.md) on 2024-03-10 14:48_

---
