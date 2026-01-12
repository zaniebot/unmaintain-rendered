```yaml
number: 1896
title: Use boolean fields instead of HashMap lookups for enabled checks
type: pull_request
state: closed
author: not-my-profile
labels: []
assignees: []
draft: true
base: main
head: enabled-bools
created_at: 2023-01-15T21:15:07Z
updated_at: 2023-01-17T10:28:42Z
url: https://github.com/astral-sh/ruff/pull/1896
synced_at: 2026-01-12T05:36:32Z
```

# Use boolean fields instead of HashMap lookups for enabled checks

---

_Pull request opened by @not-my-profile on 2023-01-15 21:15_

_No description provided._

---

_Comment by @not-my-profile on 2023-01-15 21:15_

@charliermarsh could you benchmark this? I am not sure if this does anything in terms of performance ...

---

_Comment by @charliermarsh on 2023-01-15 21:27_

It makes a tiny difference (compared to 300.2ms / 301.4ms before):

![Screen Shot 2023-01-15 at 4 26 49 PM](https://user-images.githubusercontent.com/1309177/212568158-8ab7e788-1960-484d-b02b-9cf8e06c8a59.png)

Could still be worth it? Guessing it simplifies the hash implementation, and the client code reads a bit more cleanly IMO. What do you think?


---

_Comment by @charliermarsh on 2023-01-15 21:31_

(When I said "10% improvement" in that other PR, I think I was wrong. I was setting `enabled` to `FxHashSet::default()`, but then forcing `use_ast` to `true` to ensure we still did the parse + AST walk. But, that probably has a bunch of other side effects that impact performance -- e.g., we wouldn't be surfacing _any_ checks, so check reporting would be a no-op, and so on. I'm guessing what you have here is the limit on gains from this codepath.)

---

_@charliermarsh reviewed on 2023-01-15 21:33_

---

_Review comment by @charliermarsh on `src/settings/mod.rs`:259 on 2023-01-15 21:33_

We should do the same thing for `fixable`, if we can.

---

_Comment by @not-my-profile on 2023-01-15 21:44_

I think it would be interesting to measure how much time is actually spend in the ruff codebase vs e.g. the RustPython parser. How much of a difference does it make when we don't invoke `check_ast` at all?

(And it would also be interesting how much it changes when run with `--select ALL`.)

---

_Comment by @charliermarsh on 2023-01-15 21:53_

I haven’t benchmarked it recently but the parser is the majority of the time. Invoking just the parser is at least 200ms, maybe more like 250ms (out of the 300ms above).

---

_Comment by @not-my-profile on 2023-01-17 10:28_

Closing in favor of 816c5f9d37c7f41cd9042e0180843b3206b8c49d from #1930.

---

_Closed by @not-my-profile on 2023-01-17 10:28_

---
