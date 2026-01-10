```yaml
number: 13053
title: "[red-knot] `SemanticIndexBuilder` visits value before target in named expressions"
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: red-knot-named-expr
created_at: 2024-08-22T11:45:47Z
updated_at: 2024-08-22T15:13:51Z
url: https://github.com/astral-sh/ruff/pull/13053
synced_at: 2026-01-10T21:38:32Z
```

# [red-knot] `SemanticIndexBuilder` visits value before target in named expressions

---

_Pull request opened by @dylwil3 on 2024-08-22 11:45_

The `SemanticIndexBuilder` was causing a cycle in a salsa query by attempting to resolve the target before the value in a named expression (e.g. `x := x+1`). This PR swaps the order, avoiding a panic.

Closes #13012.


---

_Review requested from @carljm by @dylwil3 on 2024-08-22 11:45_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-08-22 11:45_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-08-22 11:45_

---

_Comment by @codspeed-hq[bot] on 2024-08-22 11:51_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:red-knot-named-expr)

### Merging #13053 will **not alter performance**

<sub>Comparing <code>dylwil3:red-knot-named-expr</code> (c7bd4ba) with <code>main</code> (02c4373)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Label `red-knot` added by @MichaReiser on 2024-08-22 12:05_

---

_Comment by @github-actions[bot] on 2024-08-22 12:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2024-08-22 13:15_

Thanks. Makes sense

---

_@MichaReiser reviewed on 2024-08-22 13:16_

---

_Review comment by @MichaReiser on `crates/red_knot_workspace/resources/test/corpus/04_assign_named_expr.py`:1 on 2024-08-22 13:16_

Up to now, the corpus tests are inherited tests from other projects. I don't see a strong reason why we shouldn't use the same infrastructure to add our own smoke tests but interested to hear what others think

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:663 on 2024-08-22 13:23_

I'm not sure if it matters, but elsewhere in this file `self.current_assignment` is always `None` unless we're visiting the _target_ of the assignment. So that implies that we should do this:

```suggestion
                self.visit_expr(&node.value);
                self.current_assignment = Some(node.into());
                // TODO walrus in comprehensions is implicitly nonlocal
```

---

_@AlexWaygood reviewed on 2024-08-22 13:23_

---

_@AlexWaygood reviewed on 2024-08-22 13:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/04_assign_named_expr.py`:1 on 2024-08-22 13:23_

I'm fine with using the same infrastructure personally, I think it makes sense!

---

_@dhruvmanila reviewed on 2024-08-22 13:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:663 on 2024-08-22 13:28_

Yes to what Alex has suggested, because the `current_assignment` is used to add the definition for the target expression.

This makes me wonder why there's no failure because for something like `x := x + 1` there'll be two definitions. Maybe we might want to add a semantic index test case for named expression in https://github.com/astral-sh/ruff/blob/d37e2e5d33deadc15bf4194216e08ce1528a638c/crates/red_knot_python_semantic/src/semantic_index.rs#L309-L310.

---

_Comment by @AlexWaygood on 2024-08-22 13:29_

@dylwil3 -- just want to say how much I appreciate your contributions recently, they're very high-quality!

---

_@dylwil3 reviewed on 2024-08-22 13:33_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:663 on 2024-08-22 13:33_

Done! (re reordering - still have to add a unit test)

---

_Comment by @dylwil3 on 2024-08-22 13:35_

> @dylwil3 -- just want to say how much I appreciate your contributions recently, they're very high-quality!

Thank you! Truly a joy to contribute to this repo: well-written code, friendly maintainers, and _incredibly fast_ (and helpful!) feedback for issues and PRs!

---

_@dylwil3 reviewed on 2024-08-22 14:09_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:663 on 2024-08-22 14:09_

Thanks for the suggestion @dhruvmanila ! The plot thickens... this test fails when trying to unwrap the `first_definition`:

```rust
    #[test]
    fn walrus_assignment() {
        let TestCase { db, file } = test_case("x := x + 1");
        let scope = global_scope(&db, file);
        let global_table = symbol_table(&db, scope);

        assert_eq!(names(&global_table), vec!["x"]);

        let use_def = use_def_map(&db, scope);
        let definition = use_def
            .first_public_definition(global_table.symbol_id_by_name("x").unwrap())
            .unwrap();

        assert!(matches!(
            definition.node(&db),
            DefinitionKind::NamedExpression(_)
        ));
    }
```

Some relevant debug info:

```bash
[crates/red_knot_python_semantic/src/semantic_index.rs:494:9] &global_table = SymbolTable {
    symbols: [
        Symbol {
            name: Name("x"),
            flags: SymbolFlags(
                IS_USED,
            ),
        },
    ],
    symbols_by_name: {
        ScopedSymbolId(
            0,
        ): (),
    },
}
[crates/red_knot_python_semantic/src/semantic_index.rs:499:9] &use_def = UseDefMap {
    all_definitions: [],
    all_constraints: [],
    definitions_by_use: [
        SymbolState {
            visible_definitions: Inline(
                [
                    0,
                    0,
                    0,
                ],
            ),
            constraints: [],
            may_be_unbound: true,
        },
        SymbolState {
            visible_definitions: Inline(
                [
                    0,
                    0,
                    0,
                ],
            ),
            constraints: [],
            may_be_unbound: true,
        },
    ],
    public_definitions: [
        SymbolState {
            visible_definitions: Inline(
                [
                    0,
                    0,
                    0,
                ],
            ),
            constraints: [],
            may_be_unbound: true,
        },
    ],
}
[crates/red_knot_python_semantic/src/semantic_index.rs:500:9] &global_table.symbol_id_by_name("x").unwrap() = ScopedSymbolId(
    0,
)
thread 'semantic_index::tests::walrus_assignment' panicked at crates/red_knot_python_semantic/src/semantic_index.rs:503:14:
called `Option::unwrap()` on a `None` value
```

Will try to look into this a little later. (Not sure if it merits a follow-up PR or if it belongs in this one).

---

_Review comment by @carljm on `crates/red_knot_workspace/resources/test/corpus/04_assign_named_expr.py`:1 on 2024-08-22 14:51_

Yeah when I added the corpus I actually already added some missing tests for newer Python syntax, so even before this PR it's already been augmented in ruff.

---

_@carljm reviewed on 2024-08-22 14:51_

---

_@carljm reviewed on 2024-08-22 14:58_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:663 on 2024-08-22 14:58_

The issue here is that `x := x + 1` is actually a syntax error, while wrapping it in parens makes it syntactically valid. But our parser is error-recovering, and currently red-knot doesn't issue any diagnostics for invalid syntax, so what ends up happening here is that you just silently don't get any definition of x, because there isn't even an `ExprNamed` in the AST.

This isn't the first time invalid-syntax has caused such confusing symptoms; I think diagnostics on invalid syntax should be a priority for us. I created #13058 to track this.

---

_@carljm approved on 2024-08-22 14:59_

Looks good to me!

---

_Merged by @carljm on 2024-08-22 14:59_

---

_Closed by @carljm on 2024-08-22 14:59_

---

_Branch deleted on 2024-08-22 15:13_

---
