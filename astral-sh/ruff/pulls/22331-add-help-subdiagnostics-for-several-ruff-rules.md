```yaml
number: 22331
title: "Add `help:` subdiagnostics for several Ruff rules that can sometimes appear to disagree with ty"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: alex/override-diags
created_at: 2026-01-01T17:29:03Z
updated_at: 2026-01-02T22:10:41Z
url: https://github.com/astral-sh/ruff/pull/22331
synced_at: 2026-01-12T15:57:47Z
```

# Add `help:` subdiagnostics for several Ruff rules that can sometimes appear to disagree with ty

---

_@AlexWaygood_

## Summary

Ruff has a bunch of rules that can sometimes appear to disagree with ty. For example, this snippet of code will trigger an `FBT001` violation:

```py
# third-party file somewhere...

class Foo:
    def f(self, x: bool): ...

# first-party code...

class Bar(Foo):
    def f(self, x: bool): ...
```

But the author of this code can't change the definition of `Foo.f` (because it's third-party code), and they can't change `Bar.f` or ty will complain about a violation of the Liskov Substitution Principle.

We've already added documentation notes to a bunch of these rules in https://github.com/astral-sh/ruff/pull/21644 that tell people to use `typing.override` in this situation. Ruff will generally ignore this kind of rule if it sees that the method is decorated with `@override`, because it knows that a user can't easily change an `@override`-decorated rule without violating the Liskov Substitution Principle. However, we've still received [some reports from users](https://discord.com/channels/1039017663004942429/1279201882337705994/1451714410321285295) who are understandably confused about two Astral tools seeming to give contradictory orders about how the user should rewrite their code.

This PR adds `help:` subdiagnostics to several Ruff rules. The subdiagnostics also point the user towards `typing.override`; having the hint directly presented in the terminal should be more approachable and harder to miss.

## Test Plan

Snapshots updated.


---

_Label `diagnostics` added by @AlexWaygood on 2026-01-01 17:29_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 17:41_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @AlexWaygood on 2026-01-01 17:42_

(There are a couple more rules that can cause issues with a type checker's Liskov enforcement. I basically just added `help:` subdiagnostics to the ones where it was easy to do so.)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:142 on 2026-01-01 18:20_

I kind of prefer this phrasing over the LSP version, but I don't feel strongly. I'm guessing we can't call the helper function here because the current scope isn't set?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/too_many_arguments.rs`:132 on 2026-01-01 18:23_

Can we use the helper here? Oh, I see it's an FBT helper.

---

_@ntBre approved on 2026-01-01 18:26_

Makes sense to me, thanks!

---

_@AlexWaygood reviewed on 2026-01-01 18:46_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/too_many_arguments.rs`:132 on 2026-01-01 18:46_

Right — I could put the helper somewhere more central, but wasn't totally sure where a good place would be :/

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_function_name.rs`:142 on 2026-01-02 22:06_

> I'm guessing we can't call the helper function here because the current scope isn't set?

That wasn't really my rationale for doing something slightly different here. The reasons why I did something slightly different here is that this violation is telling you to rename the function rather than change its signature, and renaming the function isn't going to cause a Liskov violation. It's just not necessarily what you _want_ to do if the function is deliberately overriding something from a subclass 😄

---

_@AlexWaygood reviewed on 2026-01-02 22:06_

---

_Merged by @AlexWaygood on 2026-01-02 22:10_

---

_Closed by @AlexWaygood on 2026-01-02 22:10_

---

_Branch deleted on 2026-01-02 22:10_

---
