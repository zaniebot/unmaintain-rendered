```yaml
number: 17154
title: "ruff_db: switch diagnostic rendering over to `std::fmt::Display`"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/diagnostic-display
created_at: 2025-04-02T14:41:47Z
updated_at: 2025-04-02T16:06:30Z
url: https://github.com/astral-sh/ruff/pull/17154
synced_at: 2026-01-12T15:56:00Z
```

# ruff_db: switch diagnostic rendering over to `std::fmt::Display`

---

_@BurntSushi_

It was already using this approach internally, so this is "just" a
matter of rejiggering the public API of `Diagnostic`.

We were previously writing directly to a `std::io::Write` since it was
thought that this worked better with the linear typing fakery. Namely,
it increased confidence that the diagnostic rendering was actually
written somewhere useful, instead of just being converted to a string
that could potentially get lost.

For reasons discussed in #17130, the linear type fakery was removed.
And so there is less of a reason to require a `std::io::Write`
implementation for diagnostic rendering. Indeed, this would sometimes
result in `unwrap()` calls when one wants to convert to a `String`.


---

_Review requested from @carljm by @BurntSushi on 2025-04-02 14:41_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-04-02 14:41_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-04-02 14:41_

---

_Review requested from @sharkdp by @BurntSushi on 2025-04-02 14:41_

---

_Review requested from @dcreager by @BurntSushi on 2025-04-02 14:41_

---

_Review request for @dcreager removed by @BurntSushi on 2025-04-02 14:42_

---

_Review request for @carljm removed by @BurntSushi on 2025-04-02 14:42_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-04-02 14:42_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-04-02 14:42_

---

_Review comment by @MichaReiser on `crates/red_knot_ide/src/goto.rs`:874 on 2025-04-02 14:43_

Nit: In case you only created the `display` variable so that you can used name format args
```suggestion
                write!(buf, "{diag}", diag=diag.display(&self.db, &config)).unwrap();
```

---

_Comment by @github-actions[bot] on 2025-04-02 14:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:111 on 2025-04-02 14:44_

We almost exclusively use `'db` for the db lifetime. 
```suggestion
        db: &'db dyn Db,
```

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:32 on 2025-04-02 14:45_

I'm sure you thought about this but are the fine grained lifetimes required or could we just use one saying: You can display this as long as neither `self`, `db` or `config` gets dropped. 

---

_@MichaReiser approved on 2025-04-02 14:46_

---

_Label `red-knot` added by @MichaReiser on 2025-04-02 14:46_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:32 on 2025-04-02 14:53_

Yes I did. When it's crate internal, it generally shouldn't matter, because these are all shared borrows and thus subtyping comes into play. And if it does matter, well, then I guess you just fix the lifetimes.

It _can_ matter though. See [this](https://github.com/rust-lang/regex/issues/168) as an example. It's not totally obvious to me that it _will_ matter, but usually when I expose public APIs, I try not to collapse lifetimes. So I mostly just did this by reflex.

---

_@BurntSushi reviewed on 2025-04-02 14:53_

---

_@BurntSushi reviewed on 2025-04-02 14:53_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:111 on 2025-04-02 14:53_

Yeah... I suppose this will become generic at some point, but it seems fine to just use `db` for now.

---

_@BurntSushi reviewed on 2025-04-02 14:54_

---

_Review comment by @BurntSushi on `crates/red_knot_ide/src/goto.rs`:874 on 2025-04-02 14:54_

It was originally a bigger expression, but sure, yeah, fixed.

---

_Merged by @BurntSushi on 2025-04-02 15:01_

---

_Closed by @BurntSushi on 2025-04-02 15:01_

---

_Branch deleted on 2025-04-02 15:01_

---

_@MichaReiser reviewed on 2025-04-02 15:32_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:32 on 2025-04-02 15:32_

It's great to see that you have the same ambition for `ruff_db` as for the `regex` crate 😆 

Isn't this slightly different in the sense that `DisplayDiagnostic` never returns any values? I do see the point of having all the lifetimes if `DisplayDiagnostic` had field accessors where it matters that they return the right lifetime. But here it feels like it's mainly: You can only use it as long as the three borrows are valid. 

Either way, not that important. It's just very scary looking

---

_@BurntSushi reviewed on 2025-04-02 16:06_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:32 on 2025-04-02 16:06_

Yeah that's what I meant by it might not mattering. As in, I can't easily come up with an example where it matters.

Happy to change it back, but I honestly don't feel strongly.

---
