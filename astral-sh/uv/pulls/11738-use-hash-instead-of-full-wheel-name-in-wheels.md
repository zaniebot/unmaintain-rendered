```yaml
number: 11738
title: Use hash instead of full wheel name in wheels bucket
type: pull_request
state: merged
author: nkitsaini
labels:
  - bug
  - no-build
assignees: []
merged: true
base: main
head: cache-url-length
created_at: 2025-02-24T05:27:03Z
updated_at: 2025-02-27T08:13:34Z
url: https://github.com/astral-sh/uv/pull/11738
synced_at: 2026-01-10T11:10:38Z
```

# Use hash instead of full wheel name in wheels bucket

---

_Pull request opened by @nkitsaini on 2025-02-24 05:27_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->


## Summary
Closes #2410 
<!-- What's the purpose of the change? What does it do, and why? -->
This changes the name of files in `wheels` bucket to use a hash instead of the wheel name as to not exceed maximum file length limit on various systems. 

This only addresses the primary concern of #2410. It still does _not_ address:
- Path limit of 260 on windows: https://github.com/astral-sh/uv/issues/2410#issuecomment-2062020882
To solve this we need to opt-in to longer path limits on windows ([ref](https://github.com/astral-sh/uv/issues/2410#issuecomment-2150532658)), but I think that is a separate issue and should be a separate MR.
- Exceeding filename limit while building a wheel from source distribution
As per my understanding, this is out of uv's control. Name of the output wheel will be decided by build-backend used by the project. For wheels built from source distribution, pip also uses the wheel names in cache. So I have not touched `sdists` cache.


I have added a `filename: WheelFileName` field in `Archive`, so we can use it while indexing instead of relying on the filename on disk. Another way to do this was to read `.dist-info/WHEEL` and `.dist-info/METADATA` and build `WheelFileName` but that seems less robust and will be slower.
## Test Plan

<!-- How was it tested? -->
Tested by installing `yt-dlp`, `httpie` and `sqlalchemy` and verifying that cache files in `wheels` bucket use hash.

---

_@nkitsaini reviewed on 2025-02-24 05:28_

---

_Review comment by @nkitsaini on `crates/uv-distribution/src/source/built_wheel_metadata.rs`:32 on 2025-02-24 05:28_

This is an unrelated change. I got a bit confused here between directory/files while reading the code. I can move this to separate MR or remove it if required.

---

_Marked ready for review by @nkitsaini on 2025-02-24 08:38_

---

_Review requested from @charliermarsh by @konstin on 2025-02-24 08:44_

---

_Renamed from "Use hash instead of full wheel name in wheels cache" to "Use hash instead of full wheel name in wheels bucket" by @nkitsaini on 2025-02-24 08:48_

---

_Assigned to @charliermarsh by @zanieb on 2025-02-24 17:16_

---

_Comment by @charliermarsh on 2025-02-24 19:48_

Thanks! Will review.

---

_Label `bug` added by @charliermarsh on 2025-02-25 14:50_

---

_Label `no-build` added by @charliermarsh on 2025-02-25 14:50_

---

_Comment by @charliermarsh on 2025-02-25 14:51_

I think this generally looks right, though I'm undecided on whether we should _always_ do this, or only do it for wheels with filenames that exceed a certain length. It does hurt cache (plaintext) readability a bit which is inconvenient for debugging, since you can no longer infer the wheel tags etc. from the cached filename alone.

---

_Comment by @charliermarsh on 2025-02-25 14:56_

At least the hashes are consistent across these files, so you _can_ look at `WHEEL`:

![Screenshot 2025-02-25 at 9 55 25 AM](https://github.com/user-attachments/assets/254e248b-8a7c-455d-985c-90678c61b900)

That's probably good enough.


---

_@charliermarsh approved on 2025-02-25 15:00_

---

_Comment by @nkitsaini on 2025-02-25 15:39_

> so you can look at WHEEL:

I was doing a more hacky `cat <file>` and finding the tags/version.

![image](https://github.com/user-attachments/assets/57a37b4d-b4f8-400d-be7f-0e2d11c7656b)

Let me change the filenames to `{trimmed_filename}-{hash}`. Should be quick.

---

_Comment by @nkitsaini on 2025-02-25 16:09_



I went with `trimmed(version+tags)-hash` as that was informative compared to `trimmed(name+version+tags)-hash` as we already have package name as dir name.

![image](https://github.com/user-attachments/assets/bddda44f-7621-4d7e-b919-c07ea9458837)

![image](https://github.com/user-attachments/assets/f8b0492d-4bfd-4a8a-aa5d-15549448d237)

---

_@nkitsaini reviewed on 2025-02-25 16:12_

---

_Review comment by @nkitsaini on `crates/uv-distribution-filename/src/wheel.rs`:113 on 2025-02-25 16:12_

Couldn't think of a better name for this ;(
Feel free to change it or reject this commit :)

---

_Comment by @nkitsaini on 2025-02-25 16:58_

@charliermarsh Sorry for the delay. It is good from my end.

~~There seems to be one seemingly unrelated failure in windows tests. I don't have a good way to reproduce the failure locally.~~ It was just some flakiness. I have no idea what caused it, maybe some failure with disk I/O ¯\\\_(ツ)\_/¯ .

---

_@zanieb reviewed on 2025-02-25 22:41_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:625 on 2025-02-25 22:41_

I'd probably move this to a utility since it's non-trivial and repeated three times.

---

_@zanieb reviewed on 2025-02-25 22:43_

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:625 on 2025-02-25 22:43_

(doesn't need to be blocking, we can do this separately)

---

_@zanieb reviewed on 2025-02-25 22:46_

---

_Review comment by @zanieb on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-02-25 22:46_

Would it be painful to truncate to the last `-` before the trimmed length limit is reached? I think stopping mid-tag could be confusing on read, though I guess any partial tag output is a bit confusing. Also sort of tempted to just take the first few tags or drop the tags entirely? Not sure what's best in practice.

---

_@nkitsaini reviewed on 2025-02-26 03:50_

---

_Review comment by @nkitsaini on `crates/uv-cache/src/lib.rs`:625 on 2025-02-26 03:50_

:+1: Moved the reference finding part to dedicated function. 

[`b400c85` (#11738)](https://github.com/astral-sh/uv/pull/11738/commits/b400c8598ddc19e347a683810027b2efa339f7b4#diff-d0c8455b65232353aa60383cd8a80d99a8b31cf7cd76bf22c18d32de36bed34cR514)

---

_@nkitsaini reviewed on 2025-02-26 03:54_

---

_Review comment by @nkitsaini on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-02-26 03:54_

> Would it be painful to truncate to the last - before the trimmed length limit is reached

Not painful but I am not sure if it will be very helpful as it will mean dropping platform tags completely in most cases since they do not use dashes as seperator.
```
Example: sqlalchemy-1.4.52-cp38-cp38-manylinux1_x86_64.manylinux2010_x86_64.manylinux_2_12_x86_64.manylinux_2_5_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.http
```
  
  

I agree it is a bit confusing. I have changed to what I think leads to least confusion and doesn't affect readability for normal cases. So, filename will be `{version}-{tags}` if it can fit within max limit otherwise `{truncated(version)}-{digest}` ([yes there can be large versions](https://pypi.org/project/uselesscapitalquiz/) and I just realized uv can't parse them!). 

I initially wanted to keep all names consistent but I think newer implementation is better.

![image](https://github.com/user-attachments/assets/e30a324f-125b-43e4-92cc-1ee1481717fb)


---

_@zanieb reviewed on 2025-02-26 04:48_

---

_Review comment by @zanieb on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-02-26 04:48_

Cool, thanks! Let's see what @charliermarsh thinks now that it's changed.

---

_Comment by @nkitsaini on 2025-02-26 05:19_

Side Note: Is windows CI always a bit flaky or am I just having a bad luck? I don't know what was the issue for the first failure, second failure seems to be network related.

---

_@nkitsaini reviewed on 2025-02-26 14:01_

---

_Review comment by @nkitsaini on `crates/uv-distribution-filename/src/wheel.rs`:348 on 2025-02-26 14:01_

It seems these hashes are [not stable across compiler releases](https://doc.rust-lang.org/std/hash/trait.Hash.html#portability), while `cache_key` is supposed to be.

We need to use `self.name.cache_key(state)`, which will require either implementing `CacheKey` for all the nested types in `WheelFilename` or we can cut-off to a string using `format!("{}", self.tags).cache_key(state)`. 

I prefer cutting to string which albeit possibly slower will avoid hash change when we restructure inner types, but no strong preference.




---

_@charliermarsh reviewed on 2025-02-26 17:48_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:115 on 2025-02-26 17:48_

I am a fan of `version-tags` and `version-digest`. This looks good to me.

---

_@charliermarsh reviewed on 2025-02-26 17:50_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:348 on 2025-02-26 17:50_

I was hoping we could avoid the allocations but I guess you're right that this could have some negative side effects. Feel free to change.

---

_@nkitsaini reviewed on 2025-02-26 17:54_

---

_Review comment by @nkitsaini on `crates/uv-distribution-filename/src/wheel.rs`:348 on 2025-02-26 17:54_

Cool, changing it. I think since we are going with `version-tags` and `version-digest`, I will also keep the digest to just be digest of the tags instead of full stem.

---

_@zanieb reviewed on 2025-02-26 18:20_

---

_Review comment by @zanieb on `crates/uv-distribution-filename/src/wheel.rs`:485 on 2025-02-26 18:20_

We try to use inline snapshots in the project, if feasible.

---

_Comment by @charliermarsh on 2025-02-26 18:33_

I'll try to get this merged today.

---

_Comment by @zanieb on 2025-02-26 19:35_

Thanks for your work on this!

---

_@charliermarsh reviewed on 2025-02-26 22:18_

---

_Review comment by @charliermarsh on `crates/uv-distribution-filename/src/wheel.rs`:114 on 2025-02-26 22:18_

@nkitsaini -- I changed this to 64 to accommodate common manylinux tags, like `cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64`. I don't see much of a downside. What do you think?

---

_@nkitsaini reviewed on 2025-02-26 22:23_

---

_Review comment by @nkitsaini on `crates/uv-distribution-filename/src/wheel.rs`:114 on 2025-02-26 22:23_

Yeah, it is better. Older one was mostly arbitrary. As long as we stay below 140, it should not be an issue.

---

_@charliermarsh reviewed on 2025-02-26 22:30_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/index/cached_wheel.rs`:156 on 2025-02-26 22:30_

@nkitsaini -- I also removed the `Version` checks from this method. I think the version (before the `-`) isn't actually guaranteed to be valid, since it could be truncated to, like... `1.2.0+`. I might change that in the cache key to avoid those ambiguities, but still seems safer to remove the assumption here. (We already validate that the file ends in `.rev` anyway.)

---

_@nkitsaini reviewed on 2025-02-26 22:40_

---

_Review comment by @nkitsaini on `crates/uv-distribution/src/index/cached_wheel.rs`:156 on 2025-02-26 22:40_

Ah, nice. Missed that. I am also not sure how valuable these checks were. It would make sense to use cache even if the filename is wrong but it points to a valid archive. And I wouldn't assume there would be any invalid files unless created manually, which should be pretty rare hence negating any performance benefits from saved disk I/O.

Maybe I am missing something?


---

_Merged by @charliermarsh on 2025-02-26 22:41_

---

_Closed by @charliermarsh on 2025-02-26 22:41_

---

_Comment by @charliermarsh on 2025-02-26 22:42_

Thanks @nkitsaini! This was a great contribution. I hope you'll consider continuing to contribute to uv.

---

_Branch deleted on 2025-02-27 08:13_

---
