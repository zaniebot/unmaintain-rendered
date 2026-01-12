```yaml
number: 4853
title: "Refactor `fetch-download-metadata.py` script"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-fetch
created_at: 2024-07-07T07:10:35Z
updated_at: 2024-07-23T12:37:25Z
url: https://github.com/astral-sh/uv/pull/4853
synced_at: 2026-01-12T16:06:29Z
```

# Refactor `fetch-download-metadata.py` script

---

_@j178_

## Summary

Similiar to https://github.com/astral-sh/rye/pull/680, I have made a major refactor to the `fetch-download-metadata.py` script.

Some notable changes:

- Use PEP 723 inline scripts
- Fully type annotated the script
- Implemented async HTTP fetching
- Introduced a `Finder` base class and move finder logic under `CPythonFinder` subclass, which will make it easier to add a `PyPyFinder` later.
- Instead of fetching `xxx.sha256` for each file, the script now fetches a single `SHA256SUMS` file containing checksums for all files in the release.

As a result, the script now takes around 10 seconds instead of 10+ minutes.

## Plan for Future PRs

- [ ] Implement the `PyPyFinder`
- [ ] Add an GitHub Action to run `fetch-download-metadata.py` daily and create PR automatically

## Test Plan

```sh
cargo run -- run --isolated -- ./crates/uv-python/fetch-download-metadata.py
```


---

_Review comment by @j178 on `crates/uv-python/template-download-metadata.py`:73 on 2024-07-07 07:11_

Use `as_posix()` to make separator consistent across platforms.

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:114 on 2024-07-07 07:14_

Our current metadata includes `debug` flavors actually. I need to move it from `HIDDEN_FLAVORS` to keep the `download-metadata.json` file unchanged.

---

_@j178 reviewed on 2024-07-07 07:14_

---

_Marked ready for review by @j178 on 2024-07-07 07:32_

---

_Review requested from @zanieb by @konstin on 2024-07-07 09:40_

---

_@zanieb reviewed on 2024-07-07 16:38_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:365 on 2024-07-07 16:38_

We did not previously require a token and it is not required to make the number of requests we need, can we just warn here instead?

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:73 on 2024-07-07 16:40_

This feels a little weird. Is this actually the easiest way to reverse the versions?

---

_@zanieb reviewed on 2024-07-07 16:40_

---

_Comment by @zanieb on 2024-07-07 16:45_

Thanks for taking this on! I appreciate the PEP 723 changes and the improved speed.

I'm a little hesitant, there are a lot of changes here and not all of them are clearly motivated, e.g., we're not operating at a scale asyncio is superior to threads, we've previously gone out of our way to only use the standard library, and I'm not sure why a single file is preferable for the checksums. Do you have some thoughts on those?

If we're going to aim for full type coverage, we should probably follow this by adding type checking in CI too otherwise it seems too easy for it to become out of date.


---

_Label `internal` added by @zanieb on 2024-07-07 16:48_

---

_Comment by @j178 on 2024-07-13 05:32_

Thanks for your input! Sorry for the late reply (I had a really busy week, and haven't got time to write a full reply)

`asyncio` and `httpx` are used to make writing concurrent code easier. I don't see any major drawbacks to using them for a helper script. `httpx` is commonly used for HTTP requests, almost like a standard library, and it's lightweight. Additionally, `template-download-metadata.py` also depends on `chevron_blue`. If you really don't like them, I can consider rewriting it using only the standard library.

> why a single file is preferable for the checksums

Consider [the latest indygreg/python-build-standalone release](https://github.com/indygreg/python-build-standalone/releases/tag/20240415). Previously, we had to fetch 51 separate checksum files for 51 installations. Now, all checksums are combined into one file, reducing it to a single request. This is where the major speed improvement comes from.

> we should probably follow this by adding type checking in CI

That's a great suggestion. I'll explore how to add type checking for this single file. Would it be possible to incorporate it in a subsequent PR?

---

_Comment by @zanieb on 2024-07-13 15:26_

> If you really don't like them, I can consider rewriting it using only the standard library.

It's not about liking them really — I am a httpx maintainer and a big user of async Python — it just feels like unnecessary complexity for this task. I'm just trying to avoid maintaining additional complexity.  It doesn't necessarily feel worth tearing out now, but in the future it'd be nice to separate such changes so we can weigh them on their merits i.e. is there a meaningful performance improvement from async here or is it all from sending way less requests to retrieve checksums.

> Now, all checksums are combined into one file, reducing it to a single request. This is where the major speed improvement comes from.

Ah I see, I misunderstood you comment. This makes a lot of sense to me.

> Would it be possible to incorporate it in a subsequent PR?

Yes definitely.

---

_Comment by @zanieb on 2024-07-13 15:27_

I'll read through the code more closely soon.

---

_Comment by @j178 on 2024-07-16 16:17_

Just a gentle reminder, could you please take a look at this and let me know if we can proceed? @zanieb 

---

_Comment by @zanieb on 2024-07-22 14:29_

Yes I'll give this a look and merge today. It's really hard to review pull requests that make so many changes at once like this. In the future, please separate things into separate pull requests e.g. one that adds types but doesn't change the requests being made so we can review things incrementally.

---

_Comment by @j178 on 2024-07-22 15:20_

Truely sorry for the inconvenience and thank you for your patience ❤️ 

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:170 on 2024-07-22 23:32_

I would consider `pages` as the page size. However, if `pages` equals 1, this would actually result in a size of 0 (since `range(1, 1)`), which seems odd to me.

```suggestion
        for page in range(1, pages + 1):
```

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:190 on 2024-07-23 00:00_

I understand that this PR doesn't modify the code block, but since it's a refactoring PR, I've included some comments for review.

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:190 on 2024-07-23 00:01_

There's a diagnostic error:
```
1. Pylance: Argument of type "str | None" cannot be assigned to parameter "flavor" of type "str" in function "_get_flavor_priority"
     Type "str | None" cannot be assigned to type "str"
       "None" is incompatible with "str" [reportArgumentType] [reportArgumentType]
```

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:197 on 2024-07-23 00:16_

If I understand correctly, based on the comparison `if priority >= existing_priority` and the variable names, it seems more likely to be the opposite of the comment.

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:197 on 2024-07-23 00:17_

Since we're already leveraging `NamedTuple` extensively in this PR, we might consider refactoring this out
``` python
if existing and priority >= existing.priority:
```
or simply using:
```python
if existing and priority >= existing[1]:
```

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:253 on 2024-07-23 00:34_

Is there a specific reason for checking the `.sha256` extension after constructing the `filename`, rather than simply checking the `url` before construction? Is it a matter of readability?

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:332 on 2024-07-23 02:08_

Is there a specific reason why we use `index` to maintain the order here, instead of relying solely on the lexical order of the `download.implementation` (`cpython` followed by `pypy`) which is our expected order?

---

_@eth3lbert reviewed on 2024-07-23 02:18_

None of my comments are blockers 👍 

---

_@zanieb reviewed on 2024-07-23 02:30_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:332 on 2024-07-23 02:30_

I do think we want CPython to come before PyPy regardless of lexical ordering.

---

_Review comment by @j178 on `crates/uv-python/fetch-download-metadata.py`:197 on 2024-07-23 02:46_

> If I understand correctly, based on the comparison if priority >= existing_priority and the variable names, it seems more likely to be the opposite of the comment.

I think a smaller value indicates a higher priority.

---

_@j178 reviewed on 2024-07-23 02:46_

---

_@zanieb approved on 2024-07-23 03:00_

---

_Comment by @zanieb on 2024-07-23 03:00_

Thanks!

---

_@eth3lbert reviewed on 2024-07-23 03:04_

---

_Review comment by @eth3lbert on `crates/uv-python/fetch-download-metadata.py`:197 on 2024-07-23 03:04_

Indeed, I overlooked the doc in `_get_flavor_priority` for no reason 😅 .

---

_Merged by @zanieb on 2024-07-23 03:06_

---

_Closed by @zanieb on 2024-07-23 03:06_

---

_Branch deleted on 2024-07-23 12:37_

---
