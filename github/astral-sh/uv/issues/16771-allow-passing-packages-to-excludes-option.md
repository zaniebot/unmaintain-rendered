---
number: 16771
title: "Allow passing packages to `--excludes` option"
type: issue
state: open
author: terror
labels:
  - enhancement
assignees: []
created_at: 2025-11-18T18:10:29Z
updated_at: 2025-11-21T15:02:05Z
url: https://github.com/astral-sh/uv/issues/16771
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow passing packages to `--excludes` option

---

_Issue opened by @terror on 2025-11-18 18:10_

There are cases where users may want to exclude a single package, but end up having to [write it to a file](https://x.com/vllm_project/status/1988832111659479440) in order to do so, which I find a bit awkward. We should consider allowing `--excludes` for pip {compile, install, tool install} to accept package names alongside actual paths.

For instance, the following flow:

```
uv self update
echo xformers > exclude.txt
uv pip install -U vllm --extra-index-url [https://wheels.vllm.ai/nightly](https://t.co/Y5FwbIWlUu) --excludes exclude.txt
```

would become:

```
uv self update
uv pip install -U vllm --extra-index-url [https://wheels.vllm.ai/nightly](https://t.co/Y5FwbIWlUu) --exclude xformers
```

This may be a bit tricky to implement, since it will likely require us to make a guess as to what argument is a path or a package name.

---

_Label `enhancement` added by @terror on 2025-11-18 18:10_

---

_Comment by @zanieb on 2025-11-20 14:35_

I think we'll probably want a separate argument, e.g., `--exclude-package`, to avoid disambiguating between paths and names? In general, I think we want this functionality for constraints, excludes, and overrides so we'll want something coherent amongst them.

Unfortunately we named this `exclude-dependencies` (etc.) in the `pyproject.toml`, but we could consider rename that at some point too.

We could also consider changing the existing arguments, first using inference to determine if it's a file or name then deprecating one of the paths. e.g., `--constraint <requirement>` or `--exclude <package>` could be the long-term goal and we could add a `--constraint-file` and `--exclude-file` that we encourage users to use instead for the file option.

---

_Comment by @charliermarsh on 2025-11-21 02:34_

Another option would be like...

`--exclude-package ${packageName}` on the CLI, `exclude-packages` in the `pyproject.toml`, and then `--excludes ${filePath}` for a list from a file.

`--constraint-package ${packageName}>=1` on the CLI, `constraint-packages` in the `pyproject.toml`, and then `--constraints ${filePath}` for a list from a file.


---

_Comment by @terror on 2025-11-21 05:08_

Should we also consider `--exclude-packages ${packageName} ... ${packageName}` and `--constraint-packages ${packageName}>=1 ... ${packageName}>=1` on the CLI? I mention the single package case in the issue, but this might also apply to 2 or 3 or more (i.e. a small amount).

---

_Comment by @zanieb on 2025-11-21 15:02_

We generally can't use space delimited names in the CLI because it creates ambiguous parsing. We usually prefer `,`. There are various places where we allow this (and I'd expect to here) 

---

_Referenced in [astral-sh/uv#16808](../../astral-sh/uv/pulls/16808.md) on 2025-11-21 17:14_

---
