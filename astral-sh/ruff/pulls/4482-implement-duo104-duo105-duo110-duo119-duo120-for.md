```yaml
number: 4482
title: Implement DUO104, DUO105, DUO110, DUO119, DUO120 for Dlint plugin
type: pull_request
state: closed
author: qdegraaf
labels: []
assignees: []
base: main
head: feature/add-dlint
created_at: 2023-05-17T22:02:57Z
updated_at: 2023-05-24T07:56:25Z
url: https://github.com/astral-sh/ruff/pull/4482
synced_at: 2026-01-12T15:55:15Z
```

# Implement DUO104, DUO105, DUO110, DUO119, DUO120 for Dlint plugin

---

_@qdegraaf_

Rules are listed below. Some still need to be checked for relevance, already filtered out some Python 2 modules. Putting them here to keep track of work while availability is scattered.

EDIT: As discussed with @MichaReiser it's probably best to split this up in multiple PRs for ease of collaboration, review and speedy addition of improvements. This PR will add the first 5 rules as marked below. I will move the tasklist to the issue once merged

EDIT 2: Moved task list to issue

- [x] DUO104: EvalUse
- [x] DUO105: ExecUse
- [x] DUO110: CompileUse
- [x] DUO119: ShelveUse
- [x] DUO120: MarshalUse

Relates to: https://github.com/charliermarsh/ruff/issues/4479

---

_Converted to draft by @qdegraaf on 2023-05-17 22:03_

---

_Comment by @github-actions[bot] on 2023-05-17 22:37_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+9, -0, 0 error(s))

<details><summary>airflow (+3, -0)</summary>
<p>

```diff
+ airflow/policies.py:174:16: DUO110 Use of the `compile` command should be avoided
+ airflow/policies.py:176:9: DUO105 Use of the `exec` command should be avoided
+ dev/stats/get_important_pr_candidates.py:148:27: DUO104 Use of the `eval` command should be avoided
```

</p>
</details>
<details><summary>bokeh (+4, -0)</summary>
<p>

```diff
+ src/bokeh/application/handlers/code_runner.py:105:26: DUO110 Use of the `compile` command should be avoided
+ src/bokeh/application/handlers/code_runner.py:229:13: DUO105 Use of the `exec` command should be avoided
+ src/bokeh/sphinxext/bokeh_palette.py:107:9: DUO105 Use of the `exec` command should be avoided
+ src/bokeh/sphinxext/bokeh_palette.py:134:1: DUO105 Use of the `exec` command should be avoided
```

</p>
</details>
<details><summary>zulip (+2, -0)</summary>
<p>

```diff
+ scripts/lib/setup_path.py:15:13: DUO105 Use of the `exec` command should be avoided
+ tools/lib/provision.py:457:9: DUO105 Use of the `exec` command should be avoided
```

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| DUO105 | 6 | 6 | 0 |
| DUO110 | 2 | 2 | 0 |
| DUO104 | 1 | 1 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.2±0.03ms     2.9 MB/sec    1.00     14.0±0.02ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    423.7±0.73µs     7.0 MB/sec    1.00    419.2±1.00µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.01      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.8±0.01ms     6.0 MB/sec    1.00      6.7±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1471.8±3.90µs    11.3 MB/sec    1.00   1460.2±5.33µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.01    164.7±0.81µs    17.9 MB/sec    1.00    162.6±0.89µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.1±0.01ms     8.3 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
parser/large/dataset.py                    1.01      5.4±0.00ms     7.5 MB/sec    1.00      5.4±0.00ms     7.6 MB/sec
parser/numpy/ctypeslib.py                  1.00   1061.3±0.62µs    15.7 MB/sec    1.00   1059.6±1.03µs    15.7 MB/sec
parser/numpy/globals.py                    1.00    108.8±1.20µs    27.1 MB/sec    1.00    108.7±0.24µs    27.1 MB/sec
parser/pydantic/types.py                   1.00      2.3±0.00ms    11.1 MB/sec    1.00      2.3±0.00ms    11.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.3±0.24ms     2.9 MB/sec    1.00     14.2±0.21ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.09ms     4.7 MB/sec    1.00      3.5±0.04ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    355.8±4.74µs     8.3 MB/sec    1.03    365.2±7.01µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.11ms     4.3 MB/sec    1.00      5.9±0.14ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      7.1±0.10ms     5.7 MB/sec    1.01      7.2±0.10ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1435.5±21.85µs    11.6 MB/sec    1.01  1448.6±47.92µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    154.4±2.09µs    19.1 MB/sec    1.02    158.0±3.75µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.00      3.1±0.04ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.9±0.10ms     6.9 MB/sec    1.00      5.9±0.05ms     6.9 MB/sec
parser/numpy/ctypeslib.py                  1.00   1101.9±9.51µs    15.1 MB/sec    1.01   1110.3±7.63µs    15.0 MB/sec
parser/numpy/globals.py                    1.00    111.2±1.34µs    26.5 MB/sec    1.02    113.4±1.63µs    26.0 MB/sec
parser/pydantic/types.py                   1.00      2.4±0.02ms    10.5 MB/sec    1.02      2.5±0.02ms    10.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_shelve_use.rs`:40 on 2023-05-19 07:12_

You can now define your own enum to avoid having to use `panic`

```rust
#[derive(Debug, Copy, Clone)]
enum AnyStmtImport<'a> {
	Import(&'a ast::StmtImport),
	ImportFrom(&'a ast::StmtImportFrom)
}

impl AnyStmtImport<'_> {
	pub fn cast<'a>(stmt: &'a Stmt) -> Option<Self<'a>> {
		match stmt {
			Stmt::Import(import) => Some(Self::Import(import)),
			Stmt::ImportFrom(import_from) => Some(Self::ImportFrom(import_from)),
			_ => None,
		}
	}
}

impl<'a From<&'a ast::StmtImport> for AnyStmtImport<'a> {
	pub fn from(value: &'a ast::StmtImport) -> Self { Self::Import(value) }
}

impl<'a From<&'a ast::StmtImportFrom> for AnyStmtImport<'a> {
	pub fn from(value: &'a ast::StmtImportFrom) -> Self { Self::ImportFrom(value) }
}

pub(crate) fn bad_shelve_use(checker: &mut Checker, import: AnyStmtImport) {
```



---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:6 on 2023-05-19 07:13_

Nit: Can we only highlight the module name?

---

_@MichaReiser reviewed on 2023-05-19 07:13_

---

_@MichaReiser reviewed on 2023-05-19 07:14_

I think it would be good if we have a separate PR for every rule. It eases reviewing and ensures that we merge improvements quickly

---

_Comment by @qdegraaf on 2023-05-19 08:18_

> I think it would be good if we have a separate PR for every rule. It eases reviewing and ensures that we merge improvements quickly

Ah that's fair. Was unsure whether that was preferred over doing as much as possible in one batch. I'll polish up what's here and process your comments then move the task list to the issue and this PR can be reviewed/merged

---

_Renamed from "[WIP] Add Dlint plugin" to "[WIP] Implement DUO104, DUO105, DUO110, DUO119, DUO120 for Dlint plugin" by @qdegraaf on 2023-05-19 08:20_

---

_@qdegraaf reviewed on 2023-05-19 09:46_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_shelve_use.rs`:40 on 2023-05-19 09:46_

Thanks, gave it a go, let me know if I have implemented this correctly. Ran into some lifetime issues and did some trial-and-erroring to get it all to compile. Still fairly new to rust so happy for any feedback on implementation.

---

_Marked ready for review by @qdegraaf on 2023-05-19 09:47_

---

_Renamed from "[WIP] Implement DUO104, DUO105, DUO110, DUO119, DUO120 for Dlint plugin" to "Implement DUO104, DUO105, DUO110, DUO119, DUO120 for Dlint plugin" by @qdegraaf on 2023-05-19 09:47_

---

_@MichaReiser reviewed on 2023-05-19 10:45_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:22 on 2023-05-19 10:45_

Nit: It could make sense to first test if the function called by `func` matches the name "compile" to avoid the somewhat expensive `resolve_call_path` call.

Do I understand correctly that the rule is about the built-in `compile` function? If so, you can use checker.ctx.is_builtin(name)` to check if the name is not a user-defined function.

---

_@MichaReiser reviewed on 2023-05-19 10:48_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1069 on 2023-05-19 10:48_

Nit: You already now here that you're in a `ImportFrom` so you can write

```
Stmt::ImportFrom(import_from @ ast::StmtImportFrom {
                names,
                module,
                level,
                range: _,
}) => {
	...
	
	if self.settings.rules.enabled(Rule::BadShelveUse) {
         dlint::rules::bad_shelve_use(self, AnyStmtImport::from(stmt));
	}
}
```

---

_@qdegraaf reviewed on 2023-05-19 16:10_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:1069 on 2023-05-19 16:10_

It does not seem to like this. When I try for example:

```
Stmt::Import(ast::StmtImport { names, range: _ }) => {
...
  //dlint
  if self.settings.rules.enabled(Rule::BadShelveUse) {
         dlint::rules::bad_shelve_use(self, dlint::helpers::AnyStmtImport::from(stmt));
  }
  if self.settings.rules.enabled(Rule::BadMarshalUse) {
         dlint::rules::bad_marshal_use(self, dlint::helpers::AnyStmtImport::from(stmt));
  }
```
Compiler complains:

```
error[E0277]: the trait bound `AnyStmtImport<'_>: std::convert::From<&rustpython_parser::rustpython_ast::Stmt>` is not satisfied
   --> crates/ruff/src/checkers/ast/mod.rs:776:76
    |
776 |                     dlint::rules::bad_shelve_use(self, AnyStmtImport::from(stmt));
    |                                                        ------------------- ^^^^ the trait `std::convert::From<&rustpython_parser::rustpython_ast::Stmt>` is not implemented for `AnyStmtImport<'_>`
    |                                                        |
    |                                                        required by a bound introduced by this call
    |
    = help: the following other types implement trait `std::convert::From<T>`:
              <AnyStmtImport<'a> as std::convert::From<&'a StmtImport>>
              <AnyStmtImport<'a> as std::convert::From<&'a StmtImportFrom>>
```

It seems to want a convert implementation from all of `rustpython_ast::Stmt` even though it's only called in cases of `Stmt` being either `StmtImport` or `StmtImportFrom`. Am I missing anything? 

---

_@qdegraaf reviewed on 2023-05-19 16:11_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:22 on 2023-05-19 16:11_

It is, so I'll use `is_builtin` method. But good to know, I'll open a PR soon with a micro optimization for my previous implementations of similar funcs and keep it in mind for future changes!

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:1069 on 2023-05-19 16:24_

Dang, I messed up the code. I'm sorry. 

You have to bind the `StmtImportFrom` in the `match` arm with `import_from @ ast::StmtImportFrom { ... }` and then pass that to the from call:

```rust
Stmt::ImportFrom(import_from @ ast::StmtImportFrom {
                names,
                module,
                level,
                range: _,
}) => {
	...
	
	if self.settings.rules.enabled(Rule::BadShelveUse) {
         dlint::rules::bad_shelve_use(self, AnyStmtImport::from(import_from));
	}
}
```

---

_@MichaReiser reviewed on 2023-05-19 16:24_

---

_@qdegraaf reviewed on 2023-05-19 20:30_

---

_Review comment by @qdegraaf on `crates/ruff/src/checkers/ast/mod.rs`:1069 on 2023-05-19 20:30_

Ah funny I wrote that during trial-and-erroring and my IDE's Rust plugin complained about `use of a moved value` so I didn't build it, but it does seem to work despite those complaints 🤷. Thanks for the feedback and pushed 

---

_Comment by @qdegraaf on 2023-05-19 21:36_

Docstrings added and comments processed. Will wait until this is merged or gets further comments before moving rules to issue and picking up in separate PRs

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 07:41_

I think we should rename the rule to simply `compile`, `eval` etc. because the bad seems redundant, all lint rules capture something "bad". This ensures that the rule reads nicely in the `allow compile` or `deny compile` context

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:24 on 2023-05-22 07:42_

This example (and also for other rules) isn't semantically equivalent. I think we should either remove it or have a meaningful alternative.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_exec_use.rs`:14 on 2023-05-22 07:43_

It may be good to add some explanation about what `exec` does. The real problem here is if the string passed comes from an untrusted source (or contains parts from an untrusted source)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:32 on 2023-05-22 07:45_

Nit: We could change the method signature to only except `call_expressions` to avoid this additional check

```suggestion
pub(crate) fn bad_compile_use(checker: &mut Checker, expr: &ast::ExprCall) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_eval_use.rs`:31 on 2023-05-22 07:45_

```suggestion
pub(crate) fn bad_eval_use(checker: &mut Checker, expr: &ast::ExprCall) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/rules/bad_exec_use.rs`:31 on 2023-05-22 07:46_

```suggestion
pub(crate) fn bad_exec_use(checker: &mut Checker, expr: &ast::ExprCall) {
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:12 on 2023-05-22 07:47_

The range here looks wrong. Are we now highlighting the import name rather than the module name?

---

_@MichaReiser reviewed on 2023-05-22 07:47_

@charliermarsh could you share your view on the phrasing and naming of the diagnostics. 

---

_@qdegraaf reviewed on 2023-05-22 08:36_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 08:36_

Names are straight copies from DLint. Happy to deviate from those if alright for straight plugin ports.

---

_@qdegraaf reviewed on 2023-05-22 08:49_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:12 on 2023-05-22 08:49_

Ah my bad. Misunderstood what you meant by https://github.com/charliermarsh/ruff/pull/4482#discussion_r1198625178 . Will fix

---

_@qdegraaf reviewed on 2023-05-22 08:53_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 08:53_

Only problem is there is already an Eval rule in `rules::pygrep_hooks::rules::Eval`. For consistency's sake: `EvalUse`, `ExecUse`, etc.?


---

_@qdegraaf reviewed on 2023-05-22 09:42_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:12 on 2023-05-22 09:42_

Is there a way to locate that module name efficiently from the `StmtImportFrom`? If I print the AST of for example DUO119 I get (among other stuff):

```
StmtImportFrom {
            range: 16..46,
            module: Some(
                Identifier(
                    "shelve",
                ),
            ),
            names: [
                Alias {
                    range: 35..39,
                    name: Identifier(
                        "open",
                    ),
                    asname: None,
                },
                Alias {
                    range: 41..46,
                    name: Identifier(
                        "Shelf",
                    ),
                    asname: None,
                },
            ],
            level: Some(
                Int(
                    0,
                ),
            ),
        },
```

Which gives me easy access to the individual import ranges, as well as the whole import statement range, but not the module. 

---

_@MichaReiser reviewed on 2023-05-22 10:31_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:12 on 2023-05-22 10:31_

Hmm, I forgot about that. No, I don't think there is. At least, not before we include range information for `Identifier` too (by making it a proper `Node`. 

We have a specific helper for classes and functions to extract the identifier name. We could introduce a similar helper for `StmtImportFrom` or we go back to highlighting the whole statement, since we can't really do better now

https://github.com/charliermarsh/ruff/blob/45fc49773753388984f65dfa0f54bc71cf779a41/crates/ruff_python_ast/src/helpers.rs#L1092-L1108

---

_@qdegraaf reviewed on 2023-05-22 11:34_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/snapshots/ruff__rules__dlint__tests__DUO119_DUO119.py.snap`:12 on 2023-05-22 11:34_

Addded a helper using almost identical logic and it seems to do the trick! Could be folded into `identifier_range` due to almost identical logic but was unsure whether to keep separate for functional scope (i.e `ClassDef`, `FunctionDef` and `AsyncFunctionDef` are a sensible grouping. Adding a `StmtImportFrom` less so). Left it separate for now 

---

_@charliermarsh reviewed on 2023-05-22 13:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 13:33_

I agree with Micha on the rule renaming.

Though think the biggest question with this PR is that most of these rules are already implemented under different names and categorizations: `rules::pygrep_hooks::rules::Eval`, `rules::flake8_bandit::rules::ExecBuiltin`, and then the various rules in `rules/flake8_bandit/rules/suspicious_function_call.rs` which cover `marshal` and `shelve`. I don't think we have a rule for `compile`, but otherwise, these are already mostly covered, so I'm hesitant to add duplicate implementations of the same violation enforcement.


---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 15:00_

Sounds good, should probably have done that before diving in. Have asked requester if they already have one or more rules they use from DLint not covered by other plugins in Ruff. Don't think I have time to cross reference the issue lists for the next few days.

---

_@qdegraaf reviewed on 2023-05-22 15:00_

---

_@charliermarsh reviewed on 2023-05-22 15:05_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 15:05_

I wish I'd noticed it and called it out earlier -- sorry to only do that now. It's hard for me to keep up with and proactively vet issues like that before they make it to PR.

---

_@qdegraaf reviewed on 2023-05-22 15:42_

---

_Review comment by @qdegraaf on `crates/ruff/src/rules/dlint/rules/bad_compile_use.rs`:26 on 2023-05-22 15:42_

No worries. It's a busy repo, I learned a lot from @MichaReiser 's very thorough review (thanks for that!) about Rust and Ruff which I can bring with me to future edits even if this never gets to main. 

---

_Comment by @charliermarsh on 2023-05-24 02:16_

I'm going to tentatively close for now given the discussion in the comments -- sorry for the mixup, but I'd love to find ways to keep you contributing if you're open to it, I appreciate your work :)

---

_Closed by @charliermarsh on 2023-05-24 02:16_

---

_Comment by @qdegraaf on 2023-05-24 07:56_

> I'm going to tentatively close for now given the discussion in the comments -- sorry for the mixup, but I'd love to find ways to keep you contributing if you're open to it, I appreciate your work :)

That's cool! And no worries again. A bit busy with the dayjob this week but I'll see if I can do some more autofixes to get comfortable with that and then maybe move on to something a little more complex (also to leave some of the "good first issues" to others interested in contributing and looking for an easy way in). 

---
