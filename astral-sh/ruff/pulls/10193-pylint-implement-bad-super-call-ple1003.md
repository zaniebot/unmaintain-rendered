```yaml
number: 10193
title: "[`pylint`] Implement `bad-super-call` (`PLE1003`)"
type: pull_request
state: closed
author: chanman3388
labels:
  - rule
assignees: []
draft: true
base: main
head: PLE1003
created_at: 2024-03-02T02:04:14Z
updated_at: 2024-03-28T17:53:43Z
url: https://github.com/astral-sh/ruff/pull/10193
synced_at: 2026-01-10T22:47:01Z
```

# [`pylint`] Implement `bad-super-call` (`PLE1003`)

---

_Pull request opened by @chanman3388 on 2024-03-02 02:04_

## Summary

Implements the [`bad-super-call`](https://pylint.readthedocs.io/en/stable/user_guide/messages/error/bad-super-call.html) rule (`E1003`) from Pylint.

## Test Plan

A new fixture has been added

Part of #970 


---

_Comment by @github-actions[bot] on 2024-03-02 02:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 3 projects; 40 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/58e63ec12830160c29fde490e7836254068b855e/pandas/core/apply.py#L1578'>pandas/core/apply.py:1578:9:</a> PLE1003 Bad first argument 'GroupByApply' given to super()
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/pip">pypa/pip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/pip/blob/ae5fff36b0aad6e5e0037884927eaa29163c0611/src/pip/_internal/exceptions.py#L409'>src/pip/_internal/exceptions.py:409:9:</a> PLE1003 Bad first argument 'InstallationSubprocessError' given to super()
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b4d53c7930b01145aa43fef2a8fab5730c8b1bc3/zerver/tests/test_auth_backends.py#L345'>zerver/tests/test_auth_backends.py:345:9:</a> PLE1003 Bad first argument 'UserProfile' given to super()
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE1003 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:80 on 2024-03-04 10:12_

This seems to be valid python code:

```python
class Test:
...     def __init__():
...         super = test
...         
... 
```

That means, we have to test if `super` points to the builtin `super` call and not a local binding.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:79 on 2024-03-04 10:14_

```suggestion
        if let Expr::Name(ExprName { id, .. }) = &**func {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:10 on 2024-03-04 10:15_

```suggestion
/// Checks to see another argument other than the current class is given as the first argument of the super builtin.
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:17 on 2024-03-04 10:15_

What are the use cases when I would use this rule over `super-with-arguments` when targeting Python 3+ (the only version ruff supports)?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:97 on 2024-03-04 10:18_

We can avoid the explicit returns 
```suggestion
        if let Expr::Name(ExprName { id, .. }) = func.as_ref() {
            if id == "super" {
                if let Some(first) = arguments.args.first() {
                    return is_bad_super_call(first, current_class_name).map(|id| {
                        Diagnostic::new(
                            BadSuperCall {
                                bad_super_arg: id.to_owned(),
                            },
                            range.to_owned(),
                        )
                    });
                }
            }
        }
        check_expr(func, current_class_name)
    } else if let Expr::Attribute(ExprAttribute { value, .. }) = expr {
        check_expr(value, current_class_name)
    } else {
        None
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:100 on 2024-03-04 10:20_

I think this will warn about

```python
class Test:
    a = super()
```

because `traverse_body` is used to traverse the class and nested function bodies. Is this intentional?

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:56 on 2024-03-04 10:21_

Can we add a test with a nested class to show that it does not traverse into classes

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:60 on 2024-03-04 10:21_

I think the current implementation doesn't catch

```python
class Test:
    def test():
        if True:
            super(Tree, self).init()

---

_@MichaReiser reviewed on 2024-03-04 10:22_

Thanks for working on this rule. 

For me, as someone not very proficient in Python, it's unclear when I should use the rule when using Python 3. Is this rule mainly targeted at Python 2 users

---

_Label `rule` added by @MichaReiser on 2024-03-04 10:22_

---

_Comment by @chanman3388 on 2024-03-04 11:48_

> Thanks for working on this rule.
> 
> For me, as someone not very proficient in Python, it's unclear when I should use the rule when using Python 3. Is this rule mainly targeted at Python 2 users

Thanks for the thorough review!
To the best of my knowledge this is meant to be targeted at Python 2 users, though it is still valid syntax for python 3 I believe.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:70 on 2024-03-04 11:58_

I'd expect a function with a name starting with `is_` to return `bool`. Could this function be renamed to something more clear about what it does?

---

_@AlexWaygood reviewed on 2024-03-04 12:01_

---

_@chanman3388 reviewed on 2024-03-04 13:02_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:70 on 2024-03-04 13:02_

Sure, I'll think of something more suitable, maybe...`is_bad_super_call_then_some`?

---

_@chanman3388 reviewed on 2024-03-04 22:13_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:17 on 2024-03-04 22:13_

I don't think you would to be honest, `super-with-arguments` is also a "suggestion".

---

_@chanman3388 reviewed on 2024-03-04 22:34_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:80 on 2024-03-04 22:34_

Is there a canonical test for that?

---

_@chanman3388 reviewed on 2024-03-04 22:36_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:100 on 2024-03-04 22:36_

I checked it and it doesn't.

---

_@chanman3388 reviewed on 2024-03-04 22:37_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:100 on 2024-03-04 22:37_

```
$ cat bad_super.py
class Something:
    pass


class Test:
    def __init__(self):
        super = test


class O(Test):
    def __init__(self):
        super(Something, self).__init__()


class Test2:
    a = super()
```

---

_@chanman3388 reviewed on 2024-03-04 22:37_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:100 on 2024-03-04 22:37_

```
$ cargo run -p ruff -- check bad_super.py --no-cache --preview --select PLE1003
   Compiling ruff_linter v0.3.0 (/home/chris/projects/ruff/crates/ruff_linter)
   Compiling ruff_workspace v0.0.0 (/home/chris/projects/ruff/crates/ruff_workspace)
   Compiling ruff v0.3.0 (/home/chris/projects/ruff/crates/ruff)
    Finished dev [unoptimized + debuginfo] target(s) in 14.73s
     Running `target/debug/ruff check bad_super.py --no-cache --preview --select PLE1003`
bad_super.py:12:9: PLE1003 Bad first argument 'Something' given to super()
   |
10 | class O(Test):
11 |     def __init__(self):
12 |         super(Something, self).__init__()
   |         ^^^^^^^^^^^^^^^^^^^^^^ PLE1003
   |

Found 1 error.
```

---

_@chanman3388 reviewed on 2024-03-05 00:15_

---

_Review comment by @chanman3388 on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:60 on 2024-03-05 00:15_

Good catch

---

_@chanman3388 reviewed on 2024-03-05 00:58_

---

_Review comment by @chanman3388 on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:56 on 2024-03-05 00:58_

Something I had to ask about the nested class, why should we not traverse a nested class? Pylint doesn't pick up on it does it seem like it should?

---

_@chanman3388 reviewed on 2024-03-05 01:07_

---

_Review comment by @chanman3388 on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:56 on 2024-03-05 01:07_

```
class Tree:
    """something"""


class Animal:
    """animal"""


class A:
    def __init__(self):
        class B(Tree):
            def __init__(self):
                super(Tree, self).__init__()

            def ho(self):
                print("a")

        self.b = B()


if __name__ == "__main__":
    a = A()
    a.b.ho()
```
This runs fine, but if you swap out the `Tree` for `Animal` for the inner class `B` you get:
```
Traceback (most recent call last):
  File "/home/chris/projects/ruff/bad_super.py", line 22, in <module>
    a = A()
        ^^^
  File "/home/chris/projects/ruff/bad_super.py", line 18, in __init__
    self.b = B()
             ^^^
  File "/home/chris/projects/ruff/bad_super.py", line 13, in __init__
    super(Animal, self).__init__()
    ^^^^^^^^^^^^^^^^^^^
TypeError: super(type, obj): obj must be an instance or subtype of type
```
So it feels like we should actually pick up on this.

---

_@chanman3388 reviewed on 2024-03-05 01:12_

---

_Review comment by @chanman3388 on `crates/ruff_linter/src/rules/pylint/rules/bad_super_call.rs`:100 on 2024-03-05 01:12_

I am aware that I should also add all the other statements where it's possible that a `class` might be defined, I'll get on this next time.

---

_Converted to draft by @chanman3388 on 2024-03-05 01:12_

---

_@MichaReiser reviewed on 2024-03-05 09:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:56 on 2024-03-05 09:11_

It depends on how the rule is supposed to work. I would expect that the `Checker` calls the rule for every class. That means that nested classes are covered by the `Checker` and we should not traverse nested classes too avoid flagging/traversing it twice. But maybe I misread how the rule is supposed to work.

---

_Comment by @MichaReiser on 2024-03-05 09:12_

We're currently discussing internally if we want to support rules targeting Python 2. I recommend holding of with any changes until we get back to you.

---

_@chanman3388 reviewed on 2024-03-05 10:33_

---

_Review comment by @chanman3388 on `crates/ruff_linter/resources/test/fixtures/pylint/bad_super_call.py`:56 on 2024-03-05 10:33_

Hm, I believe that when I call ruff on the file that it should be calling the Checker once on the top level class, and once on the inner class...I can check this if you give the go-ahead for the rule.

---

_Comment by @MichaReiser on 2024-03-28 17:47_

Hy @chanman3388 

Thanks for implementing the rule. After discussing this internally, we decided not to support rules that mainly target Python 2, mainly because Python 2 has been EOL for a very long time (and is not supported by Ruff), and because of that, it doesn't feel worth maintaining the rule going forward.

---

_Closed by @MichaReiser on 2024-03-28 17:47_

---

_Comment by @chanman3388 on 2024-03-28 17:49_

> Hy @chanman3388 
> 
> Thanks for implementing the rule. After discussing this internally, we decided not to support rules that mainly target Python 2, mainly because Python 2 has been EOL for a very long time (and is not supported by Ruff), and because of that, it doesn't feel worth maintaining the rule going forward.

Could we indicate as such in the issue? I mean both crossing out the rule as well as adding a note to leave it up to contributors to judge if a rule primarily targets python 2 also.

---

_Comment by @MichaReiser on 2024-03-28 17:53_

Oh yeah sorry. I should have done that. Thanks for reminding me.

---
