---
number: 3435
title: Should ruff be invoked in the same venv as the python code?
type: issue
state: closed
author: mpizenberg
labels: []
assignees: []
created_at: 2023-03-10T09:26:38Z
updated_at: 2023-03-10T16:05:30Z
url: https://github.com/astral-sh/ruff/issues/3435
synced_at: 2026-01-10T01:22:42Z
---

# Should ruff be invoked in the same venv as the python code?

---

_Issue opened by @mpizenberg on 2023-03-10 09:26_

So ruff is a python linter. Are there any lints requiring access to correct `site-package` stuff or other things only available when executing in the same python context as the python code should be executed? (like for example pyright, which needs to check dependencies)

I’m mainly asking in the context of GitHub actions. I saw [the example action](https://beta.ruff.rs/docs/editor-integrations/#github-actions) in your documentation (thx for that!) which does not detail this point (maybe it’s irrelevant?).

In particular, if using [`hatch`](https://github.com/pypa/hatch), is there ever a need to run ruff with `hatch run ruff ...` or does ruff not care at all about the environment it’s run in?

---

_Comment by @charliermarsh on 2023-03-10 16:05_

Good question! No, it's not necessary. Ruff has no dependency on the current environment.

You might _want_ to manage Ruff alongside the rest of your environment if you care about keeping the dependency versions in-sync (i.e., if you have different projects that need to use different versions of Ruff). But Ruff doesn't behave differently when run inside or outside of a Python environment.

---

_Closed by @charliermarsh on 2023-03-10 16:05_

---
