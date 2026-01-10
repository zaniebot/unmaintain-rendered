```yaml
number: 15731
title: Speed symbol state merging back up
type: pull_request
state: merged
author: dcreager
labels: []
assignees: []
merged: true
base: main
head: dcreager/merge-speedup
created_at: 2025-01-24T20:20:46Z
updated_at: 2025-01-24T21:08:21Z
url: https://github.com/astral-sh/ruff/pull/15731
synced_at: 2026-01-10T19:57:22Z
```

# Speed symbol state merging back up

---

_Pull request opened by @dcreager on 2025-01-24 20:20_

This is a follow-up to #15702 that hopefully claws back the 1% performance regression.  Assuming it works, the trick is to iterate over the constraints vectors via mut reference (aka a single pointer), so that we're not copying `BitSet`s into and out of the zip tuples as we iterate.  We use `std::mem::take` as a poor-man's move constructor only at the very end, when we're ready to emplace it into the result.  (C++ idioms intended! :smile:)

With local testing via hyperfine, I'm seeing this be 1-3% faster than `main` most of the time — though a small number of runs (1 in 10, maybe?) are a wash or have `main` faster.  Fingers crossed to see what codspeed says!

---

_Review requested from @carljm by @dcreager on 2025-01-24 20:20_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-24 20:20_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-24 20:20_

---

_Review requested from @sharkdp by @dcreager on 2025-01-24 20:20_

---

_Comment by @dcreager on 2025-01-24 20:32_

> Fingers crossed to see what codspeed says!

2% faster [according to codspeed](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fmerge-speedup)! :tada: :taco: 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:307 on 2025-01-24 20:46_

A comment here would probably be useful. It wasn't immediately clear to me why we use a `&mut` here even with your explanation. Should we do the same in the other `merge` method and for `visibility_constraints?

---

_@MichaReiser approved on 2025-01-24 20:46_

Nice

---

_@dcreager reviewed on 2025-01-24 20:48_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:307 on 2025-01-24 20:48_

I think it won't be as beneficial for `visibility_constraints`, since those are already smaller.  But I can check!  (And I will add the comment)

---

_@dcreager reviewed on 2025-01-24 20:58_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:307 on 2025-01-24 20:58_

Yep as expected, it was a wash doing the same thing to `visibility_constraints`.  (Those are 32-bit IDs, which are cheap to copy, and not 24-byte `BitSet`s.)

---

_Merged by @dcreager on 2025-01-24 21:07_

---

_Closed by @dcreager on 2025-01-24 21:07_

---

_Branch deleted on 2025-01-24 21:07_

---

_Comment by @carljm on 2025-01-24 21:08_

Amazing, thank you!!

---
