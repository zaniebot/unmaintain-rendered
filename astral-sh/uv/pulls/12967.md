```yaml
number: 12967
title: minify pythons json on compile time
type: pull_request
state: merged
author: MeitarR
labels: []
assignees: []
merged: true
base: main
head: uv-python-minify-at-build-time
created_at: 2025-04-18T13:09:58Z
updated_at: 2025-07-14T15:47:49Z
url: https://github.com/astral-sh/uv/pull/12967
synced_at: 2026-01-10T06:53:01Z
```

# minify pythons json on compile time

---

_Pull request opened by @MeitarR on 2025-04-18 13:09_

## Summary

In #10939 I added the generated `crates/uv-python/src/download-metadata-minified.json` file which is a minified version of `crates/uv-python/download-metadata.json`.

The main reason for this PR is to avoid bloating the git objects as this is a single-line file.

As a bonus, I also filtered the embed json to include only the versions for the compiled target. Which should improve the binary size and performance by a bit.

## Test Plan

<!-- How was it tested? -->


---

_Converted to draft by @MeitarR on 2025-04-18 13:14_

---

_Marked ready for review by @MeitarR on 2025-04-18 15:05_

---

_Review requested from @Gankra by @zanieb on 2025-04-18 16:14_

---

_Comment by @Gankra on 2025-04-28 19:30_

> As a bonus, I also filtered the embed json to include only the versions for the compiled target. Which should improve the binary size and performance by a bit.

This is an interesting idea, I wonder if it's potentially a functionality regression. 

At least for debugging/testing `uv` it's been genuinely useful to me to be able to ask `uv` to fetch wheels for the "wrong" platform (`uv pip install --python-platform linux`). I'm not sure if the same thing applies to runtimes though?

---

_Comment by @zanieb on 2025-04-28 19:35_

I don't think we can filter these — it's a breaking change and I think cross-installs are genuinely useful and something we'll build workflows around in the future.

---

_@Gankra approved on 2025-04-28 19:37_

I'll happily accept this PR without the last commit that adds the filtering.

---

_Comment by @MeitarR on 2025-04-30 16:36_

> I'll happily accept this PR without the last commit that adds the filtering.

removed it

---

_Review requested from @Gankra by @MeitarR on 2025-04-30 19:12_

---

_@Gankra approved on 2025-04-30 19:50_

---

_Merged by @Gankra on 2025-04-30 19:51_

---

_Closed by @Gankra on 2025-04-30 19:51_

---

_Branch deleted on 2025-04-30 20:09_

---

_Renamed from "minify and filter embed managed pythons json on compile time" to "minify pythons json on compile time" by @zanieb on 2025-07-14 15:47_

---
