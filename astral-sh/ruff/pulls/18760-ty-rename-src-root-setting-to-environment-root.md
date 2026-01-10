```yaml
number: 18760
title: "[ty] Rename `src.root` setting to `environment.root`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: micha/rename-src-root-to-environment-root
created_at: 2025-06-18T16:17:30Z
updated_at: 2025-06-24T12:40:47Z
url: https://github.com/astral-sh/ruff/pull/18760
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Rename `src.root` setting to `environment.root`

---

_Pull request opened by @MichaReiser on 2025-06-18 16:17_

## Summary

Renames the `src.root` setting to `environment.root` because it doesn't configure what files to check, it configures the search paths. 

I'm open to better names. I think `environment.root` works but I don't love it. 

I also noticed that there's some overlap between `environment.root` and `src.include`. E.g. if you set `include = ["src"]`, then you probably also want to set `environment.root = "src"`. 
Although I think it's more common that people exclude certain directories rather than only including `src` or `test`.

I kept the old setting around for now and added a deprecation warning. If both `src.root` and `environment.root` are set, `environment.root` wins.

## Test Plan

Added tests


---

_Label `configuration` added by @MichaReiser on 2025-06-18 16:17_

---

_Label `ty` added by @MichaReiser on 2025-06-18 16:17_

---

_Review requested from @carljm by @MichaReiser on 2025-06-18 16:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-18 16:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-18 16:17_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-18 16:17_

---

_Comment by @github-actions[bot] on 2025-06-18 16:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
+ warning[deprecated-setting] The `src.root` setting is deprecated. Use `environment.root` instead.
- Found 905 diagnostics
+ Found 906 diagnostics

```
</details>


---

_Comment by @github-actions[bot] on 2025-06-18 16:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-06-19 21:01_

> ## `mypy_primer` results
> Changes were detected when running on open source projects
> 
> ```diff
> psycopg (https://github.com/psycopg/psycopg)
> + warning[deprecated-setting] The `src.root` setting is deprecated. Use `environment.root` instead.
> - Found 905 diagnostics
> + Found 906 diagnostics
> ```

looks like we'll need to update the mypy_primer config for psycopg (and maybe some others too? Haven't checked): https://github.com/hauntsaninja/mypy_primer/blob/7c847cc51ed164d98c0a201539d61c974d418643/mypy_primer/projects.py#L1215

---

_Comment by @AlexWaygood on 2025-06-19 21:02_

> I'm open to better names. I think `environment.root` works but I don't love it.

`environment.first_party_root` maybe? No strong opinion though :-)

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:284 on 2025-06-19 21:03_

```suggestion
                    "The `src.root` setting was ignored in favor of the `environment.root` setting",
```

---

_@AlexWaygood approved on 2025-06-19 21:06_

---

_@AlexWaygood reviewed on 2025-06-24 11:37_

---

_Review comment by @AlexWaygood on `crates/ty_project/src/metadata/options.rs`:274 on 2025-06-24 11:37_

```suggestion
                .and_then(|path| system_path_to_file(db.upcast(), path).ok())
```

---

_Review request for @sharkdp removed by @sharkdp on 2025-06-24 11:54_

---

_Merged by @MichaReiser on 2025-06-24 12:40_

---

_Closed by @MichaReiser on 2025-06-24 12:40_

---

_Branch deleted on 2025-06-24 12:40_

---
