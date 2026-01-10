```yaml
number: 8081
title: Update pip compatibility guide to note transitive URL dependency support
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/tr
created_at: 2024-10-10T10:13:52Z
updated_at: 2024-10-11T18:27:08Z
url: https://github.com/astral-sh/uv/pull/8081
synced_at: 2026-01-10T12:54:02Z
```

# Update pip compatibility guide to note transitive URL dependency support

---

_Pull request opened by @charliermarsh on 2024-10-10 10:13_

## Summary

Closes https://github.com/astral-sh/uv/issues/8080.


---

_Label `documentation` added by @charliermarsh on 2024-10-10 10:13_

---

_Merged by @charliermarsh on 2024-10-10 10:21_

---

_Closed by @charliermarsh on 2024-10-10 10:21_

---

_Branch deleted on 2024-10-10 10:21_

---

_@slochower reviewed on 2024-10-11 03:03_

---

_Review comment by @slochower on `docs/pip/compatibility.md`:195 on 2024-10-11 03:03_

When reading the original text and this version, it sounds (to me) like you are suggesting a workaround of adding the URL dependency to *your* `pyproject.toml` or `requirements.in` (in the case that you depend on registry package A which depends on package B with a URL dependency). Rather, I think the advice is to add the URL dependency directly to package A in that scenario. Would phrasing like this be more clear?

> If uv rejects a transitive URL dependency in a package from a registry, the best course of action is to provide the URL dependency as a direct dependency in the `pyproject.toml` or `requirement.in` file of the registry package, as the above constraints do not apply to direct dependencies.

Maybe it is a little clunky, but I definitely spent a few hours thinking I could just add the transitive URL dependency as a direct dependency in *my* package and that would satisfy `uv`.

---

_@charliermarsh reviewed on 2024-10-11 09:43_

---

_Review comment by @charliermarsh on `docs/pip/compatibility.md`:195 on 2024-10-11 09:43_

If you depend on a package A from the registry, which lists package B as a direct URL dependency (e.g., `B @ https://...`), adding package B to your `pyproject.toml` _will_ work. That's the recommended solution. Something like:

```toml
[project]
dependencies = ["A", "B @ https://..."]
```

---

_@slochower reviewed on 2024-10-11 14:21_

---

_Review comment by @slochower on `docs/pip/compatibility.md`:195 on 2024-10-11 14:21_

Ah, sorry -- I'm talking about one more layer of redirection. Say I'm writing a script with inline metadata. The script depends on registry package A. Registry package A depends on registry package B. In package B's `pyproject.toml` is a URL dependency. Adding that URL dependency directly to the inline metadata of a script will not satisfy `uv run`. I'm not sure there is a solution. Is there?

---

_@charliermarsh reviewed on 2024-10-11 14:45_

---

_Review comment by @charliermarsh on `docs/pip/compatibility.md`:195 on 2024-10-11 14:45_

I would actually still expect that to work. Are you running into a failure there? The way to think about it is: it's actually fine if registry packages depend on URL dependencies, but uv has to _know_ about all possible URL dependencies upfront.

---

_Review comment by @slochower on `docs/pip/compatibility.md`:195 on 2024-10-11 15:50_

If I run without adding the URL dependency, I get:

```
error: Package `openeye-toolkits-python3-osx-universal` attempted to resolve via URL: https://pypi.anaconda.org/openeye/simple/openeye-toolkits-python3-osx-universal/2023.1.1/OpenEye-toolkits-python3-osx-universal-2023.1.1.tar.gz. URL dependencies must be expressed as direct requirements or constraints....
```

If I add `openeye-toolkits-python3-osx-universal @ https://...` to my script metadata, I then see:

```
error: No `project` table found in: `/Volumes/xxx/pyproject.toml
```

If I use `uv run --no-project` I still see the same thing, confusingly.

```
❯ uv run tmp.py --no-project

error: No `project` table found in: `/Volumes/xxx/pyproject.toml`
(.venv) 
```

The script metadata looks like this:

```
# /// script
# requires-python = ">=3.10"
# dependencies = [
#   "package-hosted-on-internal-registry==3.3.0",
#   "openeye-toolkits-python3-osx-universal @ https://pypi.anaconda.org/openeye/simple/openeye-toolkits-python3-osx-universal/2023.1.1/OpenEye-toolkits-python3-osx-universal-2023.1.1.tar.gz"
# ]
# ///
```

---

_@slochower reviewed on 2024-10-11 15:50_

---

_@charliermarsh reviewed on 2024-10-11 17:54_

---

_Review comment by @charliermarsh on `docs/pip/compatibility.md`:195 on 2024-10-11 17:54_

I think you need to put the no-project flag before the filename, not after. Otherwise, it’s treated as an argument to the Python call.

---

_@slochower reviewed on 2024-10-11 18:27_

---

_Review comment by @slochower on `docs/pip/compatibility.md`:195 on 2024-10-11 18:27_

Ah! Great, thanks. 

This comment of yours really helped:

> The way to think about it is: it's actually fine if registry packages depend on URL dependencies, but uv has to know about all possible URL dependencies upfront.

And maybe some form could be put into the documentation. 

---
