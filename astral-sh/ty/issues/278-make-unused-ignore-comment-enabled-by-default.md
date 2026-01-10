---
number: 278
title: "Make `unused-ignore-comment` enabled by default"
type: issue
state: closed
author: AlexWaygood
labels:
  - configuration
  - suppression
assignees: []
created_at: 2025-05-08T16:23:50Z
updated_at: 2026-01-09T10:58:45Z
url: https://github.com/astral-sh/ty/issues/278
synced_at: 2026-01-10T01:48:23Z
---

# Make `unused-ignore-comment` enabled by default

---

_Issue opened by @AlexWaygood on 2025-05-08 16:23_

We made this rule disabled by default in https://github.com/astral-sh/ruff/pull/17955, on the following grounds:

> Right now it seems like there are too many situations where:
> 
> 1. There is a typing error on a line that all other type checkers detect
> 2. The user has suppressed that error for Reasons with a type: ignore comment
> 3. But we don't yet detect the typing error due to a missing feature, leading us to complain about the unused ignore comment
> 
> The rule feels like more of a lint anyway, and mypy's version is opt-in. I'd like for it to be enabled by default for the GA release, but right now it feels like we're getting a lot of user questions about it. Let's make it disabled-by-default until we're closer to feature-parity with other type checkers.

I think we all agree that we'd like it to be enabled by default for the GA release. This issue exists so we don't forget to do so.

---

_Added to milestone `GA` by @AlexWaygood on 2025-05-08 16:23_

---

_Label `configuration` added by @AlexWaygood on 2025-05-08 16:23_

---

_Comment by @carljm on 2025-05-08 16:33_

Possible considerations when re-enabling it:

1. Separate rules for unused `type: ignore` vs unused `ty: ignore`, since it's useful to be able to suppress the former but not the latter (if the codebase is checked by multiple type checkers).
2. And/or a config option to determine what we do with `type: ignore` comments (pretend they don't exist, warn if they are unused, ignore the error codes, treat them the same)

---

_Label `suppression` added by @AlexWaygood on 2025-05-10 17:58_

---

_Comment by @MichaReiser on 2025-12-24 14:23_

We introduced the `analysis.respect-type-ignore-comments` setting, which should put us in a position to enable this rule by default. However, I think we should wait until we fixed the instability (involving an `unused-ignore-comment` warning) on `pandas-stubs`

---

_Comment by @AlexWaygood on 2026-01-02 20:19_

> We introduced the `analysis.respect-type-ignore-comments` setting, which should put us in a position to enable this rule by default. However, I think we should wait until we fixed the instability (involving an `unused-ignore-comment` warning) on `pandas-stubs`

Hmm, but this rule is far from the only error code where we have instability currently. Most of our other unstable rules are enabled by default anyway, and the instability for this error code isn't really an issue with the error code itself (it's instability in other error codes that causes suppression comments for those codes to sometimes be unused, sometimes not!).

I'd lean towards just enabling the rule by default at this stage. Though I think we could consider adding a subdiagnostic that points the user towards the configuration setting if it's an unused `type: ignore` comment (rather than an unused `ty: ignore` comment).

---

_Closed by @MichaReiser on 2026-01-09 10:58_

---
