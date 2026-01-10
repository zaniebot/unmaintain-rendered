```yaml
number: 17987
title: "[ty] remove `lint:` prefix from top-level diagnostic preamble"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/tweak-diagnostic-preamble
created_at: 2025-05-09T15:47:50Z
updated_at: 2025-05-09T16:42:16Z
url: https://github.com/astral-sh/ruff/pull/17987
synced_at: 2026-01-10T18:57:03Z
```

# [ty] remove `lint:` prefix from top-level diagnostic preamble

---

_Pull request opened by @BurntSushi on 2025-05-09 15:47_

This also puts the lint ID into brackets attached to the severity.

That is, this:

```
error: lint:invalid-argument-type: Argument to this function is incorrect
```

is changed to:

```
error[invalid-argument-type]: Argument to this function is incorrect
```

Fixes #289 

---

_Review requested from @carljm by @BurntSushi on 2025-05-09 15:47_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-09 15:47_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-09 15:47_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-09 15:47_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-05-09 15:47_

---

_Review request for @dcreager removed by @BurntSushi on 2025-05-09 15:48_

---

_Review request for @carljm removed by @BurntSushi on 2025-05-09 15:48_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-05-09 15:48_

---

_Label `ty` added by @MichaReiser on 2025-05-09 15:52_

---

_Label `diagnostics` added by @MichaReiser on 2025-05-09 15:52_

---

_@MichaReiser reviewed on 2025-05-09 15:54_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:652 on 2025-05-09 15:54_

How involved is it to change `as_str`? Is `as_str` even still used in other places?

---

_@MichaReiser approved on 2025-05-09 15:54_

Nice

I've a slight preference to remove `as_str` but I don't know how involved that is

---

_@BurntSushi reviewed on 2025-05-09 16:17_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:652 on 2025-05-09 16:17_

Nice catch. Removing this revealed that there was still a `lint:` in the diagnostic output.

---

_@AlexWaygood approved on 2025-05-09 16:34_

I'd still prefer something that looks a bit more like English prose (spaces or round brackets) to square brackets, but this is a big improvement!

Though I do actually wonder if we even need the colon, if we're sticking with square brackets? Is this:

```
error[invalid-assignment]: Object of type `Literal["wrong"]` is not assignable to attribute `attr` of type `int`
```

any clearer than this?

```
error[invalid-assignment] Object of type `Literal["wrong"]` is not assignable to attribute `attr` of type `int`
```

---

_Comment by @BurntSushi on 2025-05-09 16:40_

I like the colon personally. Without it, it feels like the preamble is out there floating. And I still like the square brackets too. :-)

Very possible this is just my eyes being very used to `rustc`'s output. Because now our diagnostics look even more like rustc's.

I'm going to bring this in, but switching over to parentheses should be very easy if we want to go that route.

---

_Merged by @BurntSushi on 2025-05-09 16:42_

---

_Closed by @BurntSushi on 2025-05-09 16:42_

---

_Branch deleted on 2025-05-09 16:42_

---
