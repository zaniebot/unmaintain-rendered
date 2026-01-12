```yaml
number: 13299
title: Disable jemalloc decay in benchmarks
type: pull_request
state: merged
author: MichaReiser
labels:
  - ci
assignees: []
merged: true
base: main
head: micha/bench-disable-decay
created_at: 2024-09-10T01:19:36Z
updated_at: 2024-09-16T07:16:32Z
url: https://github.com/astral-sh/ruff/pull/13299
synced_at: 2026-01-12T15:55:43Z
```

# Disable jemalloc decay in benchmarks

---

_@MichaReiser_

## Summary

Another try to make our linter benchmarks more stable. 

I honestly don't understand much of it. But jemalloc has a system to yield
unused pages back to the OS. However, it doesn't do so immediately, instead, jemalloc
waits 10s before running the decay function that then shows up in profiles. 

<img width="870" alt="Screenshot 2024-09-09 at 21 43 32" src="https://github.com/user-attachments/assets/c55a2d66-f4b7-4128-8b7b-0c3ce919906c">



I don't understand why the decay very predictably happens in the same allocation in `flake8_annotations` and
mainly when running the all-preview rules benchmarks. So maybe this is another fruitless attempt. 

We may want to port this to other benchmarks in the future, but I first want to see if it does help to make the benchmarks more predictable

## Test Plan

The codspeed benchmark show improvements due to removing some `decay` stack frames in head (that exist in base)


---

_Comment by @github-actions[bot] on 2024-09-10 01:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `ci` added by @MichaReiser on 2024-09-10 01:51_

---

_Marked ready for review by @MichaReiser on 2024-09-10 01:59_

---

_Comment by @MichaReiser on 2024-09-10 18:32_

I'll merge this. Happy to revert if it turns out to have no impact

---

_Merged by @MichaReiser on 2024-09-10 18:32_

---

_Closed by @MichaReiser on 2024-09-10 18:32_

---

_Branch deleted on 2024-09-10 18:32_

---

_Comment by @GaetanLepage on 2024-09-14 22:22_

Interestingly, this seems to prevent us (nix package maintainers) to bump ruff to 0.6.5: https://github.com/NixOS/nixpkgs/pull/341674#issuecomment-2351172084

---

_Comment by @MichaReiser on 2024-09-15 06:59_

Hmm. Does the build succeed if you remove the added `extern`? Does the build succeed if you remove the `unprefixed_malloc_on_supported_platforms` in the `Cargo.toml`?

---

_Comment by @GaetanLepage on 2024-09-15 08:47_

It builds fine when only reverting the change on the `Cargo.lock`.
What do you mean by removing the "added `extern`" ?

---

_Comment by @MichaReiser on 2024-09-15 13:18_

> It builds fine when only reverting the change on the Cargo.lock.

Do you mean `Cargo.toml`?

>  What do you mean by removing the "added extern" ?

I'm referring to the code changes. But that's good to know. 

---

_Comment by @GaetanLepage on 2024-09-15 13:20_

> Do you mean `Cargo.toml`?

Yes indeed, sorry.

---

_Comment by @MichaReiser on 2024-09-15 13:23_

Okay. I think we can revert the added feature. We just need to rename the exported variable (I think it needs a leading underscore)

---

_Comment by @GaetanLepage on 2024-09-15 13:35_

> I'm referring to the code changes.

I have just done the test and I can confirm that reverting only the change in the source file but not the one in `Cargo.toml` doesn't fix the compilation issue.

---

_Comment by @MichaReiser on 2024-09-16 07:16_

I created https://github.com/astral-sh/ruff/pull/13366

---
