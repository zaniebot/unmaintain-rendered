```yaml
number: 4337
title: Fix false positives in PD002
type: pull_request
state: merged
author: evanrittenhouse
labels: []
assignees: []
merged: true
base: main
head: 4222_pandas_falsepos
created_at: 2023-05-10T02:28:21Z
updated_at: 2023-05-10T14:09:14Z
url: https://github.com/astral-sh/ruff/pull/4337
synced_at: 2026-01-12T03:56:39Z
```

# Fix false positives in PD002

---

_Pull request opened by @evanrittenhouse on 2023-05-10 02:28_

Should fix #4222.

---

_Comment by @github-actions[bot] on 2023-05-10 02:42_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+0, -3, 0 error(s))

<details><summary>bokeh (+0, -3)</summary>
<p>

```diff
- examples/topics/graph/airports_graph.py:10:33: PD002 [*] `inplace=True` should be avoided; it has inconsistent behavior
- examples/topics/graph/airports_graph.py:11:32: PD002 `inplace=True` should be avoided; it has inconsistent behavior
- examples/topics/graph/airports_graph.py:12:70: PD002 [*] `inplace=True` should be avoided; it has inconsistent behavior
```

</p>
</details>
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-05-10 03:08_

---

_Renamed from "Fix false positives in PDO002" to "Fix false positives in PD002" by @charliermarsh on 2023-05-10 03:28_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:76 on 2023-05-10 08:36_

Nit
```suggestion
        is_pandas = checker.ctx.find_binding(module).map_or(false, |binding| {
            matches!(
                binding.kind,
                BindingKind::Importation(Importation {
                    full_name: "pandas",
                    ..
                })
            )
        });
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:126 on 2023-05-10 08:37_

Nit
```suggestion
                if is_checkable && !is_pandas {
                    return None;
                }
```

---

_@MichaReiser approved on 2023-05-10 08:37_

---

_@evanrittenhouse reviewed on 2023-05-10 13:44_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/pandas_vet/rules/inplace_argument.rs`:76 on 2023-05-10 13:44_

Done - thanks!

---

_Merged by @MichaReiser on 2023-05-10 14:04_

---

_Closed by @MichaReiser on 2023-05-10 14:04_

---

_Comment by @MichaReiser on 2023-05-10 14:04_

Thanks for another awesome contribution :)

---

_Branch deleted on 2023-05-10 14:09_

---
