```yaml
number: 15616
title: uv usage for non-python repositories
type: issue
state: closed
author: julien-lecomte
labels:
  - question
assignees: []
created_at: 2025-09-01T10:24:10Z
updated_at: 2025-09-01T15:13:03Z
url: https://github.com/astral-sh/uv/issues/15616
synced_at: 2026-01-12T16:02:13Z
```

# uv usage for non-python repositories

---

_@julien-lecomte_

### Question

I like to use `uv` to handle building documentation with sphinx for non-python programs.

For this reason, the `pyproject.toml` file can contain stuff which wouldn't be valid for a python project.

For example, the `project.name` might have spaces, or the version might be `0.7.6-nbxyz7.r12`.

Currently, uv will generate a warning, and that is fine.

This ticket is just to please consider not making these warnings become errors, unless there's a flag to ignore this new behavior.

Thanks!

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @julien-lecomte on 2025-09-01 10:24_

---

_Comment by @konstin on 2025-09-01 10:35_

Unfortunately, PEP 621 mandates specific rules for name and version for a valid `pyproject.toml`. For those projects, sphinx should offer options (such as `tool.sphinx`) to override the display name and display version to avoid creating an invalid `pyproject.toml`.

---

_Comment by @julien-lecomte on 2025-09-01 13:55_

I see the point concerning adding this to sphinx. But that would require `uv venv .venv` to know it's only for a sphinx usage.

But I will counterpoint that `uv venv .venv`, or `uv pip install` doesn't need (in some cases), to check if the name or version is correct. This is the part were it shows a warning. Maybe it should check/warn only if the name/version is used for something.

Of course, would it become an error, I would change my workflow. But I think I just needed to raise the point.

---

_Comment by @julien-lecomte on 2025-09-01 14:11_

Would adding two flags such as `UV_NO_VERSION_WARNING` be acceptable/accepted?

---

_Comment by @konstin on 2025-09-01 14:30_

This is not an issue around uv's behavior, it's about `pyproject.toml` being defined with a schema that only allows specific values for `project.name` and `project.version`. I'm surprised that Sphinx allows this, given that it's bound to the same rules and will fail to publish if it uses a non-compliant `pyproject.toml` itself. uv assumes a valid Python project for its operations, and we can't guarantee that we'll continue to accept invalid `pyproject.toml` files in the future.

What Sphinx settings are you using, do you have a reproducible example? According to https://www.sphinx-doc.org/en/master/usage/configuration.html#project-information Sphinx uses strings in Python, for which the strings you shared are valid. I couldn't find references to using `pyproject.toml` for configuration in their docs.

---

_Comment by @julien-lecomte on 2025-09-01 14:50_

This isn't Sphinx behavior. Sphinx probably doesn't even use pyproject.toml. It's more of a workflow issue, or a layout issue.

In the project, pre-commit and sphinx is used. But this isn't a python project. Some tools, such as codespell (used via pre-commit), use pyproject.toml.

For Sphinx, building in a venv is recommended. Since I'm moving from (...) to uv, I now have these warnings.

Because I use template projects (think cookiecutter), the `docs/conf.py` Sphinx file uses the values from `pyproject.toml`. This allows the file to be the same amongst all other projects, with only pyproject.toml containing the project specific stuff.

So, in order not to bother you much more, I think I'll add a specific tools.sphinx section, that overrides when required the root name and version.

I just committed and pushed my WIP so you can have a look for yourself (branch uv-example): https://gitlab.com/cappysan/asustor/netboot.xyz/-/tree/uv-example?ref_type=heads 




---

_Closed by @julien-lecomte on 2025-09-01 15:13_

---
