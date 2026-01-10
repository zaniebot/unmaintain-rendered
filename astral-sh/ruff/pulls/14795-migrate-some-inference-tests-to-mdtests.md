```yaml
number: 14795
title: Migrate some inference tests to mdtests
type: pull_request
state: merged
author: dcreager
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: dcreager/infer-mdtests
created_at: 2024-12-05T19:50:56Z
updated_at: 2024-12-06T14:42:00Z
url: https://github.com/astral-sh/ruff/pull/14795
synced_at: 2026-01-10T20:42:27Z
```

# Migrate some inference tests to mdtests

---

_Pull request opened by @dcreager on 2024-12-05 19:50_

As part of #13696, this PR ports a smallish number of inference tests over to the mdtest framework.



---

_Label `red-knot` added by @dcreager on 2024-12-05 19:50_

---

_Review requested from @carljm by @dcreager on 2024-12-05 19:50_

---

_Review requested from @MichaReiser by @dcreager on 2024-12-05 19:50_

---

_Review requested from @AlexWaygood by @dcreager on 2024-12-05 19:50_

---

_Review requested from @sharkdp by @dcreager on 2024-12-05 19:50_

---

_Label `testing` added by @AlexWaygood on 2024-12-05 19:55_

---

_Comment by @AlexWaygood on 2024-12-05 19:57_

You may need to run `uvx pre-commit run --all-files` locally to placate our linters :-)

(Requires you to have `uv` installed locally, but nothing else)

---

_Comment by @github-actions[bot] on 2024-12-05 19:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-12-05 20:12_

Oh, and mdtests that contain invalid syntax need to be in files with `invalid_suntax` in their filenames, to exclude them from linters that only cope with valid Python syntax

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/nonlocal.md`:10 on 2024-12-05 20:37_

This pattern of "assign x to y, then check the type of y" was really an artifact of some limitations of the prior testing setup, here we can just elide it (and similar in below cases in this file):
```suggestion
        reveal_type(x)  # revealed: Literal[1]
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:71 on 2024-12-05 21:33_

The first and third lines here probably could be collapsed into a single `reveal_type(x)` at the top of the function body, with two diagnostics on that one line (the unresolved-reference error and the revealed Unknown type.) But in this case I think it's clearer to keep them separate.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/nonlocal.md`:1 on 2024-12-05 21:35_

I would put this file in `scopes/nonlocal.md` rather than `assignment/nonlocal.md`. These tests are about the semantics of nonlocal name lookups, the assignments are incidental (and, as I mention below, could just be removed.)

---

_Comment by @dcreager on 2024-12-05 22:13_

> You may need to run `uvx pre-commit --all-files` locally to placate our linters
>
> Oh, and mdtests that contain invalid syntax need to be in files with `invalid_suntax` in their filenames, to exclude them from linters that only cope with valid Python syntax

Thanks!  Both fixed

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:62 on 2024-12-05 22:16_

This test also feels more suitable to the `scopes/` directory than the `assignment/` directory. But it looks like that's also true of the previous two existing tests. I'd probably move them all to `scopes/unbound.md`, but it's also fine if we don't do that in this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/errors.md`:59 on 2024-12-05 22:18_

As @AlexWaygood mentioned, we'll probably need to put this in its own file with `invalid_syntax` in the name, e.g. `import/invalid_syntax.md`

---

_@carljm approved on 2024-12-05 22:19_

Looks great, thank you! Feel free to go ahead and merge without another review after addressing comments/lints.

---

_@AlexWaygood reviewed on 2024-12-05 22:23_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/errors.md`:63 on 2024-12-05 22:23_

Style micro-nit: we generally use `...` rather than `pass` in empty class bodies in mdtests, because blacken-docs (an autoformatter we run on our mdtests in CI via pre-commit) formats the classes more concisely if you do that

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/errors.md`:63 on 2024-12-05 22:29_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/assignment/nonlocal.md`:1 on 2024-12-05 22:32_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/assignment/nonlocal.md`:10 on 2024-12-05 22:32_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:62 on 2024-12-05 22:34_

Done, moved the previous two tests into `scopes/unbound.md` as well.  (To clarify, that means I left the first test here, which does seem to concern the semantics of _assigning_ a currently unbound value)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/assignment/unbound.md`:71 on 2024-12-05 22:38_

I went ahead and collapsed them — I think that produces a nice symmetry where there are different results for the same expression before and after the assignment:

``` python
    # error: [unresolved-reference]
    # revealed: Unknown
    reveal_type(x)
    x = 2
    # revealed: Literal[2]
    reveal_type(x)
```

---

_@dcreager reviewed on 2024-12-05 22:39_

---

_@AlexWaygood reviewed on 2024-12-05 22:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/scopes/nonlocal.md`:40 on 2024-12-05 22:43_

Should this be a TODO?

```suggestion
        # TODO: it's pretty weird to have an annotated assignment in a function where the
```

---

_@carljm reviewed on 2024-12-05 22:45_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/scopes/nonlocal.md`:40 on 2024-12-05 22:45_

FWIW, I intentionally didn't make it a TODO when I added it in the old test. It's in a gray area, but to me we should reserve TODO for things we definitely need to change, or at least revisit. I don't feel like it's clear that we need to change or revisit anything here. It's more like a musing that may be relevant to someone in the future considering this case.

---

_@AlexWaygood approved on 2024-12-05 23:23_

Thank you!!

---

_@sharkdp reviewed on 2024-12-06 10:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/literal/ellipsis.md`:7 on 2024-12-06 10:18_

The TODO here got lost, but it's fine; I'll address this in the statically-known branches PR.

---

_Merged by @sharkdp on 2024-12-06 10:19_

---

_Closed by @sharkdp on 2024-12-06 10:19_

---

_Branch deleted on 2024-12-06 10:19_

---

_Comment by @sharkdp on 2024-12-06 10:20_

Thanks! I hope it's fine I merged this. I can use it to write the `Ellipsis`-test in a nicer way in my PR: https://github.com/astral-sh/ruff/blob/5e1fdd3eecededdae22202dc98646d201d9e9303/crates/red_knot_python_semantic/resources/mdtest/literal/ellipsis.md

---

_Comment by @dcreager on 2024-12-06 14:41_

> Thanks! I hope it's fine I merged this.

:+1: Not a problem, thanks for merging!

---
