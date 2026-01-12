```yaml
number: 2706
title: Fine-tune the Ordering for the atomic usages
type: pull_request
state: closed
author: wang384670111
labels:
  - rollup
assignees: []
base: master
head: tune_ordering
created_at: 2024-01-08T08:25:14Z
updated_at: 2025-09-20T01:08:20Z
url: https://github.com/BurntSushi/ripgrep/pull/2706
synced_at: 2026-01-12T18:23:14Z
```

# Fine-tune the Ordering for the atomic usages

---

_@wang384670111_

`SeqCst` is overly restrictive. I believe that the ordering can be appropriately modified.

In `messages` model, I believe that `MESSAGES`, `IGNORE_MESSAGES`, and `ERRORED` are merely signals for multithreading purposes and do not synchronize with other locals. Therefore, using `Relaxed` ordering should suffice.

In `lib` and `utils` models, `COUNTER` and `NEXT_ID` are used for multithreading counting purposes, so using `Relaxed` ordering is sufficient.

In `walk` model, the `load` and `store` of `quit_now` should use `Acquire`/`Release` to ensure that the `stack` is up-to-date.

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:1720 on 2024-01-08 16:06_

I think this and the `Release` above need comments justifying why this is correct.

---

_@BurntSushi requested changes on 2024-01-08 16:08_

I used `SeqCst` in part because I didn't want to do the analysis necessary to demonstrate that a less restrictive ordering was correct, and also because I didn't perceive using `SeqCst` to be a particular issue in most of these cases.

With that said, I buy all of the changes to `Relaxed` here I think.

However, I'm unsure of the `Require`/`Release` changes in the `ignore` crate. I think those warrant comments explaining their correctness.

---

_Comment by @wang384670111 on 2024-01-09 09:58_

> I used `SeqCst` in part because I didn't want to do the analysis necessary to demonstrate that a less restrictive ordering was correct, and also because I didn't perceive using `SeqCst` to be a particular issue in most of these cases.
> 
> With that said, I buy all of the changes to `Relaxed` here I think.
> 
> However, I'm unsure of the `Require`/`Release` changes in the `ignore` crate. I think those warrant comments explaining their correctness.


My reasoning is based on the following observations:

 Line 1501 calls `quit_now` after calling `run_one` at line 1562, where `run_one` invokes `generate_work`. Inside `generate_work`, specifically at line 1655, the `send` performs a `push` on the `stack`. 
https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1498-L1504
https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1654-L1656
Then, after executing `is_quit_now` at line 1669, `send_quit` is called at line 1680 to access the `stack` and perform another `push`.
https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1669-L1682

Based on this sequence of operations, in the absence of a definitive `happen-before` relationship within a concurrent environment, I believe it is necessary to use `Acquire`/`Release` to establish synchronization, in order to properly observe these modifications to `stack`.

---

_Comment by @wang384670111 on 2024-01-10 16:00_

> > I used `SeqCst` in part because I didn't want to do the analysis necessary to demonstrate that a less restrictive ordering was correct, and also because I didn't perceive using `SeqCst` to be a particular issue in most of these cases.
> > With that said, I buy all of the changes to `Relaxed` here I think.
> > However, I'm unsure of the `Require`/`Release` changes in the `ignore` crate. I think those warrant comments explaining their correctness.
> 
> My reasoning is based on the following observations:
> 
> Line 1501 calls `quit_now` after calling `run_one` at line 1562, where `run_one` invokes `generate_work`. Inside `generate_work`, specifically at line 1655, the `send` performs a `push` on the `stack`.
> 
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1498-L1504
> 
> 
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1654-L1656
> 
> 
> Then, after executing `is_quit_now` at line 1669, `send_quit` is called at line 1680 to access the `stack` and perform another `push`.
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1669-L1682
> 
> Based on this sequence of operations, in the absence of a definitive `happen-before` relationship within a concurrent environment, I believe it is necessary to use `Acquire`/`Release` to establish synchronization, in order to properly observe these modifications to `stack`.



> > I used `SeqCst` in part because I didn't want to do the analysis necessary to demonstrate that a less restrictive ordering was correct, and also because I didn't perceive using `SeqCst` to be a particular issue in most of these cases.
> > With that said, I buy all of the changes to `Relaxed` here I think.
> > However, I'm unsure of the `Require`/`Release` changes in the `ignore` crate. I think those warrant comments explaining their correctness.
> 
> My reasoning is based on the following observations:
> 
> Line 1501 calls `quit_now` after calling `run_one` at line 1562, where `run_one` invokes `generate_work`. Inside `generate_work`, specifically at line 1655, the `send` performs a `push` on the `stack`.
> 
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1498-L1504
> 
> 
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1654-L1656
> 
> 
> Then, after executing `is_quit_now` at line 1669, `send_quit` is called at line 1680 to access the `stack` and perform another `push`.
> https://github.com/BurntSushi/ripgrep/blob/648a65f1976cc3b7eb66425024649d71d5befe1e/crates/ignore/src/walk.rs#L1669-L1682
> 
> Based on this sequence of operations, in the absence of a definitive `happen-before` relationship within a concurrent environment, I believe it is necessary to use `Acquire`/`Release` to establish synchronization, in order to properly observe these modifications to `stack`.

I've reconsidered my earlier deduction and realized I overlooked a crucial detail. 

Before calling `quit_now` at 1501, we have a successful return of `WalkState::Quit` at 1500. This indicates that the `stack` push operation, occurring at line 1655, has completed. This establishes a happen-before relationship, making it unnecessary to use `Release` in `quit_now` to ensure the completion of the `stack` push operation. Therefore, using `Relaxed` for both the store in `quit_now` and the subsequent load should suffice.

I would greatly appreciate your feedback on this observation. I'm eager for the potential merge and am ready to provide any further clarifications as needed.


---

_Comment by @BurntSushi on 2024-01-10 16:03_

It might take me some time to look into this. I basically need to convince myself of your argument.

If you want to get something merged more quickly, you could open a new PR with the less "interesting" changes and keep this PR focused on the changes to `ignore`'s parallel traversal.

---

_Label `rollup` added by @BurntSushi on 2025-07-04 14:23_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
