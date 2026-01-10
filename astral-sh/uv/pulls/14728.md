```yaml
number: 14728
title: "feat: Add PEP 751 multi-use lock file support with extras and dependency groups markers"
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
base: main
head: toml-extra
created_at: 2025-07-18T15:20:55Z
updated_at: 2026-01-09T16:57:49Z
url: https://github.com/astral-sh/uv/pull/14728
synced_at: 2026-01-10T05:49:14Z
```

# feat: Add PEP 751 multi-use lock file support with extras and dependency groups markers

---

_Pull request opened by @gaborbernat on 2025-07-18 15:20_

This PR adds [PEP 751](https://packaging.python.org/en/latest/specifications/pylock-toml/) multi-use lock file support when exporting to `pylock.toml` format. 📦

### 🎯 What is a Multi-Use Lock File?

A multi-use lock file describes **all possible packages** that might be installed across different scenarios, rather than just the packages for one specific configuration. This enables:

- **Multiple environments from one file**: Install for Python 3.8, 3.9, 3.10+ all from the same lock file
- **Optional features**: Choose which extras (like `dev`, `test`, `docs`) to install at install-time
- **Dependency groups**: Select which dependency groups to include (like PEP 735 groups)
- **Platform-specific packages**: Different wheels for Linux, macOS, Windows from one source

The lock file uses **markers** to indicate when each package should be installed, allowing installers to select the appropriate subset.

### 🎯 What This Implements

#### 1. Root-Level PEP 751 Metadata Fields

The `pylock.toml` output now includes the required PEP 751 metadata fields:

- `extras`: All optional dependencies available in the project (e.g., `["dev", "test", "docs"]`)
- `dependency-groups`: All dependency groups defined in the project (e.g., `["coverage", "type"]`)
- `default-groups`: Synthetic groups for default installation (currently empty per spec)

#### 2. Dependency Markers for Extras and Groups

Package dependencies include PEP 751 markers using the container membership syntax:

- `'test' in extras` - indicates this dependency is only needed with the `test` extra
- `'dev' in dependency_groups` - indicates this dependency is only needed with the `dev` group
- Standard environment markers like `python_full_version >= '3.10'` work alongside these

### 🔄 Why Marker Conversion?

uv's internal lock format uses "universal markers" that encode extras and groups in a special format. PEP 751 specifies a different syntax using container membership:

| Internal Format                | PEP 751 Format               |
| ------------------------------ | ---------------------------- |
| `extra == 'extra-3-pkg-cpu'`   | `'cpu' in extras`            |
| `extra == 'group-5-myapp-dev'` | `'dev' in dependency_groups` |

The conversion module parses the encoded markers and transforms them to PEP 751 syntax, making the `pylock.toml` output compliant with the specification.

### 📋 Real-World Examples

#### Extras Example: virtualenv

Here's an excerpt from `uv export --format pylock.toml` showing both root-level fields and dependency markers:

```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.8"
extras = [
    "docs",
    "test",
]

[[packages]]
name = "virtualenv"
dependencies = [
    { name = "covdefaults", version = "2.3.0", marker = "python_full_version >= '3.8' and 'test' in extras" },
    { name = "coverage", version = "7.6.1", marker = "python_full_version == '3.8.*' and 'test' in extras" },
    { name = "coverage", version = "7.10.7", marker = "python_full_version == '3.9.*' and 'test' in extras" },
    { name = "coverage", version = "7.13.1", marker = "python_full_version >= '3.10' and 'test' in extras" },
    { name = "distlib", version = "0.4.0", marker = "python_full_version >= '3.8'" },
    { name = "filelock", version = "3.16.1", marker = "python_full_version == '3.8.*'" },
    { name = "filelock", version = "3.19.1", marker = "python_full_version == '3.9.*'" },
    { name = "filelock", version = "3.20.2", marker = "python_full_version >= '3.10'" },
    { name = "furo", version = "2024.8.6", marker = "python_full_version == '3.8.*' and 'docs' in extras" },
    { name = "furo", version = "2025.12.19", marker = "python_full_version >= '3.9' and 'docs' in extras" },
    { name = "pytest", version = "8.3.5", marker = "python_full_version == '3.8.*' and 'test' in extras" },
    { name = "pytest", version = "8.4.2", marker = "python_full_version == '3.9.*' and 'test' in extras" },
    { name = "pytest", version = "9.0.2", marker = "python_full_version >= '3.10' and 'test' in extras" },
    { name = "sphinx", version = "7.1.2", marker = "python_full_version == '3.8.*' and 'docs' in extras" },
    { name = "sphinx", version = "7.4.7", marker = "python_full_version == '3.9.*' and 'docs' in extras" },
    { name = "sphinx", version = "8.1.3", marker = "python_full_version == '3.10.*' and 'docs' in extras" },
]
directory = { path = ".", editable = true }
```

Note how:

- The root-level `extras` field lists all available extras: `["docs", "test"]`
- Direct dependencies (distlib, filelock) have only version markers
- Test extra dependencies (covdefaults, coverage, pytest) include `'test' in extras` markers
- Docs extra dependencies (furo, sphinx) include `'docs' in extras` markers
- Multiple versions are included to support different Python versions from the same file

#### Dependency Groups Example: filelock

Here's an excerpt showing dependency groups:

```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.11"
dependency-groups = [
    "coverage",
    "dev",
    "docs",
    "fix",
    "pkg-meta",
    "test",
    "type",
]

[[packages]]
name = "filelock"
dependencies = [
    { name = "covdefaults", version = "2.3.0", marker = "python_full_version >= '3.11' and 'coverage' in dependency_groups" },
    { name = "coverage", version = "7.13.1", marker = "python_full_version >= '3.11' and 'coverage' in dependency_groups" },
    { name = "diff-cover", version = "10.1.0", marker = "python_full_version >= '3.11' and 'coverage' in dependency_groups" },
    { name = "mypy", version = "1.19.1", marker = "python_full_version >= '3.11' and 'dev' in dependency_groups" },
    { name = "pytest", version = "9.0.2", marker = "python_full_version >= '3.11' and 'dev' in dependency_groups" },
    { name = "sphinx", version = "9.0.4", marker = "python_full_version == '3.11.*' and 'dev' in dependency_groups" },
    { name = "sphinx", version = "9.1.0", marker = "python_full_version >= '3.12' and 'dev' in dependency_groups" },
]
directory = { path = ".", editable = true }
```

The root-level `dependency-groups` field lists all available groups, and dependencies include appropriate markers like `'coverage' in dependency_groups` or `'dev' in dependency_groups`.

### 🔧 Usage

```bash
# Export a project to pylock.toml format
uv export --format pylock.toml > pylock.toml

# Install from the multi-use lock file (base dependencies only)
uv pip install -r pylock.toml

# Install with specific extras
uv pip install -r pylock.toml --extra test --extra docs

# Install with specific dependency groups
uv pip install -r pylock.toml --group dev

# Combine extras and groups
uv pip install -r pylock.toml --extra test --group coverage
```

The pylock.toml filename must follow the pattern `pylock.*.toml` (e.g., `pylock.toml`, `pylock.dev.toml`, `pylock.prod.toml`) as specified in [PEP 751](https://packaging.python.org/en/latest/specifications/pylock-toml/).

### 📝 Epilog: `pip compile` and Project Metadata

While `uv pip compile` can output to `pylock.toml` format, it currently does not populate the root-level `extras` and `dependency-groups` fields. Here's why and when it could:

**Current behavior:** `pip compile` is designed to be **source-agnostic**. You can compile from:

- Plain requirements files: `uv pip compile requirements.in`
- Multiple heterogeneous sources: `uv pip compile req1.in pyproject.toml requirements.txt`
- Arbitrary packages on the CLI: `uv pip compile` with packages as arguments

When compiling from arbitrary sources without a clear project context, there's no single coherent answer to "what extras and groups are available?"

**Future potential:** When `pip compile` is explicitly given a project context—either by compiling a pyproject.toml directly (`uv pip compile pyproject.toml`) or from the current directory when it contains a pyproject.toml—it could extract and populate these fields. This would be a reasonable enhancement but requires additional logic to handle edge cases.

**Recommended approach:** For full project-aware multi-use lock files, use `uv export --format pylock.toml` instead. It operates on a single project with complete access to pyproject.toml metadata, ensuring accurate population of all PEP 751 fields.

**Note:** A pylock.toml with empty or missing `extras` and `dependency-groups` fields is perfectly valid per PEP 751. These fields are optional. The `pip compile` output will work fine with `uv pip install -r`.

Resolves #13060


---

_Comment by @charliermarsh on 2025-07-18 15:54_

I think this is more involved because we also need to preserve extra markers throughout the graph? Right now I believe they (intentionally) get resolved away `ExportableRequirements::from_lock`.


---

_Comment by @gaborbernat on 2025-07-18 16:04_

> I think this is more involved because we also need to preserve extra markers throughout the graph? Right now I believe they (intentionally) get resolved away `ExportableRequirements::from_lock`.

Yeah, 🤔 this just computes for now the passed in extras/groups, but populating that into packages likely is more complicated. Looking at it, but feedback welcome.

---

_Renamed from "Populate complete pylock.toml" to "Populate more information into pylock.toml" by @gaborbernat on 2025-07-18 16:04_

---

_@paveldikov reviewed on 2025-07-18 18:31_

---

_Review comment by @paveldikov on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:1 on 2025-07-18 18:31_

question: is the aim here to cover `uv export` only? Or `uv pip compile` to be covered as well?

---

_@gaborbernat reviewed on 2025-07-19 15:10_

---

_Review comment by @gaborbernat on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:1 on 2025-07-19 15:10_

For now let's start with `uv export`, `pip compile` can come afterwards.

---

_Renamed from "Populate more information into pylock.toml" to "feat: Add PEP 751 multi-use lock file support with extras and dependency groups markers" by @gaborbernat on 2026-01-08 19:45_

---

_Marked ready for review by @gaborbernat on 2026-01-08 22:19_

---
