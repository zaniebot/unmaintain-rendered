```yaml
number: 14728
title: "feat: Complete PEP 751 multi-use lock file implementation for pylock.toml"
type: pull_request
state: open
author: gaborbernat
labels: []
assignees: []
base: main
head: toml-extra
created_at: 2025-07-18T15:20:55Z
updated_at: 2026-01-13T08:15:26Z
url: https://github.com/astral-sh/uv/pull/14728
synced_at: 2026-01-13T08:23:24Z
```

# feat: Complete PEP 751 multi-use lock file implementation for pylock.toml

---

_@gaborbernat_

This PR adds [PEP 751](https://packaging.python.org/en/latest/specifications/pylock-toml/) multi-use lock file support when exporting to `pylock.toml` format.

### What is a Multi-Use Lock File?

A multi-use lock file describes **all possible packages** that might be installed across different scenarios, rather than just the packages for one specific configuration. This enables:

- **Multiple environments from one file**: Install for Python 3.8, 3.9, 3.10+ all from the same lock file
- **Optional features**: Choose which extras (like `dev`, `test`, `docs`) to install at install-time
- **Dependency groups**: Select which dependency groups to include (like PEP 735 groups)
- **Platform-specific packages**: Different wheels for Linux, macOS, Windows from one source

The lock file uses **markers** to indicate when each package should be installed, allowing installers to select the appropriate subset.

### What This Implements

This PR adds the missing PEP 751 metadata fields and marker syntax to enable full multi-use lock file support:

- **`environments`**: Resolution forks showing which platform/Python combinations are supported
- **`extras`**: List of available optional dependencies
- **`dependency-groups`**: List of available dependency groups (PEP 735)
- **`default-groups`**: Synthetic groups for default installation (currently empty per spec)
- **`[tool.uv]`**: Tool metadata with version and command used to generate the lock
- **PEP 751 marker syntax**: `'extra' in extras` and `'group' in dependency_groups` for root project dependencies

### Feature Examples

Each feature is shown individually below, followed by a complete example showing all features together.

#### 1. `environments` Field

Shows which platform/Python version combinations the lock file was created for:

**Input (`pyproject.toml`):**
```toml
[tool.uv]
environments = [
  "sys_platform == 'darwin'",
  "sys_platform == 'linux'",
]
```

**Output (`pylock.toml`):**
```toml
lock-version = "1.0"
created-by = "uv"
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]
requires-python = ">=3.12"
```

This allows consumers to check if the lock file is compatible with their target platform before attempting installation.

---

#### 2. `extras` Field

Lists all optional dependencies available in the project:

**Input (`pyproject.toml`):**
```toml
[project.optional-dependencies]
test = ["pytest>=8.0"]
docs = ["sphinx>=7.0"]
```

**Output (`pylock.toml`):**
```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.12"
extras = [
    "docs",
    "test",
]
```

Installers can use this to validate requested extras and show available options.

---

#### 3. `dependency-groups` Field

Lists all dependency groups defined in the project (PEP 735):

**Input (`pyproject.toml`):**
```toml
[dependency-groups]
dev = ["ruff>=0.4.0"]
type = ["mypy>=1.10.0"]
```

**Output (`pylock.toml`):**
```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.12"
dependency-groups = [
    "dev",
    "type",
]
```

Installers can use this to validate requested dependency groups and show available options.

---

#### 4. `default-groups` Field

Synthetic groups representing default installation (currently always empty):

**Output (`pylock.toml`):**
```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.12"
extras = []
dependency-groups = []
default-groups = []
```

Per PEP 751, this is only needed when `packages.marker` necessitates synthetic groups for `project.dependencies`. This is an edge case; uv defaults to empty which is spec-compliant.

---

#### 5. `[tool.uv]` Table

Records the uv version and command used to generate the lock file with portable paths:

**Output (`pylock.toml`):**
```toml
[tool.uv]
version = "0.0.11"
command = [
    "uv",
    "export",
    "--format",
    "pylock.toml",
]
```

Enables lock file regeneration and debugging by tracking exact tool version and command. Command paths are portable (just "uv", not full path like `/usr/bin/uv`).

---

#### 6. Root Project Extras with PEP 751 Markers

Root project dependencies use PEP 751 `'extra' in extras` syntax:

**Input (`pyproject.toml`):**
```toml
[project]
name = "my-project"
dependencies = ["flask>=3.0"]

[project.optional-dependencies]
test = ["pytest>=8.0"]
```

**Output (`pylock.toml`):**
```toml
[[packages]]
name = "my-project"
dependencies = [
    { name = "flask", version = "3.0.2" },
    { name = "pytest", version = "8.1.1", marker = "'test' in extras" },
]
directory = { path = ".", editable = true }
```

The `pytest` dependency has marker `'test' in extras` indicating it's only installed when the `test` extra is requested.

---

#### 7. Root Project Dependency Groups with PEP 751 Markers

Root project dependencies use PEP 751 `'group' in dependency_groups` syntax:

**Input (`pyproject.toml`):**
```toml
[project]
name = "my-project"
dependencies = ["flask>=3.0"]

[dependency-groups]
dev = ["ruff>=0.4.0"]
```

**Output (`pylock.toml`):**
```toml
[[packages]]
name = "my-project"
dependencies = [
    { name = "flask", version = "3.0.2" },
    { name = "ruff", version = "0.4.5", marker = "'dev' in dependency_groups" },
]
directory = { path = ".", editable = true }
```

The `ruff` dependency has marker `'dev' in dependency_groups` indicating it's only installed when the `dev` dependency group is requested.

---

#### 8. Transitive Package Extras with PEP 508 Markers

Transitive packages (not the root project) use standard PEP 508 `extra == 'name'` syntax:

**Output (`pylock.toml`):**
```toml
[[packages]]
name = "flask"
version = "3.0.2"
dependencies = [
    { name = "click", version = "8.1.7" },
    { name = "python-dotenv", version = "1.0.1", marker = "extra == 'dotenv'" },
]
```

The `python-dotenv` dependency uses PEP 508 syntax because it's a dependency of `flask`, not the root project.

---

### Complete Example with All Features

**Input (`pyproject.toml`):**
```toml
[project]
name = "my-project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["flask>=3.0"]

[project.optional-dependencies]
test = ["pytest>=8.0"]
docs = ["sphinx>=7.0"]

[dependency-groups]
dev = ["ruff>=0.4.0"]
type = ["mypy>=1.10.0"]

[tool.uv]
environments = [
  "sys_platform == 'darwin'",
  "sys_platform == 'linux'",
]
```

**Output (`pylock.toml`):**
```toml
lock-version = "1.0"
created-by = "uv"
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]
requires-python = ">=3.12"
extras = [
    "docs",
    "test",
]
dependency-groups = [
    "dev",
    "type",
]

[tool.uv]
version = "0.0.11"
command = [
    "uv",
    "export",
    "--format",
    "pylock.toml",
]

[[packages]]
name = "my-project"
marker = "sys_platform == 'darwin' or sys_platform == 'linux'"
dependencies = [
    { name = "flask", version = "3.0.2", marker = "sys_platform == 'darwin' or sys_platform == 'linux'" },
    { name = "pytest", version = "8.1.1", marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'test' in extras" },
    { name = "sphinx", version = "7.2.6", marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'docs' in extras" },
    { name = "ruff", version = "0.4.5", marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'dev' in dependency_groups" },
    { name = "mypy", version = "1.10.0", marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'type' in dependency_groups" },
]
directory = { path = ".", editable = true }

[[packages]]
name = "flask"
version = "3.0.2"
marker = "sys_platform == 'darwin' or sys_platform == 'linux'"
dependencies = [
    { name = "click", version = "8.1.7", marker = "sys_platform == 'darwin' or sys_platform == 'linux'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/.../flask-3.0.2-py3-none-any.whl", hashes = { sha256 = "..." } },
]

[[packages]]
name = "pytest"
version = "8.1.1"
marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'test' in extras"
wheels = [
    { url = "https://files.pythonhosted.org/packages/.../pytest-8.1.1-py3-none-any.whl", hashes = { sha256 = "..." } },
]

[[packages]]
name = "ruff"
version = "0.4.5"
marker = "(sys_platform == 'darwin' or sys_platform == 'linux') and 'dev' in dependency_groups"
wheels = [
    { url = "https://files.pythonhosted.org/packages/.../ruff-0.4.5-py3-none-any.whl", hashes = { sha256 = "..." } },
]
```

**Features demonstrated:**
1. `environments` lists platform markers
2. `extras` lists available extras
3. `dependency-groups` lists available groups
4. `[tool.uv]` records generation metadata
5. Packages have combined platform + extras/groups markers
6. Root project uses PEP 751 syntax (`'test' in extras`, `'dev' in dependency_groups`)

---

### Marker Syntax Summary

| Dependency Type | Marker Syntax | Example |
|----------------|---------------|---------|
| Unconditional | *(no marker)* | `{ name = "flask", version = "3.0.2" }` |
| Platform-specific | PEP 508 | `marker = "sys_platform == 'win32'"` |
| Root project extra | PEP 751 | `marker = "'test' in extras"` |
| Root project group | PEP 751 | `marker = "'dev' in dependency_groups"` |
| Transitive extra | PEP 508 | `marker = "extra == 'dotenv'"` |

---

### PEP 751 Compliance Status

#### ✅ Fully Implemented

All **required** fields:
- `lock-version`
- `created-by`
- `[[packages]]` and `packages.name`

All **optional** core fields:
- `environments` (resolution fork markers)
- `requires-python`
- `extras`
- `dependency-groups`
- `default-groups` (empty by design, spec-compliant)
- `[tool]` table with tool metadata
- All package source types (`vcs`, `directory`, `archive`, `sdist`, `wheels`)
- All sub-fields (`upload-time`, `hashes`, `marker`, `requires-python`, `dependencies`)
- PEP 751 marker syntax (`'extra' in extras`, `'group' in dependency_groups`)
- Installation from pylock.toml files via `uv pip install -r pylock.toml`

#### ⚠️ Not Yet Implemented (Optional Features)

1. **`[[packages.attestation-identities]]`**: Not implemented. PEP 751 says tools "SHOULD include the attestation identities found" for supply chain security/provenance tracking.

2. **`[packages.tool]`**: Not implemented. Allows package-level tool metadata (similar to root-level `[tool]` but per-package).

---

### Usage

```bash
# Export a project to pylock.toml format
uv export --format pylock.toml > pylock.toml

# Export with platform restrictions
# (in pyproject.toml: tool.uv.environments = ["sys_platform == 'linux'"])
uv export --format pylock.toml > pylock.linux.toml

# Install from the multi-use lock file (base dependencies only)
uv pip install -r pylock.toml

# Install with specific extras
uv pip install -r pylock.toml --extra test --extra docs

# Install with specific dependency groups
uv pip install -r pylock.toml --group dev --group type

# Install with both extras and dependency groups
uv pip install -r pylock.toml --extra test --group dev
```

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

_Review comment by @konstin on `crates/uv/tests/it/export.rs`:3652 on 2026-01-12 09:26_

Those dependencies aren't conditional - do we need a simplify/complexify, or is this a marker graph implication?

https://inspector.pypi.io/project/anyio/3.7.0/packages/68/fe/7ce1926952c8a403b35029e194555558514b365ad77d75125f521a2bec62/anyio-3.7.0-py3-none-any.whl/anyio-3.7.0.dist-info/METADATA#line.26

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/lowering.rs`:174 on 2026-01-12 09:28_

The added docstrings don't add information here. We should either keep as-is, or add docstrings that explain e.g. what list

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/marker_conversion.rs`:68 on 2026-01-12 09:31_

We should not parse this format, we know what conflicts there are from resolution input and resolution output.

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/marker_conversion.rs`:113 on 2026-01-12 09:34_

dead code

---

_@konstin reviewed on 2026-01-12 09:34_

---

_@gaborbernat reviewed on 2026-01-12 15:35_

---

_Review comment by @gaborbernat on `crates/uv/tests/it/export.rs`:3652 on 2026-01-12 15:35_

Ah, you're correct fixed it.

---

_Review requested from @konstin by @gaborbernat on 2026-01-12 15:55_

---

_@konstin reviewed on 2026-01-12 18:32_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/marker_conversion.rs`:7 on 2026-01-12 18:32_

These comments are now outdated

---

_@konstin reviewed on 2026-01-12 18:50_

---

_Review comment by @konstin on `crates/uv/tests/it/export.rs`:3881 on 2026-01-12 18:50_

Here, we need to use `extra ==`: That is not provided by the lockfile, but by flask.

Can you add a test case specifically for having conflicting dependencies and transitives extras?

---

_Review comment by @gaborbernat on `crates/uv/tests/it/export.rs`:3881 on 2026-01-12 19:16_

Done now I believe 🤔 

---

_@gaborbernat reviewed on 2026-01-12 19:16_

---

_Review requested from @konstin by @gaborbernat on 2026-01-12 19:59_

---

_Renamed from "feat: Add PEP 751 multi-use lock file support with extras and dependency groups markers" to "feat: Add PEP 751 environments field and tool metadata to pylock.toml export" by @gaborbernat on 2026-01-12 23:37_

---

_Renamed from "feat: Add PEP 751 environments field and tool metadata to pylock.toml export" to "feat: Implement PEP 751 multi-use lock files with extras, dependency groups, and environments" by @gaborbernat on 2026-01-12 23:38_

---

_Renamed from "feat: Implement PEP 751 multi-use lock files with extras, dependency groups, and environments" to "feat: Implement PEP 751 multi-use lock files with extras, groups, and environments" by @gaborbernat on 2026-01-12 23:39_

---

_Renamed from "feat: Implement PEP 751 multi-use lock files with extras, groups, and environments" to "feat: Complete PEP 751 multi-use lock file implementation for pylock.toml" by @gaborbernat on 2026-01-12 23:39_

---

_@konstin reviewed on 2026-01-13 08:13_

---

_Review comment by @konstin on `crates/uv/tests/it/export.rs`:3881 on 2026-01-13 08:13_

I was thinking about a specific scenario: there's conflicting extras a1 and a2, which depend on b[e1] and b[e2] respectively, so that we have an `"a1" in extras` that conditionally activates a `; extra == "e1"` dependency. This test should cover what the other new tests currently cover.

---

_@konstin reviewed on 2026-01-13 08:14_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:734 on 2026-01-13 08:14_

This should go in a different PR, it's a different change adding information that may change from version to version from adding fields that are always the same for a given resolution.

---

_@konstin reviewed on 2026-01-13 08:15_

---

_Review comment by @konstin on `crates/uv/tests/it/export.rs`:3881 on 2026-01-13 08:15_

We should also text that installation covers the correct subset.

---
