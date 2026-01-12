```yaml
number: 3561
title: "Implement `flake8-newspaper-style`"
type: pull_request
state: closed
author: sasanjac
labels: []
assignees: []
base: main
head: sasanjac/2436-Implement-`flake8-newspaper-style`
created_at: 2023-03-16T17:17:07Z
updated_at: 2023-06-12T22:01:12Z
url: https://github.com/astral-sh/ruff/pull/3561
synced_at: 2026-01-12T15:55:13Z
```

# Implement `flake8-newspaper-style`

---

_@sasanjac_

Fixes #2436

---

_Comment by @github-actions[bot] on 2023-03-16 17:40_

## PR Check Results
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01      8.4±0.05ms     4.9 MB/sec    1.00      8.2±0.02ms     4.9 MB/sec
linter/numpy/ctypeslib.py    1.00      2.3±0.02ms   152.4 MB/sec    1.01      2.3±0.01ms   150.3 MB/sec
linter/numpy/globals.py      1.00  1142.3±20.39µs   156.1 MB/sec    1.01   1149.2±5.94µs   155.1 MB/sec
linter/pydantic/types.py     1.00      3.8±0.01ms     6.6 MB/sec    1.01      3.9±0.02ms     6.6 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.01      8.9±0.09ms     4.6 MB/sec    1.00      8.8±0.07ms     4.6 MB/sec
linter/numpy/ctypeslib.py    1.00      2.3±0.01ms   148.9 MB/sec    1.00      2.3±0.01ms   148.2 MB/sec
linter/numpy/globals.py      1.00  1173.7±14.15µs   151.9 MB/sec    1.00  1172.4±21.85µs   152.0 MB/sec
linter/pydantic/types.py     1.03      4.2±0.21ms     6.1 MB/sec    1.00      4.0±0.04ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @JonathanPlasse on 2023-03-16 18:14_

What happens if two functions or classes call each other?

---

_Comment by @sasanjac on 2023-03-16 19:50_

`RecursionError` :D
Yeah, I'm going to have to think about solving that.

---

_Review comment by @MichaReiser on `crates/ruff/resources/test/fixtures/flake8_newspaper_style/NEW100.py`:74 on 2023-03-17 07:53_

Can we add a test case that calls an undefined function? Or is this not possible because some other ruff infrastructure then fails early?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:27 on 2023-03-17 07:53_

Nit: Delete
```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:42 on 2023-03-17 07:59_

Nit: You can compare the options directly to avoid the `unwrap_or` (and a potential panic if the `bindings` misses the `caller_name` binding for whatever reasons. 
```suggestion
                if bindings.get(caller_name) > bindings.get(&callee_name) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:44 on 2023-03-17 08:01_

Nit: `checker.ctx.bindngs[*index]` (you already have the index)

---

_@MichaReiser approved on 2023-03-17 08:02_

Looks good. Thanks for working on this. Let's add a test case for an invalid caller name and one for recursion (although I don't see any issues).

---

_@sasanjac reviewed on 2023-03-17 08:26_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:27 on 2023-03-17 08:26_

oops 🙈 

---

_@sasanjac reviewed on 2023-03-17 08:30_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:42 on 2023-03-17 08:30_

1. `bindings` can't miss `caller_name`.
2. If `bindings` misses `callee_name`, we get a panic, however, when we instead get the max. int, this conditions always evaluates to false and thus no violation is reported.

---

_Comment by @sasanjac on 2023-03-17 14:51_

> What happens if two functions or classes call each other?

Should be fixed now

---

_@charliermarsh reviewed on 2023-03-17 22:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:8 on 2023-03-17 22:12_

I think this may need to be rebased, some of these got moved around the APIs changed a bit today.

---

_@charliermarsh reviewed on 2023-03-17 22:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:43 on 2023-03-17 22:12_

Nit: don't forget to remove :)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:62 on 2023-03-17 22:14_

Do you mind documenting the various cases here? I'm having a little trouble following from the code alone (e.g., which path is intended for which combination of definitions).

---

_@charliermarsh reviewed on 2023-03-17 22:14_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:8 on 2023-03-17 22:42_

Yeah I'm already on it, however  it's taking me some time to understand some patterns in Rust 😅

---

_@sasanjac reviewed on 2023-03-17 22:42_

---

_@MichaReiser reviewed on 2023-03-18 06:50_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:8 on 2023-03-18 06:50_

Let me know if I can help you with the rebase or if you have any questions 

---

_@sasanjac reviewed on 2023-03-18 09:30_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:8 on 2023-03-18 09:30_

Thanks :) I think I got it now but maybe you could check my new changes to see if I understood correctly

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:308 on 2023-03-18 09:38_

I don't think this returns the `grandparent` scope

* `scope_stack.iter()` returns the reversed stack
* `next` returns the first element 

-> That would mean it returns the global scope. 

I believe this should instead be

```rust
self.scope_stack.iter().nth(2).map(|index| &self.scopes[*index])
```

---

_@MichaReiser reviewed on 2023-03-18 09:38_

---

_@MichaReiser reviewed on 2023-03-18 09:40_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:134 on 2023-03-18 09:40_

Does this logic recursively traverse the body of the function (and if so, this is going to be expensive)?

```python
def a():
	if True:  # child of b's body
		b()


def b(): 
	if False:
		a()
```

---

_@sasanjac reviewed on 2023-03-18 09:42_

---

_Review comment by @sasanjac on `crates/ruff_python_ast/src/context.rs`:308 on 2023-03-18 09:42_

Yeah, it used to be like this but now `.nth(2)` returns the function scope (current scope) and `.nth(0)` or `.next()` returns the module scope. Or am I thinking wrong: `function < class < module <=> current < parent < grandparent`

---

_@sasanjac reviewed on 2023-03-18 11:35_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:134 on 2023-03-18 11:35_

I don't think yet but it definitely should at least until it first encounters a mutual recursion. I'm going to fix this

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:69 on 2023-03-19 03:10_

Should we be validating here that this function is a method (instance or class), and that this call is on `self` or `cls` as appropriate?

E.g., these are false positives:

```py
class A:
    def foo(self):
        return 1

    def bar(self):
        bop.foo()  # NEW100
        b = bop.foo()  # NEW100
        return self.bar(b)
```

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:118 on 2023-03-19 03:11_

Hmm, if I understand what's going on here, I consider this an implementation detail. It's not guaranteed that these bindings are increasing as we parse through the program. Can we instead look at the `source` field on `Binding`, and compare based on location?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:29 on 2023-03-19 03:12_

Nit: this isn't necessary, right? Since we checked this a level above?

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:49 on 2023-03-19 03:15_

Should we be catching nested functions? Like this?

```py
def foo():

    def bop():
        bar()

    def bar():
        x = 1

    def baz():
        x = 1
```

Right now, neither this implementation nor `flake8-newspaper-style` catch this.


---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:147 on 2023-03-19 03:15_

Nit: use `Stmt` in lieu of `Located<StmtKind>`, and `&str` in lieu of `&String`, if possible?

---

_@charliermarsh reviewed on 2023-03-19 03:16_

Thanks, really sorry for the double review, the comments helped me understand the code better so have one or two more questions!

---

_Comment by @charliermarsh on 2023-03-19 03:17_

I'd love to get the ecosystem CI check running on this, I wonder why it's not appearing.

---

_Comment by @MichaReiser on 2023-03-19 12:23_

> I'd love to get the ecosystem CI check running on this, I wonder why it's not appearing.

The diff is too large and exceeds the GitHub summary... https://github.com/charliermarsh/ruff/actions/runs/4457214954/jobs/7828206645?pr=3561

---

_@MichaReiser reviewed on 2023-03-19 12:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/context.rs`:303 on 2023-03-19 12:25_

We should investigate why `nth(2)` doesn't give you the expected result. I recommend moving this function to your implementation if it is because of the way you traverse the AST. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:131 on 2023-03-19 12:32_

Is it intended that the rule uses bitwise and?

```suggestion
                if matches!(kind, BindingKind::FunctionDefinition)
                    && !mutual_recursion(body, &caller_name.to_string())
```

---

_@MichaReiser reviewed on 2023-03-19 12:32_

---

_@sasanjac reviewed on 2023-03-19 21:23_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:134 on 2023-03-19 21:23_

For now, it does only find mutual recursion using two methods. However we could remove that altogether and just require people to ignore the violation with `noqa` when a recursion does indeed occur.

---

_Comment by @sasanjac on 2023-03-19 21:24_

> Thanks, really sorry for the double review, the comments helped me understand the code better so have one or two more questions!

No thanks for your comments, it made me understand a bit more about the projects structure and Rust in general

---

_@sasanjac reviewed on 2023-03-20 07:42_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:131 on 2023-03-20 07:42_

No 🙈, fixed

---

_@sasanjac reviewed on 2023-03-20 07:43_

---

_Review comment by @sasanjac on `crates/ruff_python_ast/src/context.rs`:303 on 2023-03-20 07:43_

I modified it and instead get a copy of the scope stack which is the popped/pushed according to the scope.

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:147 on 2023-03-20 07:43_

Fixed

---

_@sasanjac reviewed on 2023-03-20 07:43_

---

_@sasanjac reviewed on 2023-03-20 07:44_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:49 on 2023-03-20 07:44_

Done, however we might have to think about how it is possible to completely generalize this problem. E.g. catching nested functions is done recursively, so the nested level shouldn't matter but nested classes are not caught atm.

---

_@sasanjac reviewed on 2023-03-20 07:45_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:29 on 2023-03-20 07:45_

Right, fixed

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:118 on 2023-03-20 07:45_

Fixed

---

_@sasanjac reviewed on 2023-03-20 07:45_

---

_@sasanjac reviewed on 2023-03-20 07:45_

---

_Review comment by @sasanjac on `crates/ruff/src/rules/flake8_newspaper_style/rules/check_function_calls.rs`:69 on 2023-03-20 07:45_

Yes, fixed

---

_Review requested from @MichaReiser by @sasanjac on 2023-03-21 10:57_

---

_Review request for @MichaReiser removed by @sasanjac on 2023-03-21 10:57_

---

_Review requested from @charliermarsh by @sasanjac on 2023-03-21 10:57_

---

_Review request for @charliermarsh removed by @sasanjac on 2023-03-21 10:57_

---

_Review requested from @MichaReiser by @sasanjac on 2023-03-21 10:58_

---

_Review request for @MichaReiser removed by @sasanjac on 2023-03-21 10:58_

---

_Review requested from @charliermarsh by @sasanjac on 2023-03-21 10:58_

---

_Comment by @sasanjac on 2023-03-29 12:09_

@charliermarsh @MichaReiser  Is there anything I can do to advance this PR? 

---

_Comment by @charliermarsh on 2023-03-30 02:11_

@sasanjac - No, really sorry, this is just on me/us to define a formal policy around which plugins we include in Ruff or don't, and why -- which we're working on. Some plugins are obvious or easy to include, but in this case I'm moving slowly because it's a very opinionated rule that will trigger a lot of violations on any project using `"ALL"` (which is discouraged, but does have usages out there), and so I want to have a clear decision-making process in place before we merge this.

I'm happy to be responsible for rebasing etc. though I again am having some trouble pushing to this branch, maybe because of the backticks, I'm not sure.


---

_Comment by @sasanjac on 2023-03-30 10:54_

> I'm happy to be responsible for rebasing etc. though I again am having some trouble pushing to this branch, maybe because of the backticks, I'm not sure.

That's really strange. I can't edit any settings on this PR and afaik edits are allowed for maintainers by default. Did you check out the branch in my fork?


---

_Comment by @charliermarsh on 2023-06-12 22:01_

Thank you again for all your work here. I'm sorry to have let this one linger, but... I've made the decision to close it out. I don't think this plugin has the right characteristics to be part of Ruff right now -- it'd be one of the least-downloaded existing plugins we've implemented (~5k downloads per month), but with a high flag rate, and a complex interface + implementation. It would also conflict with `ssort`, which there's been interest in implementing, and has about 10x the downloads in the wild. None of these characteristics would be individually disqualifying, but as a whole, I don't think it makes sense to merge.

I apologize for not making this call sooner, _especially_ before you'd started on the implementation -- I'm trying to get better at making those kinds of decisions earlier in the development process, but am still growing into my responsibilities as a maintainer.


---

_Closed by @charliermarsh on 2023-06-12 22:01_

---
