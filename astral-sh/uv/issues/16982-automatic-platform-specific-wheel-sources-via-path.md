```yaml
number: 16982
title: Automatic platform specific wheel sources via path
type: issue
state: open
author: danielkonecny
labels:
  - question
assignees: []
created_at: 2025-12-04T15:49:14Z
updated_at: 2025-12-10T19:29:11Z
url: https://github.com/astral-sh/uv/issues/16982
synced_at: 2026-01-12T16:02:41Z
```

# Automatic platform specific wheel sources via path

---

_@danielkonecny_

### Summary

This concerns a case where:
* wheels are provided via path - `[tool.uv.sources] package = [{path = "..."}]`; and
* wheels are compiled for different platforms/Python versions/... in separate wheel files -  `[tool.uv.sources] package = [{marker = "..."}]`

These two are hard to combine with uv - I searched the docs and haven't found any way how to specify this neatly.
When providing paths for those wheel in `pyproject.toml`, it is necessary to specify markers for each case (`python_version = ... and sys_platform = ... and platform_machine = ...`) and it becomes very long.

When resolving a package via index, this is done automatically, therefore, I would expect such thing possible with a folder with wheels instead of index while the naming convention of wheels is kept.

I would like to propose such feature, if it's already not available. If it is, please, let me know about the syntax and maybe it would be worth adding it to the docs as well. Thank you.

### Example

I would imagine it like this.

In `pyproject.toml` either (like a wildcard syntax):

```toml
[tool.uv.sources]
my_package = [{ path = "packages/my_package-*.whl" }]
```

or (via changing the index parameter from `url` to `path`):

```toml
[tool.uv.sources]
my_package = [{ index = "my_package-index" }]
[[tool.uv.index]]
name = "my_package-index"
path = "packages" # instead of url
explicit = true
```

---

_Label `enhancement` added by @danielkonecny on 2025-12-04 15:49_

---

_Comment by @konstin on 2025-12-04 15:53_

You looking for a flat index, which uses a directory and considers the files in it an index, similar to a remote index: https://docs.astral.sh/uv/concepts/indexes/#flat-indexes

---

_Label `enhancement` removed by @konstin on 2025-12-04 15:53_

---

_Label `question` added by @konstin on 2025-12-04 15:53_

---

_Comment by @danielkonecny on 2025-12-04 16:06_

Works like a charm, thank you very much! For future readers, this is the `pyproject.toml` syntax:

```toml
[tool.uv.sources]
my_package = [{ index = "my_package-index" }]
[[tool.uv.index]]
name = "my_package-index"
url = "path/to/packages"
format = "flat"
explicit = true
```

---

_Closed by @danielkonecny on 2025-12-04 16:06_

---

_Comment by @danielkonecny on 2025-12-05 10:35_

Hi @konstin, I stumbled upon one more trouble, if you could please help me. Is it possible to specify the path in the `tool.uv.index.url` with tilde (~) to get the home path? It throws an error that seems like the tilde isn't parsed in this case:
```
error: Failed to read `--find-links` directory: /path/to/project/root/~/path/to/packages
Caused by: The system cannot find the path specified. (os error 3)
```
Since the tilde is available for [path dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/#path), I guess it would be nice to have it here too.
I'm on Windows 11 and using the latest uv (0.9.15).

---

_Reopened by @danielkonecny on 2025-12-05 10:35_

---

_Comment by @konstin on 2025-12-08 11:22_

We currently don't actually support tilde expansion in uv paths, we currently support only either relative or absolute paths exclusively.

The example in the docs relies on shell expansion by the shell that `uv add` is invoked in:

```
$ echo ~/projects/bar/
/home/konsti/projects/bar/
```

---

_Comment by @danielkonecny on 2025-12-08 15:06_

Thank you for the info, I will try to find a workaround. Is there any timeline on the tilde expansion implementation? Can I propose it somewhere to get that functionality some traction? I see that this is currently being discussed in a related PR https://github.com/astral-sh/uv/pull/17019.

---

_Comment by @konstin on 2025-12-10 19:29_

Doing inference on string fields is a complex topic on its own (it goes from every path is interpreter verbatim by the OS to us adding a layer where some strings now suddenly mean something else depending on the environment you run in. e.g. `~` is technically a valid filename, and OS APIs made the choice to not support this, so we'd be making a choice above the OS APIs), I think https://github.com/astral-sh/uv/pull/17019 is a good place to discuss this.

---
