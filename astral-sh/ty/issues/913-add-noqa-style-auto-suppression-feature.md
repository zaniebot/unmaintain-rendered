```yaml
number: 913
title: "`--add-noqa`‑style auto-suppression feature"
type: issue
state: closed
author: aaronsteers
labels:
  - suppression
  - fixes
assignees: []
created_at: 2025-07-30T00:39:43Z
updated_at: 2026-01-07T10:17:06Z
url: https://github.com/astral-sh/ty/issues/913
synced_at: 2026-01-12T15:54:24Z
```

# `--add-noqa`‑style auto-suppression feature

---

_@aaronsteers_

Hi Astral/`ty` team – I’m evaluating **ty** (and Pyrefly) for static typing adoption across a very large legacy codebase (tens of thousands of lines) that never adopted MyPy.

**Ruff supports `--add-noqa`** to append `# noqa: E###` comments for lint errors and ease migration.

**Pyrefly similarly offers an auto-suppress feature** — specifically, `pyrefly check --suppress-errors` can insert `# pyrefly: ignore` comments automatically to silence type-checking errors inline, making gradual adoption far more feasible.

### 🙏 Why this matters

- Without auto suppression, introducing **ty** into a massive legacy codebase would generate an overwhelming wall of errors.
- We need a way to silence all current type-checking issues inline, then iteratively fix them over time.
- This is a hard requirement for us to consider adopting **ty**, especially given our codebase size and legacy state. (We can wait a little while but we'd like to add `ty` support soon, as long as it is somewhere in the mid-term version. We are completely ignoring the legacy options, so our next-best option would be `pyrefly`.)

### ✅ Proposal

Add **ty** support for a `--add-noqa` or `--add-type-ignore` arg, which:

- Automatically inserts suppression comments like `# type: ignore` or ideally `# ty: ignore[...]`
- Includes rule codes or kinds where feasible
- Applies across files/directories in one command

Even a basic version — suppressing *all* present errors — would be incredibly useful. Ideally the feature could later evolve to support finer‑grained control.

If this isn’t currently on the roadmap, I’d love to know if you're open to a design proposal or PR. I’d be glad to collaborate on an initial implementation or spec.

Thanks for the effort you’ve put into **ruff**, **uv**, and **ty** — the ecosystem of high‑performance Python tooling from Astral has been inspiring, and I'm honestly very much looking forward to adding **ty** to our stack.

— AJ


---

_Renamed from "🚀 Feature Request: `--add-noqa`‑style auto-suppression for **ty**" to "🚀 Feature Request: `--add-noqa`‑style auto-suppression for `ty`" by @aaronsteers on 2025-07-30 00:39_

---

_Label `feature` added by @MichaReiser on 2025-07-30 06:33_

---

_Label `fixes` added by @MichaReiser on 2025-07-30 06:33_

---

_Comment by @MichaReiser on 2025-07-30 06:35_

Hi and thanks for the great write up. Yes, that's a feature we want. We haven't had the time to work on it. It does require some upfront CLI design work. Should this be a flag similar to pyrefly or should it be a separate command like in Ruff? Should this be a rule (with a fix) or is it an entirely separate feature.

---

_Label `wish` added by @MichaReiser on 2025-07-30 06:36_

---

_Comment by @aaronsteers on 2025-07-31 02:20_

> Hi and thanks for the great write up. 

🙏 Thanks! 😄 

> Yes, that's a feature we want.

Awesome. Thanks for confirming.

> Should this be a flag similar to pyrefly or should it be a separate command like in Ruff?

I think the `ruff` implementation is already great in terms of ergonomics.

IIRC, in Ruff it can be invoked with `ruff check --add-noqa`.

And, helpfully, I believe there's a means also of identifying and _removing_ stale noqas, although I don't recall if that is only with 'normal' `ruff check --fix`.

> Should this be a rule (with a fix) or is it an entirely separate feature.

I think in an ideal world, the noqa statements are specific to the error being ignored (meaning there's a meta aspect of a rule needing to know all the other rules), so I don't know if it is feasible to apply as a rule in itself, versus a command (or command-like behavior triggered via CLI arg).

I do think the **_removal_** of unused noqas might be feasible as a fixable rule.

And to be very specific, I don't have a strong preference in terms of ergonomics whether it is a rule, a command, or a CLI flag that triggers a command-like action. Others who better understand the internals might have stronger opinions here, and I'm happy to defer. 

## Argument for a dedicated command...

That said, I did use a now very-old tool called `flakehell`, and that tool specifically had a '[baseline](https://flakehell.readthedocs.io/commands/baseline.html)' command. For my use case (below), I'm totally okay if this follows that paradigm: almost on onboarding-type activity - done once and then forgotten. I would be completely okay with a `ty baseline` command, or similar, which just treated current state as "baseline" and noqa-ed whatever was needed.

This would probably just be run once, or perhaps run again if/when stricter rules are opted-into.

### More Context

For my part, this is _most_ valuable when baselining existing codebases quickly (like overnight) so that _incrementally_ things are sane, and can be progressively improved as people find ways to remove those noqas. To do so frequently in the course of regular development (such as in a PR slash command) does give some convenience - but seems to trivialize the value of typing in the first place. Hopefully the IDE can put squigglies where they need to be, and a developer can add noqas when/if they disagree with the feedback or don't have an immediate fix. But for my part, after initial adoption, these should ideally be infrequent. For my use cases, I'm happy if I can quickly onboard a large codebase, knowing that (1) I'm not making anything worse and (2) unused/unneeded noqas will disappear organically when they are no longer needed.

---

_Label `feature` removed by @AlexWaygood on 2025-10-06 16:32_

---

_Renamed from "🚀 Feature Request: `--add-noqa`‑style auto-suppression for `ty`" to "`--add-noqa`‑style auto-suppression feature" by @AlexWaygood on 2025-10-07 08:51_

---

_Comment by @janosh on 2025-10-29 23:17_

we use a monorepo and lack of `--add-noqa` auto-suppression is preventing us from updating to newer `ty` versions due to the large number of legacy errors. would be super useful to have this! 👍 

---

_Comment by @carljm on 2025-10-30 14:51_

I think we should probably bump up the priority on this, as I don't think it's that _hard_ (though it does require some design decisions), and it can be a real blocker to adoption. (I.e. I think we probably shouldn't mark it "wish", and should maybe even put it under the GA milestone.)

---

_Label `wish` removed by @carljm on 2025-10-30 16:43_

---

_Added to milestone `GA` by @carljm on 2025-10-30 16:43_

---

_Comment by @MichaReiser on 2025-11-14 08:24_

Related to https://github.com/astral-sh/ty/issues/1501

---

_Comment by @MichaReiser on 2025-11-23 14:10_

Same as for https://github.com/astral-sh/ty/issues/1501#issuecomment-3567994631. The main outstanding decision here is whether we want a `ty fix` command or `ty check --fix`

---

_Comment by @janosh on 2025-11-23 15:10_

definitely ty check --fix for consistency with ruff

---

_Comment by @MichaReiser on 2025-11-23 15:20_

> definitely ty check --fix for consistency with ruff

There's a two-year-old proposal from @zanieb to deprecate `ruff check-- fix` in favor of `ruff fix`. We just never got to implement the change. 

---

_Comment by @MichaReiser on 2025-11-23 16:23_

The proposal, is unfortunately, an internal notion document. But it also says

> We want to retain a good single command interface, so ruff check --fix should remain available. This pattern allows users to fix violations with the same options they’ve provided to check with minimal effort.

So starting with `ty check --fix` might be a good first step. The main concern mentioned in the proposal are the `--fix-only` and other flags that require `--fix`. 

---

_Label `suppression` added by @MichaReiser on 2025-11-29 15:05_

---

_Comment by @MichaReiser on 2025-11-29 17:17_

I started hacking on something but there are a few more wrinkles to solve (e.g. ty has a watch mode, how to support notebooks, ..., we need to render all remaining diagnostics)

---

_Comment by @danielgafni on 2025-12-18 11:41_

I can confirm this is an adoption blocker for us. I assume it will be for most large projects. 

But I also believe a [baseline](https://github.com/astral-sh/ty/issues/1074) feature - the way `basedpyright` has it implemented - is a superior alternative, since it allows tracking and gradually eliminating these initial ignores separately from all the others, "legitimate" ones. 

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:36_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:36_

---

_Closed by @MichaReiser on 2026-01-07 10:17_

---
