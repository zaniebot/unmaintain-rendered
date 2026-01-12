```yaml
number: 3070
title: "new cli option: --disable-noqa"
type: issue
state: closed
author: onerandomusername
labels:
  - cli
assignees: []
created_at: 2023-02-20T21:26:49Z
updated_at: 2023-03-02T03:28:14Z
url: https://github.com/astral-sh/ruff/issues/3070
synced_at: 2026-01-12T15:54:43Z
```

# new cli option: --disable-noqa

---

_@onerandomusername_

There are times when I want to go through the codebase and fix every error that uses a specific error code. It would be helpful to have a cli option to disable all noqa comments for a single command invoke.

For example, if I have a noqa comment for `E501`, I should be able to view all of my existing E501 errors by running `ruff . --disable-noqa --select E501`

---

_Label `cli` added by @charliermarsh on 2023-02-20 22:33_

---

_Comment by @charliermarsh on 2023-02-20 22:33_

We already support this internally (we use this behavior for `--add-noqa`), so in theory we could expose it with a flag yeah.

---

_Comment by @onerandomusername on 2023-02-21 23:41_

> We already support this internally (we use this behavior for `--add-noqa`), so in theory we could expose it with a flag yeah.

Thank you! I chose `--disable-noqa` because its what flake8 uses, but I wouldn't be opposed to `--ignore-noqa` or any other name.

---

_Comment by @charliermarsh on 2023-02-21 23:45_

Yeah I'd probably prefer `--ignore-noqa` personally.

---

_Comment by @sciyoshi on 2023-02-22 14:38_

Should there be a way to only disable/ignore noqa for certain error codes? For example, a codebase may want to disable using noqa for specific errors altogether. Maybe as a new config in `[tool.ruff]` for this purpose, e.g. `ignore-noqa = [ "E501" ]` would prevent `# noqa: E501` from working altogether.

---

_Comment by @not-my-profile on 2023-02-23 07:03_

> Should there be a way to only disable/ignore noqa for certain error codes?

I'd rather have us implement a `forbid` or `force-deny` lint level as per https://github.com/charliermarsh/ruff/issues/1256#issuecomment-1414165258.

> Yeah I'd probably prefer `--ignore-noqa` personally.

I think the CLI option should be called something more generic since we might want to also support `# pylint:` directives in the future.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-02 02:54_

---

_Comment by @charliermarsh on 2023-03-02 03:09_

Added in #3296. I left it as `--ignore-noqa` for now since it's consistent with (e.g.) `--add-noqa` but I'm open to suggestions.

---

_Closed by @charliermarsh on 2023-03-02 03:28_

---
