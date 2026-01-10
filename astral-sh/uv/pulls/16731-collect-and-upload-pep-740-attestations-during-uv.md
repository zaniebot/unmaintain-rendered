```yaml
number: 16731
title: "Collect and upload PEP 740 attestations during `uv publish`"
type: pull_request
state: merged
author: woodruffw
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ww/upload-attestations
created_at: 2025-11-13T21:34:46Z
updated_at: 2025-11-24T21:47:17Z
url: https://github.com/astral-sh/uv/pull/16731
synced_at: 2026-01-10T05:58:11Z
```

# Collect and upload PEP 740 attestations during `uv publish`

---

_Pull request opened by @woodruffw on 2025-11-13 21:34_

## Summary

~~Still working on this.~~

TL;DR: This makes `uv publish` behave like `twine upload`: when a user does `uv publish dist/*` and `dist/*` includes attestations, we now group those attestations with their matching distribution and include them in the upload. This changes the behavior from the previous behavior, which silently skipped these (since they don't match the distribution filename format).

This is a step towards #15618: we don't produce attestations *within* uv itself yet, but this allows uv to upload them if they're already present as part of the distribution paths.

## Test Plan

I've broken the `uv-publish` crate's functionality for collecting upload inputs a part a bit to make testing of the grouping logic easier; `files_for_publishing` is now `group_files_for_publishing`, with an interior helper (`group_files`) that does no I/O or filesystem ops. I've added unit tests for that inner helper to confirm our matching/grouping works as expected and doesn't regress on other publishing tests.

Separately, it'd be nice to have some kind of integration test with an index that supports attestations, like PyPI or TestPyPI. I'll need to think a bit about how best to do that 🙂 


---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:248 on 2025-11-14 11:02_

`UploadDistribution` or `UploadFile` maybe, "group" is also used for dependency groups.

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:287 on 2025-11-14 11:04_

Can we avoid a validate/parse split, e.g. with `extension` and `file_stem`?

---

_@konstin reviewed on 2025-11-14 11:07_

---

_@woodruffw reviewed on 2025-11-14 13:18_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:248 on 2025-11-14 13:18_

UploadDistribution sounds good, thanks!

---

_Comment by @woodruffw on 2025-11-14 14:17_

Flagging one thing: unlike `twine upload` this currently sends attestations unconditionally if they're present, which may not play super well with various indices (they may reject the `attestations` form part rather than silently skipping it). I'll need to add this to our upload tests.

---

_@woodruffw reviewed on 2025-11-18 19:06_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py.lock`:1 on 2025-11-18 19:06_

Sorry for the big delta here...`pypi-attestations` and `sigstore` are not lightweight packages.

---

_Marked ready for review by @woodruffw on 2025-11-18 19:07_

---

_@messerli-wallace approved on 2025-11-18 19:28_

---

_Review requested from @konstin by @woodruffw on 2025-11-18 19:30_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:124 on 2025-11-19 09:01_

Is there a way for us to check that the attestation was actually uploaded? That's the end-to-end test coverage I'm most interested in.

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:245 on 2025-11-19 09:02_

```suggestion
/// Represents a single "to-be-uploaded" distribution, along with zero
```

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:262 on 2025-11-19 09:08_

This slipped through last time but we should box large error variants: https://gist.github.com/konstin/c54456482a14c7528829fdf4567040dd



---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:297 on 2025-11-19 09:12_

nit: Move above the if

I've been trying to figure out that parser until i found the comment was below

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:1162 on 2025-11-19 09:33_

We can use fastrand, it's already in our dependencies

---

_@konstin reviewed on 2025-11-19 09:36_

---

_@woodruffw reviewed on 2025-11-19 13:43_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:124 on 2025-11-19 13:43_

We could check the simple index for it, yeah. I'll add that to the  test.

---

_@woodruffw reviewed on 2025-11-19 15:29_

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:1162 on 2025-11-19 15:29_

Thanks! Didn't notice that because it wasn't a workspace dep, but I'll tweak that.

---

_@woodruffw reviewed on 2025-11-19 16:03_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:124 on 2025-11-19 16:03_

Added a helper that'll poke the JSON index response for provenance. That deviates slightly from our ad hoc use of the HTML index for version listings, but IMO that's fine in this case (since attestations are currently only tested against indices that support the JSON index).

---

_Review comment by @woodruffw on `crates/uv-publish/src/lib.rs`:64 on 2025-11-19 16:13_

Noting: I've boxed both of these fields: `DisplaySafeUrl` creeps over the member size on Windows, and `PublishSendError` is consistently over it on both Windows and Linux.

---

_@woodruffw reviewed on 2025-11-19 16:13_

---

_Review requested from @konstin by @woodruffw on 2025-11-19 16:58_

---

_Label `enhancement` added by @woodruffw on 2025-11-19 16:59_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:516 on 2025-11-20 13:16_

We need to move `wait_for_index` ahead of this, and ideally ensure that we use the same URL as `uv pip compile`: Indexes cache pages, an sometimes it takes a while until the new page is available, so we have to check in a loop for a while which `wait_for_index`.

I don't know the exact details of the caching involved, but I could see how different index pages are updated at different times.

---

_@konstin reviewed on 2025-11-20 13:16_

We need to fix the index cache invalidation handling, otherwise r+.

---

_@woodruffw reviewed on 2025-11-20 14:54_

---

_Review comment by @woodruffw on `scripts/publish/test_publish.py`:516 on 2025-11-20 14:54_

> We need to move `wait_for_index` ahead of this

Makes sense!

> ideally ensure that we use the same URL as `uv pip compile`

This is going to be moderately annoying, since we'd probably need to move to something more structured (`lxml`?) for parsing of the HTML index 😅 -- I *think* in practice using the JSON index here is fine, since we're only testing this against PyPI at the moment and (last I checked) PyPI's invalidation is simultaneous for the HTML and JSON views of the same simple index route.

If you feel strongly about it I'll make that change, but my first thought here would be to keep the JSON index usage for now, with a comment that it might need to be replaced/removed once we expand the attestation check to other indices. Thoughts?

---

_Comment by @zanieb on 2025-11-20 14:58_

We'll want documentation for this as well. Let me know if you need help finding a place for it. If you sketch the content I can help with making it fit.

---

_Comment by @woodruffw on 2025-11-20 15:08_

> We'll want documentation for this as well. Let me know if you need help finding a place for it.

Thanks! I'm guessing maybe this belongs under `guides/package.md`, i.e. the building/publishing guide? Longer term it probably also makes sense to mention/describe in the GitHub integration guide, although maybe not until attestation generation happens in uv itself.

---

_Comment by @woodruffw on 2025-11-20 15:34_

I added a small section to the packaging guide, but I'm not sure how much detail to include. One thing that made me realize is that we don't really expose a knob for this at the moment -- maybe it makes sense to add `--no-attestations` / `UV_PUBLISH_NO_ATTESTATIONS` for non-supporting indices that misbehave and fail rather than ignoring attestations?

---

_Comment by @konstin on 2025-11-20 15:34_

The knobs and their names sound good

---

_@konstin reviewed on 2025-11-20 15:36_

---

_Review comment by @konstin on `scripts/publish/test_publish.py`:516 on 2025-11-20 15:36_

iirc `uv pip compile` prefer the JSON version, so that should be fine, and that PyPI uses simultaneous invalidation helps. I'd try this version and fine-tune if we see it failing too often.

---

_Review requested from @konstin by @woodruffw on 2025-11-20 16:54_

---

_Review requested from @zanieb by @woodruffw on 2025-11-20 16:54_

---

_Assigned to @woodruffw by @woodruffw on 2025-11-20 16:54_

---

_@zanieb reviewed on 2025-11-20 18:35_

---

_Review comment by @zanieb on `docs/guides/package.md`:178 on 2025-11-20 18:35_

Should we link or suggest how they would be created?

---

_@woodruffw reviewed on 2025-11-20 18:49_

---

_Review comment by @woodruffw on `docs/guides/package.md`:178 on 2025-11-20 18:49_

The wrinkle here is that there's no _good_ way to do this currently 😅 -- we could point users to `pypi-attestations` as a DIY approach, but that's a glue package that isn't really intended for direct usage.

(What `gh-action-pypi-publish` does is have a helper script that uses the `pypi-attestations` APIs.)

One option here would be to create `astral-sh/pypi-attest` as an action, which we'd then recommend at least until we have in-client attesting. That would only take me an hour or two to build.

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:7156 on 2025-11-24 10:41_

We should update the description for `files` above to note that it now includes attestations when found by the glob.

---

_@konstin approved on 2025-11-24 10:43_

---

_@woodruffw reviewed on 2025-11-24 14:06_

---

_Review comment by @woodruffw on `crates/uv-cli/src/lib.rs`:7156 on 2025-11-24 14:06_

[`b6823e2` (#16731)](https://github.com/astral-sh/uv/pull/16731/commits/b6823e2cfa60698f39de0c8f2e5f73774f75b363)

---

_Merged by @woodruffw on 2025-11-24 21:47_

---

_Closed by @woodruffw on 2025-11-24 21:47_

---

_Branch deleted on 2025-11-24 21:47_

---
