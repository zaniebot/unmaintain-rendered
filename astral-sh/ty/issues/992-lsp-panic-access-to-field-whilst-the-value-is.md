```yaml
number: 992
title: "LSP Panic: access to field whilst the value is being initialized"
type: issue
state: open
author: MichaReiser
labels:
  - fatal
assignees: []
created_at: 2025-08-14T18:14:13Z
updated_at: 2025-12-31T16:36:56Z
url: https://github.com/astral-sh/ty/issues/992
synced_at: 2026-01-10T01:56:40Z
```

# LSP Panic: access to field whilst the value is being initialized

---

_Issue opened by @MichaReiser on 2025-08-14 18:14_

### Summary

```
LSP[ty] ty encountered a problem. Check the logs for more details.
lsp_signatur handler RPC[Error] code_name = InternalError, message = "request handler panicked at /Users/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/d66
fe33/src/tracked_struct.rs:912:21:\
access to field whilst the value is being initialized\
run with `RUST_BACKTRACE=1` environment variable to display a backtrace\
"
```

This is a bug in Salsa

### Version

_No response_

---

_Label `fatal` added by @MichaReiser on 2025-08-14 18:14_

---

_Comment by @MichaReiser on 2025-08-14 19:26_

This normally suggests that we access a tracked struct that has been deleted. I'm not entirely sure how we'd end up here other than that it's probably something, something fixpoint. @ibraheemdev do you have any idea why this might happen?

This might be https://github.com/salsa-rs/salsa/pull/969#issuecomment-3189567411

---

_Comment by @MichaReiser on 2025-08-14 19:30_

I think the reason this could be an issue for cycle heads only is that we never back date queries that have cycle heads (that is, queries that participated in a cycle but aren't the head). That means, any query dependent on those re-executes and sees any newly created tracked struct. 

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 12:30_

---

_Assigned to @MichaReiser by @carljm on 2025-08-15 15:39_

---

_Comment by @MichaReiser on 2025-08-15 18:12_

Okay, I think I have a fix for this. It's not related to my refactors (but I did discover something else that I broke). 

I believe the issue is that `deep_verify_memo` might return unchanged for a provisional memo from a previous iteration. This seems wrong, at least if the cycle heads weren't finalized. Or we, at least, need to propagate the cycle heads because we may not hit the same cycle heads during verification 

---

_Comment by @MichaReiser on 2025-08-19 05:54_

There might be two reasons why this happening:

1. We access a tracked struct that was removed due to when salsa runs `diff_outputs` for queries participating in a cycle: https://github.com/salsa-rs/salsa/pull/980


2. I don't have a repro for the second case yet but let's say we have the following query:

  ```
  a -> b
    -> d(b.x) (reads interned returned by b)

  b -> c
    -> interns x

  c -> a
  ```

  The execution is as follow when `a` is the entry point:
  
  * `maybe_changed_after(a)` -> `deep_verify_memo(a)`
  * `maybe_changed_after(b)` -> `deep_verify_memo(b)`
  * `maybe_changed_after(c)` -> `deep_verify_memo(c)`
  * return fixpoint initial for `a` (unchanged)
  * `maybe_changed_after(x)`
  * `maybe_changed_after(d(b.x))` -> `deep_verify_memo(d(b.x))` (locks `b.x`)

  However, if Salsa now calls into `b` first, the execution is:

  * `maybe_changed_after(b)` -> `deep_verify_memo(b)`
  * `maybe_changed_after(c)` -> `deep_verify_memo(c)`
  * `maybe_changed_after(a)` -> `deep_verify_memo(a)`
  * return fixpoint initial for `b` (unchanged)
  * `maybe_changed_after(d(b.x))` -> `deep_verify_memo(d(b.x))` this panics because `b.x` hasn't been verified (we haven't fully walked `b`s dependencies yet)


The crucial difference to non-cyclic queries is that cyclic queries aren't guaranteed to form a tree; they can form a graph where different queries depend on each other's results from different iterations. The challenge with this is that `maybe_changed_after` is order sensitive AND it's not guaranteed that `maybe_changed_after` is guaranteed to be called on the same cycle head as when the value was computed in the last revision. 


I'm not sure if it would be okay to just call `maybe_changed_after` on the memo's `cycle heads from the last revision. I'd have to double-check if we don't run into any ordering issues 


---

_Comment by @MichaReiser on 2025-08-20 15:19_

I'm starting to consider rewriting the fix point implementation to store a separate memo for each iteration instead of aggregating the results of all iterations into a single `Memo`. The aggregation adds a fair amount of complexity and I suspect that things like `validate_same_iteration` could become much easier if we have separate memos. 

Separate memos would also guarantee that `maybe_changed_after` remains a tree walk and not a graph traversal as it is today. It may even allow us to enable backdating for queries participating in cycles and verifying them eagerly in `maybe_changed_after`. 

We did discuss this a bit in today's salsa meeting: [#Contributing to Salsa > 2025-08-20 meeting @ 💬](https://salsa.zulipchat.com/#narrow/channel/333573-Contributing-to-Salsa/topic/2025-08-20.20meeting/near/535333528)

One challenge that I see with this is how to store multiple memo's on a single argument: From my conversation with @ibraheemdev 

> the memos are type-erased, so  I think you could store a different type of memo for cycle queries that included multiple iterations

oh, like a super memo that has a boxcar vec with the memos of each iteration?

> something like that yeah


---

_Comment by @MichaReiser on 2025-09-19 08:30_

Notes from another salsa meeting where we discussed how to fix this https://hackmd.io/L5wDAaqySqacXH1_yzwzXw

---

_Removed from milestone `Beta` by @MichaReiser on 2025-09-19 08:31_

---

_Added to milestone `GA` by @MichaReiser on 2025-09-19 08:31_

---

_Comment by @MichaReiser on 2025-10-15 14:34_

Some more notes: https://hackmd.io/@michareiser/S11NEQ6pll

---

_Comment by @AlexWaygood on 2025-10-15 14:37_

> Some more notes: [hackmd.io/@michareiser/S11NEQ6pll](https://hackmd.io/@michareiser/S11NEQ6pll)

<img width="934" height="922" alt="Image" src="https://github.com/user-attachments/assets/4b802b85-1f62-4938-83ce-2725fcb7bb3a" />

😢

---

_Comment by @MichaReiser on 2025-11-12 15:58_

We discussed two options in today's [salsa meeting](https://hackmd.io/SGYCIVxxTzCnAuzhLmXuzg) and the option 2 sounds promising. 

---

_Label `server` added by @carljm on 2025-11-13 01:10_

---

_Label `server` removed by @MichaReiser on 2025-11-16 11:34_

---

_Comment by @MichaReiser on 2025-11-16 11:36_

The new approach should greatly simplify `maybe_changed_after`, which is nice. The part that isn't clear to me yet is how `diff_outputs` is going to work, specifically when:

* `a` is a cycle head in revision 1. It stores the aggregated input/outputs of all queries participating in this cycle
* `a` becomes a normal query (or a nested cycle of another query) in revision 2

Simply removing all unused input/outputs in revision 2 feels incorrect because we may then end up removing the same inputs twice (once when `a` runs and a second time when the nested query that originally created that input/output runs). 



---

_Comment by @MichaReiser on 2025-11-16 18:01_

Some more thoughts:

* This new model requires an eager traversal over all cycle participants after each iteration. The fact that we do an eager traversal allows us to switch to an eager finalization of all cycle participants
* Fixing diff outputs is related to this change but separate. Each query should continue to own its own inputs/outputs, but we have to defer `diff_outputs` up to the finalization step. 
* We need to "back-up" the input/outputs from the last revision's memo for every cycle participant before overriding it with cycle initial (or the value from the first iteration)



---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-31 16:36_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 16:36_

---
