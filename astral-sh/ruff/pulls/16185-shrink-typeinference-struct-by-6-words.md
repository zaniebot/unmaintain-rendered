```yaml
number: 16185
title: "Shrink `TypeInference` struct by 6 words"
type: pull_request
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
base: main
head: micha/shrinkg-type-inference
created_at: 2025-02-16T14:32:26Z
updated_at: 2025-02-19T18:21:57Z
url: https://github.com/astral-sh/ruff/pull/16185
synced_at: 2026-01-12T15:55:53Z
```

# Shrink `TypeInference` struct by 6 words

---

_@MichaReiser_

## Summary

Red Knot creates a `TypeInference` result for every standalone expression, definition, and each scope. 
For tomllib, that's around 10k instances. This PR reduces the size of `TypeInference` by 6 words (from 24 to 18) 
by `Option` boxing `TypeCheckDiagnostics` (which gives us a 60KB memory reduction on tomllib). 
The idea behind this is that most files don't have diagnostics and saving memory should outweight the cost of dereferencing and one extra allocation in the few cases
where a file/scope/expression/definition has diagnostics. 

We may want to look into other ways on how to shrink `TypeInferenceResult` further but I'll leave at this for now. 

## Test Plan

`cargo test`


---

_Label `red-knot` added by @MichaReiser on 2025-02-16 14:32_

---

_Marked ready for review by @MichaReiser on 2025-02-16 14:51_

---

_Review requested from @carljm by @MichaReiser on 2025-02-16 14:51_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-16 14:51_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-16 14:51_

---

_@AlexWaygood reviewed on 2025-02-16 15:06_

Hmm... it makes sense to me to shrink `TypeInference`, but this is actually showing a tiny regression on the cold benchmark on codspeed: https://codspeed.io/astral-sh/ruff/branches/micha%2Fshrinkg-type-inference.

Does this show up as an improvement when running on black, or one of the other codebases we can run with the `knot_benchmark` script? If not, I wonder if it's worth the added complexity right now

---

_Comment by @MichaReiser on 2025-02-16 21:10_

The first run showed an improvement on the incremental benchmark. I then fixed a lint and now it's neutral.

I also think the regression does make sense because there are diagnostics. So it doesn't represent the use case we're optimising for.

I consider the complexity minimal. It's all encapsulated in a single type

---

_Comment by @sharkdp on 2025-02-17 08:21_

I saw this and thought it would be a neat testing environment for my (unfinished) peak-memory-usage feature in hyperfine. The results for checking `tomllib` are shown below. We use ~34 MiB of memory in both cases. There is no statistically significant difference between this branch and `main` (p = 0.81). But it is true that the *minimum* peak memory usage on this feature branch is 96 KiB smaller than on main (96 KiB / 34 MiB = 0.3%), which seems to be in the same order of magnitude than your estimate/measurement of 60 kB. 

![1](https://github.com/user-attachments/assets/c2e01400-890d-4063-8f1b-9d7d793d4388)

and zoomed in:

![2](https://github.com/user-attachments/assets/9f0ad1e8-ac93-47e7-9e24-b56e0e612f51)


---

_Closed by @MichaReiser on 2025-02-17 08:31_

---

_Comment by @MichaReiser on 2025-02-17 08:32_

I still think this is worth it. While true that it's not load bearing (because of ASTs and source text), it's still a worthwhile trade off to trade close to 0 perf implication with less memory usages. 

---

_Comment by @sharkdp on 2025-02-17 08:51_

> I still think this is worth it.

I did not want to imply that you need to close this. I just wanted to add some measurements for discussion.

> I also think the regression does make sense because there are diagnostics.

I am not sure I'm following this argument. We currently report 5 diagnostic messages for tomllib. Even if all of them are from different scopes, it would still just be a tiny 0.05% fraction of all scopes. If there *were* a significant benefit to not allocating for the other 99.95% of scopes, shouldn't it show up in the tomllib benchmark?

> So it doesn't represent the use case we're optimising for.

What are we optimizing for? The zero-diagnostics case? I thought I read elsewhere that we mostly care about incremental/LSP performance, where the default state is that there *are* diagnostics?

---

_Comment by @MichaReiser on 2025-02-17 08:58_

> I did not want to imply that you need to close this. I just wanted to add some measurements for discussion.

I don't care enough about this change that it's worth debating right now. I considered it a nice and small improvement but maybe it isn't as impactful as I thought (We currently crate 1k TypeInfernce scopes for tomllib for 20 files. This gives us ~50 instances and we save 6 bytes for each (minus the overhead for tracking the allocation if there is one). It comes down to maybe a 30MB memory improvement if you have 100k files

> If there were a significant benefit to not allocating for the other 99.95% of scopes, shouldn't it show up in the tomllib benchmark?

The improvement isn't about reducing allocation. In fact, the current implementation doesn't allocate because it only stores empty `Vec`s and the new implementation has to allocate if there are diagnostics (it allocates more often). The improvement is exclusively about avoiding an unnecessarily large `TypeInference` strcut

> What are we optimizing for? The zero-diagnostics case? I thought I read elsewhere that we mostly care about incremental/LSP performance, where the default state is that there are diagnostics?

It's correct that errors are the default in the LSP case, but there are only few scopes that have diagnostics. It's not the majority. That still means it's worth optimizing for the "no" or "very few" diagnostic cases. 

---

_Comment by @carljm on 2025-02-19 18:21_

> most files don't have diagnostics

I do think it will be interesting to collect real-world data on this from some real codebases; I don't have a strong intuition about how true this is. I certainly expect it's a solid majority, but I wouldn't be surprised if it turns out to be closer to, say, 85% rather than like 99% or something. Mostly I would expect this to be due to suppressions.

---
