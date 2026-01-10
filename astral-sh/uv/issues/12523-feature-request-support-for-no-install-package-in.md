```yaml
number: 12523
title: "Feature Request: Support for no-install-package in pyproject.toml"
type: issue
state: closed
author: garam-kim1
labels:
  - enhancement
assignees: []
created_at: 2025-03-28T04:08:45Z
updated_at: 2025-04-02T01:40:16Z
url: https://github.com/astral-sh/uv/issues/12523
synced_at: 2026-01-10T03:41:47Z
```

# Feature Request: Support for no-install-package in pyproject.toml

---

_Issue opened by @garam-kim1 on 2025-03-28 04:08_

### Summary

# Summary
I would like to request the ability to configure the `no-install-package` option directly in the `pyproject.toml` file for uv. This would allow users to persistently exclude specific packages from installation when running `uv sync`, without needing to pass the `--no-install-package` flag every time.

# Motivation
Currently, the `--no-install-package` flag must be provided as a command-line argument, which can be inconvenient for projects that frequently need to exclude certain packages (e.g., platform-specific dependencies or optional packages). Adding support for this configuration in `pyproject.toml` would:
- Improve developer workflow by reducing repetitive command-line options.
- Allow teams to share consistent exclusion rules across environments via version-controlled configuration files.
- Enhance flexibility and usability of uv for complex dependency management scenarios.

# Proposed Solution
Introduce a new configuration option under `[tool.uv]` in `pyproject.toml`, such as:

```toml
[tool.uv]
no-install-packages = ["package1", "package2"]
```

When `uv sync` is run, any packages listed under `no-install-packages` would be excluded from installation automatically.

#### Example Usage
1. Add the following to `pyproject.toml`:
   ```toml
   [tool.uv]
   no-install-packages = ["example-package", "another-package"]
   ```

2. Run `uv sync`. The specified packages would be excluded from installation without requiring additional flags.

### Example

_No response_

---

_Label `enhancement` added by @garam-kim1 on 2025-03-28 04:08_

---

_Comment by @zanieb on 2025-04-01 19:37_

> would allow users to persistently exclude specific packages from installation when running uv sync

This is intentionally not allowed. `--no-install-package` is only intended for temporarily excluding packages for Docker layer caching and such.

> projects that frequently need to exclude certain packages (e.g., platform-specific dependencies or optional packages).

You should use platform markers and `project.optional-dependencies` for these?

Can you share a more concrete use-case?


---

_Assigned to @zanieb by @zanieb on 2025-04-01 19:38_

---

_Comment by @garam-kim1 on 2025-04-02 01:07_

Thank you for your response. I'd like to clarify my use case by drawing a parallel to Rye's implementation:

In Rye, there's a `tool.rye.excluded-dependencies` [configuration](https://rye.astral.sh/guide/pyproject/#toolryeexcluded-dependencies) that permanently excludes specific packages even when they're pulled in as indirect dependencies. Some other examples when we used it for:

1. Excluding dependencies with licenses incompatible with commercial use that appear as transitive dependencies
2. Preventing problematic dependencies that interfere with IDE functionality
3. Replacing certain dependencies with custom implementations without installing it

---

_Comment by @zanieb on 2025-04-02 01:14_

We support that now by setting an impossible marker, e.g., `override-dependencies = ["package; sys_platform = "never"]` (https://docs.astral.sh/uv/reference/settings/#override-dependencies). We could add a dedicated setting, but haven't yet. Unlike `--no-install-package`, I think this would omit that whole dependency tree.

---

_Comment by @garam-kim1 on 2025-04-02 01:30_

Thanks for the workaround! Using overrides-dependencies = ["package; sys_platform = 'never'"] solves. It would be great if this approach could be documented more prominently, or perhaps implementing a dedicated excluded-dependencies option in the future for simplicity. Appreciate your help!

---

_Comment by @zanieb on 2025-04-02 01:40_

I opened an issue to track a dedicated setting https://github.com/astral-sh/uv/issues/12616

---

_Closed by @zanieb on 2025-04-02 01:40_

---
