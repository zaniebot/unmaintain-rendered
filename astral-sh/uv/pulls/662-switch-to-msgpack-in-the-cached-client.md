```yaml
number: 662
title: Switch to msgpack in the cached client
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/msgpack-cached-client
created_at: 2023-12-15T13:08:36Z
updated_at: 2023-12-16T21:01:36Z
url: https://github.com/astral-sh/uv/pull/662
synced_at: 2026-01-10T15:44:44Z
```

# Switch to msgpack in the cached client

---

_Pull request opened by @konstin on 2023-12-15 13:08_

This gives a 1.23 speedup on transformers-extras. We could change to msgpack for the entire cache if we want. I only tried this format and postcard so far, where postcard was much slower (like 1.6s).

I don't actually want to merge it like this, i wanted to figure out the ballpark of improvement for switching away from json.

```
hyperfine --warmup 3 --runs 10 "target/profiling/puffin pip-compile --cache-dir cache-msgpack scripts/requirements/transformers-extras.in" "target/profiling/branch pip-compile scripts/requirements/transformers-extras.in"
Benchmark 1: target/profiling/puffin pip-compile --cache-dir cache-msgpack scripts/requirements/transformers-extras.in
  Time (mean ± σ):     179.1 ms ±   4.8 ms    [User: 157.5 ms, System: 48.1 ms]
  Range (min … max):   174.9 ms … 188.1 ms    10 runs

Benchmark 2: target/profiling/branch pip-compile scripts/requirements/transformers-extras.in
  Time (mean ± σ):     221.1 ms ±   6.7 ms    [User: 208.1 ms, System: 46.5 ms]
  Range (min … max):   213.5 ms … 235.5 ms    10 runs

Summary
  target/profiling/puffin pip-compile --cache-dir cache-msgpack scripts/requirements/transformers-extras.in ran
    1.23 ± 0.05 times faster than target/profiling/branch pip-compile scripts/requirements/transformers-extras.in
```

Disadvantage: We can't manually look into the cache anymore to debug things

- [ ] Check more formats, i currently only tested json, msgpack and postcard, there should be other formats, too
- [x] Switch over `CachedByTimestamp` serialization (for the interpreter caching)
- [x] Switch over error handling and make sure puffin is still resilient to cache failure

---

_Comment by @BurntSushi on 2023-12-15 14:12_

Other than dropping the `unwrap()`, it seems like we ought to merge this given the win? Or is there some other downside I'm missing here?

---

_Comment by @konstin on 2023-12-15 14:30_

* Check more formats, i currently only tested json, msgpack and postcard, there should be other formats, too
* Switch over `CachedByTimestamp` serialization (for the interpreter caching)
* Switch over error handling and make sure puffin is still resilient to cache failure

---

_Comment by @BurntSushi on 2023-12-15 14:32_

With respect to other formats, I just figured that could be done as future work. If msgpack gives a nice speed-up for this small of a change, I'd say merge it. Assuming cache failure resiliency is still okay. (How is that tested?)

---

_Comment by @charliermarsh on 2023-12-15 16:48_

I think it's fine to merge/change this without testing other formats, it seems purely beneficial. But yeah, we should probably move over other opaque cache structures to it too.

---

_Marked ready for review by @charliermarsh on 2023-12-16 20:29_

---

_Comment by @charliermarsh on 2023-12-16 20:29_

Let's give this a shot for now -- easy to change in the future, and worth playing with on `main`. I added to the appropriate places and removed the unwraps.

---

_Comment by @charliermarsh on 2023-12-16 20:37_

I also did some testing to validate that msgpack is robust to bad cache entries by running on main, then running against on this branch without changing the cache extensions to `.msgpack`:

```
0.001927s  WARN puffin_interpreter::interpreter Ignoring invalid cached markers for: /Users/crmarsh/workspace/guffin/.venv/bin/python
...
0.658065s  WARN puffin_client::cached_client Broken cache entry at /Users/crmarsh/workspace/guffin/foo/simple-v0/pypi/aiosignal.json, removing: invalid type: integer `123`, expected struct DataWithCachePolicy
...
```

---

_Merged by @charliermarsh on 2023-12-16 21:01_

---

_Closed by @charliermarsh on 2023-12-16 21:01_

---

_Branch deleted on 2023-12-16 21:01_

---
