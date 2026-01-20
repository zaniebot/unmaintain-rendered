```yaml
number: 2570
title: "Add completion benchmarks to `ty_walltime`"
type: issue
state: open
author: BurntSushi
labels:
  - performance
  - testing
  - completions
assignees: []
created_at: 2026-01-20T14:19:42Z
updated_at: 2026-01-20T14:20:34Z
url: https://github.com/astral-sh/ty/issues/2570
synced_at: 2026-01-20T14:40:26Z
```

# Add completion benchmarks to `ty_walltime`

---

_@BurntSushi_

In https://github.com/astral-sh/ruff/pull/22630, I added a small ad hoc CLI tool for ad hoc benchmarking/profiling of completions. But ideally, we'd have some benchmarks for completions in our LSP benchmarks too. A good start would be to add them to `ty_walltime`. Currently, I think all of the benchmarks in `ty_walltime` are just running type checking:

https://github.com/astral-sh/ruff/blob/0a1dddbf73e368a2019617ee907b0eb271eca979/crates/ruff_benchmark/benches/ty_walltime.rs#L74-L90

So we'll want to add a new kind of benchmark that, instead of running type checking, it asks for completions instead.

For completions, the uncached and cached cases can be markedly different. Ideally we could benchmark both. But at minimum, we should be clear about which case is being tested (perhaps in the name of the benchmark).

(It may also be a good idea to add completion benchmarks to `scripts/ty_benchmark`, but I think that's a separate task.)

---

_Added to milestone `Stable` by @BurntSushi on 2026-01-20 14:19_

---

_Label `performance` added by @BurntSushi on 2026-01-20 14:19_

---

_Label `testing` added by @BurntSushi on 2026-01-20 14:19_

---

_Label `completions` added by @BurntSushi on 2026-01-20 14:19_

---
