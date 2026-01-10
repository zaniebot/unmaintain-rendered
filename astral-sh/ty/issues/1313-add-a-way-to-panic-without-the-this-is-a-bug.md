---
number: 1313
title: "add a way to panic without the \"this is a bug\" message"
type: issue
state: open
author: carljm
labels: []
assignees: []
created_at: 2025-10-06T15:44:57Z
updated_at: 2026-01-09T03:06:35Z
url: https://github.com/astral-sh/ty/issues/1313
synced_at: 2026-01-10T01:48:23Z
---

# add a way to panic without the "this is a bug" message

---

_Issue opened by @carljm on 2025-10-06 15:44_

We have some user-triggerable cases that we just don't want to support at all (e.g. a custom typeshed without `object`), and want to just fail with an error. Currently we do this by panicking, but this emits an error message indicating that there's a bug in ty, which isn't the case here. We should have another way to "fatal error" that doesn't emit the "this is a bug in ty, please report it" message.

---

_Comment by @MichaReiser on 2025-10-06 16:01_

We could consider panicking (unwinding) with a `Diagnostic` type that we than handle differently from other panics

---

_Comment by @carljm on 2026-01-09 03:06_

Leaving this out of stable for now; doesn't seem like we need to prioritize it unless we start getting a lot of bug reports about these "not a bug" panic cases.

---
