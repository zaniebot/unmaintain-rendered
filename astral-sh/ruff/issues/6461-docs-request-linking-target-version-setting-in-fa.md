---
number: 6461
title: "Docs request: linking `target-version` setting in FA docs"
type: issue
state: closed
author: jamesbraza
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-08-09T21:15:55Z
updated_at: 2023-08-12T19:01:05Z
url: https://github.com/astral-sh/ruff/issues/6461
synced_at: 2026-01-10T01:22:45Z
---

# Docs request: linking `target-version` setting in FA docs

---

_Issue opened by @jamesbraza on 2023-08-09 21:15_

[`FA102`](https://beta.ruff.rs/docs/rules/future-required-type-annotation/) only happens if one has the [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) set below Python 3.10.

Thus, it would be useful to link the `target-version` docs in the [FA general section](https://beta.ruff.rs/docs/rules/#flake8-future-annotations-fa) or [FA102 instructions](https://beta.ruff.rs/docs/rules/future-required-type-annotation/
), as it's a closely related configuration parameter.

---

_Label `documentation` added by @zanieb on 2023-08-09 21:20_

---

_Comment by @zanieb on 2023-08-09 21:21_

@jamesbraza makes sense to me! Are you interested in contributing it?

---

_Label `good first issue` added by @zanieb on 2023-08-09 21:21_

---

_Comment by @jamesbraza on 2023-08-11 06:04_

I tried to add this tonight here to the general rules page.  I decided to add here (as opposed to rule-specific pages), as `target-version` impacts all sub-rules.

https://beta.ruff.rs/docs/rules/#flake8-future-annotations-fa

![screenshot of current rules section](https://github.com/astral-sh/ruff/assets/8990777/e3588327-6414-4ad0-b2a5-377093301868)

Honestly, I couldn't figure out what file to add the text:

> Configuring the [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) setting to newer Python versions can nullify these rules.

Should I put this text somewhere in `crates/ruff/rules/flake8_future_annotations/rules/mod.rs`?

---

_Comment by @charliermarsh on 2023-08-11 06:16_

I would prefer that we put those notes on the specific rules -- it's more consistent with the rest of the documentation, and the text you're referencing there is auto-generated, it would require special-casing in the documentation.

---

_Referenced in [astral-sh/ruff#6520](../../astral-sh/ruff/pulls/6520.md) on 2023-08-12 05:10_

---

_Comment by @jamesbraza on 2023-08-12 05:11_

Okay sounds good, per noting this on specific rules.

I came across https://github.com/astral-sh/ruff/pull/5105, which actually links `target-version` in the docs, just the linking is so low in the rules page that I didn't actually see it in the docs 👀 

Anyways, I opened https://github.com/astral-sh/ruff/pull/6520 to clarify `target-versions`'s role even more

---

_Closed by @charliermarsh on 2023-08-12 19:01_

---
