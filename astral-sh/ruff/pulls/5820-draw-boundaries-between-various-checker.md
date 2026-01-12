```yaml
number: 5820
title: "Draw boundaries between various `Checker` visitation phases"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/bind
created_at: 2023-07-17T04:22:31Z
updated_at: 2023-07-17T17:02:23Z
url: https://github.com/astral-sh/ruff/pull/5820
synced_at: 2026-01-12T15:55:19Z
```

# Draw boundaries between various `Checker` visitation phases

---

_@charliermarsh_

## Summary

This PR does some non-behavior-changing refactoring of the AST checker. Specifically, it breaks the `Stmt`, `Expr`, and `ExceptHandler` visitors into four distinct, consistent phases:

1. **Phase 1: Analysis**: Run any lint rules on the node.
2. **Phase 2: Binding**: Bind any symbols declared by the node.
3. **Phase 3: Recursion**: Visit all child nodes.
4. **Phase 4: Clean-up**: Pop scopes, etc.

There are some fuzzy boundaries in the last three phases, but the most important divide is between the Phase 1 and all the others -- the goal here is (as much as possible) to disentangle all of the vanilla lint-rule calls from any other semantic analysis or model building.

Part of the motivation here is that I'm considering re-ordering some of these phases, and it was just impossible to reason about that change as long as we had miscellaneous binding-creation and scope-modification code intermingled with lint rules. However, this could also enable us to (e.g.) move the entire analysis phase elsewhere, and even with a more limited API that has read-only access to `Checker` (but can push to a diagnostics vector).


---

_@charliermarsh reviewed on 2023-07-17 04:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:270 on 2023-07-17 04:22_

Moved into the Binding phase.

---

_@charliermarsh reviewed on 2023-07-17 04:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:313 on 2023-07-17 04:22_

Moved into the Binding phase.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:837 on 2023-07-17 04:22_

Moved into the Binding phase.

---

_@charliermarsh reviewed on 2023-07-17 04:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2266 on 2023-07-17 04:23_

Moved into the binding phase.

---

_@charliermarsh reviewed on 2023-07-17 04:23_

---

_Comment by @github-actions[bot] on 2023-07-17 04:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.06ms     4.3 MB/sec    1.01      9.5±0.04ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1865.6±3.83µs     8.9 MB/sec    1.00   1866.0±4.93µs     8.9 MB/sec
formatter/numpy/globals.py                 1.00    209.4±0.34µs    14.1 MB/sec    1.01    210.8±2.04µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.3 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.06ms     3.0 MB/sec    1.01     13.5±0.09ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.02ms     5.0 MB/sec    1.00      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    438.8±1.28µs     6.7 MB/sec    1.00    439.3±0.56µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.05ms     4.2 MB/sec    1.00      6.1±0.06ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1399.6±1.96µs    11.9 MB/sec    1.00   1402.6±3.63µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    156.2±2.70µs    18.9 MB/sec    1.00    156.2±0.28µs    18.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.9±0.01ms     8.7 MB/sec    1.01      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.14ms     3.6 MB/sec    1.00     11.3±0.14ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.02ms     7.6 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.00    230.8±3.88µs    12.8 MB/sec    1.02   236.0±18.22µs    12.5 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.10ms     5.3 MB/sec    1.00      4.9±0.16ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.14ms     2.6 MB/sec    1.00     15.8±0.20ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.10ms     3.9 MB/sec    1.00      4.2±0.13ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    426.5±9.13µs     6.9 MB/sec    1.01   429.4±13.39µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.3±0.17ms     3.5 MB/sec    1.00      7.2±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.09ms     5.0 MB/sec    1.00      8.2±0.07ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1665.2±50.79µs    10.0 MB/sec    1.00  1669.0±45.54µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    173.5±3.80µs    17.0 MB/sec    1.00    174.0±3.90µs    17.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.03ms     7.0 MB/sec    1.00      3.6±0.04ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:208 on 2023-07-17 06:19_

Can we add some documentation to `Checker` that explains the different phases? Especially `Analysis` is unclear to me (`Syntax`?)

---

_@MichaReiser reviewed on 2023-07-17 06:23_

Okay, I now understand the phases. They are very different from what I understood from Phases and the fact that they overlap (The pre-processing of expression happens during a statements recursion phase) is confusing to me. The issue could just be that it they are named *Phases*. 

I didn't review the changes line by line because the PR seems to mainly move code around. 
I don't have a good recommendation on what to do differently or an alternate name suggestion (maybe step?))


For maintainability. How do we ensure that new rules are added to the right *phase* in the future? 

How does it work that lint rules can run and use the semantic model before the binder phase for that node completed? For example, wouldn't that mean that we miss a *read* of the `callee` when analysing a call expression?

Written before I understood the *phases* concept. Kept for completness

What's the motivation for splitting `Syntax` and `Binding` into two phases? 

I don't see the two as so different: Lint rules only generate diagnostics whereas `Binding` constructs a new representation. 

The way I think about it is that *visitors* take different inputs and can produce different outputs:
* Binder: 
  * Input: Syntax Node
  * output: Bound semantic model
* Syntax Visitor
	* Input: Syntax Node
	* Output: Diagnostic
* Semantic Visitor
	* Input: Semantic Model, Syntax Node
	* Output: Diagnostic

That means, we only need two passes
As you can see, th

---

_Comment by @charliermarsh on 2023-07-17 14:15_

@MichaReiser - Couple quick things (I almost just wrote this in Discord but want to err on the side of putting things in GitHub -- but _also_ trying to let myself write brief and informal comments on GitHub):

1. I was going to reorder the "phases" in a second PR such that we run lint rules after binding. I think it probably works right now because we tend to only do all-usage-renames in deferred phases.
2. If I were to merge the last three "phases", would it make sense to you to retain the "phase" terminology? Since we'd have one model-building and visitation phase, and then the linting phase.
3. RE "For maintainability. How do we ensure that new rules are added to the right phase in the future?" -- I was thinking of doing something very dumb and lightweight, like moving the analysis phase to its own module (maybe a trait, maybe just a method on `Checker`). Then, we'd point to that separate module in the docs, etc. Eventually we can enforce this with better API, which would have other benefits (e.g., the rules should only have mutable access to `diagnostics`, not to anything else on `Checker`), but I'm hesitant to do larger API refactors without spending more time thinking it through.

---

_Comment by @MichaReiser on 2023-07-17 16:06_

> I was going to reorder the "phases" in a second PR such that we run lint rules after binding. I think it probably works right now because we tend to only do all-usage-renames in deferred phases.

Would this be moving to something close to what I outlined above? Where we have two phases, one syntax based and phase where we also have the semantic model?

> If I were to merge the last three "phases", would it make sense to you to retain the "phase" terminology? Since we'd have one model-building and visitation phase, and then the linting phase.

I guess that works too. My gut feeling is that I would prefer running the syntax rules together with the binder to avoid that the semantic model construction prevents them from running (and we could even emit these diagnostics earlier). 

Feel free to merge. I think I'm mainly interested in how you lay out the phases in your next PR (with explanations)

---

_Merged by @charliermarsh on 2023-07-17 17:02_

---

_Closed by @charliermarsh on 2023-07-17 17:02_

---

_Branch deleted on 2023-07-17 17:02_

---
