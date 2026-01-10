```yaml
number: 497
title: Streaming diagnostics
type: issue
state: open
author: sharkdp
labels:
  - cli
  - performance
assignees: []
created_at: 2025-05-23T14:26:41Z
updated_at: 2025-08-15T12:10:08Z
url: https://github.com/astral-sh/ty/issues/497
synced_at: 2026-01-10T02:06:24Z
```

# Streaming diagnostics

---

_Issue opened by @sharkdp on 2025-05-23 14:26_

This has been on my mind for some time. I also briefly discussed it with @MichaReiser some weeks ago. The progress bar that we have looks cool, but it would be even more useful if we could start showing diagnostics as early as possible [^1]. This reduces latency (time to first diagnostic), but also has the potential to reduce the overall runtime (time until all diagnostics are printed), I believe. This is because we could start with the IO-heavy work of printing out the diagnostics while the CPU-intense work is still ongoing, instead of waiting for it to be completely over [^2]. The obvious problem here is determinism. We currently sort all diagnostics before printing them. So we cannot simply send the diagnostics over a channel and print them out as soon as we generate them. But there might be some strategy where streaming is still possible. We know all files (that can produce diagnostics) before we start. So if we order diagnostics by file, we could print out all diagnostics of file `a/a/a.py` if no other file comes before it in the list of ordered files. It's questionable if this would work in practice though. `a/a/a.py` might be a file that depends on a lot of other files [^3]. It might therefore take a long time until we can start to even print out the diagnostics of the very first file.

Another option might be to give up on determinism in the output. Users probably value performance over a deterministic sort order. And we could still have a `--sorted` CLI flag or similar that restores determinism at the cost of performance.

[^1]: I think it's technically possible to do this in addition to showing the progress bar.
[^2]: Another thing that we could look into is to render the diagnostics on the worker threads (instead of the receiving main thread). Right now, if there are many diagnostics, we do quite a lot of single-threaded work at the end of a ty run.
[^3]: That actually makes me think if we can tune performance by changing the order in which we check files. Maybe it makes sense to check files in `deeply/nested/folders` first, as they are more likely to be dependencies of other files? Or the other way around?

---

_Label `cli` added by @sharkdp on 2025-05-23 14:26_

---

_Label `performance` added by @sharkdp on 2025-05-23 14:26_

---

_Comment by @BurntSushi on 2025-05-23 14:37_

AIUI, `rustc` prints diagnostics as they are found. I think there are some scenarios where they are buffered, but from what I could tell, they just stream right to stderr and generally aren't collected anywhere.

IDK if it makes sense to swing all the way to that model, but I think it's worth mentioning as a possible choice.

---

_Comment by @MichaReiser on 2025-08-15 12:10_

This should be easier to implement now that we added streaming support in the LSP

---
