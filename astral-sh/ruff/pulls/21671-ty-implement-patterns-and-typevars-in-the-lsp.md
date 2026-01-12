```yaml
number: 21671
title: "[ty] Implement patterns and typevars in the LSP"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/stmtz
created_at: 2025-11-27T22:44:08Z
updated_at: 2025-11-28T13:41:23Z
url: https://github.com/astral-sh/ruff/pull/21671
synced_at: 2026-01-12T15:57:30Z
```

# [ty] Implement patterns and typevars in the LSP

---

_@Gankra_

## Summary

**This is the final goto-targets with missing goto-definition/declaration implementations! 
You can now theoretically click on all the user-defined names in all the syntax. 🎉**

This adds:

* goto definition/declaration on patterns/typevars
* find-references/rename on patterns/typevars
* fixes syntax highlighting of `*rest` patterns

This notably *does not* add:

* goto-type for patterns/typevars 
* hover for patterns/typevars (because that's just goto-type for names)

Also I realized we were at the precipice of one of the great GotoTarget sins being resolved, and so I made import aliases also resolve to a ResolvedDefinition. This removes a ton of cruft and prevents further backsliding.

Note however that import aliases are, in general, completely jacked up when it comes to find-references/renames (both before and after this PR). Previously you could try to rename an import alias and it just wouldn't do anything. With this change we instead refuse to even let you try to rename it.

Sorting out why import aliases are jacked up is an ongoing thing I hope to handle in a followup.

## Test Plan

You'll surely not regret checking in 86 snapshot tests

---

_Renamed from "[ty] Implement goto-definition/declaration for all syntax" to "[ty] Implement patterns/typevars in the LSP" by @Gankra on 2025-11-27 22:45_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 22:46_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Renamed from "[ty] Implement patterns/typevars in the LSP" to "[ty] Implement patterns and typevars in the LSP" by @Gankra on 2025-11-27 22:47_

---

_Label `server` added by @Gankra on 2025-11-27 22:47_

---

_Label `ty` added by @Gankra on 2025-11-27 22:47_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 22:47_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 500 diagnostics
+ Found 498 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra reviewed on 2025-11-27 22:49_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1482 on 2025-11-27 22:49_

This one is actually useless I think (I added it while thrashing and trying to get imports rename/reference working... it did not work).

---

_Marked ready for review by @Gankra on 2025-11-28 00:57_

---

_Review requested from @carljm by @Gankra on 2025-11-28 00:57_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-28 00:57_

---

_Review requested from @sharkdp by @Gankra on 2025-11-28 00:57_

---

_Review requested from @dcreager by @Gankra on 2025-11-28 00:57_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-28 00:57_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto.rs`:380 on 2025-11-28 08:13_

Nit: You could reduce the boilerplate here, I think, by doing something like:

```
let definitions = match self {
    GotoTarget::Expression(expression) => {
        definitions_for_expression(model, *expression)
    },
    GotoTarget::FunctionDef(function) => {
        Some(vec![ResolvedDefinition::Definition(
            function.definition(model),
        )])
    },
    ...
};

definitions.map(Definitions)
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1068 on 2025-11-28 08:17_

Should we add a `record_name` method or at least rename the struct (or update its documentation) to explain that, yes, it's mostly expressions but sometimes it's names too

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1482 on 2025-11-28 08:17_

Should we remove it then? Seems easy enough to add once we know why we need it

---

_@MichaReiser approved on 2025-11-28 08:18_

Nice, this actually streamlines things :)

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1068 on 2025-11-28 13:37_

This is probably a good idea, I'll integrate it into some followup work I have planned that will involve more reckoning about ""expressions""

---

_@Gankra reviewed on 2025-11-28 13:37_

---

_Merged by @Gankra on 2025-11-28 13:41_

---

_Closed by @Gankra on 2025-11-28 13:41_

---

_Branch deleted on 2025-11-28 13:41_

---
