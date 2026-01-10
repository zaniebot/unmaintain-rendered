```yaml
number: 21799
title: "[ty] Add modules to auto-import completions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/auto-import-includes-modules
created_at: 2025-12-04T20:13:25Z
updated_at: 2025-12-08T13:07:01Z
url: https://github.com/astral-sh/ruff/pull/21799
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Add modules to auto-import completions

---

_Pull request opened by @BurntSushi on 2025-12-04 20:13_

Basically, when one writes `rando<CURSOR>`, ty should
suggest `import random` as a completion. And similarly,
`random.randint(0, 1)` should provide a fix to import
`random` when it isn't already in scope. Previously,
we didn't include modules in our `all_symbols`
implementation. This PR adds support for that.

Note that this includes matching submodules in completions
returned. This doesn't match what we do elsewhere, but it was
very easy to do. I think it'd be nice to get experience with it
and see about doing it in `import` and `from` as well. It is very
easy to back this out and only include top-level modules though.

Reviewers are encouraged to read commit by commit.

Closes https://github.com/astral-sh/ty/issues/1530, Closes https://github.com/astral-sh/ty/issues/1753


---

_Review requested from @carljm by @BurntSushi on 2025-12-04 20:13_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-04 20:13_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-04 20:13_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-04 20:13_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-04 20:13_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-04 20:13_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-04 20:13_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-12-04 20:13_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-04 20:13_

---

_Review requested from @Gankra by @BurntSushi on 2025-12-04 20:13_

---

_Comment by @BurntSushi on 2025-12-04 20:14_

Demo for auto-import completions and code action:

https://github.com/user-attachments/assets/1e8da56c-d705-43d4-b793-bdfe826a4fba

Demo for including submodules:

https://github.com/user-attachments/assets/f6e6561e-bff2-478d-9a73-d09c0ed5d863



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 20:15_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 20:17_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 39 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @AlexWaygood on 2025-12-04 20:20_

---

_Label `ty` added by @AlexWaygood on 2025-12-04 20:20_

---

_Review comment by @Gankra on `crates/ty_ide/src/importer.rs`:495 on 2025-12-04 20:55_

```suggestion
                // then it can never be satisfied by a `from ... import ...`
```

Also: this statement isn't true in several senses, most notably `from collections import abc`. I assume you mean something more nuanced.

---

_@Gankra approved on 2025-12-04 20:55_

Nice! Makes sense to me.

---

_@BurntSushi reviewed on 2025-12-04 21:41_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/importer.rs`:495 on 2025-12-04 21:41_

I was thinking about `from` not being able to do `import collections.abc` in order to make `collections.abc` available. We _could_ reuse a `from collections import abc` import and rewrite the text under the cursor. But this PR doesn't go that far.

Anywho, I expanded on this comment:

```rust
                // If the request is for a module itself, then we
                // assume that it can never be satisfies by a
                // `from ... import ...` statement. For example, a
                // `request for collections.abc` needs an
                // `import collections.abc`. Now, there could be a
                // `from collections import abc`, and we could
                // plausibly consider that a match and return a
                // symbol text of `abc`. But it's not clear if that's
                // the right choice or not.
                let member = request.member?;
```

And added a test covering your example (where we today add a `import collections.abc`).

---

_Comment by @BurntSushi on 2025-12-04 22:37_

I'm happy to do follow-up PRs for subsequent reviews!

---

_Merged by @BurntSushi on 2025-12-04 22:37_

---

_Closed by @BurntSushi on 2025-12-04 22:37_

---

_Branch deleted on 2025-12-04 22:37_

---

_Comment by @AlexWaygood on 2025-12-05 14:21_

> Note that this includes matching submodules in completions
> returned. This doesn't match what we do elsewhere, but it was
> very easy to do. I think it'd be nice to get experience with it
> and see about doing it in `import` and `from` as well. It is very
> easy to back this out and only include top-level modules though.

Oh, that's what I suggested in https://github.com/astral-sh/ty/issues/1206, right?

---

_@AlexWaygood reviewed on 2025-12-05 15:17_

Really cool, thank you! Just one followup thing I thought of when playing around with it: https://github.com/astral-sh/ty/issues/1773

---

_Comment by @BurntSushi on 2025-12-08 13:07_

> > Note that this includes matching submodules in completions
> > returned. This doesn't match what we do elsewhere, but it was
> > very easy to do. I think it'd be nice to get experience with it
> > and see about doing it in `import` and `from` as well. It is very
> > easy to back this out and only include top-level modules though.
> 
> Oh, that's what I suggested in [astral-sh/ty#1206](https://github.com/astral-sh/ty/issues/1206), right?

Yup!

---
