```yaml
number: 12745
title: "Configuration for --no.dev as a default for \"uv run\""
type: issue
state: closed
author: bgschaid
labels:
  - question
assignees: []
created_at: 2025-04-08T14:31:40Z
updated_at: 2025-07-29T12:20:34Z
url: https://github.com/astral-sh/uv/issues/12745
synced_at: 2026-01-12T16:01:11Z
```

# Configuration for --no.dev as a default for "uv run"

---

_@bgschaid_

### Summary

I set up a Docker image with a Flask-project. During development this is tested with py,test and I added pytest to the dev dependency-group

When building the image the Dockerfile uses 
`uv sync --frozen --no-dev` 
to install only the things necessary to run the app. But when I start it with 
`uv run flask` 
it first installs the unnecessary packages before running. 

I am aware that 
`uv run --no-dev flask` 
allows avoiding it. But it is a bit surprising/annoying

### Example

It would be nice if there was an option that "fixes" the --no-dev (for instance --as-no-dev for "as a non-developer") where when setting up with 
`uv sync --frozen --as-no-dev` 
installs without the dev-packages and marks the project in such a way that 
`uv run flasks`
runs without installing the dev-packages while 
`uv run --with-dev py.test`
makes sure that the development packages are installed

This would allow "opting in" to the new behaviour

---

_Label `enhancement` added by @bgschaid on 2025-04-08 14:31_

---

_Comment by @charliermarsh on 2025-04-08 14:34_

You can configure this by setting `tool.uv.default-groups` to `[]` (rather than the default, which is `["dev"]`).

---

_Label `question` added by @zanieb on 2025-04-08 17:22_

---

_Label `enhancement` removed by @zanieb on 2025-04-08 17:22_

---

_Comment by @bgschaid on 2025-04-14 07:24_

The Problem for me is that pyproject.toml is under version control and I have to change that depending on the purpose of the installation (which makes pulls of updates awkward). 
So I thought "OK. The standard is in pyproject.toml. Local differences I'll put into uv.toml (that is supposed to override pyproject.toml)" but uv says
```
warning: Found both a `uv.toml` file and a `[tool.uv]` section in an adjacent `pyproject.toml`. The `[tool.uv]` section will be ignored in favor of the `uv.toml` file.
error: Failed to parse: `uv.toml`. The `default-groups` field is not allowed in a `uv.toml` file. `default-groups` is only applicable in the context of a project, and should be placed in a `pyproject.toml` file instead.
```
which means that doesn't work on two levels

- I can not selectively override defaults 
- I can not override that setting at all


---

_Comment by @konstin on 2025-04-14 07:32_

Does `uv run --no-sync` work for the docker container?

---

_Comment by @paduszyk on 2025-07-29 12:03_

Don't get me wrong. I'm in love with ruff, but installing `dev` dependencies by default is the only feature I don't like. IMHO `uv sync` should just install "production" dependencies only, without any groups nor extras.

Therefore, `default-groups = []` is the most reasonable choice. For me this is the most natural approach. When I want to include development (or any other) dependencies I should emphasize that, e.g. by passing a flag `--dev`. If something in development doesn't work, then... I see, I forgot about the `--dev` flag. This is an easy fix. On the other hand, if I forget to add `--no-dev` in production, my environment (e.g. Docker image) is polluted by packages that aren't actually needed...



---

_Comment by @zanieb on 2025-07-29 12:20_

You set up a production image once, you run uv all day while developing. It doesn't really make sense to make the latter more verbose instead of the former by default.

I don't think we'll be changing this as described here, I'm going to close this issue so it's not on our backlog.

---

_Closed by @zanieb on 2025-07-29 12:20_

---
