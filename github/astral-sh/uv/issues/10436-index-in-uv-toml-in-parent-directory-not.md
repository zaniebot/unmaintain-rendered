---
number: 10436
title: "`index` in uv.toml in parent directory not recognized"
type: issue
state: closed
author: NicholasPini
labels:
  - question
assignees: []
created_at: 2025-01-09T16:41:43Z
updated_at: 2025-01-10T09:02:12Z
url: https://github.com/astral-sh/uv/issues/10436
synced_at: 2026-01-07T13:12:18-06:00
---

# `index` in uv.toml in parent directory not recognized

---

_Issue opened by @NicholasPini on 2025-01-09 16:41_

I have a bunch of separated python projects. Since they all have the same private index in their pyproject.toml, I figured it is better to move the index definition to a uv.toml file in a parent directory:

```toml
# uv.toml

required-version = ">=0.5.16"

[[index]]
name = "my_index"
url = "..."
explicit = true
```

```toml
# pyproject.toml

# other stuff...

[tool.uv.sources]
my_private_dependency = { index = "my_index" }

```

The problem is that this is not recognized by uv, it says that
```
Failed to parse entry: `my_private_dependency`
  ╰─▶ Package `my_private_dependency` references an undeclared index: `my_index`
```

Note that putting the exact same index definition inside the pyproject.toml works, so the definition itself is correct.

Maybe indices are not supposed to go into a common uv.toml file, but I don't get any error about `index` not being permitted in uv.toml (like I would with, for example, the `source` settings). 

Also, the uv.toml file itsefil is read: again, if I for example put a `source` definition, I get an error saying that it is not permitted, thus uv is reading the uv.toml file as expected.

EDIT: Ok, after a bit more testing, the uv.toml is completely ignored. It does give an error if I add the `source` section, but otherwise it is not read at all. For example, setting `required-version = ">=0.5.17"` doesn't make uv fail the command (I'm using version 0.5.16). So basically the uv.toml is parent directories is completely ignored if a pyproject.toml file is present, but for some reason it complained about `source` being present.

EDIT2: I think I get what is happening. If there is a single `[tool.uv]` in the pyproject.toml file, the uv.toml file in parent directories is ignored. The problem is that I found no way to define `source` without using `[tool.uv.sources]`, and I'm forced to have that in the pyproject.toml file, so I think uv prevents me from using a global uv.toml as I intended.

---

_Comment by @charliermarsh on 2025-01-09 19:22_

You could define that as a user- or system-level configuration file, which would get merged into your configuration, e.g., at `~/.config/uv/uv.toml`. See: https://docs.astral.sh/uv/configuration/files/. But yeah, we don't merge arbitrary configuration files in the directory hierarchy.

---

_Label `question` added by @charliermarsh on 2025-01-09 19:22_

---

_Comment by @charliermarsh on 2025-01-09 19:23_

Oh, but we do _not_ allow indexes to be referenced by name that are defined outside of the current project. So the use of an explicit index like that wouldn't work. It was an intentional decision but there's a related issue somewhere asking for it.

---

_Comment by @charliermarsh on 2025-01-09 19:23_

Here it is: #9440

---

_Closed by @charliermarsh on 2025-01-10 03:39_

---

_Comment by @NicholasPini on 2025-01-10 09:02_

> You could define that as a user- or system-level configuration file, which would get merged into your configuration, e.g., at `~/.config/uv/uv.toml`. See: https://docs.astral.sh/uv/configuration/files/. But yeah, we don't merge arbitrary configuration files in the directory hierarchy.

I see. I understand why `index` and `sources` should not be in a global uv.toml. I think that not being able to reference a uv.toml in a parent directory is a bit of a missed opportunity for things like `required-version`. It would be nice to have a uv.toml in the root directory of a project with basic constraints and rules that all projects in the children directories must follow (like, again, `required-version`). Right now, I would need to update by hand all pyproject.toml files if I want to bump the minimum required version of uv for all projects.

I could define the uv.toml anyway and use the `--config-file` flag in all uv commands, possibly putting all of it in a nice justfile, but it's a bit of a hacky approach.

---
