```yaml
number: 11619
title: Test failure for 0.4.6 on s390x
type: issue
state: closed
author: WhyNotHugo
labels:
  - bug
assignees: []
created_at: 2024-05-30T11:04:21Z
updated_at: 2024-06-05T08:47:09Z
url: https://github.com/astral-sh/ruff/issues/11619
synced_at: 2026-01-10T11:09:53Z
```

# Test failure for 0.4.6 on s390x

---

_Issue opened by @WhyNotHugo on 2024-05-30 11:04_

The test `rules::pyflakes::tests::preview_rules::rule_unusedimport_path_new_f401_28_all_multiple_init_py_expects` is failing on s390x:

```
---- rules::pyflakes::tests::preview_rules::rule_unusedimport_path_new_f401_28_all_multiple_init_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_28__all_multiple____init__.py.snap
Snapshot: preview__F401_F401_28__all_multiple/__init__.py
Source: crates/ruff_linter/src/rules/pyflakes/mod.rs:228
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    8     8 │ 5 5 | from . import unused, renamed as bees  # F401: add to __all__
    9     9 │ 6 6 | 
   10    10 │ 7 7 | 
   11    11 │ 8   |-__all__ = [];
   12       │-  8 |+__all__ = ["bees", "unused"];
         12 │+  8 |+__all__ = ["unused", "bees"];
   13    13 │ 
   14    14 │ __init__.py:5:34: F401 [*] `.renamed` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
   15    15 │   |
   16    16 │ 5 | from . import unused, renamed as bees  # F401: add to __all__
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   22    22 │ 5 5 | from . import unused, renamed as bees  # F401: add to __all__
   23    23 │ 6 6 | 
   24    24 │ 7 7 | 
   25    25 │ 8   |-__all__ = [];
   26       │-  8 |+__all__ = ["bees", "unused"];
         26 │+  8 |+__all__ = ["unused", "bees"];
────────────┴───────────────────────────────────────────────────────────────────
```

(full CI run log here: https://gitlab.alpinelinux.org/WhyNotHugo/aports/-/jobs/1403448)

I think that the order of the items here is non-deterministic somehow and varies depending on CPU architecture.

---

_Comment by @MichaReiser on 2024-05-30 11:11_

@plredmond do you want to take a look at this? It probably requires sorting the items or to use an `IndexSet`.

---

_Comment by @plredmond on 2024-05-30 16:17_

@MichaReiser The implementation creates multiple insertion edits that use the same insertion point. AFAIK, their order is deterministic. The implementation has no logic for resolving the order that ruff applies the insertions. I.e. It should be deterministic.

Otherwise I guess this ordering could change if there was a change to how multi-edit fixes get applied.

> ... varies depending on CPU architecture.

Yikes. I'll uh take a look.

---

_Comment by @MichaReiser on 2024-05-30 16:18_

@plredmond I don't think it's about the edit ordering that is non deterministic but it is the order of the items in `__all__` that is non deterministic (because it uses a hash set?)

---

_Comment by @plredmond on 2024-05-30 16:25_

@MichaReiser I'm not sure what you mean by "the items in `__all__`" because the rule implementation never creates or stores items in a data structure that represents `__all__` nor uses the AST's "export" code. You might be referring to the [list of unused import-bindings](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs#L223-L224), which yes, could be introduce a nondeterministic order. I'll try to apply your advice there and rule it out. Is there any way for me to test this within our (astral) ci/cd infra?

---

_Assigned to @plredmond by @plredmond on 2024-05-30 16:55_

---

_Closed by @plredmond on 2024-05-30 17:54_

---

_Reopened by @plredmond on 2024-05-30 17:55_

---

_Comment by @plredmond on 2024-05-30 17:56_

@WhyNotHugo Can you check whether this issue is resolved on dcabd04caf85d7c0081ebba9f9028b9af9d686d3 or in the next release?

---

_Comment by @WhyNotHugo on 2024-05-31 13:14_

I applied dcabd04caf85d7c0081ebba9f9028b9af9d686d3 onto v0.4.6 and that failed:

```
>>> ruff: dcabd04caf85d7c0081ebba9f9028b9af9d686d3.patch
patching file crates/ruff_linter/src/rules/pyflakes/rules/unused_import.rs
patching file crates/ruff_python_semantic/src/binding.rs

[...]

---- rules::pyflakes::tests::preview_rules::rule_unusedimport_path_new_f401_28_all_multiple_init_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff_linter/src/rules/pyflakes/snapshots/ruff_linter__rules__pyflakes__tests__preview__F401_F401_28__all_multiple____init__.py.snap
Snapshot: preview__F401_F401_28__all_multiple/__init__.py
Source: crates/ruff_linter/src/rules/pyflakes/mod.rs:228
────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────
    8     8 │ 5 5 | from . import unused, renamed as bees  # F401: add to __all__
    9     9 │ 6 6 | 
   10    10 │ 7 7 | 
   11    11 │ 8   |-__all__ = [];
   12       │-  8 |+__all__ = ["bees", "unused"];
         12 │+  8 |+__all__ = ["unused", "bees"];
   13    13 │ 
   14    14 │ __init__.py:5:34: F401 [*] `.renamed` imported but unused; consider removing, adding to `__all__`, or using a redundant alias
   15    15 │   |
   16    16 │ 5 | from . import unused, renamed as bees  # F401: add to __all__
┈┈┈┈┈┈┈┈┈┈┈┈┼┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈┈
   22    22 │ 5 5 | from . import unused, renamed as bees  # F401: add to __all__
   23    23 │ 6 6 | 
   24    24 │ 7 7 | 
   25    25 │ 8   |-__all__ = [];
   26       │-  8 |+__all__ = ["bees", "unused"];
         26 │+  8 |+__all__ = ["unused", "bees"];
────────────┴───────────────────────────────────────────────────────────────────
```

---

_Comment by @charliermarsh on 2024-05-31 15:10_

I think the issue is that `for binding_id in scope.binding_ids()` is not deterministic. So we should enforce determinism elsewhere (e.g., by sorting in `fix_by_reexporting` or similar).

---

_Label `bug` added by @charliermarsh on 2024-05-31 15:11_

---

_Comment by @plredmond on 2024-05-31 17:09_

~~Wouldn't the nondeterminism of `scope.binding_ids()` result order be resolved by insertion into the `BTreeMap`? The only place this data comes back out is when we iterate over the `BTreeMaps`.~~

[Edit: Nevermind, I see that the test that is failing is the case where we have multiple import-bindings from a single import-statement. The order they're pushed into the import binding depends on the order they come from `scope.binding_ids()`.]

I'll try sorting in `fix_by_reexporting`, anyhow.

---

_Comment by @plredmond on 2024-05-31 19:32_

@WhyNotHugo could you try out 15977ed2bae6c53b9b70520699d54d1ce995896c?

---

_Comment by @MichaReiser on 2024-06-04 11:50_

 I'll mark this as closed. @WhyNotHugo feel free to reopen if you keep running into this issue with 0.4.8. Thanks @plredmond for looking into and fixing the issue.

---

_Closed by @MichaReiser on 2024-06-04 11:50_

---

_Comment by @plredmond on 2024-06-04 15:34_

Thanks micha 

> On Jun 4, 2024, at 04:50, Micha Reiser ***@***.***> wrote:
> 
> ﻿
> I'll mark this as closed. @WhyNotHugo feel free to reopen if you keep running into this issue with 0.4.8. Thanks @plredmond for looking into and fixing the issue.
> 
> —
> Reply to this email directly, view it on GitHub, or unsubscribe.
> You are receiving this because you were mentioned.


---

_Comment by @WhyNotHugo on 2024-06-05 08:47_

0.4.7 builds okay on s390x. Thanks!

---
