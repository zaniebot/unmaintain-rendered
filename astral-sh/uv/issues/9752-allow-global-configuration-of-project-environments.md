---
number: 9752
title: allow global configuration of project environments
type: issue
state: open
author: mbtuckersimm
labels:
  - configuration
assignees: []
created_at: 2024-12-09T21:58:47Z
updated_at: 2025-01-07T19:24:41Z
url: https://github.com/astral-sh/uv/issues/9752
synced_at: 2026-01-10T01:24:45Z
---

# allow global configuration of project environments

---

_Issue opened by @mbtuckersimm on 2024-12-09 21:58_

I recently encountered [this problem installing ansible-lint](https://github.com/ansible/ansible-lint/issues/4348) for a work project. I won't reproduce all of that info here, but the gist of it is that in order to install a recent version of `ansible-lint`, I had to include the following chunk in my `pyproject.toml` file:

```
[tool.uv]
environments = ["platform_system != 'Windows'"]
```

This is fine, and I understand that this isn't really a problem with `uv` itself, but my work uses only Linux and macOS, and we'd like to configure this globally to avoid any potential problems with dependencies on Windows, and not have to remember to add this bit of config to the `pyproject.toml` file every time we start a new python project.

I tried adding

```
environments = ["platform_system != 'Windows'"]
```

to my `~/.config/uv/uv.toml` file, but it didn't help. My reading of the relevant [documentation](https://docs.astral.sh/uv/reference/settings/#environments) is that this is just a per-project setting, and that it can't be set globally. I also did not see any relevant environment variables that could be set, although perhaps I missed something.

So, I guess the question is: is there a way to set this globally? If not, please consider adding this feature.

---

_Comment by @cassieopea on 2024-12-09 22:26_

This affects not only my work but also my personal projects. It would be great if I could globally specify a list of default platform or platforms like:

```toml
platforms = ["linux", "macos"]
```

Another option would be to have project templates that allow you to easily reproduce core settings without braving the uncertainties of copypasta.

---

_Comment by @zanieb on 2025-01-07 19:24_

I don't think we can have a global configuration for environments — that would invalidate the lockfile in projects.

We could have a global default for `uv init`.

---

_Label `configuration` added by @zanieb on 2025-01-07 19:24_

---

_Referenced in [ansible/ansible-lint#4348](../../ansible/ansible-lint/issues/4348.md) on 2025-01-12 07:10_

---
