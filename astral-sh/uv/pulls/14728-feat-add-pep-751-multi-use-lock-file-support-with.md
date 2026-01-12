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
updated_at: 2026-01-12T19:59:54Z
url: https://github.com/astral-sh/uv/pull/14728
synced_at: 2026-01-12T20:26:40Z
```

# feat: Add PEP 751 multi-use lock file support with extras and dependency groups markers

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

#### 1. Root-Level PEP 751 Metadata Fields

The `pylock.toml` output now includes the required PEP 751 metadata fields:

- `extras`: All optional dependencies available in the project
- `dependency-groups`: All dependency groups defined in the project
- `default-groups`: Synthetic groups for default installation (currently empty per spec)

#### 2. Dependency Markers for Extras and Groups

Package dependencies include PEP 751 markers using the container membership syntax:

- `'test' in extras` - indicates this dependency is only needed with the `test` extra
- `'dev' in dependency_groups` - indicates this dependency is only needed with the `dev` group

Dependency markers are simplified relative to `requires-python`. Unconditional dependencies have no marker; conditional ones only include the additional constraints.

#### 3. Marker Conversion

uv's internal lock format uses "universal markers" that encode extras and groups in a special format. PEP 751 specifies a different syntax:

| Internal Format                | PEP 751 Format               |
| ------------------------------ | ---------------------------- |
| `extra == 'extra-3-pkg-cpu'`   | `'cpu' in extras`            |
| `extra == 'group-5-myapp-dev'` | `'dev' in dependency_groups` |

### Example Output

```toml
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.12"
extras = ["docs", "test"]
dependency-groups = ["dev"]

[[packages]]
name = "click"
version = "8.1.7"
dependencies = [{ name = "colorama", version = "0.4.6", marker = "sys_platform == 'win32'" }]
index = "https://pypi.org/simple"
...

[[packages]]
name = "flask"
version = "3.0.2"
dependencies = [
    { name = "blinker", version = "1.7.0" },
    { name = "click", version = "8.1.7" },
    { name = "itsdangerous", version = "2.1.2" },
    { name = "jinja2", version = "3.1.3" },
    { name = "python-dotenv", version = "1.0.1", marker = "'dotenv' in extras" },
    { name = "werkzeug", version = "3.0.1" },
]
index = "https://pypi.org/simple"
...

[[packages]]
name = "project"
dependencies = [
    { name = "flask", version = "3.0.2" },
    { name = "pytest", version = "8.0.0", marker = "'test' in extras" },
    { name = "sphinx", version = "7.0.0", marker = "'docs' in extras" },
    { name = "mypy", version = "1.0.0", marker = "'dev' in dependency_groups" },
]
directory = { path = ".", editable = true }
```

Note:
- Unconditional dependencies (blinker, click, etc.) have no marker
- Platform-specific dependencies include only that condition (`sys_platform == 'win32'`)
- Extra dependencies include the extras marker (`'dotenv' in extras`)
- Dependency group dependencies include the groups marker (`'dev' in dependency_groups`)

### Usage

```bash
# Export a project to pylock.toml format
uv export --format pylock.toml > pylock.toml

# Install from the multi-use lock file (base dependencies only)
uv pip install -r pylock.toml

# Install with specific extras
uv pip install -r pylock.toml --extra test --extra docs

# Install with specific dependency groups
uv pip install -r pylock.toml --group dev
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
