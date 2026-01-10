```yaml
number: 10820
title: "[pylint] Implement global-variable-undefined (W0601)"
type: pull_request
state: closed
author: tibor-reiss
labels:
  - rule
  - preview
assignees: []
base: main
head: add-W0601
created_at: 2024-04-07T19:20:10Z
updated_at: 2025-04-28T07:02:25Z
url: https://github.com/astral-sh/ruff/pull/10820
synced_at: 2026-01-10T19:33:02Z
```

# [pylint] Implement global-variable-undefined (W0601)

---

_Pull request opened by @tibor-reiss on 2024-04-07 19:20_

Add pylint rule global-variable-undefined (W0601)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`


---

_Comment by @github-actions[bot] on 2024-04-07 19:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+4 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/6f986d2b6c518f60eab175afb4c360aea9984d50/tests/listeners/class_listener.py#L39'>tests/listeners/class_listener.py:39:13:</a> PLW0601 Global variable `stopped_component` is undefined at the module
+ <a href='https://github.com/apache/airflow/blob/6f986d2b6c518f60eab175afb4c360aea9984d50/tests/listeners/class_listener.py#L71'>tests/listeners/class_listener.py:71:13:</a> PLW0601 Global variable `stopped_component` is undefined at the module
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 18:2:5: PLW0601 Global variable `_last_kname` is undefined at the module
+ examples/output/jupyter/push_notebook/Numba Image Example.ipynb:cell 18:3:5: PLW0601 Global variable `_last_out` is undefined at the module
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0601 | 4 | 4 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @tusharsadhwani on 2024-04-07 20:21_

Does this break on wildcard imports?

```python
from os import *

def foo():
    global system  # W0601 should not be raised
    system = lambda _: None
```

---

_Comment by @tibor-reiss on 2024-04-14 09:16_

> Does this break on wildcard imports?
> 
> ```python
> from os import *
> 
> def foo():
>     global system  # W0601 should not be raised
>     system = lambda _: None
> ```

Yes, it does. I have not found a function in ruff which resolves star-imports, e.g. there is F403 and F405 which are raised for star-imports and warn about "...unable to detect..." and "...may be undefined...".

---

_Comment by @MichaReiser on 2024-04-15 07:10_

I think there's a way to detect if there's a star import in which case you could avoid creating any diagnostics (to avoid false-positives). 

The main open question to me are
* how this fits into ruff long term when Ruff catches more static analyzable errors (undefined variables in general)
* What's the motivation that this rule is specific to globals and doesn't apply to all variables.

---

_Label `rule` added by @MichaReiser on 2024-04-15 07:10_

---

_Comment by @tibor-reiss on 2024-04-15 18:57_

@MichaReiser 
Regarding star imports: when I do `from os import *`, pylint resolves this and e.g. has `system` in the namespace. Ruff is not able to do this (yet). For now, I simply left this out in this PR. What is possible though is to add something similar to F403/F405, i.e. if there is a start import, then raise "global variable undefined but maybe imported via star import". What's your preference?

---

_Comment by @MichaReiser on 2024-04-16 07:31_

@tibor-reiss my preference would be that we don't flag any variables if a file contains a star import. I think there's already precedence for this in Ruff but I would need to search it to. @charliermarsh should know where to find it.

---

_Comment by @tibor-reiss on 2024-05-16 09:47_

@MichaReiser thanks for feedback, I added the check for star imports together with a new test file.

---

_Comment by @tibor-reiss on 2024-09-20 17:46_

Hi @MichaReiser, this got lost in translation. I have updated now to the latest master, could you please have a look and let me know if something is still missing?

---

_Comment by @MichaReiser on 2024-09-23 09:48_

Thanks @tibor-reiss 

I took a quick look at the ecosystem result and it's not clear why the rule raises

+ [airflow/settings.py:441:5:](https://github.com/apache/airflow/blob/ee87fa0cba4d83084b4bc617d63d117101d9e069/airflow/settings.py#L441) PLW0601 Global variable `Session` is undefined at the module

Is it because there's only a declaration of `Session`

https://github.com/apache/airflow/blob/ee87fa0cba4d83084b4bc617d63d117101d9e069/airflow/settings.py#L99

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:279 on 2024-09-23 09:48_

```suggestion
        (Pylint, "W0601") => (RuleGroup::Preview, rules::pylint::rules::GlobalVariableUndefined),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:70 on 2024-09-23 09:50_

Do you think we could use `global_scope` here instead?

```suggestion
    let module_scope = checker
        .semantic()
        .global_scope();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:101 on 2024-09-23 09:51_

Why do we only look at imports of the current scope? What about imports in the enclosing scope?

---

_@MichaReiser reviewed on 2024-09-23 09:54_

---

_Comment by @tibor-reiss on 2024-09-29 15:06_

> Thanks @tibor-reiss
> 
> I took a quick look at the ecosystem result and it's not clear why the rule raises
> 
>     * [airflow/settings.py:441:5:](https://github.com/apache/airflow/blob/ee87fa0cba4d83084b4bc617d63d117101d9e069/airflow/settings.py#L441) PLW0601 Global variable `Session` is undefined at the module
> 
> 
> Is it because there's only a declaration of `Session`
> 
> https://github.com/apache/airflow/blob/ee87fa0cba4d83084b4bc617d63d117101d9e069/airflow/settings.py#L99

You are right: there was a case missing, i.e. the global variable was just declared (with type annotation). I have added this case now (with test).

@MichaReiser what do you think about this case? I have it now covered. However, the rule reads **undefined**, so I think there is a strong case for raising the warning if the global was only declared.

---

_@tibor-reiss reviewed on 2024-09-29 19:20_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:101 on 2024-09-29 19:20_

This covers the case when the `global` statement is before the `import` statement - see tests. I additionally added the check for imports in `is_global_binding`.

---

_Review requested from @MichaReiser by @tibor-reiss on 2024-09-29 19:20_

---

_Label `preview` added by @dhruvmanila on 2024-10-01 03:52_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:111 on 2024-10-14 10:28_

Nit:

```suggestion
    let Some(ast::StmtFunctionDef { body, .. }) = checker
        .semantic()
        .current_scopes()
        .find_map(|scope| scope.kind.as_function())
    else {
        return vec![];
    };
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:123 on 2024-10-14 10:35_

Should this also respect imports in conditional branches. E.g. what about:

```python
def test(a):
	if a:
		from b import bar

	globals bar

```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/global_variable_undefined.py`:4 on 2024-10-14 12:28_

We can remove those comments because our tests run in isolation

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/global_variable_undefined.rs`:70 on 2024-10-14 12:30_

My python knowledge is too limited to understand why it is okay to ignore imports from the enclosing function's scope. Could you help me understand why it is okay to ignore imports?

I read https://docs.python.org/3/reference/simple_stmts.html#the-global-statement and couldn't find any special handling of imports in the enclosing scope.

---

_@MichaReiser reviewed on 2024-10-14 12:30_

---

_Closed by @MichaReiser on 2025-04-28 07:02_

---
