```yaml
number: 7313
title: uv run and pyproject configuration
type: issue
state: closed
author: patrickporto
labels:
  - question
assignees: []
created_at: 2024-09-11T21:45:16Z
updated_at: 2024-10-21T21:52:10Z
url: https://github.com/astral-sh/uv/issues/7313
synced_at: 2026-01-12T15:59:12Z
```

# uv run and pyproject configuration

---

_@patrickporto_

I searched some similar issue but I didn't find anything.

I have the following structure in my project:

```
workspace
├──  packages
│   ├── package-a
│   │    ├── pyproject.toml
│   └──  package-b
│          └── pyproject.toml
└──  pyproject.toml 

```

I have installed `pytest` and `pytest-env` at `package-a`. After that, I put the following configuration on package-a's pyproject.toml:

```toml
[tool.pytest_env]
AWS_ACCESS_KEY_ID = ""
AWS_SECRET_ACCESS_KEY = ""
AWS_REGION = ""
```

Finally I have tried to execute `uv run --package package-a pytest` from workspace root when I got a error for it didn't find the configuration on pyproject.toml. I only got execute this one if it is executed on package folder.

uv platform: Linux
uv version: 0.4.9

---

_Comment by @sirosc on 2024-09-13 17:59_

Are you using uv's [workspace](https://docs.astral.sh/uv/concepts/workspaces/) feature or using the term "workspace" to refer to a parent folder/repo?

---

_Comment by @patrickporto on 2024-09-13 22:00_

> Are you using uv's [workspace](https://docs.astral.sh/uv/concepts/workspaces/) feature or using the term "workspace" to refer to a parent folder/repo?

I am using workspace feature as it is documented 

---

_Comment by @charliermarsh on 2024-09-14 00:34_

This may depend on how pytest discovers that configuration. If it has to be in the current working directory specifically, then you may have to run `uv run pytest` from `./packages/package-a` instead of the workspace root, since we can't control pytest's config discovery. Does that work?

---

_Label `question` added by @charliermarsh on 2024-09-14 00:34_

---

_Comment by @patrickporto on 2024-09-17 01:39_

@charliermarsh I think a good way could be consider package as root on execute `uv run --package package-a pytest`. I don't believe that there is any use case for using pyproject.toml on workspace root instead of package. What do you think about setting package as the root on execution?

---

_Comment by @zanieb on 2024-10-21 21:51_

I don't think we can change the working directory, it'd break relative paths which kind of defeats the purpose of working from the workspace root.

---

_Closed by @zanieb on 2024-10-21 21:51_

---

_Comment by @zanieb on 2024-10-21 21:52_

Maybe you can use `uv --directory <path>`?

---
