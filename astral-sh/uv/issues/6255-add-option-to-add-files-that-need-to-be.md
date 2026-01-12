```yaml
number: 6255
title: Add option to add files that need to be considered for the cache key in pyproject.toml
type: issue
state: closed
author: albertferras-vrf
labels:
  - enhancement
assignees: []
created_at: 2024-08-20T15:36:34Z
updated_at: 2024-09-10T01:43:07Z
url: https://github.com/astral-sh/uv/issues/6255
synced_at: 2026-01-12T15:59:02Z
```

# Add option to add files that need to be considered for the cache key in pyproject.toml

---

_@albertferras-vrf_

Consider the following `pyproject.toml` file, which uses dynamic metadata to reference `requirements.in` files.

```
[project]
name = "myproject"
version = "0.1.0"
requires-python = ">=3.9"
dynamic = ["dependencies"]

[tool.setuptools.dynamic]
dependencies = {file = ["requirements.in"]}
```

When invoking the command `uv pip compile -o requirements.txt pyproject.toml` then setuptools is used to build the project, which is the tool that understands the `dynamic` and `tool.setuptools.dynamic` configuration. Once the build is finished, uv reads the package's `requires.txt` and generates `requirements.txt`.
The problem is that after (only) modifying `requirements.in`, the command `uv pip compile` does not update the `requirements.txt`file. This is because the content `pyproject.toml` did not change and `uv` decide to not reinstall the package (call setuptools) again.
The current workaround is to add `--reinstall` or `--reinstall-package=myproject` to the command or configuration and force setuptools to run.

This was discussed in discord, and the user `@ThiefMaster` suggested that we could add a new configuration under `[tool.uv]` which allows specifying which files have to be considered as part of 'pyproject.toml's cache key. 
One example would be:

```
[tool.uv]
cache_key_files = ["requirements.in", "requirements.dev.in"]
```

Worth mentioning also (not part of this ticket) that it would be good if `uv` supported popular dynamic implementations natively and not need to invoke setuptools for it.

---

_Label `enhancement` added by @charliermarsh on 2024-08-20 17:15_

---

_Comment by @charliermarsh on 2024-09-04 02:38_

I want to work on this but I don't yet have a good solution for `setuptools_scm` or other things that depend on the Git state.

---

_Comment by @charliermarsh on 2024-09-04 02:49_

I could special-case `.git` in the array though it's not a great API.

---

_Comment by @charliermarsh on 2024-09-04 02:51_

@konstin @zanieb -- any ideas? If we're gonna support this, I think we need an answer for `setuptools_scm` too since it's so popular.

---

_Comment by @zanieb on 2024-09-04 02:53_

Well... something like `cache_key_paths = ["path.txt", { file = "other-path.txt }, { git = "." })` is an option? e.g. allow dictionaries so we can define types and default to paths if a simple value is provided? (`git = "."` being a Git repository rooted in the project directory, we could also do like `git = true` but I'm not sure it's better)

---

_Comment by @charliermarsh on 2024-09-04 02:58_

That seems reasonable to me.

---

_Comment by @zanieb on 2024-09-04 03:01_

How complicated does support for Git state need to be? Like... the commit hash changed? There are unstaged changes? The tree is dirty? Number of commits since a tag? etc.

---

_Comment by @charliermarsh on 2024-09-04 03:03_

I _think_ "commit hash changed" is fine.

---

_Comment by @charliermarsh on 2024-09-04 03:07_

I guess `setuptools-scm` will also reflect uncommitted changes, but that may not be necessary.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-06 21:16_

---

_Closed by @charliermarsh on 2024-09-09 20:19_

---

_Closed by @charliermarsh on 2024-09-09 20:19_

---

_Comment by @charliermarsh on 2024-09-10 01:43_

This exists as of v0.4.8. You can do things like:

```toml
[tool.uv]
cache-keys = [{ file = "pyproject.toml" }, { file = "requirements.txt" }, { git = true }]
```

---
