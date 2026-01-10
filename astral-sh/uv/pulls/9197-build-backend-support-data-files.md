```yaml
number: 9197
title: "Build backend: Support data files "
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/build-backend-wheel-data
created_at: 2024-11-18T13:50:52Z
updated_at: 2025-06-06T09:40:55Z
url: https://github.com/astral-sh/uv/pull/9197
synced_at: 2026-01-10T11:10:34Z
```

# Build backend: Support data files 

---

_Pull request opened by @konstin on 2024-11-18 13:50_

Allow including data files in wheels, configured through `pyproject.toml`. This configuration is currently only read in the build backend. We'd only start using it in the frontend when we're adding a fast path.

Each data entry is a directory, whose contents are copied to the matching directory in the wheel in `<name>-<version>.data/(purelib|platlib|headers|scripts|data)`. Upon installation, this data is moved to its target location, as defined by <https://docs.python.org/3.12/library/sysconfig.html#installation-paths>:
- `data`: Installed over the virtualenv environment root. Warning: This may override existing files!
- `scripts`: Installed to the directory for executables, `<venv>/bin` on Unix or `<venv>\Scripts` on Windows. This directory is added to PATH when the virtual environment is activated or when using `uv run`, so this data type can be used to install additional binaries. Consider using `project.scripts` instead for starting Python code.
- `headers`: Installed to the include directory, where compilers building Python packages with this package as built requirement will search for header files.
- `purelib` and `platlib`: Installed to the `site-packages` directory. It is not recommended to uses these two options.

For simplicity, for now we're just defining a directory to be copied for each data directory, while using the glob based include mechanism in the background. We thereby introduce a third mechanism next to the main includes and the PEP 639 mechanism, which is not what we should finalize on.

---

_Label `preview` added by @konstin on 2024-11-18 13:50_

---

_Review requested from @BurntSushi by @konstin on 2024-11-18 13:50_

---

_Comment by @cthoyt on 2024-11-18 17:30_

@konstin thanks for all of the great work on this. If you ever need some external testers, please ping me!

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/lib.rs`:347 on 2024-11-18 18:12_

Might be more ceremony, but if you need to match on them, I might use an enum for the `&'static str` here.

(But if it's really just used as a label in string form, I think what you have is totally fine.)

---

_@BurntSushi approved on 2024-11-18 18:13_

Makes sense to me!

---

_@konstin reviewed on 2024-11-19 10:59_

---

_Review comment by @konstin on `crates/uv-build-backend/src/lib.rs`:347 on 2024-11-19 10:59_

Here, I'm just mapping the key name to the folder name.

---

_Merged by @konstin on 2024-11-19 11:59_

---

_Closed by @konstin on 2024-11-19 11:59_

---

_Branch deleted on 2024-11-19 12:00_

---

_Comment by @ehrenfeu on 2025-06-06 09:38_

(Sorry for hijacking this PR thread, please let me know if you prefer me to open a separate issue...)

Quick question: I'd love to use this feature, but from my brief tests using version `0.7.11`, this is not yet possible via `uv build`, correct?

I'm having an entry like this in my `pyproject.toml`:

```toml
[tool.uv.wheel.data]
data = "conffiles"
```

When calling `uv build` it tells me (output truncated):

```text
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 22, column 10
     |
  22 | [tool.uv.wheel.data]
     |          ^^^^^
  unknown field `wheel`
```

💡  Which is probably what is stated in the 3rd phrase of this PR issue (at least that's my interpretation):

> We'd only start using it in the frontend when we're adding a fast path.


❓ So, is it currently possible somehow to include extra stuff into wheels built by `uv`, in a similar fashion as through [setuptool's Data Files][1] fashion? I've spent quite some time reading docs and testing stuff in my project config, without success though...

Many thanks! 💚 
~Niko


[1]: https://setuptools.pypa.io/en/latest/userguide/datafiles.html

---

_Comment by @ehrenfeu on 2025-06-06 09:40_

Never mind. Found it _right after_ submitting this message:

https://docs.astral.sh/uv/reference/settings/#build-backend_data

These entries do the job for me:

```toml
[tool.uv]
package = true

[tool.uv.build-backend]
data = { "data" = "extra_files" }

[build-system]
requires = ["uv_build>=0.7.11,<0.8.0"]
build-backend = "uv_build"
```

Thanks again! 💚
~Niko

---
