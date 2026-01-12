```yaml
number: 17371
title: "first run after `uv sync` is ~20x slower in MacOs; subsequent runs fast (not reproduced on Linux / miniconda)"
type: issue
state: open
author: joamatab
labels:
  - question
assignees: []
created_at: 2026-01-09T02:16:47Z
updated_at: 2026-01-10T16:11:21Z
url: https://github.com/astral-sh/uv/issues/17371
synced_at: 2026-01-12T02:26:26Z
```

# first run after `uv sync` is ~20x slower in MacOs; subsequent runs fast (not reproduced on Linux / miniconda)

---

_Issue opened by @joamatab on 2026-01-09 02:16_

### Question

### Summary
We’re seeing a large “cold start” penalty on **macOS** when running a minimal repro via `make test`. The **second** run is consistently much faster. This does **not** reproduce on Linux, and we also don’t see it when running the same package in **miniconda**.

I’m trying to understand what in the `uv` environment on macOS could be making the first invocation so slow (import/bytecode compilation/linking/dylib validation/indexing/quarantine, etc.).

### Repro
1. `uv sync` (fresh env)
2. `make test`  ← slow on first run
3. `make test`  ← fast on second run
4. `make test2` ← slow again (re-triggers the “first run” behavior)

> `make test` is the smallest repro to observe the behavior.
> `make test2` is the smallest repro to observe the behavior again after it has “warmed up”.

### Observed timings

#### macOS
- ~50 seconds on first run
- ~10 seconds on second run

#### Linux
- ~10 seconds on first run
- ~10 seconds on second run

### Expected
Similar cold-start behavior across platforms, or at least no ~5x penalty on the first run on macOS.

### Not observed in miniconda
Running the same package in a miniconda environment does **not** show this large first-run penalty.  
Why would the macOS + `uv` environment exhibit this, but miniconda does not?

### Questions
- Any known macOS-specific causes for a large first-run penalty with `uv` environments?
- Does `uv` do anything on first execution that could trigger expensive work on macOS (e.g., bytecode compilation strategy, shared library loading/validation, codesign/quarantine checks, Spotlight/AV scanning, etc.)?
- Any recommended debugging steps to pinpoint where the time is going (e.g., env vars, `uv` verbosity flags, tracing tools you recommend)?

### Environment
- OS: macOS (repro), Linux (no repro)
- Tool: uv
- (I can add exact `uv --version`, Python version, and machine details if needed.)




### Platform

Darwin 25.2.0 arm64

### Version

uv 0.9.22 (82a6a66b8 2026-01-06)

---

_Label `question` added by @joamatab on 2026-01-09 02:16_

---

_Comment by @konstin on 2026-01-09 11:05_

You already named the two prime suspects: Bytecode compilation and gateakeeper. You can test the first one by running the installation with `--bytecode-compile`, and the second one by deactivating gatekeeper for the directory (see e.g. https://nexte.st/docs/installation/macos/#gatekeeper, don't forget to restart). 40s is a lot though, could it be there's some library donwloading large artifacts on first run or doing something similarly slow?

---

_Comment by @joamatab on 2026-01-09 15:47_

Thanks — I tried both of those.

- Installed with `--bytecode-compile`
- Disabled Gatekeeper for the directory (and restarted)

Unfortunately, neither changed the behavior. The first run is still ~20s, while the second run is ~1s (≈20× faster). 
If there are any uv-specific tracing flags, environment variables, or recommended macOS tools you’d suggest to pinpoint where those ~50 seconds are going, I’m happy to dig deeper and report back.

See table below with a simplified package


Summary Table: Cold Start Test Results (macOS)
### Summary Table: Cold Start Test Results (macOS)

| Test  | Strategy                     | 1st Run | 2nd Run | Ratio | Bytecode Pre-compiled              |
|-------|------------------------------|---------|---------|-------|------------------------------------|
| test  | uv sync (baseline)            | 21.66s  | 1.51s   | 14×   | No                                 |
| test2 | uv sync + compileall          | 20.69s  | 1.00s   | 21×   | Yes (post-sync)                    |
| test3 | uv run compileall .venv       | 19.42s  | 0.93s   | 21×   | Yes (post-sync)                    |
| test4 | uv sync --compile-bytecode    | 20.65s  | 1.28s   | 16×   | Yes (during sync, ~3.1s overhead)  |
| test5 | miniconda    | 1.88s  | 1.36s   | Same  |

  ---
  Key Findings

  1. Bytecode compilation is NOT the bottleneck - Pre-compiling .pyc files (test2, test3, test4) does not reduce the first-run time. The ~20s penalty persists regardless of whether bytecode is pre-compiled.
  2. CPU time vs wall time disparity - First run shows ~2.77s user + 0.67s sys = ~3.4s CPU time, but 21.66s wall time. This ~18s gap indicates the process is waiting on something external, not doing CPU work.
  3. Second run is consistently fast - All tests show sub-2s second runs with user+sys closely matching wall time, indicating no external blocking.
  4. Something is cached at the OS/system level between runs - The "warming" effect persists across different bytecode strategies, suggesting it's not Python-related but macOS-specific.

 
  The large wall-time vs CPU-time gap (18+ seconds of waiting) strongly suggests macOS code signing/security verification on the many native extensions (numpy, scipy, pandas, matplotlib, klayout, etc.) that gdsfactory depends on.


---

_Renamed from "first run after `uv sync` is ~5x slower in MacOs; subsequent runs fast (not reproduced on Linux / miniconda)" to "first run after `uv sync` is ~20x slower in MacOs; subsequent runs fast (not reproduced on Linux / miniconda)" by @joamatab on 2026-01-09 15:57_

---

_Comment by @joamatab on 2026-01-09 15:57_

BTW: some GDSFactory Windows users have reported similar issues and we believe is related to windows defender

---

_Comment by @zanieb on 2026-01-09 15:58_

You can run with `UV_LOG_CONTEXT=1` and `-vvv` for timestamps on logs

---

_Comment by @konstin on 2026-01-09 16:00_

Is the time spent in uv or in python code? For the format, you can use `-vvv` like zanie said, for the latter, I recommend starting the code with py-spy and see what's taking so much time.

---

_Comment by @woodruffw on 2026-01-09 16:19_

> BTW: some GDSFactory Windows users have reported similar issues and we believe is related to windows defender

Are you using the Defender endpoint product on macOS? That would be a very strong candidate as well; it's common for EDRs that do file scanning to introduce significant latency/runtime costs in development tools.

---

_Comment by @joamatab on 2026-01-09 16:28_

Hi Konstin, 

the time is spent on python code when using UV enviroment

Hi William,

I did some research with claude-code

 The Problem

  macOS performs per-inode code signature validation on first load of any .so/.dylib file. This is a security feature that cannot be disabled.

  Why uv is slow on macOS (first run)

  1. uv sync downloads packages and extracts them to ~/.cache/uv/archive-v0/
  2. By default, uv copies files from cache to .venv/ (new inodes)
  3. Each .so file gets a new inode → requires fresh macOS validation
  4. 344 native libraries × ~0.05-0.15s each = ~20-30s cold start

  Why miniconda is fast

  - /Users/j/miniconda3/ is a long-lived directory
  - All .so files have already been validated by macOS
  - The validation is cached per-inode indefinitely

  Why second run is fast

  - macOS caches the validation result per-inode
  - Same files = same inodes = instant validation

  ---
  Test Results Summary

### Scenario Comparison (macOS)

| Scenario                                   | 1st Run | 2nd Run | Notes                           |
|--------------------------------------------|---------|---------|---------------------------------|
| uv sync (default copy mode)                | 25.4s   | 1.5s    | New inodes, cold validation     |
| uv sync --link-mode hardlink (cold cache)  | 25.4s   | 1.5s    | Cache is cold, still slow       |
| uv sync --link-mode hardlink (warm cache)  | 5.1s    | 1.2s    | Shares cached validation        |
| miniconda                                  | 1.5s    | 1.1s    | Already validated               |

---

### Top Slow Packages (First Load)

| Package | Time  | Files | Notes                |
|---------|-------|-------|----------------------|
| scipy   | 15.3s | 114   | Largest contributor  |
| skimage | 7.0s  | 54    |                      |
| pandas  | 5.7s  | 44    |                      |
| klayout | 3.6s  | 21    |                      |
| PIL     | 3.1s  | 8     |                      |
| numpy   | 2.8s  | 19    |                      |

  ---
  Solution

  Use --link-mode hardlink and keep the cache warm:

  # In your shell profile or before first run:
  export UV_LINK_MODE=hardlink

  # Or per-command:
  uv sync --link-mode hardlink

  This ensures:
  1. .venv files share inodes with the cache
  2. Once any package is validated (in cache or .venv), all hardlinked copies are instantly validated
  3. Recreating .venv reuses the cached validation

  ---
  Remaining ~5s overhead

  The 5.1s "warm cache" first run vs 1.2s second run is due to:
  - Python bytecode compilation (if not pre-compiled)
  - One-time module initialization
  - JIT warmup in numpy/scipy

  This can be reduced with uv sync --link-mode hardlink --compile-bytecode.


I added this

```
test6:
	echo 'using uv sync --link-mode hardlink (SOLUTION: shares macOS dylib validation cache)'
	rm -rf build .venv
	uv sync --link-mode hardlink
	time uv run --no-sync python mycspdk/sample1.py
	time uv run --no-sync python mycspdk/sample1.py
```

which cuts the start from 20x slower to 5x slower


---

_Comment by @zanieb on 2026-01-09 16:41_

> By default, uv copies files from cache to .venv/ (new inodes)

Note we use reflinks or clones, not copies. This does get a new inode though.

Switching to hardlinks seems appropriate, but note you'll lose copy-on-write semantics.

---

_Comment by @woodruffw on 2026-01-09 19:04_

> macOS performs per-inode code signature validation on first load of any .so/.dylib file. This is a security feature that cannot be disabled.

Could you see if enabling your terminal (or IDE) as a "developer tool" helps a bit here?

You can do that by running:

```
spctl developer-mode enable-terminal
```

and then going to `System Preferences > Security & Privacy > Privacy > Developer Tools` and enabling whatever tools you're running your uv commands through. I *believe* that will bypass any Gatekeeper checks on things like development binaries.

For example, here's what a Developer Tools configuration might look like:

<img width="508" height="366" alt="Image" src="https://github.com/user-attachments/assets/45793d1b-c0fe-4cc5-a195-8d417958df2b" />



---

_Comment by @joamatab on 2026-01-10 13:54_

Hi Zanie,

at least I can reduce the slowdown from ~20× to ~5×.

I’m still puzzled why we don’t see this behavior on Linux; it’s been an interesting issue to dig into.


Hi William.

I tried but didn't improve the speed, thanks for the suggestion though




---

_Comment by @joamatab on 2026-01-10 15:37_

I added in my .bashrc
export UV_LINK_MODE=hardlink

and now the delay is tolerable

I also forgot to mention that all this started since I updated to the latest Tahoe MacOs 

---

_Comment by @zanieb on 2026-01-10 16:11_

> I’m still puzzled why we don’t see this behavior on Linux

Linux doesn't have invasive security features like this.

---
