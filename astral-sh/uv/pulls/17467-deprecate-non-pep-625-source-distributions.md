```yaml
number: 17467
title: Deprecate non-PEP 625 source distributions
type: pull_request
state: open
author: woodruffw
labels: []
assignees: []
base: main
head: ww/deprecate-non-pep625
created_at: 2026-01-14T15:58:33Z
updated_at: 2026-01-15T03:30:28Z
url: https://github.com/astral-sh/uv/pull/17467
synced_at: 2026-01-15T03:52:25Z
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

_Comment by @konstin on 2026-01-14 17:54_

zips can use a number of compression options, such as stored (no compression), DEFLATE (the default) but also LZMA. There's a list unter "Compression method" in https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html. There's nothing that says what kind of zips the wheel are, but generally, they are either DEFLATE (basically all published zips) or stored (e.g. for editables).

---

_@zanieb reviewed on 2026-01-14 18:02_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:384 on 2026-01-14 18:02_

Note you could also use `warn_user_once`

---

_@zanieb reviewed on 2026-01-14 18:07_

---

_Review comment by @zanieb on `crates/uv-distribution/src/distribution_database.rs`:390 on 2026-01-14 18:07_

This needs to include the dist name, I think?

I'd say something like "`{dist}` is not a standards-compliant source distribution: expected extension `{}` but found `{}`. A future version of uv will reject source distributions that do not match the specification defined in PEP 625."

---

_Comment by @woodruffw on 2026-01-14 18:19_

> zips can use a number of compression options, such as stored (no compression), DEFLATE (the default) but also LZMA. There's a list unter "Compression method" in [users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html](https://users.cs.jmu.edu/buchhofp/forensics/formats/pkzip.html). There's nothing that says what kind of zips the wheel are, but generally, they are either DEFLATE (basically all published zips) or stored (e.g. for editables).

Ah yeah, sorry -- I misunderstood you to be saying that `.whl` could sometimes by a `.tar.xz`, not a ZIP with a different interior compression for a specific entry.

So, there are two distinct things we want to warn on/deprecate:

1. We want to deprecate non-PEP 625 sdists
2. We want to deprecate non-DEFLATE/stored file entries in ZIPs (which will typically be wheels, but could also be sdists right now)

This PR is focusing on (1) right now, but I could also do (2) (or in a follow-up).

---

_Comment by @konstin on 2026-01-14 18:26_

(1) is definitely a good unit to merge on it's own. I'm mainly bringing up (2) cause we also need it to remove the xz/lzma dependencies.

---

_Marked ready for review by @woodruffw on 2026-01-15 00:11_

---

_@zanieb reviewed on 2026-01-15 02:34_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:12672 on 2026-01-15 02:34_

```suggestion
    warning: workspace @ https://github.com/user-attachments/files/16592193/workspace.zip is not a standards-compliant source distribution: expected '.tar.gz' but found '.zip'. A future version of uv will reject source distributions that do not match the specification defined in PEP 625
```

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 02:35_

`wsgiref==0.1.2` doesn't seem quite right here. I think we need the filename too?

---

_@zanieb reviewed on 2026-01-15 02:35_

---

_@woodruffw reviewed on 2026-01-15 02:40_

---

_Review comment by @woodruffw on `crates/uv/tests/it/lock.rs`:12672 on 2026-01-15 02:40_

Oops, thanks!

---

_@woodruffw reviewed on 2026-01-15 02:41_

---

_Review comment by @woodruffw on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 02:41_

I was thinking about that, and I think the filename might not be super useful information to a user who sees this: the average sdist consumer is _probably_ getting one via an index resolution, so showing them `wsgiref-0.1.2.zip` isn't going to mean much to them (whereas `wsgiref==0.1.2` at least tells them that it's a post-resolution candidate, so they could change it by updating their constraints).

But I'm speculating, if you think the filename is helpful here I'll change!

---

_@zanieb reviewed on 2026-01-15 02:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 02:52_

I think we need the filename since the rest of the error is focused on it, I would maybe say `{filename} ({package}=={version}) is not ...`?

Unless you want to change the error in the case where the user has not requested a file directly, which may be better. In that case, I think we'd change the whole error message to be something like

> {package}=={version} uses a legacy source distribution format (`.zip`) that is not compliant with the specification in PEP 625. A future version of uv will .... Consider upgrading to a newer version of {package}

I think different messages for these scenarios might make a lot of sense?

---

_@zanieb reviewed on 2026-01-15 02:53_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 02:53_

Also, I didn't realize there were packages on the index that were using the legacy format. We might want to drop support for that _later_ / separately. We might also want to ignore them during resolution but allow opt-in during a transition period? Lots to consider there.

---

_@woodruffw reviewed on 2026-01-15 03:15_

---

_Review comment by @woodruffw on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 03:15_

> I think we need the filename since the rest of the error is focused on it, I would maybe say `{filename} ({package}=={version}) is not ...`?

Ah yeah, that's a good point. I'll add the filename.

I'm inclined to keep it as a single warning for both direct and indirect resolved candidates, since I think in both cases it's not super directly actionable. Although now that I say that I guess it *is* actionable when the user does e.g. a GitHub zipball, so maybe that deserves a discrete warning. I'll look at that tomorrow.


> Also, I didn't realize there were packages on the index that were using the legacy format. We might want to drop support for that _later_ / separately. We might also want to ignore them during resolution but allow opt-in during a transition period? Lots to consider there.

Yeah, there's lots of potential nuance. The "good" news is that the legacy sdists on the index *should* generally be pretty old (as in 2016 or earlier), meaning that they shouldn't be selected as candidates super often. `wsgiref==0.1.2` is from 2006, for example -- I'm actually shocked that installs at all 😅 

---

_@zanieb reviewed on 2026-01-15 03:30_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9578 on 2026-01-15 03:30_

Upgrading to a newer version seems more actionable than regenerating a local tarball too 🤷‍♀️ 

---
