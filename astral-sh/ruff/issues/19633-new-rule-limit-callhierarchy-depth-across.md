---
number: 19633
title: "New rule: limit call‑hierarchy depth across functions/methods"
type: issue
state: closed
author: Spenhouet
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-30T09:41:48Z
updated_at: 2025-07-31T07:04:13Z
url: https://github.com/astral-sh/ruff/issues/19633
synced_at: 2026-01-10T01:23:00Z
---

# New rule: limit call‑hierarchy depth across functions/methods

---

_Issue opened by @Spenhouet on 2025-07-30 09:41_

### Motivation
Ruff already checks block‑nesting depth ([PLR1702](https://docs.astral.sh/ruff/rules/too-many-nested-blocks/)) and cyclomatic complexity ([C901](https://docs.astral.sh/ruff/rules/complex-structure/)). These don’t catch code that hops through many functions/methods (A→B→C→D…), which reduces readability, testability and maintainability. [WPS233](https://github.com/astral-sh/ruff/issues/8022) limits chained calls in one expression (e.g., obj.a().b().c()). It does not measure cross‑function call depth. A rule to cap inter‑procedural call depth would fill this gap.

Enforcing a flatter software code architecture is something that we'd like to have.

### Proposal

Add a new rule, e.g. RUF1xxx: max-call-hierarchy-depth.

#### Behavior

- Build a static call graph from imports + AST within a module (optionally across the project).
- From configurable “entry points” (public functions, __main__, CLI handlers, views), measure longest call path.
- Emit a diagnostic when depth exceeds a threshold.

**Example**

```python
def a(): b()
def b(): c()
def c(): d()
def d(): pass  # depth = 4  -> violation if max-depth=3
```

### Limitations / notes

- Python’s dynamism means resolution is best‑effort: dynamic dispatch, monkey‑patching, and reflective calls may be skipped or approximated.
- Decorators, first‑class functions, and callbacks may need conservative handling.
- Start as opt‑in and preview (like PLR1702) while feedback is gathered.

### Why in Ruff
Complements PLR1702 (local nesting) and C901 (branching) with inter‑procedural depth, giving teams a simple knob to keep designs shallow without extra tools.

### Alternatives considered
No alternative known.

---

_Comment by @MichaReiser on 2025-07-30 09:58_

Thanks for this detailed write up.

There are a few challenges that I see with this rule:

* Where should ruff stop? Should it count the complexity of third party modules to, what about built ins?
* Doing this recursively is probably very expensive and will slow down your analysis a lot
* I don't think the rule is very actionable. I don't know what I'm supposed to do if I'm `a` but I have to call `b`. Should I duplicate `b`? 

---

_Comment by @Spenhouet on 2025-07-30 10:05_

> Where should ruff stop? Should it count the complexity of third party modules to, what about built ins?

I would not count third party modules or build ins.
But I would count editable packages (workspace packages) if that does make it even more complicated to implement than it might already be.

> Doing this recursively is probably very expensive and will slow down your analysis a lot

Without any knowledge on how ruff is implemented, I have a similar fear but I hoped that maybe ruff already does internal caching of details about the code which might enable something like this or that something like this could be done. 

I guess having to perform this recursive scan every time is not feasible. 

> I don't think the rule is very actionable. I don't know what I'm supposed to do if I'm a but I have to call b. Should I duplicate b?

You are thinking about existing/legacy code (which indeed is most of the code out there). I'm thinking more about newly written code or code where the developer has the ability to refactor the code, not duplicate but reimplement in a flatter structure.


---

_Label `rule` added by @ntBre on 2025-07-30 12:46_

---

_Label `needs-decision` added by @ntBre on 2025-07-30 12:46_

---

_Comment by @MichaReiser on 2025-07-31 06:31_

> You are thinking about existing/legacy code (which indeed is most of the code out there). I'm thinking more about newly written code or code where the developer has the ability to refactor the code, not duplicate but reimplement in a flatter structure.

Even for new code, it's unclear to me what a user should do if they see this lint warning. It could mean that they have to rewrite a bunch of functions to make a fix. 

It's not clear to me that this improves code quality and Ruff currently also doesn't have the necessary multifile analysis capabilities to implement this rule (and even ty couldn't implement this rule today because it doesn't track if a search path comes from an editable install). I'll close this issue because I want to keep the issue tracker actionable and this rule isn't something that can be implemented anytime soon (ignoring my concerns with the rule itself)

---

_Closed by @MichaReiser on 2025-07-31 06:31_

---

_Comment by @Spenhouet on 2025-07-31 07:04_

> this rule isn't something that can be implemented anytime soon

I feared so. Thanks for the clarification.

> It could mean that they have to rewrite a bunch of functions to make a fix.

But that is the case with many ruff linting rules. 

> It's not clear to me that this improves code quality

In my experience deeply nested call chains makes changes very hard. In our team we have a larger percentage of developers/data-scientists with research background coming from other domains than computer science. Code structure decisions often do not consider programming best practices leading to things like deeply nested call chains, forwarding a parameter through many hops, ... 
Now changing something 10 levels down can have many side effects. Changes become impossible without doing large refactorings.

This rule was intended to prevent that.

---
