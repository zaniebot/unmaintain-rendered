---
number: 6604
title: "Compatibility with Briefcase Projects: Support for New Configuration File"
type: issue
state: open
author: NADOOITChristophBa
labels: []
assignees: []
created_at: 2024-08-25T13:17:17Z
updated_at: 2025-01-17T23:45:36Z
url: https://github.com/astral-sh/uv/issues/6604
synced_at: 2026-01-10T01:24:03Z
---

# Compatibility with Briefcase Projects: Support for New Configuration File

---

_Issue opened by @NADOOITChristophBa on 2024-08-25 13:17_

**Title:** Compatibility with Briefcase Projects: Support for New Configuration File

**Issue Description:**

I am working on a project using Briefcase and encountered compatibility issues when trying to integrate `uv`. The main problem arises from the different configurations expected by `uv` and Briefcase, specifically in the `pyproject.toml` file.

### Problem Description

- **Project Structure:** Briefcase projects have a specific structure where the `pyproject.toml` file is located in the root directory. Here's a simplified version of the structure:
  ```
  .
  ├── pyproject.toml
  ├── src
  │   └── project_name
  │       ├── __init__.py
  │       ├── app.py
  │       └── ...
  └── tests
  ```

- **Configuration Differences:**
  - **Briefcase** uses a `[tool.briefcase]` section in the `pyproject.toml` file.
  - **uv** expects a `[project]` section and a `[tool.uv]` section for dependencies.

### Steps to Reproduce

1. Create a Briefcase project with the structure mentioned above.
2. Navigate to the project directory and try to run a `uv` command, e.g., `uv add ruff`.
3. Observe the error indicating that the `pyproject.toml` file does not have the expected `[project]` table.

### Error Messages

When running `uv add ruff` in the project root:
```
error: No `pyproject.toml` found in current directory or any parent directory
```

When running `uv add ruff` in the `project_name` subdirectory:
```
error: No `project` table found in: `/path/to/project/pyproject.toml`
```

### Proposed Solution

To resolve this compatibility issue, I propose the following:

- **Introduce a New Configuration File:** Allow `uv` to recognize a new configuration file in the base folder that can be used to specify the `[project]` and `[tool.uv]` sections. This file could be named `uv.toml` or similar.
- **Configuration Flexibility:** Provide an option to configure the location and name of this file, so it can coexist with the Briefcase `pyproject.toml` without conflicts.

### Additional Information

- **Command Used:** `uv add ruff`
- **uv Platform:** macOS
- **uv Version:** 0.3.3 (deea6025a 2024-08-23)
- **macOS Version:** [Please specify your macOS version, e.g., macOS Sonoma 14.6.1]

Thank you for considering this feature request. This enhancement would greatly improve the integration of `uv` with projects managed by Briefcase, allowing for more seamless development workflows.

---

_Comment by @freakboy3742 on 2024-09-05 01:57_

BeeWare/Briefcase maintainer here. Definitely keen to work with Astral/uv maintainers on anything related to Briefcase integration; the exact form of that integration is the open question. 

I wouldn't *necessarily* recommend adding support for Briefcase's configuration conventions to uv. I'd argue it's likely better for *Briefcase* to support *uv's* conventions, and use uv internally. beeware/briefcase#1367 is an existing ticket asking for Poetry support in Briefcase; I imagine the uv-based approach would be similar. 

The constraint on this is usually whether the installation tool can support four key features:
1. Platform-specific dependencies (i.e., only install library X if we're installing for platform Y)
2. Support for iOS, Android, and web as recognised platforms. 
3. "Host-mode" installation - installing dependencies into a specific target location that *isn't* the site-packages of the currently running Python interpreter
4. Cross-platform targeting - installing dependencies for a platform that *isn't* the currently running platform (e.g., installing iOS dependencies when on macOS.

To date, I haven't done any exploration of uv to know the current state of these features. If they already exist, then it might be possible to add uv support to Briefcase.

---

_Comment by @KeeonTabrizi on 2025-01-17 21:43_

What's recommended if then I need to maintain a uv pyproject.toml and a briefcase pyproject.toml what dir structure is recommended?

---

_Comment by @freakboy3742 on 2025-01-17 23:45_

> What's recommended if then I need to maintain a uv pyproject.toml and a briefcase pyproject.toml what dir structure is recommended?

At least for now, if you need to use Briefcase, the recommendation is "don't use uv". Briefcase doesn't currently have *any* uv integration, for the reasons listed above. 

This is something we'd *like* to address - but no solution exists at present.

In terms of project structure, though - both uv and briefcase use pyproject.toml for configuration, so there's no special considerations involved.

---
