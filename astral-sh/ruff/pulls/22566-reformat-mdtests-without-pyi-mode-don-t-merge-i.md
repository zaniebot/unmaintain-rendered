```yaml
number: 22566
title: "Reformat mdtests without pyi mode (don't merge, I want to know if this is a deal breaker or not)"
type: pull_request
state: closed
author: MichaReiser
labels:
  - do-not-merge
  - testing
  - ty
assignees: []
base: main
head: micha/mdtest-no-pyi-mode
created_at: 2026-01-14T09:48:37Z
updated_at: 2026-01-14T12:17:06Z
url: https://github.com/astral-sh/ruff/pull/22566
synced_at: 2026-01-14T12:43:59Z
```

# Reformat mdtests without pyi mode (don't merge, I want to know if this is a deal breaker or not)

---

_@MichaReiser_

We plan on implementing blackendocs for ruff and we then want to migrate ty to use it instead of blackendocs. One question that has come up is whether a `--pyi` mode that forces all code to be formatted as stub files is a must have or not. I'd prefer to exclude it from an initial version unless this is an absolute deal breaker. 

There's also a design question here: Should the language on the fenced code block take precedence over the `--pyi` mode? 

---

_Label `testing` added by @MichaReiser on 2026-01-14 09:48_

---

_Label `ty` added by @MichaReiser on 2026-01-14 09:48_

---

_Comment by @MichaReiser on 2026-01-14 09:54_

Doesn't look like the end of the world to me. Yes, it adds quiet a bit of whitespace (about 40 lines per testfile). But it isn't that the files become completely unreadable or twice as long. 

I wouldn't consider it a blocker for adopting ruffendocs but @AlexWaygood might feel differently (we would support `pyi` as language which I think would help with some of the most affected files, as long as they don't test any non-pyi behavior)

---

_Comment by @AlexWaygood on 2026-01-14 10:25_

Definitely not the end of the world, but to me it still feels like a regression in readability/formatting that I'd rather not have :P

I also think the feature more broadly is a reasonable one -- to me it makes sense that you might want your documentation examples formatted more concisely than your production code! You just want to get the semantic meaning of the code across to the reader as quickly as possible in that scenario.

Anyway, I'm curious what others on the ty think too :-)

---

_Comment by @MichaReiser on 2026-01-14 10:49_

> Definitely not the end of the world, but to me it still feels like a regression in readability/formatting that I'd rather not have :P

I actually find that some tests improve when more space is added between class definitions. But now we're in opinionated formatter territory. 

The main crux that I see is that `ruff format` doesn't support a `--pyi` option. So the request here is really such an option to ruff formatter's which, to this point, no one has requested. 

We also use the normal Python formatting when formatting code blocks in docstrings, and this has never been raised as an issue. That's why I don't feel a strong need to add this option, even if it might be nice for our mdtests.

I opened https://github.com/astral-sh/ruff/issues/22568

---

_Marked ready for review by @MichaReiser on 2026-01-14 10:53_

---

_Review requested from @carljm by @MichaReiser on 2026-01-14 10:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2026-01-14 10:53_

---

_Review requested from @sharkdp by @MichaReiser on 2026-01-14 10:53_

---

_Review requested from @dcreager by @MichaReiser on 2026-01-14 10:53_

---

_Renamed from "micha/mdtest no pyi mode" to "Reformat mdtests without pyi mode" by @MichaReiser on 2026-01-14 10:54_

---

_Renamed from "Reformat mdtests without pyi mode" to "Reformat mdtests without pyi mode (don't merge, I want to know if this is a deal breaker or not)" by @MichaReiser on 2026-01-14 10:54_

---

_Label `do-not-merge` added by @MichaReiser on 2026-01-14 10:56_

---

_Comment by @MichaReiser on 2026-01-14 12:17_

I'm coming to the conclusion that I feel very strongly about not having a pyi option https://github.com/astral-sh/ruff/issues/22568 and it's certainly a separate discussion from supporting formatting of fenced code blocks.

---

_Closed by @MichaReiser on 2026-01-14 12:17_

---
