```yaml
number: 12764
title: "Add b040: exception with note added not re-raised or used (#12097)"
type: pull_request
state: closed
author: divident
labels:
  - rule
  - preview
assignees: []
base: main
head: add-b040-rule
created_at: 2024-08-08T21:30:09Z
updated_at: 2024-10-05T11:51:11Z
url: https://github.com/astral-sh/ruff/pull/12764
synced_at: 2026-01-10T20:59:36Z
```

# Add b040: exception with note added not re-raised or used (#12097)

---

_Pull request opened by @divident on 2024-08-08 21:30_

## Summary
This PR ports the B040 implementation from https://github.com/PyCQA/flake8-bugbear/pull/477.

It checks whenever the exception is used, except for the `add_note' method.

```
def foo():
  my_exc = ValueError() 
  my_exc.add_note("I want to use this variable")
  # ... but then completely forgets to raise, should trigger B040
```


---

_Comment by @github-actions[bot] on 2024-08-08 21:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @dhruvmanila on 2024-08-09 05:32_

---

_Label `preview` added by @dhruvmanila on 2024-08-09 05:32_

---

_@MichaReiser reviewed on 2024-08-15 17:00_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-15 17:00_

The rule name is very close to `SIM105` [`suppressible-exception`](https://docs.astral.sh/ruff/rules/suppressible-exception/). 

Which makes me wonder if the rule should be integrated into `PLW0133`? @AlexWaygood what do you think?

---

_@divident reviewed on 2024-08-16 14:45_

---

_Review comment by @divident on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-16 14:45_

I agree the name is similar, but this check is about checking if there is only `add_note` executed on the exception, so it's quite different from SIM105. On the other hand it is similar to PLW0133, but this rule exists in the flake8 bugbear, so it might be worth keeping the same set of rules with upstream..

---

_@AlexWaygood reviewed on 2024-08-16 15:26_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-16 15:26_

Integrating it into `PLW0133` is appealing because it's a more general rule, and I agree that this is just trying to tackle a variant of that problem.

But flake8-bugbear is more popular and widely used than pylint, I think, and I believe people much more often select our `B` rules than our `PLW` rules. So I agree that consistency with flake8-bugbear is useful here.

I think that when doing rule recategorisation (a medium-term goal that you don't have to worry about here @divident), we'll probably no longer have categories that refer to older linters, and at that point it would probably make sense to merge `PLW0133` and this rule. But for now I guess I'd go for a separate check, despite how similar the two rules are.

---

_@MichaReiser reviewed on 2024-08-17 11:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-17 11:57_

It feels strange to me to introduce a new rule if we know we'll want to merge it eventually anyway. But I do see the point and redirects are relatively cheap for users.

---

_@MichaReiser reviewed on 2024-08-17 11:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-17 11:57_

But I think we have to come up a more specific name to also make it clear that this is a very specific rule and it doesn't aim to catch all suppressed exceptions.

---

_@divident reviewed on 2024-08-18 16:39_

---

_Review comment by @divident on `crates/ruff_linter/src/checkers/ast/analyze/statement.rs`:1477 on 2024-08-18 16:39_

What about the name "add_note_exception_suppression" ? 

A few more suggestions:
1. add_note_only_exception
2. note_only_exception_suppression
3. exclusive_add_note_exception
4. exception_note_only
5. suppressed_with_add_note

---

_@MichaReiser reviewed on 2024-08-19 09:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/suppressed_exception.rs`:103 on 2024-08-19 09:05_

I wonder if we can simplify this code by making more use of `Binding`:

* Resolve the `bindings` of `name`
* Iterate over all references of `binding` and 
  * exit if any is statement (use `SemanticModel::statement(binding.expression_id())`) is a a raise statement and the target or cause is the binding node
  * count the `add_note` calls
* Create a diagnostic if there's at least one `add_note` call. 

---

_@divident reviewed on 2024-08-19 18:25_

---

_Review comment by @divident on `crates/ruff_linter/src/rules/flake8_bugbear/rules/suppressed_exception.rs`:103 on 2024-08-19 18:25_

According to the upstream implementation, the following code is safe, which requires checking all `raise` statements in scope, not just those with binding 

```
try:
    ...
except Exception as e:
    e.add_note("...")
    raise  # safe
```
On the last point, just checking for the `add_note` call is not enough, because exceptions can be used in the print, function calls etc that are considered to be safe, so I'm counting all references to them. Not sure if there is a better way to find all usages of the exception that are referenced.
   

---

_@MichaReiser reviewed on 2024-08-20 07:19_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/suppressed_exception.rs`:103 on 2024-08-20 07:19_

I'm not sure I follow. Doesn't 

```python
try: ... 
except Exception as e: 
    e.add_note("...") raise # safe
````

introduce a binding for `e`?


---

_@divident reviewed on 2024-08-20 14:00_

---

_Review comment by @divident on `crates/ruff_linter/src/rules/flake8_bugbear/rules/suppressed_exception.rs`:103 on 2024-08-20 14:00_

It does, but my point is that `raise` is not included in the references list when I filter by exception name.
```
            let bindings: Vec<&Binding> = semantic
                .current_scope()
                .get_all(exception_name.id.as_str())
                .map(|binding_id| semantic.binding(binding_id))
                .filter(|binding| matches!(binding.kind, BindingKind::UnboundException(_)))
                .collect();


            for binding in bindings.iter() {
                for reference_id in binding.references() {
                    let reference = checker.semantic().reference(reference_id);
                    if let Some(node_id) = reference.expression_id() {
                        let stmt = checker.semantic().statement(node_id);
                        
   
                        println!("stmt={:?}", stmt);
                       ...
```

prints just a `add_note` call
```
stmt=Expr(StmtExpr { range: 76..93, value: Call(ExprCall { range: 76..93, func: Attribute(ExprAttribute { range: 76..86, value: Name(ExprName { range: 76..77, id: Name("e"), ctx: Load }), attr: Identifier { id: Name("add_note"), range: 78..86 }, ctx: Load }), arguments: Arguments { range: 86..93, args: [StringLiteral(ExprStringLiteral { range: 87..92, value: StringLiteralValue { inner: Single
```

---

_Closed by @divident on 2024-10-05 11:51_

---
