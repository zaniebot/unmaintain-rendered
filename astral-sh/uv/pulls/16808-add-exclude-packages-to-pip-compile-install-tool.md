```yaml
number: 16808
title: "Add `--exclude-packages` to pip compile/install/tool install"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: exclude-packages
created_at: 2025-11-21T17:13:09Z
updated_at: 2025-11-21T21:49:25Z
url: https://github.com/astral-sh/uv/pull/16808
synced_at: 2026-01-12T16:12:27Z
```

# Add `--exclude-packages` to pip compile/install/tool install

---

_@terror_

Part of https://github.com/astral-sh/uv/issues/16771

This diff introduces `--exclude-packages` to pip {compile, install, tool install}, accepting comma-delimited package names (with `--exclude-package` as an alias) to exclude packages without an excludes file.

---

_@zanieb reviewed on 2025-11-21 21:35_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1385 on 2025-11-21 21:35_

I think we need to copy the additional content from `--excludes` above.

> When a package is excluded, it will be omitted from the dependency list entirely and its own dependencies will be ignored during the resolution phase

---

_@zanieb reviewed on 2025-11-21 21:37_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1385 on 2025-11-21 21:37_

I'd also just say "Exclude the given packages from resolution".

We'll often also say something like "May be provided multiple times or multiple values can be provided separated by commas"


---

_Comment by @zanieb on 2025-11-21 21:40_

I need to think a bit more about what we want long-term, i.e., if we want to go this route or something like `--exclude` / `--exclude-file` long-term. I'm wary of adding more options without a clear plan since it'll just make it harder.

Otherwise, this looks good!

---

_Assigned to @zanieb by @zanieb on 2025-11-21 21:40_

---

_Comment by @terror on 2025-11-21 21:48_

> I need to think a bit more about what we want long-term, i.e., if we want to go this route or something like `--exclude` / `--exclude-file` long-term. I'm wary of adding more options without a clear plan since it'll just make it harder.
> 
> Otherwise, this looks good!

I'm more a fan of what you just mentioned, i.e. `--exclude` / `--exclude-file`. IMO, the generic `--exclude` taking package names makes a bit more sense here.

---
