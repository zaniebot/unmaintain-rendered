```yaml
number: 21515
title: "[ty] Keep colorizing `mypy_primer` output"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-strip-ansi-hyperlinks
created_at: 2025-11-18T19:12:47Z
updated_at: 2025-11-19T08:52:54Z
url: https://github.com/astral-sh/ruff/pull/21515
synced_at: 2026-01-12T15:57:26Z
```

# [ty] Keep colorizing `mypy_primer` output

---

_@sharkdp_

## Summary

After an update to `mypy_primer`, we now need to set the environment variable ourselves.

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 19:16_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-18 19:18_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Marked ready for review by @sharkdp on 2025-11-18 19:28_

---

_Renamed from "[ty] Strip hyperlinks from mypy_primer diffs" to "[ty] Strip OSC 8 hyperlinks from `mypy_primer` diffs" by @sharkdp on 2025-11-18 19:29_

---

_Label `ci` added by @sharkdp on 2025-11-18 19:29_

---

_Label `ty` added by @sharkdp on 2025-11-18 19:29_

---

_Comment by @astral-sh-bot[bot] on 2025-11-18 19:33_


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

_Comment by @AlexWaygood on 2025-11-18 20:03_

> Colorized output in the GitHub Actions terminal is still working

hmm, but it looks like the error code has now been stripped as well? 😄 It's now just `error[]` instead of `error[invalid-await]`

I don't mind too much if we just disable colour entirely for ty in mypy_primer. I do occasionally find colourised logs useful, but it's probably not worth spending a lot of time on right now :-)

---

_Comment by @AlexWaygood on 2025-11-18 20:29_

> hmm, but it looks like the error code has now been stripped as well?

oh, but I guess they're missing on the `main` branch too? E.g. <https://github.com/astral-sh/ruff/actions/runs/19479467091/job/55747619369?pr=21467>. I think that might have also regressed in https://github.com/astral-sh/ruff/pull/21502...

---

_Comment by @sharkdp on 2025-11-18 21:16_

> hmm, but it looks like the error code has now been stripped as well? 😄 It's now just `error[]` instead of `error[invalid-await]`

Oh, I didn't even notice in the screenshot. Well, that's not my fault. We show the unmodified output in the terminal. I guess the GitHub Actions ANSI parser can't handle hyperlinks?

> I don't mind too much if we just disable colour entirely for ty in mypy_primer. I do occasionally find colourised logs useful, but it's probably not worth spending a lot of time on right now :-)

Ok, I guess I'll just change it

---

_@MichaReiser approved on 2025-11-18 21:29_

> Oh, I didn't even notice in the screenshot. Well, that's not my fault. We show the unmodified output in the terminal. I guess the GitHub Actions ANSI parser can't handle hyperlinks?

Hmm. Terminals should skip unsupported ansi escaped without stripping the content. 

We can also consider using a crate to dedect supported terminals so that our users can keep using color too. Happy to own that. 




---

_Comment by @MichaReiser on 2025-11-19 08:33_

Sorry for breaking mypy primer, I just published https://github.com/astral-sh/ruff/pull/21519

---

_Renamed from "[ty] Strip OSC 8 hyperlinks from `mypy_primer` diffs" to "[ty] Keep colorizing `mypy_primer` output" by @sharkdp on 2025-11-19 08:49_

---

_Comment by @sharkdp on 2025-11-19 08:52_

> I just published #21519

Thanks. No worries.

---

_Merged by @sharkdp on 2025-11-19 08:52_

---

_Closed by @sharkdp on 2025-11-19 08:52_

---

_Branch deleted on 2025-11-19 08:52_

---
