```yaml
number: 16354
title: "[red-knot] Handle pipe-errors gracefully"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/pipe-errors
created_at: 2025-02-24T19:17:59Z
updated_at: 2025-02-25T07:42:55Z
url: https://github.com/astral-sh/ruff/pull/16354
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] Handle pipe-errors gracefully

---

_@MichaReiser_

## Summary

This PR removes the use of `println` from the Red Knot CLI in favor of using `writeln` which isn't prone to panicking when writing to a broken pipe. 

This PR also adds some code to exit without an error code if the root cause for erroring is a broken pipe (copied over from Ruff)

Fixes https://github.com/astral-sh/ruff/issues/15586

## Test Plan

@carljm tested it



---

_Review requested from @carljm by @MichaReiser on 2025-02-24 19:17_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-24 19:17_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-24 19:17_

---

_Label `red-knot` added by @MichaReiser on 2025-02-24 19:18_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-02-24 19:18_

---

_@carljm approved on 2025-02-24 19:31_

Thank you for fixing this!

---

_@BurntSushi approved on 2025-02-24 19:32_

LGTM!

---

_Comment by @carljm on 2025-02-24 19:32_

> I haven't found a good way to trigger the broken pipe error in Red Knot. So this is mostly untested

I thought @JelleZijlstra mentioned [in Discord](https://discord.com/channels/1039017663004942429/1279201882337705994/1342726479229882429) that this is triggerable by piping the output from red-knot to `less` and then Ctrl-C? Does that not fail for you?

---

_Comment by @BurntSushi on 2025-02-24 19:35_

That or `head`. You usually need "enough" output to trigger it (because it needs to overflow a buffer somewhere).

---

_Comment by @carljm on 2025-02-24 19:37_

Ah that makes sense, yeah I think Jelle ran into it running red-knot on a bigger project that would have triggered lots of diagnostic output.

---

_Comment by @MichaReiser on 2025-02-24 19:38_

Let me try `head` tomorrow. 

> I thought @JelleZijlstra mentioned [in Discord](https://discord.com/channels/1039017663004942429/1279201882337705994/1342726479229882429) that this is triggerable by piping the output from red-knot to less and then Ctrl-C? Does that not fail for you?

That's why I wanted to get this fixed :)

---

_Comment by @carljm on 2025-02-24 19:39_

Yeah I can trigger the panic on `main` by running red-knot on Black and piping it to `head`. With this PR, same scenario doesn't panic. 🎉 

---

_Comment by @JelleZijlstra on 2025-02-24 19:45_

> I thought @JelleZijlstra mentioned [in Discord](https://discord.com/channels/1039017663004942429/1279201882337705994/1342726479229882429) that this is triggerable by piping the output from red-knot to `less` and then Ctrl-C?

I think I just did "q" to quit less, but that's effectively the same.

---

_Comment by @MichaReiser on 2025-02-24 21:45_

> I think I just did "q" to quit less, but that's effectively the same.

I tried that but i was never successful at hitting Ctrl+C right when red knot was about to print a diagnostic.

---

_Comment by @carljm on 2025-02-24 21:50_

> I tried that but i was never successful at hitting Ctrl+C right when red knot was about to print a diagnostic.

From my testing on macOS, no particular timing is necessary, just a large amount of output. `./target/debug/red_knot check --project ../black --target-version 3.12 | less` followed by a `q` (any amount of time later) reproduces it; the key factor is just that running red-knot on black generates lots of diagnostics.

It may be different on Linux? Output buffer sizes may differ.

---

_Merged by @MichaReiser on 2025-02-25 07:42_

---

_Closed by @MichaReiser on 2025-02-25 07:42_

---

_Branch deleted on 2025-02-25 07:42_

---
