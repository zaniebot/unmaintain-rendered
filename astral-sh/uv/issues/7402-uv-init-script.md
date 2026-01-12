```yaml
number: 7402
title: uv init --script
type: issue
state: closed
author: jbvsmo
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-09-15T01:15:47Z
updated_at: 2024-09-28T20:03:16Z
url: https://github.com/astral-sh/uv/issues/7402
synced_at: 2026-01-12T15:59:13Z
```

# uv init --script

---

_@jbvsmo_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


According to the docs, you can `uv add --script foo bar.py` and it will add the PEP 723 boilerplate to an existing script

https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies

There is no way to do this without a adding a dependency nor without an existing python file. E.g. this kinda works as a uv init

```
touch foo.py
uv add --script foo.py requests --python 3.12
```

It would also be nice to add the main function and `if __name__ == "__main__":` 

So I propose `uv init --script foo.py` to exist and be consistent with `uv init foo`.

Rationale: PEP 723 syntax is very forgettable and I don't know of a single person or [ai bot](https://chatgpt.com/share/66e634db-4ccc-8002-9fcd-c47143723d0b) that can write it by heart

Thank you!




---

_Comment by @zanieb on 2024-09-15 13:13_

I'm supportive of this

---

_Label `help wanted` added by @zanieb on 2024-09-15 13:13_

---

_Label `cli` added by @zanieb on 2024-09-15 13:13_

---

_Comment by @charliermarsh on 2024-09-28 20:03_

Added in https://github.com/astral-sh/uv/pull/7565.

---

_Closed by @charliermarsh on 2024-09-28 20:03_

---
