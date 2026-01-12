```yaml
number: 19059
title: "Run mypy-primer with `TY_MAX_PARALLELISM=1`"
type: pull_request
state: closed
author: ibraheemdev
labels:
  - ci
  - ty
assignees: []
base: main
head: ibraheem/mypy-primer-determinism
created_at: 2025-07-01T00:22:42Z
updated_at: 2025-07-02T11:19:12Z
url: https://github.com/astral-sh/ruff/pull/19059
synced_at: 2026-01-12T15:56:31Z
```

# Run mypy-primer with `TY_MAX_PARALLELISM=1`

---

_@ibraheemdev_

## Summary

The memory usage numbers are currently flaky partly due to memory usage being non-deterministic in multi-threaded runs. Running ty in single-threaded mode in the mypy-primer mode should fix this. From what I understand, mypy-primer has parallelism internally so this might not be significantly slower, but if it is then we'll need a separate CI job for memory usage.


---

_Label `ci` added by @ibraheemdev on 2025-07-01 00:22_

---

_Label `ty` added by @ibraheemdev on 2025-07-01 00:22_

---

_Comment by @github-actions[bot] on 2025-07-01 00:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

schemathesis (https://github.com/schemathesis/schemathesis)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~156MB
+     memo fields = ~171MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB

```
</details>


---

_Marked ready for review by @ibraheemdev on 2025-07-01 00:42_

---

_Comment by @ibraheemdev on 2025-07-01 00:43_

It looks like this is about a minute slower (4m as compared to 3m), which seems reasonable? A separate job would likely take more than a minute anyways.

---

_@dhruvmanila approved on 2025-07-01 08:29_

> It looks like this is about a minute slower (4m as compared to 3m), which seems reasonable? A separate job would likely take more than a minute anyways.

The increase in time seems reasonable to me given that cargo test takes around 4 minutes on Linux.

---

_Comment by @dhruvmanila on 2025-07-01 08:30_

> Running ty in single-threaded mode in the mypy-primer mode should fix this.

I'm assuming this can only be verified after merging this PR?

---

_Comment by @AlexWaygood on 2025-07-01 11:52_

> It looks like this is about a minute slower (4m as compared to 3m), which seems reasonable? A separate job would likely take more than a minute anyways.

It looks more like ~1m30s slower to me -- it looks like primer generally takes around 3m10s on `main`, and it takes 4m50s with this PR.

I _really_ love how quickly we get feedback from mypy_primer on PRs currently -- it makes it very easy to quickly spot bugs and iterate on PRs. From that perspective, I would selfishly sort-of prefer the memory report to be a separate primer job that runs concurrently with the existing primer job, so that we can still get feedback on the impact on diagnostics as quickly as possible. I'd expect ty PRs to change the diagnostics being emitted much more often than they have a significant impact on memory usage, anyway.

Neither CI job would be marked as "required", so we would still be able to merge PRs even if one or neither job hadn't yet completed and we wanted to merge a trivial PR quickly. The only cost would be additional CI resources -- but I actually think that having primer reports be as quick as possible might be worth that cost.

Curious if @sharkdp has opinions

---

_Comment by @sharkdp on 2025-07-01 12:18_

> Curious if @sharkdp has opinions

I was hesitant to bring it up, but I fully agree with you. mypy_primer feedback is also a crucial development tool for me. I am often waiting for its output. So in that sense, every saved minute is a developer-experience improvement. But it's probably also a bit annoying to create a completely separate job for this.

---

_Comment by @ibraheemdev on 2025-07-02 11:19_

I guess the problem with this approach is that we're bottlenecked on the largest project instead of evenly spreading the load. I'm fine to make this a separate job if the speed is important. There's also probably some things we could do to speed it up as is.

---

_Closed by @ibraheemdev on 2025-07-02 11:19_

---
