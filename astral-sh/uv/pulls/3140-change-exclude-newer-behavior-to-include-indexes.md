```yaml
number: 3140
title: Change exclude-newer behavior to include indexes missing upload-time
type: pull_request
state: closed
author: yorickvP
labels: []
assignees: []
base: main
head: yorickvp/exclude-newer-unavailable
created_at: 2024-04-19T14:25:32Z
updated_at: 2024-04-22T18:12:58Z
url: https://github.com/astral-sh/uv/pull/3140
synced_at: 2026-01-12T16:05:27Z
```

# Change exclude-newer behavior to include indexes missing upload-time

---

_@yorickvP_

## Summary

Currently, when --exclude-newer is given, all indexes that are missing it are treated entirely as unavailable. This makes it impossible to use this flag in combination with many indexes.

The new behavior warns about the missing field, but treats the distribution as available when the upload date it unknown.

This makes it possible to use this flag while still using packages from indexes that don't support `upload-time`.

### Rationale
If any extra-indexes are incompatible with exclude-newer, it would be more useful to have a best-effort attempt instead of ignoring the extra indexes and likely failing the solve.

## Test Plan
I'm going to see what CI says about this.
<!-- How was it tested? -->


---

_Comment by @zanieb on 2024-04-19 14:42_

I'm a little unsure about the "correctness" of this behavior, but have felt this pain as well. Maybe we'll want to toggle this with a configuration option so it's opt-in?

---

_Comment by @BurntSushi on 2024-04-19 14:48_

Yeah I'm not sure this is the right behavior either. To me, if you're using `--exclude-newer`, then it's a very intentional opt-in to only wanting packages before a specific date. It's a kind of hack to get a semblance of reproducibility for example. And if packages without a timestamp are allowed even when `--exclude-newer` is used, then it somewhat defeats the purpose for at least some use cases.

---

_Comment by @zanieb on 2024-04-22 18:12_

Going to close this for now as I believe it breaks a correctness contract. Feel free to open an issue if you want to discuss a configuration option.

---

_Closed by @zanieb on 2024-04-22 18:12_

---
