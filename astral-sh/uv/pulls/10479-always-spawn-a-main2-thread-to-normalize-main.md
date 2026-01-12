```yaml
number: 10479
title: Always spawn a main2 thread to normalize main stack size issues
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/stacked
created_at: 2025-01-10T20:11:00Z
updated_at: 2025-01-15T03:35:18Z
url: https://github.com/astral-sh/uv/pull/10479
synced_at: 2026-01-12T16:09:19Z
```

# Always spawn a main2 thread to normalize main stack size issues

---

_@Gankra_

Also removes UV_STACK_SIZE and uses RUST_MIN_STACK instead, tweaking docs to reflect the differences.

Fixes #10367 

---

_Comment by @Gankra on 2025-01-10 20:11_

I'll do a followup PR that just removes RUST_MIN_STACK from our CI to see if it's actually doing anything anymore.

---

_Comment by @charliermarsh on 2025-01-10 20:15_

Oh excellent

---

_Comment by @charliermarsh on 2025-01-10 20:23_

Is there any way for us to set this "automatically" like in `config.toml`?

---

_Comment by @Gankra on 2025-01-10 20:32_

> Is there any way for us to set this "automatically" like in config.toml?

Not to my knowledge, no. There's no build-time var, only this runtime var (read once in life-before-main aiui).

Our CI and tests set this aggressively on windows, and past comments suggest it "doesn't happen" on release builds (and would therefore be a memory waster to set?).

---

_@konstin reviewed on 2025-01-10 21:15_

---

_Review comment by @konstin on `crates/uv/src/lib.rs`:1834 on 2025-01-10 21:15_

How much does this change the overhead of something like `uv run echo "hi"` or `uv run python -c "print(\"hello world\")"`?

---

_Comment by @Gankra on 2025-01-10 21:15_

I am glad I finally understand why windows main thread stack size kept coming up even though our var can't change it (setting it also undocumentedly-forced us to make main2).

---

_@Gankra reviewed on 2025-01-10 21:22_

---

_Review comment by @Gankra on `crates/uv/src/lib.rs`:1834 on 2025-01-10 21:22_

looking now on windows, but it'll be platform-specific to say the least (could test linux in WSL2?)

---

_Comment by @Gankra on 2025-01-10 22:02_

Windows testing has this in the noise even for trivial ops where this can dominate:

![image](https://github.com/user-attachments/assets/d48c9e30-3b90-402b-ae91-b5d96665ce8a)
![image](https://github.com/user-attachments/assets/0bb4a4fd-c35a-4f7e-a6bb-528871a28592)


---

_Comment by @Gankra on 2025-01-14 16:42_

macos is similar
<img width="608" alt="Screenshot 2025-01-14 at 11 41 06 AM" src="https://github.com/user-attachments/assets/f105f7c8-9d3a-400a-aef0-f68f20686721" />
<img width="613" alt="Screenshot 2025-01-14 at 11 41 44 AM" src="https://github.com/user-attachments/assets/9731b680-252b-468a-a3aa-091d21de5e48" />


---

_Comment by @konstin on 2025-01-14 16:44_

linux doesn't differ much either:
```
$ hyperfine --runs 200 './uv-main run python -c "print(\"hello world\")"' './uv-stacked run python -c "print(\"hello world\")"'
Benchmark 1: ./uv-main run python -c "print(\"hello world\")"
  Time (mean ± σ):      14.8 ms ±   1.4 ms    [User: 8.8 ms, System: 6.0 ms]
  Range (min … max):    11.1 ms …  17.7 ms    200 runs
 
Benchmark 2: ./uv-stacked run python -c "print(\"hello world\")"
  Time (mean ± σ):      15.2 ms ±   1.3 ms    [User: 8.8 ms, System: 6.3 ms]
  Range (min … max):    11.9 ms …  17.8 ms    200 runs
 
Summary
  ./uv-main run python -c "print(\"hello world\")" ran
    1.02 ± 0.13 times faster than ./uv-stacked run python -c "print(\"hello world\")"
```

---

_@konstin approved on 2025-01-14 16:45_

---

_Comment by @Gankra on 2025-01-14 16:51_

Outstanding: the current impl actually "regresses" configurability by unconditionally setting a 4mb main2 thread, ignoring RUST_MIN_STACK. I should tweak the code to check if it's set and prefer that when it is (or maybe take the max so people don't accidentally break uv by making stack too small?).

---

_Comment by @Gankra on 2025-01-14 20:04_

> I'll do a followup PR that just removes RUST_MIN_STACK from our CI to see if it's actually doing anything anymore.

Decided to add this to this PR now that I have a better understanding of the constraints and ensure normalized size between OSes.

---

_Renamed from "remove UV_STACK_SIZE and use RUST_MIN_STACK instead" to "Always spawn a main2 thread to normalize main stack size issues" by @Gankra on 2025-01-14 20:05_

---

_Label `internal` added by @Gankra on 2025-01-14 20:06_

---

_Closed by @Gankra on 2025-01-15 03:05_

---

_Reopened by @Gankra on 2025-01-15 03:05_

---

_Closed by @Gankra on 2025-01-15 03:05_

---

_Reopened by @Gankra on 2025-01-15 03:07_

---

_Merged by @Gankra on 2025-01-15 03:35_

---

_Closed by @Gankra on 2025-01-15 03:35_

---

_Branch deleted on 2025-01-15 03:35_

---
