---
number: 8820
title: "Add autofix for `PYI024`"
type: issue
state: closed
author: Skylion007
labels: []
assignees: []
created_at: 2023-11-22T16:05:51Z
updated_at: 2023-11-22T18:29:51Z
url: https://github.com/astral-sh/ruff/issues/8820
synced_at: 2026-01-07T13:12:15-06:00
---

# Add autofix for `PYI024`

---

_Issue opened by @Skylion007 on 2023-11-22 16:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
`PYI024` seems like a relatively straightforward import rewrite fix `collections.namedtuple` -> `typing.NamedTuple`. We should add an autofix and it seems relatively straightforward. Might be a good first issue.

---

_Comment by @dhruvmanila on 2023-11-22 17:29_

Thanks for the suggestion but I don't think so as it's not just an import rewrite but also updating the references as highlighted in the example: https://docs.astral.sh/ruff/rules/collections-named-tuple/#example

It's probably impossible as the `typing` version requires the type information which the `collections` version doesn't provide. Happy to hear your thoughts on this but I'll close this to keep the issue tracker actionable.

---

_Closed by @dhruvmanila on 2023-11-22 17:29_

---

_Comment by @Skylion007 on 2023-11-22 18:29_

Well, in the trivial case listed in the example, we could infer the types. I agree though this is not quite as simple as originally envisioned though.

---

_Referenced in [astral-sh/ruff#16491](../../astral-sh/ruff/issues/16491.md) on 2025-03-04 08:53_

---
