---
number: 9940
title: "`uv lock` doesn't accept `--env-file` as a flag"
type: issue
state: closed
author: hdrwilkinson
labels:
  - question
assignees: []
created_at: 2024-12-16T16:46:14Z
updated_at: 2024-12-16T22:08:16Z
url: https://github.com/astral-sh/uv/issues/9940
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv lock` doesn't accept `--env-file` as a flag

---

_Issue opened by @hdrwilkinson on 2024-12-16 16:46_

I'm wanting to install a github private repo as part of a `uv sync` however, in the `uv lock` stage there is an error because ` > [lock 5/5] RUN --mount=type=secret,id=_env     bash -c 'uv lock --env-file /run/secrets/_env':
#0 0.070 error: unexpected argument '--env-file' found`


```bash
docker build --secret=id=_env,src=.build_secrets.env -f DOCKERFILE -t project_name .
```

```DOCKERFILE
...
FROM base AS lock

# Set working directory
WORKDIR /app

# Copy required files
COPY pyproject.toml .
COPY .pre-commit-config.yaml .
COPY .build_secrets.env .

# Use BuildKit secrets and pass the env file directly to uv lock
RUN --mount=type=secret,id=_env \
    bash -c 'uv lock --env-file /run/secrets/_env'
...
```

```pyproject.toml
<before>
dependencies = [
    <other_dependencies>,
    "desired_repo",
]

[tool.uv.sources]
desired_repo = { git = "https://${ACCESS_TOKEN}@github.com/path/to/repo"}
```

How can I make this work?

---

_Comment by @charliermarsh on 2024-12-16 22:06_

Only `uv run` accepts an env file. The env file only applies to the command that's executed by `uv run`, and not to uv itself.

---

_Label `question` added by @charliermarsh on 2024-12-16 22:06_

---

_Comment by @charliermarsh on 2024-12-16 22:08_

If you want general `.env` support, you should follow https://github.com/astral-sh/uv/issues/8862.

---

_Closed by @charliermarsh on 2024-12-16 22:08_

---
