---
number: 21647
title: ruff detects problems in CI, but not locally
type: issue
state: closed
author: vadimkantorov
labels:
  - question
assignees: []
created_at: 2025-11-26T21:24:37Z
updated_at: 2025-12-10T21:52:50Z
url: https://github.com/astral-sh/ruff/issues/21647
synced_at: 2026-01-07T13:12:16-06:00
---

# ruff detects problems in CI, but not locally

---

_Issue opened by @vadimkantorov on 2025-11-26 21:24_

### Summary

In one occasion, I observed that `rm .ruff_cache` reverts local behavior to match GitHub CI's pre-commit behavior.

But now it's back and dropping the cache does not fix it anymore. I observed this problem with isort rule. It's as if the offending file is not actually checked locally.

I also could not find a verbose mode which would explain which files ruff actually checked/formatted under which rules

### Version

ruff 0.14.6

---

_Comment by @ntBre on 2025-11-26 21:50_

Is there a chance that there's a directory with the same name as one of your dependencies on one machine and not the other? See this comment from earlier today: https://github.com/astral-sh/ruff/issues/21638#issuecomment-3580583306. It also mentions the `-v` flag to activate verbose logging.

---

_Label `question` added by @ntBre on 2025-11-26 21:50_

---

_Comment by @vadimkantorov on 2025-11-28 02:17_

I'll try the verbose output next time it happens. Last time I tried, I think it was not even detailing if it ran isort at all and how exactly it touched the cache. Is there some strictly `--no-cache` option to fully disable any caching?

Also strangely, the local ruff kept reverting back the import order for me. So to satisfy the CI ruff I had to keep the local ruff unhappy. Extremely confusing :( 

---

_Comment by @vadimkantorov on 2025-11-28 21:04_

@ntBre It seems you're right.

My colleague @nkkarpov suggested a plausible explanation:

`wandb` python package creates a local folder named also `wandb` for storing its temporary files which confuses ruff.

I think isort should not change behavior based on existence of local folders - at least not by default.

And in verbose mode ruff should certainly print any of these decisions taken, and all cache access events as well.

---

_Referenced in [wandb/wandb#10997](../../wandb/wandb/issues/10997.md) on 2025-11-28 21:15_

---

_Comment by @MichaReiser on 2025-11-28 21:25_

Yes, wandb is a known problem maker, see https://github.com/astral-sh/ruff/issues/10519#issuecomment-3164059570. We might add special casing for wandb specifically. 

> I think isort should not change behavior based on existence of local folders - at least not by default.

The way isort detects whether something is first or third-party is solely based on the precense of folders and files. Even empty folders can be namespace packages in Python, making this tricky. 

> And in verbose mode ruff should certainly print any of these decisions taken, and all cache access events as well.

Ruff does print the decision making when using `ruff check -v`. 

---

_Comment by @vadimkantorov on 2025-11-28 22:18_

At least please add a config option to suppress this behavior (dependence of sorting on existence of local folders). It can clearly make hard-to-debug troubles. And import order isn' t that important in many cases (but isort is still enabled in many places). Or maybe please  introduce `isort-ignoring-local-folders` or sth similar

---

_Comment by @vadimkantorov on 2025-11-28 22:20_

I can easily imagine someone stumbling on this for some other package or folder name... It's a very confusing thing to debug...

---

_Comment by @charliermarsh on 2025-11-28 22:20_

How would we suppress this? How would we detect if something is a first-party import other than by looking at the presence of files on-disk?

---

_Closed by @MichaReiser on 2025-12-10 17:22_

---

_Comment by @vadimkantorov on 2025-12-10 21:52_

Another idea: make ruff print in non-verbose mode if it treats some packages as local and as third-party (and especially so if it proposes to reorder imports). Like so one can at least compare ruff logs and find out that local run has a different printout than CI run and have a pointer to the explanation...

---
