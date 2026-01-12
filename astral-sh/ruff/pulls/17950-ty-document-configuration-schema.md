```yaml
number: 17950
title: "[ty] Document configuration schema"
type: pull_request
state: merged
author: MichaReiser
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: micha/ty-options
created_at: 2025-05-08T13:08:35Z
updated_at: 2025-05-09T08:47:47Z
url: https://github.com/astral-sh/ruff/pull/17950
synced_at: 2026-01-12T15:56:08Z
```

# [ty] Document configuration schema

---

_@MichaReiser_

## Summary

The ultimate goal of this PR is to have a generate documentation of ty's configuration schema. To get there, I had to

* Extract `options_base` from `ruff_workspace` into the shared `ruff_options_metadata` crate
* Derive `OptionsMetadata` for ty's `Options` struct and attribute all fields with `#[option]` or `#[option_group]`
* Copy paste ruff's dev-script that generates the options. Rename `tool.ruff` to `tool.ty`


Ideally, we'd include the schema in the `README.md`... but the readme isn't in this repository but in the ty repository. 
For now, it should be good enough if we link to the `configuration.md` from ty's `README.md`


## Test Plan

See commited markdown file


---

_Label `documentation` added by @MichaReiser on 2025-05-08 13:11_

---

_Label `ty` added by @MichaReiser on 2025-05-08 13:11_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_options.rs`:1 on 2025-05-08 13:11_

This is 99% copy paste. It's not great but it works

---

_@MichaReiser reviewed on 2025-05-08 13:11_

---

_Comment by @github-actions[bot] on 2025-05-08 13:25_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @MichaReiser on 2025-05-08 13:34_

---

_Review requested from @carljm by @MichaReiser on 2025-05-08 13:34_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-08 13:34_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-08 13:34_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-08 13:34_

---

_Comment by @github-actions[bot] on 2025-05-08 13:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Renamed from "[ty] Configuration schema documentation" to "[ty] Document configuration schema" by @MichaReiser on 2025-05-08 14:56_

---

_@MichaReiser reviewed on 2025-05-08 14:56_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_options.rs`:281 on 2025-05-08 14:56_

Too much copy paste, remove the env var from here

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_options.rs`:155 on 2025-05-08 14:57_

remove anchor

---

_@MichaReiser reviewed on 2025-05-08 14:57_

---

_@MichaReiser reviewed on 2025-05-08 14:57_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_ty_options.rs`:154 on 2025-05-08 14:57_

remove anchor

---

_Review comment by @carljm on `crates/ruff_dev/src/generate_ty_options.rs`:3 on 2025-05-08 18:46_

Update/remove this?

---

_Review comment by @carljm on `crates/ruff_dev/src/generate_ty_options.rs`:1 on 2025-05-08 18:46_

I didn't review this (except for the module doc comment :laughing:)

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:272 on 2025-05-08 19:00_

Not changed in this PR, but our understanding of `sys.version_info` branches is not limited to type stub files, it works just as well in code files. Suggested update:
```suggestion
    /// It will also understand conditionals based on comparisons with `sys.version_info`, such
    /// as are commonly found in typeshed to reflect the differing contents of the standard
    /// library across Python versions.
```
Same thing would apply to the similar comment below on platform (which I can't inline-comment on, since it's out of the modified range of this PR.)

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:277 on 2025-05-08 19:01_

Should we add a comment at the spot where we define the `latest_ty` type to remind us to change this as well?

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:45 on 2025-05-08 19:09_

Is this following any formalized notation, or is it just meant to be some human-readable intuitive representation? The `dict[...]` and `str` parts seem like Python type notation, but in that case it should be `Literal["ignore", "warn", "error"]`, not `ignore | warn | error`.

Whatever the intended format, it's slightly odd to me that here we use an unquoted bare name both to mean "a type" (`str`) and "a string literal" (`ignore`, `warn`, `error`).

I see in some other cases below you use quoted strings with vertical bars, so maybe this is what you meant here (still not Python typing syntax quite, but a more readable variant of it):
```suggestion
        value_type = r#"dict[str, "ignore" | "warn" | "error"]"#,
```

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:337 on 2025-05-08 19:14_

As of today, it _has_ to be a virtual environment, or else we error because we can't find `pyvenv.cfg`. This is a limitation we should fix, so maybe the text here can reflect the way it should work, not the current limitations.

---

_Review comment by @carljm on `crates/ty_project/src/metadata/options.rs`:353 on 2025-05-08 19:15_

```suggestion
    /// The root(s) of the project, used for finding first-party modules.
```

---

_@carljm approved on 2025-05-08 19:16_

Fantastic, thank you!

---

_@MichaReiser reviewed on 2025-05-09 07:31_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:45 on 2025-05-09 07:31_

It's a Python-typing inspired notation but it's mostly made up (e.g. we don't use `Literal["abcd"]` because that's just too verbose). It's what we use in ruff

Let me double check what notation we use for enum values in ruff

---

_@MichaReiser reviewed on 2025-05-09 07:41_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:337 on 2025-05-09 07:41_

Makes sense. I don't think I'm the right person to write this because my understanding of the entire venv/system python is very limited. @AlexWaygood could you help me out here and pep up the description (in a separate PR?)

---

_@AlexWaygood reviewed on 2025-05-09 07:44_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:337 on 2025-05-09 07:44_

Absolutely 

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:337 on 2025-05-09 08:47_

Thank you!

---

_@MichaReiser reviewed on 2025-05-09 08:47_

---

_Merged by @MichaReiser on 2025-05-09 08:47_

---

_Closed by @MichaReiser on 2025-05-09 08:47_

---

_Branch deleted on 2025-05-09 08:47_

---
