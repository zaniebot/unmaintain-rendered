```yaml
number: 15874
title: "`uv lock` with optional dependency restricting main dependencies"
type: issue
state: closed
author: djsaunde
labels:
  - question
assignees: []
created_at: 2025-09-15T15:17:00Z
updated_at: 2025-09-16T18:53:00Z
url: https://github.com/astral-sh/uv/issues/15874
synced_at: 2026-01-12T16:02:18Z
```

# `uv lock` with optional dependency restricting main dependencies

---

_@djsaunde_

I'm running into an issue with the following (simplified):

```toml
[project]
dependencies = ["torch>=2.60"]

[project.optional-dependencies]
vllm = ["vllm>=0.10.0"]
```

When not installing vllm, I'd like to be able to resolve to `torch==2.8.0` by default, but latest vllm is pinned to `torch==2.7.1` for [reasons](https://github.com/vllm-project/vllm/issues/9554#issuecomment-2426840976).

Any solve for this?

 _Originally posted by @djsaunde in [#9349](https://github.com/astral-sh/uv/issues/9349#issuecomment-3285480827)_

---

_Comment by @zanieb on 2025-09-15 15:47_

I think we might solve different versions of torch if you do

```
[project]
dependencies = []

[project.optional-dependencies]
vllm = ["vllm>=0.10.0"]
default = ["torch>=2.60"]

[tool.uv]
conflicts = [
    [
      { extra = "vllm" },
      { extra = "default" },
    ],
]
```

I'm not sure if there's a better way today.

---

_Label `question` added by @zanieb on 2025-09-15 15:51_

---

_Comment by @djsaunde on 2025-09-16 18:43_

Thanks! When I run `uv sync` with this pyproject.toml, nothing is installed since all deps are optional. Is it recommended to do `uv sync --extra default` in this case, or is there a way to configure this to happen automatically?

---

_Comment by @zanieb on 2025-09-16 18:51_

Yeah that's what you need to do. We're considering https://github.com/astral-sh/uv/pull/12965 but it's sort of in limbo. You can do this hack

```
[tool.uv]
default-groups = ["default"]

[dependency-groups]
default = ["your_project_name[default]"]
```

to sync the extra by default. Then you'd need to use `--no-group default --extra vllm` for vllm (or `--no-default-groups`).

---

_Comment by @djsaunde on 2025-09-16 18:52_

Cheers, thanks!

---

_Closed by @djsaunde on 2025-09-16 18:53_

---
