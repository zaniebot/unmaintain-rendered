```yaml
number: 12692
title: "[`ruff`] Mark `RUF023` fix as unsafe if `__slots__` is not a set and the binding is used elsewhere"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: alex/unsafe-ruf023
created_at: 2024-08-05T16:46:21Z
updated_at: 2024-08-07T09:52:37Z
url: https://github.com/astral-sh/ruff/pull/12692
synced_at: 2026-01-12T15:55:41Z
```

# [`ruff`] Mark `RUF023` fix as unsafe if `__slots__` is not a set and the binding is used elsewhere

---

_@AlexWaygood_

Fixes #12653. This required a little bit of refactoring: in order to know whether the `__slots__` binding is used or not, the rule must run after the AST tree has been visited in its entirety; that meant that the rule's hook had to be moved out of `statements.rs` and into `bindings.rs`. This, in turn, meant that the function implementing the rule had to accept a `Binding` rather than two AST nodes, and had to return an `Option<Diagnostic>` rather than `()`.

---

_Label `fixes` added by @AlexWaygood on 2024-08-05 16:46_

---

_Label `preview` added by @AlexWaygood on 2024-08-05 16:46_

---

_Comment by @github-actions[bot] on 2024-08-05 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +0 -6 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+0 -0 violations, +0 -6 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L189'>disnake/components.py:189:34:</a> RUF023 [*] `Button.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L189'>disnake/components.py:189:34:</a> RUF023 `Button.__slots__` is not sorted
- <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L269'>disnake/components.py:269:34:</a> RUF023 [*] `BaseSelectMenu.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L269'>disnake/components.py:269:34:</a> RUF023 `BaseSelectMenu.__slots__` is not sorted
- <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L646'>disnake/components.py:646:34:</a> RUF023 [*] `TextInput.__slots__` is not sorted
+ <a href='https://github.com/DisnakeDev/disnake/blob/9383d15ff4a5bb53087db5f7a0fc23370a020d7d/disnake/components.py#L646'>disnake/components.py:646:34:</a> RUF023 `TextInput.__slots__` is not sorted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF023 | 6 | 0 | 0 | 0 | 6 |

</p>
</details>




---

_Comment by @dscorbett on 2024-08-05 18:00_

I don’t think it is possible to know that the `__slots__` binding is not used, because it could be used outside the module, or even outside the whole project. [The documentation](https://docs.python.org/3/reference/datamodel.html#slots) doesn’t explicitly say whether this is supported or whether `__slots__` is meant to be treated as private, but it does say:

> If a [`dictionary`](https://docs.python.org/3/library/stdtypes.html#dict) is used to assign *\_\_slots\_\_*, the dictionary keys will be used as the slot names. The values of the dictionary can be used to provide per-attribute docstrings that will be recognised by [`inspect.getdoc()`](https://docs.python.org/3/library/inspect.html#inspect.getdoc) and displayed in the output of [`help()`](https://docs.python.org/3/library/functions.html#help).

which implies that `__slots__` is meant to be accessible by other modules (e.g. `inspect`) so it is not enough to check for usages within the same module.

---

_Comment by @AlexWaygood on 2024-08-05 18:08_

Yeah -- I agree with all that @dscorbett. And by the same logic, we could also argue that the fix for RUF022 should always be marked as unsafe.

I do however feel like it's pretty uncommon to iterate over the `__slots__` of a class from an external module. I agree that `__slots__` is in a grey area where it's not really private API -- but practically speaking, I think the vast majority of library users think of the presence of `__slots__` on a class as an optimisation/implementation detail, and the order of the items in `__slots__` even more so.

Overall I feel like this is a good compromise that reflects the fact that for arguably 99% of cases, this fix is safe if we can see that a class's `__slots__` are not read in the file in which that class is defined. 

> [The documentation](https://docs.python.org/3/reference/datamodel.html#slots) doesn’t explicitly say whether this is supported or whether `__slots__` is meant to be treated as private, but it does say:
> 
> > If a [`dictionary`](https://docs.python.org/3/library/stdtypes.html#dict) is used to assign ___slots___, the dictionary keys will be used as the slot names. The values of the dictionary can be used to provide per-attribute docstrings that will be recognised by [`inspect.getdoc()`](https://docs.python.org/3/library/inspect.html#inspect.getdoc) and displayed in the output of [`help()`](https://docs.python.org/3/library/functions.html#help).

FWIW, I wrote that bullet point in the documentation ;-)

---

_Comment by @AlexWaygood on 2024-08-05 18:13_

To be clear: you may be right. This isn't an opinion I'm certain in; possibly this rule (and RUF022 as well?) should just have their autofixes marked as unsafe in nearly all instances. I'm interested to hear what other maintainers think here. (@carljm, maybe?)

---

_Comment by @carljm on 2024-08-05 18:27_

I don't think it's _that_ hard to imagine a scenario where `__slots__` are iterated from outside the module, e.g. I can totally imagine some framework/ORM doing something like that (though I wouldn't recommend it.)

I also generally feel like we have unsafe fixes for a reason, and we should err on the side of marking fixes unsafe if there's a plausible scenario where they could break someone's code. ("Plausible" is of course doing a lot of heavy lifting there, and leaves a lot of room for debate.)

I would probably mark this fix as unsafe, unless `__slots__` are defined as a set. But it's quite possible I'm too conservative on this. We could also go with the approach in this PR and wait to see if anyone ever files an issue for this rule breaking their code due to introspection of `__slots__` from another module.

---

_Comment by @AlexWaygood on 2024-08-05 18:43_

> I also generally feel like we have unsafe fixes for a reason, and we should err on the side of marking fixes unsafe if there's a plausible scenario where they could break someone's code.

agreed -- but there is also a cost to marking a fix as unsafe. It means users don't get the autofix unless they either opt into _all_ unsafe fixes (the `--unsafe-fixes` flag), or they use the [`extend-safe-fixes` config option](https://docs.astral.sh/ruff/settings/#lint_extend-safe-fixes) (which just overrides the default to mark this fix as safe _all the time_). I sort-of feel like the autofix is much more useful than the actual lint in this case -- to me (as the rule's author!), it feels too pedantic to bother with if it's not going to be autofixed -- so I'm slightly reluctant to put the fix behind another flag, when I feel like most users who've opted into the rule will probably have chiefly done so for the autofix in the first place anyway.

---

_Comment by @dscorbett on 2024-08-05 20:38_

[The documentation for fix safety](https://docs.astral.sh/ruff/linter/#fix-safety) says “The meaning and intent of your code will be retained when applying safe fixes” but meaning and intent are hard to determine. Technically, no fix is 100% safe because a module’s source code can be inspected at runtime. That is an extreme case, of course; I’m just acknowledging that there’s always a trade-off.

In the particular example with `__match_args__`, the fix is not merely unsafe: the order of `__match_args__` is central to its purpose, so indirectly changing its order will almost certainly break something. In a case like that I would still report the rule violation but not offer any fix, because the rule is clearly violated but it’s not clear what to do about it.

Maybe for edge cases (i.e. the fixes which this PR still marks as safe) it would be sufficient to mark the fix as safe but document in the rule’s “Fix safety” section that the fix is actually unsafe in certain circumstances. I leave to you all to determine where to draw the line in this case and how complex to make the check, but I hope the unsafety will be taken into account somewhere, whether code or documentation.

---

_Comment by @AlexWaygood on 2024-08-05 21:03_

> Maybe for edge cases (i.e. the fixes which this PR still marks as safe) it would be sufficient to mark the fix as safe but document in the rule’s “Fix safety” section that the fix is actually unsafe in certain circumstances.

Great suggestion. I pushed some more docs on fix safety -- for this rule and for RUF022 -- in https://github.com/astral-sh/ruff/pull/12692/commits/77bf58087c522575ba6fd345e3f0d0b2726645c4.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:127 on 2024-08-07 06:59_

Nit nit: We may want to consider adding an `is_unused` method. It reads more naturally

---

_@MichaReiser approved on 2024-08-07 06:59_

---

_@AlexWaygood reviewed on 2024-08-07 09:40_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:127 on 2024-08-07 09:40_

Agreed -- but there's quite a few other places where we might consider using that method, so I'll propose it in a followup

---

_Merged by @AlexWaygood on 2024-08-07 09:41_

---

_Closed by @AlexWaygood on 2024-08-07 09:41_

---

_Branch deleted on 2024-08-07 09:41_

---

_@AlexWaygood reviewed on 2024-08-07 09:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:127 on 2024-08-07 09:52_

See https://github.com/astral-sh/ruff/pull/12729

---
