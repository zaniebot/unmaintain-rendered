---
number: 10437
title: "Add a --no-dev option to `uv pip` commands"
type: issue
state: closed
author: soapergem
labels: []
assignees: []
created_at: 2025-01-09T16:50:53Z
updated_at: 2025-01-10T13:26:47Z
url: https://github.com/astral-sh/uv/issues/10437
synced_at: 2026-01-10T01:24:54Z
---

# Add a --no-dev option to `uv pip` commands

---

_Issue opened by @soapergem on 2025-01-09 16:50_

It would be nice to include a `--no-dev` option to commands like:
* uv pip install
* uv pip freeze

This option is already supported in other contexts (uv sync, uv run, etc.). And this is frankly necessary for the specific use case of working with Cloud Functions. Both Google Cloud Functions and Azure Functions require you package a requirements.txt file in order to deploy. And AWS Lambda effectively requires you to run `uv pip install --target . .` to bundle the dependencies directly within the same folder. In other words, all three require the use of uv's pip interface.

And with any cloud function, it is desirable to keep the package size as small as possible, which means excluding dev dependencies from the final artifact. There doesn't seem to be a way to do that through uv's pip interface at the moment but would be easily remedied by simply adding a `--no-dev` option.

---

_Comment by @zanieb on 2025-01-09 18:06_

What does `--no-dev` do in this context? `uv pip install` doesn't include development dependencies by default.

---

_Comment by @soapergem on 2025-01-10 01:15_

Here's an example of the problem:

```bash
uv init
uv add boto3
uv add --dev pytest
uv pip freeze > requirements.txt
```

That last command will include pytest, iniconfig, packaging, and pluggy. I want to be able to call `uv pip freeze --no-dev` and exclude those dev depedencies.

EDIT: it looks like you are right, this isn't a problem with `uv pip install`. So it's really just an issue with `uv pip freeze`.

---

_Comment by @charliermarsh on 2025-01-10 01:33_

You should probably use uv export, not uv pip freeze. uv export has a no-dev flag.

---

_Comment by @notatallshaw on 2025-01-10 01:36_

Yeah, `uv pip freeze` should do what `pip freeze` does and look at your currently installed environment and not the uv project information.

Otherwise it's going to get really confusing trying to use `uv pip` and think about where it starts to dip into the uv project api.

---

_Comment by @zanieb on 2025-01-10 04:42_

Hopefully these answers cleared things up. The `uv pip` interface does not read from projects (matching the upstream `pip` behavior) and cannot determine if packages in the environment are development dependencies.

---

_Closed by @zanieb on 2025-01-10 04:42_

---

_Comment by @soapergem on 2025-01-10 13:26_

It does clear things up! I was previously unaware of `uv export` so thanks for highlighting that.

---
