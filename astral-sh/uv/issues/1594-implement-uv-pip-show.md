```yaml
number: 1594
title: "Implement \"uv pip show\""
type: issue
state: closed
author: jab
labels:
  - enhancement
  - help wanted
  - compatibility
assignees: []
created_at: 2024-02-17T15:09:29Z
updated_at: 2024-03-06T15:25:43Z
url: https://github.com/astral-sh/uv/issues/1594
synced_at: 2026-01-12T15:58:30Z
```

# Implement "uv pip show"

---

_@jab_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
```
❯ uv --version
uv 0.1.2

❯ uv pip show uv
error: unrecognized subcommand 'show'

Usage: uv pip [OPTIONS] <COMMAND>

For more information, try '--help'.

❯ pip show uv
Name: uv
Version: 0.1.2
Summary: An extremely fast Python package installer and resolver, written in Rust.
Home-page: https://pypi.org/project/uv/
Author: uv
Author-email: "Astral Software Inc." <hey@astral.sh>
License: MIT OR Apache-2.0
Location: /Users/jab/src/bidict/.venv/dev/lib/python3.12/site-packages
Requires:
Required-by:

❯ pip show -qq uv && echo uv installed || echo uv not installed
uv installed
```
As in the last example, the `-qq` form is particularly useful for querying just whether _any_ version of a particular package is installed, which is all I actually need.

It's possible to run `uv pip freeze | grep ...` instead, but it seems like it might be easy enough and worth it to add `uv pip show <pkg>` (or at least `uv pip show -qq <pkg>`), which should also be faster.

---

_Comment by @jab on 2024-02-17 15:11_

Relatedly, `uv pip list` is also missing, which has some overlap here. I'll hold off on creating a separate ticket for that for now though.

---

_Comment by @ismail on 2024-02-17 15:45_

@jab https://github.com/astral-sh/uv/issues/1401 for `pip list`.

---

_Label `enhancement` added by @zanieb on 2024-02-17 16:24_

---

_Label `compatibility` added by @zanieb on 2024-02-17 16:24_

---

_Label `help wanted` added by @zanieb on 2024-02-17 16:24_

---

_Comment by @charliermarsh on 2024-03-06 15:25_

This was added in https://github.com/astral-sh/uv/pull/2115.

---

_Closed by @charliermarsh on 2024-03-06 15:25_

---

_Comment by @charliermarsh on 2024-03-06 15:25_

Thanks @ChannyClaus!

---
