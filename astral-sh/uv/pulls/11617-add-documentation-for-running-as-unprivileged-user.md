```yaml
number: 11617
title: Add documentation for running as unprivileged user
type: pull_request
state: open
author: returnDanilo
labels: []
assignees: []
base: main
head: main
created_at: 2025-02-19T11:27:14Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11617
synced_at: 2026-01-12T16:09:55Z
```

# Add documentation for running as unprivileged user

---

_@returnDanilo_

Hey! I took the initiative of writing a bit of docs for the things I had to figure out myself. Plus, it was asked in #10047.

I think the docs could use a paragraph explaining the case of using cache mounts. But I've encountered the limits of my understanding of docker and uv.

Here's the problem I found when trying to document it:

 `--mount=type=cache` leaves a root-owned directory after the `RUN` instruction has run; even when using the `uid=...`/`gid=...` options. I don't know if this is intended behavior or a bug. 

This makes it so that you need to either (1) `rmdir` it before allowing the command in `CMD ["uv", "run", "foobar.py"]` to run, or (2) *reset*  `UV_CACHE_DIR` to point to somewhere else.

Regardless of 1 or 2, the cache at "build" time becomes useless for the program at run time, and I don't know how important that is.

---
