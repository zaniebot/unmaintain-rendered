```yaml
number: 8159
title: "[pylint] Implement redefined-argument-from-local (R1704)"
type: pull_request
state: merged
author: danbi2990
labels: []
assignees: []
merged: true
base: main
head: redefined-argument-from-local
created_at: 2023-10-24T09:08:12Z
updated_at: 2023-11-10T19:13:07Z
url: https://github.com/astral-sh/ruff/pull/8159
synced_at: 2026-01-10T23:40:55Z
```

# [pylint] Implement redefined-argument-from-local (R1704)

---

_Pull request opened by @danbi2990 on 2023-10-24 09:08_

## Summary

It implements Pylint rule R1704: redefined-argument-from-local

Problematic code:
```python
def show(host_id=10.11):
    # +1: [redefined-argument-from-local]
    for host_id, host in [[12.13, "Venus"], [14.15, "Mars"]]:
        print(host_id, host)
```

Correct code:
```python
def show(host_id=10.11):
    for inner_host_id, host in [[12.13, "Venus"], [14.15, "Mars"]]:
        print(host_id, inner_host_id, host)
```

References:
[Pylint documentation](https://pylint.readthedocs.io/en/latest/user_guide/messages/refactor/redefined-argument-from-local.html)
[Related Issue](https://github.com/astral-sh/ruff/issues/970)

## Test Plan

`cargo test`


---

_Comment by @github-actions[bot] on 2023-10-24 09:36_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@charliermarsh reviewed on 2023-10-25 23:37_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/redefined_argument_from_local.rs`:132 on 2023-10-25 23:37_

I'm wondering if there's a slightly different way to structure this rule. Bear with me...

Instead of analyzing the AST, I think we can achieve the same effect by looking at _bindings_.

When we have something like:

```python
def f(x):
    for x in y:
        ...
```

We create a binding for the `x` in `def f(x)`, and then later, a binding for `x` in `for x in y`, and we track that this second binding shadows the first.

If you look in `deferred_scopes.rs`, you'll see we have some rules based on looking at these shadows:

```rust
if checker.enabled(Rule::ImportShadowedByLoopVar) {
    for (name, binding_id) in scope.bindings() {
        for shadow in checker.semantic.shadowed_bindings(scope_id, binding_id) {
            // If the shadowing binding isn't a loop variable, abort.
            let binding = &checker.semantic.bindings[shadow.binding_id()];
            if !binding.kind.is_loop_var() {
                continue;
            }

            // If the shadowed binding isn't an import, abort.
            let shadowed = &checker.semantic.bindings[shadow.shadowed_id()];
            if !matches!(
                shadowed.kind,
                BindingKind::Import(..)
                    | BindingKind::FromImport(..)
                    | BindingKind::SubmoduleImport(..)
                    | BindingKind::FutureImport
            ) {
                continue;
            }

            // If the bindings are in different forks, abort.
            if shadowed.source.map_or(true, |left| {
                binding.source.map_or(true, |right| {
                    checker.semantic.different_branches(left, right)
                })
            }) {
                continue;
            }

            #[allow(deprecated)]
            let line = checker.locator.compute_line_index(shadowed.start());

            checker.diagnostics.push(Diagnostic::new(
                pyflakes::rules::ImportShadowedByLoopVar {
                    name: name.to_string(),
                    line,
                },
                binding.range(),
            ));
        }
    }
}
```

I think this rule could be structured similarly: iterate over the pairs of shadowed bindings, and look for pairs in which the original binding is `BindingKind::Argument`, and the new binding is `BindingKind::LoopVar`.

---

_@charliermarsh reviewed on 2023-10-25 23:38_

Thank you for contributing! I'd like us to explore one other approach and see if it produces the same result -- are you up for that?

---

_Comment by @danbi2990 on 2023-10-26 00:38_

Appreciate the comment.
I'll check it out and let you know the result. (after daily work 😅)

---

_Comment by @danbi2990 on 2023-11-10 01:41_

Applied the comment
but there's an issue about `with` stmt.

In the current code, it looks there's no BindingKind for withitem variable.
I tried adding WithItemVar to the BindingKind but failed.

Could you give an advice for the task?
You can check my trial code in the two reverted commits.

[Revert "add WithItemVar"](https://github.com/astral-sh/ruff/pull/8159/commits/6426c0efa4dd9c71e5834bc53a53fd3f7ddc277f)

[Revert "add binding with stmt"](https://github.com/astral-sh/ruff/pull/8159/commits/818980be66b945fac4a623d93982d1becdeea86d)


---

_Review requested from @charliermarsh by @danbi2990 on 2023-11-10 01:41_

---

_@charliermarsh reviewed on 2023-11-10 01:45_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:108 on 2023-11-10 01:45_

Is this supposed to check if the argument is unused? Or does it fire in either case?

---

_Comment by @charliermarsh on 2023-11-10 01:46_

Thanks @danbi2990! Overall looks good. Let me take a look at adding a binding kind for `WithItem`, it might be more involved.

---

_Comment by @github-actions[bot] on 2023-11-10 01:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+24 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/8312e915691fd7d59a9a2372d8adcd4bfa4a6733/dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py#L659'>dev/breeze/src/airflow_breeze/commands/kubernetes_commands.py:659:51:</a> PLR1704 Redefining argument with the local name `python`
+ <a href='https://github.com/apache/airflow/blob/8312e915691fd7d59a9a2372d8adcd4bfa4a6733/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L379'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:379:9:</a> PLR1704 Redefining argument with the local name `provider_id`
+ <a href='https://github.com/apache/airflow/blob/8312e915691fd7d59a9a2372d8adcd4bfa4a6733/dev/breeze/src/airflow_breeze/commands/sbom_commands.py#L450'>dev/breeze/src/airflow_breeze/commands/sbom_commands.py:450:26:</a> PLR1704 Redefining argument with the local name `provider_version`
+ <a href='https://github.com/apache/airflow/blob/8312e915691fd7d59a9a2372d8adcd4bfa4a6733/scripts/in_container/verify_providers.py#L188'>scripts/in_container/verify_providers.py:188:15:</a> PLR1704 Redefining argument with the local name `prefix`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/document/callbacks.py#L361'>src/bokeh/document/callbacks.py:361:13:</a> PLR1704 Redefining argument with the local name `callback_obj`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/plotting/_figure.py#L384'>src/bokeh/plotting/_figure.py:384:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/plotting/_figure.py#L425'>src/bokeh/plotting/_figure.py:425:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/plotting/_figure.py#L475'>src/bokeh/plotting/_figure.py:475:17:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/plotting/_figure.py#L564'>src/bokeh/plotting/_figure.py:564:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/src/bokeh/plotting/_figure.py#L606'>src/bokeh/plotting/_figure.py:606:13:</a> PLR1704 Redefining argument with the local name `kw`
+ <a href='https://github.com/bokeh/bokeh/blob/2e85cfb76b23c8537a704ec1a0af94437a5834f0/tests/support/util/examples.py#L161'>tests/support/util/examples.py:161:9:</a> PLR1704 Redefining argument with the local name `path`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/ibis-project/ibis">ibis-project/ibis</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/6172f07bbd04a6b65a1225e0e9105473669eaeae/ibis/__init__.py#L125'>ibis/__init__.py:125:9:</a> PLR1704 Redefining argument with the local name `name`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+7 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/core/computation/eval.py#L323'>pandas/core/computation/eval.py:323:9:</a> PLR1704 Redefining argument with the local name `expr`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/core/generic.py#L4747'>pandas/core/generic.py:4747:13:</a> PLR1704 Redefining argument with the local name `axis`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/core/generic.py#L4747'>pandas/core/generic.py:4747:19:</a> PLR1704 Redefining argument with the local name `labels`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/io/common.py#L591'>pandas/io/common.py:591:24:</a> PLR1704 Redefining argument with the local name `compression`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/plotting/_matplotlib/boxplot.py#L540'>pandas/plotting/_matplotlib/boxplot.py:540:27:</a> PLR1704 Redefining argument with the local name `ax`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/tests/io/test_parquet.py#L218'>pandas/tests/io/test_parquet.py:218:35:</a> PLR1704 Redefining argument with the local name `path`
+ <a href='https://github.com/pandas-dev/pandas/blob/09ed69edc3a83141244b3b7e1098d0083c8a9440/pandas/tests/reshape/merge/test_merge.py#L566'>pandas/tests/reshape/merge/test_merge.py:566:17:</a> PLR1704 Redefining argument with the local name `kwarg`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/sphinx-doc/sphinx/blob/bb74aec2b6aa1179868d83134013450c9ff9d4d6/sphinx/ext/inheritance_diagram.py#L316'>sphinx/ext/inheritance_diagram.py:316:13:</a> PLR1704 Redefining argument with the local name `name`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/a7f02c89d7baf3c18785cf4251c8e9eb1ada45e9/zerver/lib/scim.py#L232'>zerver/lib/scim.py:232:13:</a> PLR1704 Redefining argument with the local name `path`
+ <a href='https://github.com/zulip/zulip/blob/a7f02c89d7baf3c18785cf4251c8e9eb1ada45e9/zerver/webhooks/clubhouse/view.py#L178'>zerver/webhooks/clubhouse/view.py:178:9:</a> PLR1704 Redefining argument with the local name `action`
+ <a href='https://github.com/zulip/zulip/blob/a7f02c89d7baf3c18785cf4251c8e9eb1ada45e9/zerver/webhooks/clubhouse/view.py#L465'>zerver/webhooks/clubhouse/view.py:465:13:</a> PLR1704 Redefining argument with the local name `action`
+ <a href='https://github.com/zulip/zulip/blob/a7f02c89d7baf3c18785cf4251c8e9eb1ada45e9/zerver/webhooks/clubhouse/view.py#L669'>zerver/webhooks/clubhouse/view.py:669:13:</a> PLR1704 Redefining argument with the local name `action`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR1704 | 24 | 24 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2023-11-10 03:45_

@danbi2990 - I added a with-item kind in #8594. Can you try updating against `main`?

---

_@danbi2990 reviewed on 2023-11-10 07:01_

---

_Review comment by @danbi2990 on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:108 on 2023-11-10 07:01_

It's not checking the usage.

It looks sensible to check `binding.is_used()` as below
```rust
if !shadowed.kind.is_argument() || !binding.is_used() {
    continue;
}
```

In this case, results are following
```python
def foo(i):
    for i in range(10):  # emit error
        print(i)
        ...

def foo(i):
    for i in range(10):  # no error because it's unused
        ...
```
Is it good to go?

---

_Review requested from @charliermarsh by @danbi2990 on 2023-11-10 07:53_

---

_Comment by @danbi2990 on 2023-11-10 07:54_

It's working with updated main.
Please check updated code 🙏

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:108 on 2023-11-10 14:50_

I was referring to the inverse -- should this depend on the _argument_ being unused? I'll check Pylint.

---

_@charliermarsh reviewed on 2023-11-10 14:50_

---

_@charliermarsh reviewed on 2023-11-10 18:54_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs`:108 on 2023-11-10 18:54_

I erred on the side of removing the `!binding.is_used()` piece here. I think it still makes sense to raise, even if the new binding is unused, since it will cause the same problems?

---

_@charliermarsh approved on 2023-11-10 18:55_

Looks good, I just need to review the ecosystem checks once they finish.

---

_Merged by @charliermarsh on 2023-11-10 19:13_

---

_Closed by @charliermarsh on 2023-11-10 19:13_

---
