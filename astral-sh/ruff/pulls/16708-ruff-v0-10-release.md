```yaml
number: 16708
title: Ruff v0.10 Release
type: pull_request
state: merged
author: MichaReiser
labels:
  - release
assignees: []
merged: true
base: main
head: micha/ruff-0.10-changelog
created_at: 2025-03-13T13:40:15Z
updated_at: 2025-03-13T17:53:13Z
url: https://github.com/astral-sh/ruff/pull/16708
synced_at: 2026-01-10T19:49:02Z
```

# Ruff v0.10 Release

---

_Pull request opened by @MichaReiser on 2025-03-13 13:40_

_No description provided._

---

_Comment by @github-actions[bot] on 2025-03-13 13:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Ruff v0.10 Relase" to "Ruff v0.10 Release" by @MichaReiser on 2025-03-13 14:37_

---

_Label `release` added by @MichaReiser on 2025-03-13 14:43_

---

_Comment by @dylwil3 on 2025-03-13 15:35_

Ok @MichaReiser I think the changelog is complete but someone should spot check. From what I can tell, all the "Rule changes" were stabilizations of preview behavior so I removed that subsection.

---

_Comment by @MichaReiser on 2025-03-13 15:55_

I'll do a quick rebase to make reviewing easier

---

_@MichaReiser reviewed on 2025-03-13 16:00_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:13 on 2025-03-13 16:00_

Should we just copy over the text from the release blog post?

In previous versions of Ruff, you could specify your Python version with:
* The
  [`target-version`](https://docs.astral.sh/ruff/settings/#target-version)
  option in a `ruff.toml` file or the `[tool.ruff]` section of a `pyproject.toml`
  file.
* The `project.requires-python` field in a `pyproject.toml` file with a `[tool.ruff]` section.

These options worked well in most cases, but because of the way Ruff [discovers
config files](https://docs.astral.sh/ruff/configuration/#config-file-discovery),
`pyproject.toml` files without a `[tool.ruff]` section would be ignored,
including the `requires-python` setting. Ruff would then use the default Python
version (3.9 as of this writing) instead, which is surprising when you've attempted
to request another version.

In v0.10, config discovery has updated to address this issue:

1. If Ruff finds a `ruff.toml` file without a `target-version`, it will check
   for a `pyproject.toml` file in the same directory and respect its
   `requires-python` version, even if it does not contain a `[tool.ruff]`
   section.
2. If there is *no* config file (`ruff.toml` or `pyproject.toml` with a
   `[tool.ruff]` section) in the directory of the file being checked, Ruff will
   search for the closest `pyproject.toml` in the parent directories and use its
   `requires-python` setting.[^2]

---

_Review comment by @MichaReiser on `CHANGELOG.md`:17 on 2025-03-13 16:03_

```suggestion
    Previously, Ruff only recognized typechecking blocks that tested the `typing.TYPE_CHECKING` symbol. Now, Ruff recognizes any local variable named `TYPE_CHECKING`. This release also removes support for the legacy `if 0:` and `if False:` typechecking checks. Use a local `TYPE_CHECKING` variable instead.
```

---

_@MichaReiser reviewed on 2025-03-13 16:03_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:72 on 2025-03-13 16:04_

```suggestion
- [`blanket-noqa`](https://docs.astral.sh/ruff/rules/blanket-noqa/) (`PGH004`): Also detect blanked file-level noqa comments (and not just line level comments).
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:71 on 2025-03-13 16:04_

Nit?
```suggestion
- [`bad-str-strip-call`](https://docs.astral.sh/ruff/rules/bad-str-strip-call/) (`PLE1310`): The rule now applies to objects which are known to have type `str` or `bytes`.
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:74 on 2025-03-13 16:05_

```suggestion
- [`invalid-argument-name`](https://docs.astral.sh/ruff/rules/invalid-argument-name/) (`N803`): Ignore argument names of functions decorated with `typing.override`
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:77 on 2025-03-13 16:06_

```suggestion
- [`redundant-open-modes`](https://docs.astral.sh/ruff/rules/redundant-open-modes/) (`UP015`): The diagnostic range is now the range of the redundant mode argument where it previously was the range of the entire open call. You may have to replace your `noqa` comments when suppressing `UP015`.
```

---

_Review comment by @MichaReiser on `CHANGELOG.md`:78 on 2025-03-13 16:07_

```suggestion
- [`stdlib-module-shadowing`](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/): Changes the default value of `lint.flake8-builtins.strict-checking` from `true` to `false`.
```

It may be worth linking to the setting if we don't explain what the behavior change is.

---

_Review comment by @MichaReiser on `CHANGELOG.md`:78 on 2025-03-13 16:09_

We should mention the rule here: (`A005`)

---

_@MichaReiser reviewed on 2025-03-13 16:10_

Thank you!

---

_Marked ready for review by @MichaReiser on 2025-03-13 16:10_

---

_Comment by @MichaReiser on 2025-03-13 16:11_

I can't approve my own PR. Consider it approved and feel free to merge it when we start the release

---

_@T-256 reviewed on 2025-03-13 16:30_

---

_Review comment by @T-256 on `CHANGELOG.md`:29 on 2025-03-13 16:30_

```suggestion
    Alpine 3.21 was released in Dec 2024 and is used in the official Alpine-based Python images. Now the ruff:alpine image will use 3.21 instead of 3.20 and ruff:alpine3.20 will no longer be updated.
```

---

_@dylwil3 reviewed on 2025-03-13 16:32_

---

_Review comment by @dylwil3 on `CHANGELOG.md`:29 on 2025-03-13 16:32_

good catch!

---

_@dylwil3 approved on 2025-03-13 16:49_

---

_@ntBre approved on 2025-03-13 17:19_

---

_Merged by @ntBre on 2025-03-13 17:53_

---

_Closed by @ntBre on 2025-03-13 17:53_

---

_Branch deleted on 2025-03-13 17:53_

---
