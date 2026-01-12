```yaml
number: 10062
title: "CPY001: copyright without a year"
type: issue
state: closed
author: nijel
labels:
  - bug
assignees: []
created_at: 2024-02-20T12:47:59Z
updated_at: 2024-05-04T21:19:22Z
url: https://github.com/astral-sh/ruff/issues/10062
synced_at: 2026-01-12T15:54:49Z
```

# CPY001: copyright without a year

---

_@nijel_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

There are good reasons to omit year in copyright notice, see for example https://daniel.haxx.se/blog/2023/01/08/copyright-without-years/, however ruff CPY001 enforces it:

Code:

```py
# Copyright (c) Michal Čihař <michal@weblate.org>
```

Fails:

```console
$ ruff check --preview weblate/wsgi.py 
weblate/wsgi.py:1:1: CPY001 Missing copyright notice at top of file
  |
1 | # Copyright (c) Michal Čihař <michal@weblate.org>
  |  CPY001
```

---

_Label `bug` added by @charliermarsh on 2024-02-20 16:27_

---

_Comment by @charliermarsh on 2024-02-20 16:27_

Makes sense.

---

_Comment by @charliermarsh on 2024-02-20 16:41_

For the record, you can customize the regex here. It's exposed as a setting: https://docs.astral.sh/ruff/settings/#lint_flake8-copyright_notice-rgx.

---

_Comment by @nijel on 2024-02-20 17:24_

I know, but I would expect that out of the box the most common situations are handled gracefully. Following https://reuse.software/spec/#comment-headers would be really great 

---

_Comment by @nijel on 2024-02-20 18:42_

Here are the regexps reuse-tool uses:

https://github.com/fsfe/reuse-tool/blob/00a8d949b245218541fd0ece8a64c5480a9e84d7/src/reuse/_util.py#L115-L131

---

_Comment by @charliermarsh on 2024-03-29 20:01_

The reuse link does say: "The copyright notice SHOULD contain the year of publication."

---

_Comment by @nijel on 2024-03-30 08:20_

Indeed, there is SHOULD and not MUST. A copyright statement without a year is valid, and there can be good reasons for that (see before mentioned https://daniel.haxx.se/blog/2023/01/08/copyright-without-years/ for reasoning from curl developers).

---

_Comment by @charliermarsh on 2024-03-30 17:17_

That's true, but it seems reasonable for the rule to enforce a SHOULD by default. You can use https://docs.astral.sh/ruff/settings/#lint_flake8-copyright_notice-rgx to change the regex if you'd like.

---

_Comment by @charliermarsh on 2024-05-04 21:19_

I'm going to leave this as-is given that the spec is a SHOULD, and so we include it by default, but allow users to override it.

---

_Closed by @charliermarsh on 2024-05-04 21:19_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-04 21:19_

---
