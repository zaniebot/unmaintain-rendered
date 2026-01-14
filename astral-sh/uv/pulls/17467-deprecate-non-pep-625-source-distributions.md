```yaml
number: 17467
title: Deprecate non-PEP 625 source distributions
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
draft: true
base: main
head: ww/deprecate-non-pep625
created_at: 2026-01-14T15:58:33Z
updated_at: 2026-01-14T17:10:27Z
url: https://github.com/astral-sh/uv/pull/17467
synced_at: 2026-01-14T17:38:04Z
```

# Deprecate non-PEP 625 source distributions

---

_@woodruffw_

## Summary

This adds a deprecation warning for source distributions that don't follow the PEP 625 filename scheme, which requires a `.tar.gz` suffix. I've also updated the docs with a warning about behavior + deprecation notice.

Towards #16911.

## Test Plan

The only functional change here is a user-level warning. I'll bump the snapshots that change.

---

_Assigned to @woodruffw by @woodruffw on 2026-01-14 15:58_

---

_Comment by @woodruffw on 2026-01-14 16:20_

Noting: I put this warning at the sdist filename parsing layer to start, but that's going to be *way* too noisy since it'll ding on every legacy sdist on PyPI (and there are a lot of them in the tail of popular projects). I think the better place to put this is in the installation planner, and limit it to just to-be-fetched remote dists. 

Edit: more precisely, `build_wheel` seems like the right place, since it's where we actually use a given selected sdist and materialize it into a wheel.

---

_Comment by @konstin on 2026-01-14 16:54_

One option is to warn specifically when we unpack an xz/lzma source dist, where we could also do this for xz/lzma wheels, another is checking after resolution or after loading a lockfile for the files in the resolution type.

---

_Comment by @woodruffw on 2026-01-14 17:06_

I have it in `build_wheel` locally ATM, just waiting for tests for complete. Does that seem like a reasonable place to you? My intuition is that that's a good place in terms of balancing noise/actionability (and making it clear that this specific deprecation is about sdists), but I could move it to the unpacking or lockfile layers if you think that makes more sense!

> where we could also do this for xz/lzma wheels

Oh, I didn't realize we actually allowed wheels to also be xz/lzma 😅 -- I was looking at `WheelFilename` and could only find `.whl`, which should always imply ZIP unless we're doing something more nuanced under the hood?


---
