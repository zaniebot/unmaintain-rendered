---
number: 8701
title: "[FEATURE] Only report errors with safe fixes"
type: issue
state: open
author: alanhdu
labels:
  - wish
  - linter
assignees: []
created_at: 2023-11-15T18:24:27Z
updated_at: 2023-11-27T03:11:57Z
url: https://github.com/astral-sh/ruff/issues/8701
synced_at: 2026-01-07T13:12:15-06:00
---

# [FEATURE] Only report errors with safe fixes

---

_Issue opened by @alanhdu on 2023-11-15 18:24_

Apologies if this is already possible somewhere, but I was wondering whether there was a way to configure Ruff to *only* report errors that it has *safe* fixes for. Ideally this could be done on a per-rule level.

My primary use-case is SIM118 (unnecessary `dict.keys()` call) -- unfortunately, we have some internal objects that implement a `def keys(self)` method that that brings up a bunch of false positives when using SIM118. But we *would* like to enforce them for `dict` objects, and the "safe" fix for SIM118 only fires when Ruff infers that it's an actual dictionary under-the-hood.

I guess this specific example could also be addressed by making SIM118 only fire if it can infer an actual dictionary as well... or by having some other configuration to control the false-positive vs false-negative tradeoffs here.

---

_Comment by @eli-schwartz on 2023-11-22 23:33_

> I guess this specific example could also be addressed by making SIM118 only fire if it can infer an actual dictionary as well... or by having some other configuration to control the false-positive vs false-negative tradeoffs here.

e.g. splitting it into two different lint codes, and having a new lint code for cases where it cannot guarantee an actual dictionary?

---

_Label `wish` added by @MichaReiser on 2023-11-27 03:11_

---

_Label `linter` added by @MichaReiser on 2023-11-27 03:11_

---
