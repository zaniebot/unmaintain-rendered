```yaml
number: 20746
title: "[ty] Add an evaluation for completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: ag/completion-eval
created_at: 2025-10-07T15:11:08Z
updated_at: 2025-10-08T12:44:23Z
url: https://github.com/astral-sh/ruff/pull/20746
synced_at: 2026-01-10T17:34:34Z
```

# [ty] Add an evaluation for completions

---

_Pull request opened by @BurntSushi on 2025-10-07 15:11_

This is still early days, but I hope the framework introduced here makes
it very easy to add new truth data. Truth data should be seen as a form
of regression test for non-ideal ranking of completion suggestions.

I think it would help to read `crates/ty_completion_eval/README.md`
first to get an idea of what you're reviewing.


---

_Review requested from @carljm by @BurntSushi on 2025-10-07 15:11_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-10-07 15:11_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-10-07 15:11_

---

_Review requested from @sharkdp by @BurntSushi on 2025-10-07 15:11_

---

_Review requested from @dcreager by @BurntSushi on 2025-10-07 15:11_

---

_Review request for @dcreager removed by @BurntSushi on 2025-10-07 15:11_

---

_Review request for @carljm removed by @BurntSushi on 2025-10-07 15:11_

---

_Comment by @github-actions[bot] on 2025-10-07 15:13_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 15:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `ty` added by @AlexWaygood on 2025-10-07 15:16_

---

_Comment by @github-actions[bot] on 2025-10-07 15:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@sharkdp reviewed on 2025-10-07 16:58_

---

_Review comment by @sharkdp on `crates/ty_completion_eval/truth/type-var-typing-over-ast/main.py`:11 on 2025-10-07 16:58_

A slightly more principled way of doing this might be to get [a list of standard library modules](https://github.com/python/typeshed/blob/main/stdlib/VERSIONS), and then do some statistics across the ecosystem to see which modules are often imported from. We'd probably not want to break alphabetical order completely, but if some modules are used a lot more (orders of magnitude more), this might be an argument for putting them in an extra section on top of the other completions, or similar?

For example, comparing `ast` to `typing` across all mypy_primer projects, we can see that the latter is imported 370x more often:
```
▶ rg 'from (typing|ast) import' --only-matching --no-filename | sort | uniq -c
     76 from ast import
  28146 from typing import
```

---

_@AlexWaygood reviewed on 2025-10-07 17:00_

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/type-var-typing-over-ast/main.py`:11 on 2025-10-07 17:00_

> For example, comparing `ast` to `typing` across all mypy_primer projects, we can see that the latter is imported 370x more often:

although, mypy_primer _is_ somewhat intentionally biased towards projects with type annotations, so that we can see how well type checkers do on reasonably well-typed code 😄

though there are a lot of exceptions to that rule, to ensure well-rounded coverage for various edge cases

---

_@sharkdp reviewed on 2025-10-07 17:24_

---

_Review comment by @sharkdp on `crates/ty_completion_eval/truth/type-var-typing-over-ast/main.py`:11 on 2025-10-07 17:24_

Fair, but ty users will also work on sample of python projects that is biased towards more `typing` usage ;-)

---

_@AlexWaygood reviewed on 2025-10-07 17:48_

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/type-var-typing-over-ast/main.py`:11 on 2025-10-07 17:48_

well, maybe, but that's less true for autocompletion users than for our other users!

---

_@BurntSushi reviewed on 2025-10-07 18:02_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/type-var-typing-over-ast/main.py`:11 on 2025-10-07 18:02_

I think using some kind of background frequency distribution to influence ranking is a plausibly good idea. At least worth a try. It's hard to know how often it will produce sub-optimal results though. We could perhaps limit it to _just_ standard library modules to keep it restricted in scope. (And reading @sharkdp's comments again, I think this is perhaps what is meant.)

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:714 on 2025-10-07 19:14_

Should we limit this task to only run on ty changes
```suggestion
    if: ${{ needs.determine_changes.outputs.ty == 'true' || github.ref == 'refs/heads/main' }}
```

---

_Review comment by @MichaReiser on `crates/ty_completion_eval/src/main.rs`:138 on 2025-10-07 19:19_

Could we use the `tempdir` crate here, which works cross platform and cleans up once it's done

---

_@MichaReiser approved on 2025-10-07 19:23_

This is great. I didn't review the CLI code in detail. 

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/higher-level-symbols-preferred/uv.lock`:1 on 2025-10-07 20:56_

why is numpy listed in this uv.lock file? It doesn't seem to be listed as a dependency in `higher-level-symbols-preferred/pyproject.toml`.

Was this accidentally copied and pasted from the example that does include numpy as a dependency, by any chance? ;)

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py`:6 on 2025-10-07 20:58_

GitHub is adding a weird link here to a non-existent webpage :/

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py`:9 on 2025-10-07 21:01_

`typing.AwaitableGenerator` is a bit of a weird one because it doesn't actually exist at runtime -- and it's decorated with `@type_check_only`, so we should actually suppress it from autocompletions altogether (https://github.com/astral-sh/ty/issues/816).

`NoneType` might be quite a good one here? It exists in `_typeshed/__init__.pyi` and `types.pyi`, but (assuming you're using Python 3.10+), you'd never want it to be imported from the `_typeshed` module (which doesn't exist at runtime). You'd always want it imported from `types` (which does).

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/src/main.rs`:53 on 2025-10-07 21:04_

```suggestion
    /// The mean recriprocal rank threshold that the evaluation must
```

---

_@AlexWaygood approved on 2025-10-07 21:05_

Very cool! I also haven't reviewed the CLI code in depth

---

_Label `testing` added by @AlexWaygood on 2025-10-07 21:12_

---

_@sharkdp reviewed on 2025-10-08 07:02_

---

_Review comment by @sharkdp on `crates/ty_completion_eval/src/main.rs`:53 on 2025-10-08 07:02_

<img width="115" height="80" alt="image" src="https://github.com/user-attachments/assets/c03420bf-1423-48e8-aaaa-45b343775014" />

Almost, but not quite :smile: 

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/src/main.rs`:53 on 2025-10-08 07:14_

Dang

---

_@AlexWaygood reviewed on 2025-10-08 07:14_

---

_Review comment by @sharkdp on `crates/ty_completion_eval/src/main.rs`:210 on 2025-10-08 07:42_

The inner physicist in me always gets slightly nervous if statistical measures are presented to full numerical precision (*"mean reciprocal rank: 0.20409790112917506"*) :smile:. The actual precision of the MRR measure could be estimated by computing the standard deviation alongside the mean, but that obviously seems like overkill here. Given that individual reciprocal ranks can have values like 1/100 = 0.01, and given that we only have ~10 tasks, three or four decimal digits seem more than enough, so maybe
```suggestion
            writeln!(out, "mean reciprocal rank: {mrr:.4}")?;
```
(Due to the large spread in reciprocal ranks and the small number of tasks, the actual standard deviation is around 0.34 at the moment, higher than the MRR. That would mean that even one digit is enough)

---

_Review comment by @sharkdp on `crates/ty_completion_eval/src/main.rs`:204 on 2025-10-08 07:45_

This seems wrong to me? Shouldn't we be normalizing by the total number of tasks, instead of the number of source files? If I split a file with multiple tasks across several files, the MRR should stay the same?

---

_Review comment by @sharkdp on `crates/ty_completion_eval/src/main.rs`:215 on 2025-10-08 07:46_

If someone is not familiar with the MRR measure, this message could read like a failure, so maybe:
```suggestion
                writeln!(out, "Success: MRR exceeds threshold of {threshold}")?;
```
(and similarly, *"Failure: MRR does not exceed threshold …"* above)?

---

_@sharkdp approved on 2025-10-08 07:48_

Very nice! Since Micha and Alex didn't review the CLI tool, I did the opposite, and only reviewed the actual MRR computation.

---

_@BurntSushi reviewed on 2025-10-08 12:13_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/src/main.rs`:138 on 2025-10-08 12:13_

I did it this way intentionally to avoid clean-up so that `uv sync` doesn't have to do as much work on repeated runs. It's also easier to debug when something goes wrong. But maybe that is a premature optimization.

I've updated this to use `tempfile` and added a flag that lets you opt into keeping the temporary directory around after the eval is done.

---

_@BurntSushi reviewed on 2025-10-08 12:16_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/higher-level-symbols-preferred/uv.lock`:1 on 2025-10-08 12:16_

Oh nice catch. And yes, it was a copy-and-paste error!

But actually, this also revealed that I didn't have `uv.lock` files in each of the project directories and we probably should. So I fixed that as well.

---

_@BurntSushi reviewed on 2025-10-08 12:17_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py`:6 on 2025-10-08 12:17_

Where do you see the link rendered? Anyway, I tried to fix this.

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py`:9 on 2025-10-08 12:19_

Yes, perfect! Thank you for catching this.

---

_@BurntSushi reviewed on 2025-10-08 12:19_

---

_@BurntSushi reviewed on 2025-10-08 12:23_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/src/main.rs`:210 on 2025-10-08 12:23_

Thanks! I'll do `.4` for now. And yeah, definitely plan to add more tasks. Hopefully lots more.

The thing driving the higher standard deviation here is that completion requests (aside from auto-import) don't take what has been typed into account. Instead, that work is done by the client. I haven't yet decided whether non-auto-import completions should do this on the server side (in which case, the eval as it exists is correct), or if we should continue relying on the client (in which case, the eval should mimic the client).

---

_Review comment by @AlexWaygood on `crates/ty_completion_eval/truth/internal-typeshed-hidden/main.py`:6 on 2025-10-08 12:25_

Right here, on the PR view (and I think it would also be rendered if I was just browsing the file on GitHub):

---

<img width="1638" height="698" alt="image" src="https://github.com/user-attachments/assets/90f01507-0280-4491-a96d-c6cf0b5e2c93" />

---

if I click on that link, it takes me to a 404 page

---

_@AlexWaygood reviewed on 2025-10-08 12:25_

---

_@BurntSushi reviewed on 2025-10-08 12:26_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/src/main.rs`:210 on 2025-10-08 12:26_

Also, we have 16 evaluation tasks right now. Each truth directory can contain 1 or more tasks.

---

_@BurntSushi reviewed on 2025-10-08 12:26_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/src/main.rs`:204 on 2025-10-08 12:26_

Nice catch. My original eval had one task per source. But I refactored to permit each source project to be able to have multiple tasks. And of course, forgot to fix this.

---

_@BurntSushi reviewed on 2025-10-08 12:28_

---

_Review comment by @BurntSushi on `crates/ty_completion_eval/src/main.rs`:215 on 2025-10-08 12:28_

Yup that's fair. I also added "minimum threshold" to make it clear that we want to be above the set threshold.

---

_Merged by @BurntSushi on 2025-10-08 12:44_

---

_Closed by @BurntSushi on 2025-10-08 12:44_

---

_Branch deleted on 2025-10-08 12:44_

---
