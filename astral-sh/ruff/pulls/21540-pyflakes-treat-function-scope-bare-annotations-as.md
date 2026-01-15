```yaml
number: 21540
title: "[pyflakes] Treat function-scope bare annotations as locals per PEP 526 (F821)"
type: pull_request
state: open
author: Ruchir28
labels: []
assignees: []
base: main
head: Issue-21494
created_at: 2025-11-20T15:49:52Z
updated_at: 2026-01-15T14:25:22Z
url: https://github.com/astral-sh/ruff/pull/21540
synced_at: 2026-01-15T14:51:20Z
```

# [pyflakes] Treat function-scope bare annotations as locals per PEP 526 (F821)

---

_@Ruchir28_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #21494 

Treat function-scope annotated variables as locals per PEP 526 , preventing F821 false positives.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
<!-- How was it tested? -->
Added tests for these cases in `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_34.py`

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 16:08_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/38c4757662455691b32460a8eb14027ef2d35fed/task-sdk/src/airflow/sdk/execution_time/callback_runner.py#L109'>task-sdk/src/airflow/sdk/execution_time/callback_runner.py:109:28:</a> RUF100 [*] Unused `noqa` directive (unused: `F821`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/38c4757662455691b32460a8eb14027ef2d35fed/task-sdk/src/airflow/sdk/execution_time/callback_runner.py#L109'>task-sdk/src/airflow/sdk/execution_time/callback_runner.py:109:28:</a> RUF100 [*] Unused `noqa` directive (unused: `F821`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>





---

_@ntBre reviewed on 2025-11-20 19:32_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_34.py`:8 on 2025-11-20 19:32_

I still haven't looked too closely at this, but I agree that it's unfortunate not to flag this case. It might be worth looking into how ty handles this since it seems to get the expected result in both cases:

https://play.ty.dev/c6f45910-a2bd-4745-827b-f29a148f4e63

It doesn't seem to be related to `nonlocal`, though, because commenting it out doesn't change ty's diagnostics.

---

_@Ruchir28 reviewed on 2025-11-21 15:03_

---

_Review comment by @Ruchir28 on `crates/ruff_linter/resources/test/fixtures/pyflakes/F821_34.py`:8 on 2025-11-21 15:03_

One option to do this is then something like 
```
                    BindingKind::Annotation if !self.in_stub_file() => {
                        if self.scopes[self.bindings[binding_id].scope]
                            .kind
                            .is_function()
                        {
                            // If we're in a function scope, and the annotation is local to that scope,
                            // treat it as unbound (since it's not assigned).
                            // If it's an enclosing scope, we treat it as resolved (to support closures).
                            if index == 0 {
                                self.unresolved_references.push(
                                    name.range,
                                    self.exceptions(),
                                    UnresolvedReferenceFlags::empty(),
                                );
                                return ReadResult::UnboundLocal(binding_id);
                            }
                            
                            self.resolved_names.insert(name.into(), binding_id);
                            return ReadResult::Resolved(binding_id);
                        
                        }
                        continue;
                    }
```

if we do this then both the cases will be handled i.e. error in this case 

```
def demonstrate_bare_local_annotation():
    x: int
    print(x)
```

and no error in the closure cases i.e. the setter,getter example 


but then again we will have edge cases like these where no error will be reported [although even ty doesn't report them]

```
def demo_closure():
    x: int
    def func():
        return x
```

What do you think about this 

---

_Review requested from @ntBre by @MichaReiser on 2026-01-15 14:25_

---
