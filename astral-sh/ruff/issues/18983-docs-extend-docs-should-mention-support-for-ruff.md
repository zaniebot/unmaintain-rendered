---
number: 18983
title: "docs: `extend` docs should mention support for `ruff.toml` format"
type: issue
state: open
author: silverwind
labels:
  - documentation
assignees: []
created_at: 2025-06-27T12:39:50Z
updated_at: 2025-08-11T06:46:28Z
url: https://github.com/astral-sh/ruff/issues/18983
synced_at: 2026-01-10T01:23:00Z
---

# docs: `extend` docs should mention support for `ruff.toml` format

---

_Issue opened by @silverwind on 2025-06-27 12:39_

The [`extend` option documentation](https://docs.astral.sh/ruff/settings/#extend) says:

> A path to a local `pyproject.toml` file to merge into this configuration.

Actually, the option also supports files in `ruff.toml` format, so the docs need to be updated to mention this.

---

_Referenced in [astral-sh/ruff#12352](../../astral-sh/ruff/issues/12352.md) on 2025-06-27 12:40_

---

_Renamed from "docs: `extends` documentation should mention support for `ruff.toml` format" to "docs: `extend` docs should mention support for `ruff.toml` format" by @silverwind on 2025-06-27 12:41_

---

_Label `documentation` added by @ntBre on 2025-06-27 13:00_

---

_Comment by @ntBre on 2025-06-27 13:03_

Thanks for opening this! Are you interested in opening a PR updating the docs? lf not, I can add the `help-wanted` tag :)

---

_Comment by @silverwind on 2025-06-27 13:12_

Yes I can open a doc PR. Would like to first verify in the code whether this dual-format loading is really working as it seems to do.

---

_Comment by @ntBre on 2025-06-27 13:14_

Sounds good, I'll assign you to mark it in progress, but no pressure.

And good idea to check that it's working as expected. I'm hoping there's an existing test demonstrating this, but if not we could also consider adding one.

---

_Assigned to @silverwind by @ntBre on 2025-06-27 13:14_

---

_Comment by @pakal on 2025-07-09 15:25_

Hi, I've seen in the tests of the repository lots of mentions of **extending** "ruff.toml" and its derivatives, but only one mention of pyproject.toml and it was in an example structure (crates/ruff_linter/resources/test/project/examples/docs/ruff.toml). 

So maybe it's the pyproject.toml extending case which lacks testing, instead  ^^

---

_Comment by @swanysimon on 2025-08-10 19:27_

Relevant to the docs because of my question here: https://github.com/astral-sh/ruff/issues/12352#issuecomment-3172841089

> User home directory and environment variables will be expanded.

Are there any environment variables that would allow me to reliably point at a dependency's installation path in the virtual environment? I'm using `uv` for package management (yes, I know it's a different project), current I only see `VIRTUAL_ENV=<repo>/.venv` which isn't any more useful than pointing to a string

---

_Comment by @MichaReiser on 2025-08-11 06:46_

@swanysimon not that I'm aware of. The [documentation](https://docs.astral.sh/uv/reference/environment) lists all known and supported environment variables. 

---
