```yaml
number: 10949
title: "Suggested lint rule: disallow implicit-cast-to-bool"
type: issue
state: open
author: patrick-kidger
labels:
  - rule
assignees: []
created_at: 2024-04-15T07:55:43Z
updated_at: 2025-02-16T04:27:04Z
url: https://github.com/astral-sh/ruff/issues/10949
synced_at: 2026-01-12T15:54:50Z
```

# Suggested lint rule: disallow implicit-cast-to-bool

---

_@patrick-kidger_

I'm thinking of writing a new lint rule for Ruff, to disallow implicit-cast-to-bool.

That is, to disallow statements like `foo = None; if foo:`. Our team finds that these are a common source of bugs. For example if `foo: Optional[int]` then was the intention for `if foo:` to not trigger on just `foo = None`, or did the author want it to not trigger on `foo = 0` as well?

As per the contributing guidelines I'm opening an issue to see if this is a rule that is in-scope for Ruff?
In terms of technical feasibility of implementing this -- to what extent do the ruff internals track the types of each variable?

---

_Label `rule` added by @MichaReiser on 2024-04-15 08:08_

---

_Comment by @MichaReiser on 2024-04-15 08:10_

Hy. I haven't checked if Ruff already has a similar rule and I'm not familiar enough with Python to know if that's a rule that Ruff should support (@AlexWaygood knows more) but I think this isn't a rule that we can add until #1774 is done because it's not a rule that we want to enable by default and many users are using `--select  ALL`

---

_Comment by @kkom on 2024-04-16 14:10_

Big ➕1️⃣  to a rule like this! FWIW, I created a discussion about it here: https://github.com/astral-sh/ruff/discussions/8757#discussion-5865172 

<details>

<summary>Alternative option: Typechecker</summary>

As a sidenote, if you want to, you could also support this discussion where I pitched this rule in Pyright: https://github.com/microsoft/pyright/discussions/6488#discussion-5865204

I think implementing this in the typechecker would be much more reliable.

Pyright is known to implement opinionated rules like this (e.g. unnecessary comparison), but the maintainer is currently not convinced about this one:

> I'm not convinced that it represents a sufficiently common source of bugs to merit adding such a rule, but I could perhaps be persuaded with some additional evidence.

</details>

---

_Comment by @Garrett-R on 2025-01-05 05:08_

+1 for this rule.  Typescript-ESLint provides an [similar rule](https://typescript-eslint.io/rules/strict-boolean-expressions/) (discussion [here](https://github.com/typescript-eslint/typescript-eslint/issues/49)) which has been quite helpful (TypeScript also has falsey values for primitives).   It doesn't ban _all_ implicit casts to bool, but does ban it for primitives like string, number, etc.

And my Stack Overflow [request](https://stackoverflow.com/questions/68200814/prevent-implicit-casts) for this (in case someone remembers to update there if this rule gets implemented).  

> because it's not a rule that we want to enable by default and many users are using `--select  ALL`

Can't it be disabled by default, but enabled if you choose to do `--select ALL`?  I feel like if you go the `--select ALL` route, you're committing to getting _everything_ and using a blocklist.  So, it's just expected when I update Ruff at my startup (we use this) that I'll need to need to review new rules and possibly add some to the blocklist.





---
