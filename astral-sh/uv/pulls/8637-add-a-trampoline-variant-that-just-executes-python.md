```yaml
number: 8637
title: "Add a trampoline variant that just executes `python`"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/bounce-python
created_at: 2024-10-28T15:57:06Z
updated_at: 2024-10-30T00:26:58Z
url: https://github.com/astral-sh/uv/pull/8637
synced_at: 2026-01-12T16:08:24Z
```

# Add a trampoline variant that just executes `python`

---

_@zanieb_

Currently, our trampoline is used to convert `<command> [args]` to `python <command> [args]` for script entrypoints installed into virtual environments. For #8458, it'd be nice to convert a shim `python3.12 [args]` to `python [args]`. Here, we modify the trampolines to support this use-case.

The only change we really need here is to avoid injecting `<command>` into the child process. We change the "magic number" at the end of the trampoline executables from `UVUV` to `UVSC` and `UVPY` which define "script" and "python" variants to the trampoline. We then omit the `<command>` injection in the latter case. We also omit writing the zip script payload when constructing the trampoline.

To support construction of the new variant, a new `uv-trampoline-builder` crate is introduced — this avoids requirements on `uv-install-wheel` in future work. I also use `uv-trampoline-builder` to consolidate some of the test setup for `uv-trampoline`.

There should be no backwards compatibility concerns, since trampolines are fully self-referential.

I rebased to fix the commits at the end, as this took many iterations to get working via CI. This should roughly be reviewable by commit if you prefer. 

---

_@konstin reviewed on 2024-10-28 16:05_

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:56 on 2024-10-28 16:05_

we need to add `UVUV` here to preserve compatibility with environments created pre-0.5.

---

_@zanieb reviewed on 2024-10-28 16:08_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:56 on 2024-10-28 16:08_

Why's that? Isn't the trampoline always self-contained?

---

_@konstin reviewed on 2024-10-28 16:09_

---

_Review comment by @konstin on `crates/uv-trampoline/src/bounce.rs`:56 on 2024-10-28 16:09_

oh right, because we won't actually be cross reading newer entrypoints

---

_Review requested from @konstin by @zanieb on 2024-10-28 19:30_

---

_Review requested from @BurntSushi by @zanieb on 2024-10-28 19:30_

---

_Marked ready for review by @zanieb on 2024-10-28 19:38_

---

_@zanieb reviewed on 2024-10-28 19:56_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/wheel.rs`:166 on 2024-10-28 19:56_

This moved ~unchanged into `uv-trampoline-builder`

---

_@zanieb reviewed on 2024-10-28 19:56_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/wheel.rs`:1080 on 2024-10-28 19:56_

This moved into `uv-trampoline-builder`, size limit bumped

---

_@zanieb reviewed on 2024-10-28 19:56_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:183 on 2024-10-28 19:56_

These tests pulled from `uv-trampoline::tests::harness` and `uv-install-wheel`

---

_@zanieb reviewed on 2024-10-28 19:57_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:153 on 2024-10-28 19:57_

This is new

---

_@zanieb reviewed on 2024-10-28 19:57_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:104 on 2024-10-28 19:57_

This was copied ~unchanged

---

_@zanieb reviewed on 2024-10-28 19:57_

---

_Review comment by @zanieb on `crates/uv-trampoline/tests/harness.rs`:1 on 2024-10-28 19:57_

This moved ~unchanged into `uv-trampoline-builder`

---

_Renamed from "Add a trampoline variant that executes `python`" to "Add a trampoline variant that just executes `python`" by @zanieb on 2024-10-28 21:57_

---

_Comment by @samypr100 on 2024-10-28 23:21_

> The trampolines increased from ~45kb to ~80kb

Weird, I noticed none of the code added should've increased it this much

Decided to build it locally (release) it's still in the ~45kb. We should probably update the sizes test to keep using the release build size, not the dev size 😅 

<img width="485" alt="bounce_build" src="https://github.com/user-attachments/assets/015e26b3-1b0b-41dc-b7e7-f8f1a20183d6">

Dev Sizes were about ~80kb



---

_Comment by @zanieb on 2024-10-28 23:40_

Ohhh I see what's happening here. I was also surprised they increased in size. Thanks for pointing that out. That's... annoying to fix. 

---

_@konstin approved on 2024-10-29 08:36_

---

_Review comment by @BurntSushi on `crates/uv-install-wheel/src/wheel.rs`:38 on 2024-10-29 11:41_

I believe you can just write `b"UVSC"` and `b"UVPY"` here. (Fun fact, byte string literals have type `&'static [u8; N]` where as string literals have type `&'static str`.)

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:51 on 2024-10-29 11:46_

Byte string literals should work here too (as above).

---

_Review comment by @BurntSushi on `crates/uv-trampoline/src/bounce.rs`:51 on 2024-10-29 11:52_

Ah whoops, you already did this. :P 

---

_Review comment by @BurntSushi on `crates/uv-trampoline-builder/src/lib.rs`:202 on 2024-10-29 11:54_

Would using `cfg(not(debug_assertions))` as a proxy for "compiling with optimizations" help here? Otherwise, you can't `cfg` on `opt-level` at present. So probably the only alternative is a Cargo feature. (Assuming I'm understanding the problem correctly.)

---

_@BurntSushi approved on 2024-10-29 11:54_

LGTM. How did you resolve the test failures from yesterday?

---

_@zanieb reviewed on 2024-10-29 13:45_

---

_Review comment by @zanieb on `crates/uv-install-wheel/src/wheel.rs`:38 on 2024-10-29 13:45_

Yeah I was following the existing format but Clippy put me on the right path here :)

---

_@zanieb reviewed on 2024-10-29 13:46_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:202 on 2024-10-29 13:46_

That'd be interesting. I can also just.. run these tests separately from the ones we run to test changes to the trampolines. In CI, we replace the production trampolines with a debug build.

---

_@konstin reviewed on 2024-10-29 13:52_

---

_Review comment by @konstin on `crates/uv-trampoline-builder/src/lib.rs`:202 on 2024-10-29 13:52_

Is the threshold bump still relevant? In the git diff it looks like the binaries even shrank a little 

---

_@zanieb reviewed on 2024-10-29 13:55_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:202 on 2024-10-29 13:55_

Since I moved the tests, they run against the debug builds we create for testing.

I can fix that somehow, or we can just assert on the size of the debug builds (80kb is still very reasonable).

---

_Comment by @zanieb on 2024-10-29 13:59_

@BurntSushi I resolved the test failures by moving the tests from `uv-trampoline/src/lib.rs` into `uv-trampoline-builder/src/lib.rs`. Still no idea what was going on there.

---

_@zanieb reviewed on 2024-10-29 14:00_

---

_Review comment by @zanieb on `crates/uv-trampoline/src/bounce.rs`:51 on 2024-10-29 14:00_

Sorry; this is one of those things that wasn't worth moving in the rebase since it was a mess

---

_Comment by @zanieb on 2024-10-29 14:20_

Now we test both explicitly

<img width="1373" alt="Screenshot 2024-10-29 at 9 19 54 AM" src="https://github.com/user-attachments/assets/b7471a62-0d93-4934-ab2f-6d4f8898c795">


---

_@zanieb reviewed on 2024-10-29 14:20_

---

_Review comment by @zanieb on `crates/uv-trampoline-builder/src/lib.rs`:202 on 2024-10-29 14:20_

See https://github.com/astral-sh/uv/pull/8637#issuecomment-2444412153 https://github.com/astral-sh/uv/pull/8637/commits/cbb90bad697c04aa68be259fa36e34fe847c208a

---

_Comment by @zanieb on 2024-10-29 14:21_

Thanks for the reviews!

---

_Merged by @zanieb on 2024-10-29 14:21_

---

_Closed by @zanieb on 2024-10-29 14:21_

---

_Branch deleted on 2024-10-29 14:21_

---

_Label `internal` added by @zanieb on 2024-10-29 14:21_

---
