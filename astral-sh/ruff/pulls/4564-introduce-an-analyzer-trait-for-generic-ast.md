```yaml
number: 4564
title: "Introduce an `Analyzer` trait for generic, AST-oriented lint rules"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
draft: true
base: main
head: charlie/dispatch
created_at: 2023-05-21T19:13:43Z
updated_at: 2023-06-12T18:45:10Z
url: https://github.com/astral-sh/ruff/pull/4564
synced_at: 2026-01-12T15:55:15Z
```

# Introduce an `Analyzer` trait for generic, AST-oriented lint rules

---

_@charliermarsh_

## Summary

This PR is a proposal (not expected to be merged -- only to facilitate discussion) for a design that would enable us to replace our ad-hoc AST rule interface with a standardized trait, _thereby_ enabling us to remove all of the repetitive, imperative logic throughout the `Checker` that confirms the validity of and calls rules whenever relevant. Instead, we can collect the relevant set of rules for each AST node upfront, and iterate over those rules when we visit the relevant node.

I looked at Rome's `Rule` and `RegistryRule` traits when designing this. There's a lot of nice stuff in that API, but this PR doesn't attempt to implement or support it all... Instead, this PR just introduces a simple `Analyzer` trait that looks like:

```rust
/// Trait for a lint rule that can be run on an AST node of type `T`.
pub(crate) trait Analyzer<T>: Sized {
    /// The [`Rule`] that this analyzer implements.
    fn rule() -> Rule;

    /// Run the analyzer on the given node.
    fn run(diagnostics: &mut Vec<Diagnostic>, checker: &RuleContext, node: &T);
}
```

Instead of implementing standalone functions, like `pub(crate) fn type_of_primitive(...)`, our violations can instead implement `Analyzer` on the relevant AST node kind.

Further, similar to Rome's `RegistryRule`, we have an internal-only struct to enable us to invoke the rule as a generic function pointer:

```rust
/// Internal representation of a single [`Rule`] that can be run on an AST node of type `T`.
pub(super) struct RegisteredRule<T> {
    rule: Rule,
    run: Executor<T>,
}
```

In the `Checker`, we can then create vectors of rules by node type:

```rust
...
call_rules: Vec<RegisteredRule<ast::ExprCall>>,
...
```

I'm opening this early to get feedback, especially from @MichaReiser who has seen this done in Rome.

A few misc. comments:

- We don't want to pass the full `&mut Checker` to `Analyzer` (in fact, we can't, since the rules are now stored on `Checker` and represent an immutable borrow). So, instead, we pass the vector of diagnostics (which the rules mutate), and we have a new `RuleContext` struct that represents all the immutable state that rules can access. I think separating the mutable from immutable `Checker` state is a good thing to do regardless of whether we pursue this design.
- This PR doesn't attempt to create a _single_ vector indexed by AST node type. We can try that, but the downside seems to be that you then need to have an unsafe cast within the rule executor? Like, you have `Vec<Vec<Rule>>`, and within `Rule`, you have to ensure that the node you're passed is of the right type, or panic? (In Rome: `<R::Query as Queryable>::unwrap_match(params.services, query_result);`.) The approach here _will_ require that we curate a separate vector for each node kind, but... that doesn't bother me much. There may be other considerations I'm overlooking here.
- This PR doesn't attempt to split the rule behavior into multiple methods. Rome has a method to generate the diagnostic, then the code action, then the suppression action. We could introduce these eventually, nothing in this design prevents them or makes it more difficult to do so -- I see that as orthogonal to this work, and compatible with the current design.
- Our `RegisteredRule` is mostly empty, whereas in Rome, there's a lot of generic logic in there to help with suppressions and other functionality. That could be useful eventually. We could probably get away with omitting `RegisteredRule` right now and just using function pointers, but I think it's worth introducing the struct.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-21 19:13_

---

_@charliermarsh reviewed on 2023-05-21 19:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/locals_in_render_function.rs`:47 on 2023-05-21 19:14_

(The changes in the rule files can mostly be ignored.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/locals_in_render_function.rs`:54 on 2023-05-21 19:14_

(I would suggest inlining this logic and removing functions like `locals_in_render_function` entirely.)

---

_@charliermarsh reviewed on 2023-05-21 19:14_

---

_@charliermarsh reviewed on 2023-05-21 19:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2675 on 2023-05-21 19:15_

(Only applying this change to the `pyupgrade` and `flake8-django` `ast::ExprCall` rules.)

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_django/rules/locals_in_render_function.rs`:66 on 2023-05-21 19:17_

We would / will need to change every rule to take the relevant AST node, rather than the individual fields-as-arguments, which is a good change IMO (and the thing that prohibited us from doing this earlier).

---

_@charliermarsh reviewed on 2023-05-21 19:17_

---

_@charliermarsh reviewed on 2023-05-21 19:18_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/traits.rs`:10 on 2023-05-21 19:18_

Including `Rule` as part of the trait lets us implement `enabled` below on `RegisteredRule`.

---

_@charliermarsh reviewed on 2023-05-21 19:19_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/traits.rs`:38 on 2023-05-21 19:19_

One unsolved problem is that we have some "lint rule functions" that add diagnostics for multiple rules in a single pass, like `rules/flake8_annotations/rules.rs`. These wouldn't _really_ fit into the paradigm of a "each rule gets invoked for each instance of its relevant AST node".

---

_Comment by @github-actions[bot] on 2023-05-21 19:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     15.3±0.57ms     2.7 MB/sec    1.00     15.1±0.58ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.7±0.30ms     4.5 MB/sec    1.00      3.6±0.14ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.01   457.3±27.09µs     6.5 MB/sec    1.00   454.5±26.13µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.29ms     4.0 MB/sec    1.00      6.3±0.30ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.2±0.28ms     5.6 MB/sec    1.00      7.2±0.36ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1549.5±74.88µs    10.7 MB/sec    1.00  1538.0±60.67µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   188.0±13.72µs    15.7 MB/sec    1.00   188.7±16.53µs    15.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.13ms     7.9 MB/sec    1.04      3.3±0.19ms     7.6 MB/sec
parser/large/dataset.py                    1.00      5.8±0.25ms     7.1 MB/sec    1.03      5.9±0.36ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00  1119.8±50.28µs    14.9 MB/sec    1.05  1176.2±87.78µs    14.2 MB/sec
parser/numpy/globals.py                    1.00    114.2±6.94µs    25.8 MB/sec    1.06   121.6±10.36µs    24.3 MB/sec
parser/pydantic/types.py                   1.00      2.5±0.14ms    10.1 MB/sec    1.06      2.7±0.20ms     9.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.6±0.33ms     2.4 MB/sec    1.00     16.7±0.32ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.07ms     3.9 MB/sec    1.00      4.2±0.04ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    508.7±7.98µs     5.8 MB/sec    1.00    504.3±6.85µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.10ms     3.7 MB/sec    1.01      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.12ms     4.9 MB/sec    1.01      8.4±0.12ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1754.5±26.51µs     9.5 MB/sec    1.00  1749.6±18.17µs     9.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    194.0±3.24µs    15.2 MB/sec    1.04    201.8±6.37µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.03      3.8±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.8±0.05ms     5.9 MB/sec    1.11      7.6±0.06ms     5.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1304.6±17.01µs    12.8 MB/sec    1.10  1436.8±23.25µs    11.6 MB/sec
parser/numpy/globals.py                    1.00    134.4±1.97µs    22.0 MB/sec    1.07    143.6±3.03µs    20.6 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.03ms     8.8 MB/sec    1.10      3.2±0.03ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-21 19:49_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/traits.rs`:38 on 2023-05-21 19:49_

`abstract_base_class` is a concrete example: it's a single function that adds violations for both `AbstractBaseClassWithoutAbstractMethod` and `EmptyMethodWithoutAbstractDecorator`, which have intertwined logic (they both rely on assessing whether a class has an abstract method).

We could break those out into separate rules and just accept that we're doing re-computation between them. Or we could make this trait one-to-many, such that a single Analyzer can produce violations for multiple rules.

---

_@MichaReiser reviewed on 2023-05-22 08:08_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/traits.rs`:13 on 2023-05-22 08:08_

Nit: I would change the argument order:

* `node` seems to be the most important, should come first 
* `checker` (we should rename this to `context`)
* `diagnostics` last

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/traits.rs`:14 on 2023-05-22 08:09_

We should use this PR as a chance to abstract away the `diagnostics` vec. Rules should only ever have to push a new diagnostic. They don't need to inspect the diagnostics in any way. 

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:91 on 2023-05-22 08:12_

Could we add the diagnostics to the `RuleContext`? It may require an additional lifetime but it has the added benefit that the `run` method requires fewer arguments.

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:86 on 2023-05-22 08:13_

Nit: Rename to `AnalyzerContext`

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:87 on 2023-05-22 08:13_

Nit: Should we hide the fields and instead expose accessor methods?

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/traits.rs`:1 on 2023-05-22 08:15_

Nit: I prefer to name my files after the concept they solve rather than what the things are that I put in there. Doing so helps with encapsulation (use the lowest visibility), eases moving things around, and tends to be easier discoverable (I don't know that something is a trait by just reading the type name: Is `RuleContext` a trait? I don't know)

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/traits.rs`:10 on 2023-05-22 08:18_

I don't mind this but I think I would favor renaming `Rule` to e.g. `RuleKind` and name this trait `Rule` instead (it's this trait that implements the logic). Ideally, we would move more metadata to this rule to avoid having `Violation`, `RuleKind` and `Rule`

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/traits.rs`:38 on 2023-05-22 08:21_

The way this is solved in Rome is that Rome differentiates between `Visitor`s (stateful) and `Rule`s (stateless). Rule's can depend on `Visitor`s by defining a custom `Queryable`. Rome then runs the `Visitor` first to extract the metadata and calls into the `Rule`s once the data is resolved (at least, that's how I remember it). 

I think it could be useful for Ruff to also support such a layered approach: 
* Simple: Covers the majority of rules
* Custom: More involved, but provides more flexibility and allows further optimisations. 

I think a similar problem are rules that depend on some aggregated state. 

---

_@MichaReiser reviewed on 2023-05-22 08:28_

I haven't reviewed it in full detail yet, but I overall like where this is going. 

How do you envision the relationship between `Violation` and the `Analyzer` trait. To me it seems that both define metadata about a rule. Could we unify them?

I also think that it makes sense to spend some time evaluating how we want to support rules that track state (could be used to implement rules that use shared data and only differ by the emitted diagnostic).


Ideally, we could use the new `Analyzer` trait for all kind of rules, including physical and logical lines.

---

_Comment by @charliermarsh on 2023-05-22 16:01_

> How do you envision the relationship between Violation and the Analyzer trait. To me it seems that both define metadata about a rule. Could we unify them?

> I also think that it makes sense to spend some time evaluating how we want to support rules that track state (could be used to implement rules that use shared data and only differ by the emitted diagnostic).

> Ideally, we could use the new Analyzer trait for all kind of rules, including physical and logical lines.

To me all of these are intertwined, and I need to spend a bit more time fleshing out the answers... But, e.g., if we were to proceed with the above design _just_ for AST-focused rules (for now), then I'd probably suggest removing `rule()` from `Analyzer`, and framing `Analyzer` as the rule _logic_ (a trait for diagnostics that run over AST nodes), whereas `Violation` is the struct capturing the result for a diagnostic of a specific kind (generic to the source of the diagnostic).

---

_Comment by @charliermarsh on 2023-06-12 18:45_

Closing for now, can always re-open when this gets picked back up.

---

_Closed by @charliermarsh on 2023-06-12 18:45_

---
