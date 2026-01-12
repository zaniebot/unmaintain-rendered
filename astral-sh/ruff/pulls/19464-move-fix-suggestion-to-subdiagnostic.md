```yaml
number: 19464
title: Move fix suggestion to subdiagnostic
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/new-diagnostic-suggestion
created_at: 2025-07-21T14:11:54Z
updated_at: 2025-07-22T16:48:38Z
url: https://github.com/astral-sh/ruff/pull/19464
synced_at: 2026-01-12T15:56:40Z
```

# Move fix suggestion to subdiagnostic

---

_@ntBre_

Summary
--

This PR tweaks Ruff's internal usage of the new diagnostic model to more closely
match the intended use, as I understand it. Specifically, it moves the fix/help
suggestion from the primary annotation's message to a subdiagnostic. In turn, it
adds the secondary/noqa code as the new primary annotation message. As shown in
the new `ruff_db` tests, this more closely mirrors Ruff's current diagnostic
output.

I also added `Severity::Help` to render the fix suggestion with a `help:` prefix
instead of `info:`.

These changes don't have any external impact now but should help a bit with #19415.

Test Plan
--

New full output format tests in `ruff_db`

Rendered Diagnostics
--

Full diagnostic output from `annotate-snippets` in this PR:

``` 
error[unused-import]: `os` imported but unused
  --> fib.py:1:8
   |
 1 | import os
   |        ^^
   |
 help: Remove unused import: `os`
```

Current Ruff output for the same code:

```
fib.py:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
  |
  = help: Remove unused import: `os`
```

Proposed final output after #19415:

``` 
F401 [*] `os` imported but unused
  --> fib.py:1:8
   |
 1 | import os
   |        ^^
   |
 help: Remove unused import: `os`
```

These are slightly updated from https://github.com/astral-sh/ruff/pull/19464#issuecomment-3097377634 below to remove the extra noqa codes in the primary annotation messages for the first and third cases.

---

_Label `internal` added by @ntBre on 2025-07-21 14:12_

---

_Label `diagnostics` added by @ntBre on 2025-07-21 14:12_

---

_Comment by @github-actions[bot] on 2025-07-21 14:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-21 14:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fnew-diagnostic-suggestion?runnerMode=Instrumentation)

### Merging #19464 will **not alter performance**

<sub>Comparing <code>brent/new-diagnostic-suggestion</code> (10676cb) with <code>main</code> (c82fa94)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-07-21 14:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-07-21 15:43_

I'm not coming up with much to do about the perf regression. At first I thought using a `SmallVec` for the subdiagnostics might help, as I think we discussed briefly before, but that made things slightly worse. Looking at the flamegraphs on codspeed, the difference is just from the extra `IntoDiagnosticMessage`/`Display` call rather than any `Vec` allocations.

---

_Marked ready for review by @ntBre on 2025-07-21 15:44_

---

_Review requested from @carljm by @ntBre on 2025-07-21 15:44_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-21 15:44_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-21 15:44_

---

_Review requested from @sharkdp by @ntBre on 2025-07-21 15:44_

---

_Review requested from @dcreager by @ntBre on 2025-07-21 15:44_

---

_Review request for @dcreager removed by @ntBre on 2025-07-21 15:44_

---

_Review request for @carljm removed by @ntBre on 2025-07-21 15:44_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-21 15:44_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-21 15:44_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-21 15:48_

---

_Comment by @AlexWaygood on 2025-07-21 15:49_

> I also added `Severity::Help` to render the fix suggestion with a `help:` prefix
> instead of `info:`.

Nice -- I've wanted this for a while for ty's diagnostics, but never got round to writing up the issue! :-)

---

_Comment by @MichaReiser on 2025-07-21 15:54_

> Nice -- I've wanted this for a while for ty's diagnostics, but never got round to writing up the issue! :-)

Was it `help` or `note`? Trying to remember...


>  In turn, it adds the secondary/noqa code as the new primary annotation message.

It's not clear to me if this is indeed better. I find the code mostly redudant as it's already rendered in the diagnostic summary. But it's hard to judge from the changes in this PR. Can you show how the full diagnostic rendering is changing and what you propose to be the final rendering?

---

_Comment by @ntBre on 2025-07-21 15:56_

> It's not clear to me if this is indeed better. I find the code mostly redudant as it's already rendered in the diagnostic summary.

I kind of agree, and that would probably fix the benchmark regression too! I'll revert that part

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 15:57_

I find it very hard to judge which methods are Ruff specific and which aren't. Can we update the doc comments and ideally prefix the ruff specific methods?

---

_Comment by @AlexWaygood on 2025-07-21 15:58_

> Was it `help` or `note`? Trying to remember...

`note` might also be useful in some situations, but `help` would I think be perfect for things like this, where we're trying to steer the user directly towards a potential solution:

https://github.com/astral-sh/ruff/blob/5cace28c3ecc29f050aa5a57c8659e3154e3211e/crates/ty_python_semantic/src/types/infer.rs#L4749-L4751

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:1154 on 2025-07-21 16:00_

I'm a bit torn if we want `Help` as a `Diagnostic` severity or only allow it at the sub-diagnostic level/annotations. Curious what @BurntSushi thinks

---

_@MichaReiser reviewed on 2025-07-21 16:01_

I don't think I've the full context to review this PR just yet. My main concern is that we over emphesize on not changing the output format vs picking the format that's most intuitive.

---

_Comment by @ntBre on 2025-07-21 16:09_

> Can you show how the full diagnostic rendering is changing and what you propose to be the final rendering?

Full diagnostic output from `annotate-snippets` in this PR:

``` 
error[unused-import]: `os` imported but unused
  --> fib.py:1:8
   |
 1 | import os
   |        ^^ F401
   |
 help: Remove unused import: `os`
```

Current Ruff output for the same code:

```
fib.py:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
  |
  = help: Remove unused import: `os`
```

Proposed final output after #19415:

``` 
F401 [*] `os` imported but unused
  --> fib.py:1:8
   |
 1 | import os
   |        ^^ F401
   |
 help: Remove unused import: `os`
```

The main difference from the current Ruff diagnostic is that the filename is on the second line, and the main/only difference from current ty diagnostics is `F401 [*]` replacing `error[unused-import]`.

Maybe it was a bad idea to split this out from #19415 since the final product is a combination of the two. I just thought it might be a little easier to do this mostly-internal rearrangement separately.


---

_Comment by @MichaReiser on 2025-07-21 16:14_

Thanks. I do appreciate this being a separate PR. It just requires a few more details in the PR description to review it.

```
F401 [*] `os` imported but unused
  --> fib.py:1:8
   |
 1 | import os
   |        ^^ F401
   |
 help: Remove unused import: `os`
```

I think the repeated `F401` makes ltitle sense. It also prevents any lint to set a more specific message for the primary annotation (which is a very prominent and useful message!). 

Keeping `suggestion` as a sub diagnostic does make sense to me

---

_@ntBre reviewed on 2025-07-21 16:19_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 16:19_

Hmm, I see what you mean, but this isn't necessarily Ruff-specific, unless we think we would store the fix suggestion in a different part of the `Diagnostic` for ty (or not have "fix suggestions" in the same way). I can definitely add a note about this only currently being used by Ruff, if that would help, or rename it anyway.

The only other ambiguous method that I saw was `Diagnostic::body`, which is just an alias for `Diagnostic::primary_message`. It might help to delete that one now.

---

_@MichaReiser reviewed on 2025-07-21 16:29_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 16:29_

I think the method is misleading because the first sub diagnostic isn't guaranteed to always be the fix suggestion. It can also be a second diagnostic that shows a second code frame (e.g. to highlight the definition side of a missing argument). 

Overall, `Diagnostic` is intentionally unstructured to give rules a very flexible framework on how they want to structure their diagnostics and I suspect that we'll move Ruff into a similar direction. 

This does raise the question on how we'll handle `suggestions` in the output formats that currently emit them as a separate field. We may need a different way to mark those.

---

_@MichaReiser reviewed on 2025-07-21 16:33_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 16:33_

This discussion makes me realize that we'll need to rework the cache before Ruff can make use of the new `Diagnostic` features. The cache only stores exactly the fields required by `ViolationMetadata`. This means, any extra field will be lost after deserializing the message again. Instead, what the cache should do is serialize/deserialize the entire `Diagnostic` as is.

We also need to review all other places where `suggestion` is used to ensure they don't make any assumption about how a diagnostic is structured.

---

_@MichaReiser reviewed on 2025-07-21 16:41_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 16:41_

I would probably rename the method to `first_help_text` and clearly document that it is the message of the first sub-diagnostic with a `Help` severity and that this is used as the fix text in some of Ruff's output formats. 

Making the `Caching` so that it can restore any diagnostic is a follow up task. 

---

_@ntBre reviewed on 2025-07-21 17:09_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:387 on 2025-07-21 17:09_

Thanks, that makes a lot of sense. I'll rename and document it as you suggested and follow up on the caching separately.

I looked at the references to `suggestion`, and they all assume that this will be the fix suggestion, but I don't think they make any other assumption about the diagnostic's structure.

```
crates/ruff/src/cache.rs
457:                    suggestion: msg.suggestion().map(ToString::to_string),
crates/ruff_db/src/diagnostic/mod.rs
388:    pub fn suggestion(&self) -> Option<&str> {
crates/ruff_db/src/diagnostic/render/json.rs
90:        message: diagnostic.suggestion(),
crates/ruff_linter/src/message/text.rs
189:        let suggestion = self.message.suggestion();
crates/ruff_linter/src/test.rs
275:                !(fixable && diagnostic.suggestion().is_none()),
crates/ruff_server/src/lint.rs
241:    let suggestion = diagnostic.suggestion();
crates/ruff_wasm/src/lib.rs
237:                    message: msg.suggestion().map(ToString::to_string),
```

Fortunately there aren't as many as I expected, and the ones in the cache and `text.rs` should go away soon, it sounds like.

---

_@MichaReiser approved on 2025-07-22 06:14_

Thanks. This looks good to me now. 

The only thing I suggest changing is to add a `SubDiagnosticSeverity` (or similar) which is the same as `Severity` but it also has a `Help` variant. 

This is a bit annoying but it allows us to skip the decision if we neeed/want a `Help` severity at the diagnostic level, which we don't need now.

---

_Merged by @ntBre on 2025-07-22 14:03_

---

_Closed by @ntBre on 2025-07-22 14:03_

---

_Branch deleted on 2025-07-22 14:03_

---

_@BurntSushi reviewed on 2025-07-22 16:27_

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1154 on 2025-07-22 16:27_

Ah sorry, just catching up to this now. I would have been fine adding this to `Severity`. :-) But the extra type gives us a bit more precision, so that's nice.

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/mod.rs`:1199 on 2025-07-22 16:29_

It might be helpful to add a comment here explaining that this type was added to represent the additional `Help` variant that _isn't_ present on `Severity`. And that, in particular, if we wanted to add `Severity::Help`, it would be fine to delete this type and move back to one type.

I say this because it clarifies that `Help` is the only reason for this type's existence, and that there isn't some other need for it. So hopefully it's easier for someone down the line to remove it if we do add `Severity::Help`.

---

_@BurntSushi reviewed on 2025-07-22 16:29_

---

_@ntBre reviewed on 2025-07-22 16:48_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/mod.rs`:1199 on 2025-07-22 16:48_

Oh good idea. I'll sneak this into #19398 since I haven't merged it yet. Thanks for the reviews!

Let me know if you find anything else. I'll be working in the same area on `full` output next.

---
