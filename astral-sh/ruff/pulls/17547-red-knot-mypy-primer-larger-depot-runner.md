```yaml
number: 17547
title: "[red-knot] mypy_primer: larger depot runner"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-larger-depot-runner
created_at: 2025-04-22T12:16:32Z
updated_at: 2025-04-23T22:38:18Z
url: https://github.com/astral-sh/ruff/pull/17547
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] mypy_primer: larger depot runner

---

_@sharkdp_

## Summary

A switch from 16 to 32 cores reduces the `mypy_primer` CI time from 3.5-4 min to 2.5-3 min. There's also a 64-core runner, but the 4 min -> 3 min change when doubling the cores once does suggest that it doesn't parallelize *this* well.

---

_Label `ci` added by @sharkdp on 2025-04-22 12:16_

---

_Label `red-knot` added by @sharkdp on 2025-04-22 12:16_

---

_Comment by @github-actions[bot] on 2025-04-22 12:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-04-22 12:24_

---

_Comment by @AlexWaygood on 2025-04-22 12:30_

I wonder if we should experiment with passing `--concurrency=1` to mypy_primer. red-knot uses multithreading internally, so the concurrency that mypy_primer uses might just be leading to thread contention without any extra speedup? By default, mypy_primer spawns as many processes as there are cores available: https://github.com/hauntsaninja/mypy_primer/blob/ebaa9fd27b51a278873b63676fd25490cec6823b/mypy_primer/globals.py#L208-L214

---

_Comment by @sharkdp on 2025-04-22 12:34_

> I wonder if we should experiment with passing `--concurrency=1` to mypy_primer. red-knot uses multithreading internally, so the concurrency that mypy_primer uses might just be leading to thread contention without any extra speedup?

Certainly worth doing an experiment, but note that we profit massively from large concurrency during the IO-heavy setup stage (cloning + dependency installation). That would suggest that we should rather limit the number of cores for our red knot runs. Or just hope that the OS scheduler does a good job.

---

_Comment by @AlexWaygood on 2025-04-22 12:35_

Right, we could experiment with setting `RAYON_NUM_THREADS` in the environment for the github workflow, which would disable red-knot's concurrency while keeping mypy_primer's?

---

_@AlexWaygood approved on 2025-04-22 12:38_

Seems fine to me, but I don't know how much upgrading to the larger runner might cost us financially (@zanieb?)

---

_Comment by @sharkdp on 2025-04-22 14:38_

> but I don't know how much upgrading to the larger runner might cost us financially

I looked at [this table](https://depot.dev/docs/github-actions/runner-types#ubuntu-2204-runners) which shows that the -32 runners have a 16x minutes-multiplier, compared to the 8x multiplier for the -16 runners. So it looks to me like they're twice as expensive, but we only use them for ~75% of the time. So probably 50% more expensive overall.

Edit: I'm merging this with the assumption that this is not a significant raise in CI costs, but let me know if this is a concern, and we can simply roll it back.

---

_Comment by @carljm on 2025-04-22 15:30_

I wouldn't want to drop `RAYON_NUM_THREADS` all the way to 1, because if we have multi-threading bugs (especially ones that don't repro reliably), flaky failures of mypy-primer is one of our best ways to see that they are happening.

---

_Merged by @sharkdp on 2025-04-22 15:36_

---

_Closed by @sharkdp on 2025-04-22 15:36_

---

_Branch deleted on 2025-04-22 15:36_

---

_Comment by @zanieb on 2025-04-22 18:01_

In general, I'm supportive of biasing towards spending money to speed up CI :) I'll keep an eye on the total spend.

---

_Comment by @AlexWaygood on 2025-04-23 22:38_

> I wonder if we should experiment with passing `--concurrency=1` to mypy_primer. red-knot uses multithreading internally, so the concurrency that mypy_primer uses might just be leading to thread contention without any extra speedup? By default, mypy_primer spawns as many processes as there are cores available: [hauntsaninja/mypy_primer@`ebaa9fd`/mypy_primer/globals.py#L208-L214](https://github.com/hauntsaninja/mypy_primer/blob/ebaa9fd27b51a278873b63676fd25490cec6823b/mypy_primer/globals.py#L208-L214)

I experimented with this change to mypy_primer locally to see if it sped up running mypy_primer with red-knot at all (the idea is to keep mypy_primer's concurrency for the setup stage but limit it to one subprocess actually running red-knot at any one time, since red-knot internally uses concurrency anyway):

```diff
--- a/mypy_primer/model.py
+++ b/mypy_primer/model.py
@@ -319,6 +319,7 @@ class Project:
             check=False,
             cwd=ctx.get().projects_dir / self.name,
             env=env,
+            use_checker_lock=True,
         )
         if ctx.get().debug:
             debug_print(f"{Style.BLUE}{knot} on {self.name} took {runtime:.2f}s{Style.RESET}")
diff --git a/mypy_primer/utils.py b/mypy_primer/utils.py
index 8d2aacd..cb60595 100644
--- a/mypy_primer/utils.py
+++ b/mypy_primer/utils.py
@@ -67,6 +67,7 @@ def debug_print(obj: Any) -> None:
 
 
 _semaphore: asyncio.Semaphore | None = None
+_checker_lock = asyncio.Lock()
 
 
 async def run(
@@ -75,6 +76,7 @@ async def run(
     shell: bool = False,
     output: bool = False,
     check: bool = True,
+    use_checker_lock: bool = False,
     **kwargs: Any,
 ) -> tuple[subprocess.CompletedProcess[str], float]:
     if output:
@@ -87,7 +89,7 @@ async def run(
     global _semaphore
     if _semaphore is None:
         _semaphore = asyncio.BoundedSemaphore(ctx.get().concurrency)
-    async with _semaphore:
+    async with _checker_lock if use_checker_lock else _semaphore:
         if ctx.get().debug:
             log = cmd if shell else shlex.join(cmd)
             log = f"{Style.BLUE}{log}"
```

Unfortunately it seems like it just slows mypy_primer down rather than speeding it up at all!

---
