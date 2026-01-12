```yaml
number: 3445
title: "uv pip compile complains that my pyproject.toml dependencies aren't pinned after update to 0.1.40"
type: issue
state: closed
author: VerdantFox
labels:
  - bug
assignees: []
created_at: 2024-05-08T00:29:08Z
updated_at: 2024-05-08T01:50:56Z
url: https://github.com/astral-sh/uv/issues/3445
synced_at: 2026-01-12T15:58:43Z
```

# uv pip compile complains that my pyproject.toml dependencies aren't pinned after update to 0.1.40

---

_@VerdantFox_

In `uv==0.1.39` the command `uv pip compile --output-file=requirements-prod.txt  pyproject.toml` works without error. On `uv==0.1.40`, when I run the same command, I get the following error:

```bash
❯ uv pip compile --output-file=requirements-prod.txt  pyproject.toml
error: Failed to parse `pyproject.toml`
  Caused by: Failed to parse entry for: `aiocache`
  Caused by: Must specify a version constrain
```

If I pin every single file in `pyproject.toml`, it works, but I don't want to pin those versions in `pyproject.toml`. That's what the lockfile is for. Do you have any idea what's going on? Is this breaking change intended, or is this a bug? Is there a new flag I should be using?

Thanks for the help! :)

I'm running this on Ubuntu 22.04.

---

_Comment by @kpeez on 2024-05-08 00:31_

I'm getting the same exact error when attempting to compile dependencies from my `pyproject.toml` file. Not sure if this is intended/expected behavior but it's breaking installations that previously worked without issue.

---

_Label `bug` added by @zanieb on 2024-05-08 00:53_

---

_Comment by @zanieb on 2024-05-08 00:53_

Sounds like a bug! Thanks for the report. We'll investigate. Can you share a minimal example `pyproject.toml` file?

---

_Comment by @charliermarsh on 2024-05-08 00:53_

Thanks, this is already fixed in https://github.com/astral-sh/uv/pull/3443.

---

_Closed by @charliermarsh on 2024-05-08 00:53_

---

_Comment by @charliermarsh on 2024-05-08 00:54_

I will cut a new release tonight.

---

_Comment by @VerdantFox on 2024-05-08 01:50_

Thanks! Quick work. I appreciate the effort and this project. :)

---
