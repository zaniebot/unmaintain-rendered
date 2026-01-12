```yaml
number: 49
title: Store unzipped wheels in a cache
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/zip
created_at: 2023-10-08T03:53:32Z
updated_at: 2023-10-08T04:04:50Z
url: https://github.com/astral-sh/uv/pull/49
synced_at: 2026-01-12T16:03:43Z
```

# Store unzipped wheels in a cache

---

_@charliermarsh_

This PR massively speeds up the case in which you need to install wheels that already exist in the global cache.

The new strategy is as follows:

- Download the wheel into the content-addressed cache.
- Unzip the wheel into the cache, but ignore content-addressing. It turns out that writing to `cacache` for every file in the zip added a ton of overhead, and I don't see any actual advantages to doing so. Instead, we just unzip the contents into a directory at, e.g., `~/.cache/puffin/django-4.1.5`.
- (The unzip itself is now parallelized with Rayon.)
- When installing the wheel, we now support unzipping from a directory instead of a zip archive. This required duplicating and tweaking a few functions.
- When installing the wheel, we now use reflinks (or copy-on-write links). These have a few fantastic properties: (1) they're extremely cheap to create (on macOS, they are allegedly faster than hard links); (2) they minimize disk space, since we avoid copying files entirely in the vast majority of cases; and (3) if the user then edits a file locally, the cache doesn't get polluted. Orogene, Bun, and soon pnpm all use reflinks.

Puffin is now ~15x faster than `pip` for the common case of installing cached data into a fresh environment.

Closes https://github.com/astral-sh/puffin/issues/21.

Closes https://github.com/astral-sh/puffin/issues/39.


---

_Merged by @charliermarsh on 2023-10-08 04:04_

---

_Closed by @charliermarsh on 2023-10-08 04:04_

---

_Branch deleted on 2023-10-08 04:04_

---
