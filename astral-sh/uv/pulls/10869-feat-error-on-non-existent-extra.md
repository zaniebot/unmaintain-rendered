```yaml
number: 10869
title: "feat: error on non-existent extra"
type: pull_request
state: closed
author: mkniewallner
labels:
  - breaking
assignees: []
base: main
head: feat/error-on-non-existent-extra
created_at: 2025-01-22T18:14:50Z
updated_at: 2025-02-09T14:46:05Z
url: https://github.com/astral-sh/uv/pull/10869
synced_at: 2026-01-12T16:09:32Z
```

# feat: error on non-existent extra

---

_@mkniewallner_

## Summary

Closes #10597.

Check for invalid extras specified on the CLI, through either `--extra <extra>` or `--all-extras --no-extra <extra>`

If an usage of dynamic optional dependencies is detected, validation is not performed, as in this case, we cannot determine which extras exist, as this is delegated to the build backend.

This concretely means that we do not perform the validation if:
- for workspace, at least one project uses dynamic optional dependencies
- for project, it uses dynamic optional dependencies

Adding this check required to update quite a lot of things. I've tried to split the changes as much as possible, especially since there are some changes I'm not sure about, in terms of architecture.

First, to my knowledge, optional dependencies work similarly to dependency groups for the validation, so I thought it would be ok to make `DependencyGroupsTarget` more generic by renaming it to `SpecificationTarget`, so that the same logic can be reused both for dependency groups and optional dependencies, but maybe you have a different opinion.

I also needed to add `dynamic` field to `Project`, and a few methods to check the usage of a dynamic key in a project or workspace. Not sure if there are better ways or places to do that.

## Test Plan

Snapshot tests.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-01-22 18:16_

---

_Comment by @Gankra on 2025-01-22 20:00_

cross-talk note from #10861 which is working on a similar thing:

The `pip install` and `pip compile` commands also take `--extra`, and with my PR will also take `--dependency-group`. However those commands have a quirky ability to accept *multiple* pyproject.tomls, which raises questions of "how should diagnostics even work for that".

(It's fine if this PR doesn't handle the pip commands, just doing these ones is an Improvement!)

---

_Comment by @mkniewallner on 2025-01-22 23:41_

> cross-talk note from #10861 which is working on a similar thing:
> 
> The `pip install` and `pip compile` commands also take `--extra`, and with my PR will also take `--dependency-group`. However those commands have a quirky ability to accept _multiple_ pyproject.tomls, which raises questions of "how should diagnostics even work for that".
> 
> (It's fine if this PR doesn't handle the pip commands, just doing these ones is an Improvement!)

Thanks for the heads up! Depending on how complex it is, it might be easier to implement as a follow-up PR yep, not sure, I'm not familiar with the pip interface.

---

_Marked ready for review by @mkniewallner on 2025-01-23 03:34_

---

_Review comment by @mkniewallner on `crates/uv/tests/it/lock_conflict.rs`:4416 on 2025-01-23 03:35_

Not sure if the test is still relevant as this now errors out, but since there's also a lock assertion right after, I've added a `sync` call without argument below, as I wasn't sure.

---

_@mkniewallner reviewed on 2025-01-23 03:36_

---

_Assigned to @zanieb by @zanieb on 2025-01-23 05:05_

---

_Comment by @Gankra on 2025-01-23 14:12_

For the record I landed my checking here:

https://github.com/astral-sh/uv/blob/e3a09888abaa2cd504e7511b1bd38c1f4efc9c56/crates/uv-requirements/src/source_tree.rs#L118-L131

My implementation is a bit flawed, notably because it runs "per pyproject.toml" and in concurrent async. Because this wasn't the primary focus of my PR I put in a hacky filter to leave better checking as future work:

https://github.com/astral-sh/uv/blob/e3a09888abaa2cd504e7511b1bd38c1f4efc9c56/crates/uv/tests/it/pip_install.rs#L8617-L8620

The "better" implementation of this would shove a bunch of extra state in `SourceTreeResolution` and return it from `SourceTreeResolver::resolve_source_tree`, so that `SourceTreeResolver::resolve` could see the defined extras/dependency-groups of all pyprojects at once and produce deterministic output (and potentially error on totally undefined keys?). I don't have plans to implement that myself, at the moment.

---

_Review requested from @Gankra by @Gankra on 2025-01-23 14:12_

---

_Comment by @charliermarsh on 2025-01-23 14:13_

I think we should be doing this against the lockfile instead -- same for dependency groups, probably. Then it will work with `--frozen`, and we won't need any sort of special-casing for dynamic.

---

_Label `breaking` added by @zanieb on 2025-01-23 14:15_

---

_Comment by @mkniewallner on 2025-01-24 01:48_

> I think we should be doing this against the lockfile instead -- same for dependency groups, probably. Then it will work with `--frozen`, and we won't need any sort of special-casing for dynamic.

Tried to implement the validation at lock file level in https://github.com/astral-sh/uv/pull/10925, but one case that cannot be handled is if we have an extra with no dependencies, like so:

```toml
[project.optional-dependencies]
http = []
```

as in that case, the empty list would not get added to the lock file, so this will be reported as not existing.

Don't know how ok this is, but if it is fine, we'll probably have to rephrase the errors to not confuse the users.

---

_Comment by @charliermarsh on 2025-01-24 02:24_

Ah dang... I did think about this before posting, and I thought we included these in `project.requires-dist`, but I guess that's only true for empty _groups_ (which are included in `project.requires-dev`). Arguably we should start writing them to the lockfile as `provides-extra`?


---

_Comment by @charliermarsh on 2025-01-24 14:54_

@mkniewallner -- Do you want me to first PR a change to add `provides-extras` to the lockfile, so we can use that?

---

_Comment by @mkniewallner on 2025-01-24 16:36_

> @mkniewallner -- Do you want me to first PR a change to add `provides-extras` to the lockfile, so we can use that?

Won't have time to do so right away, so feel free to :)

Just curious though, since this would be a new field, that would I believe only be present if we have `project.optional-dependencies` (even if empty), how will the retro-compatibility be handled for older versions?

I believe we won't be able to differentiate between:
- previous uv versions that did not set the new lock file attribute, even if `project.optional-dependencies` is set
- new uv versions that set the new attribute, but could not be present if there is no `project.optional-dependencies` (which seems to be the behaviour of `requires-dev` from what I see)

I believe since the check would now live after locking it might be fine, but even though, this means that people working on the same project would all have to update uv to the minimum version that sets the new attribute to avoid adding/removing it depending on the version used for locking?

But I guess that since the check would land in 0.6.0, this is less problematic than shipping this in a patch version though, as this could be advertised.

---

_Comment by @charliermarsh on 2025-01-24 17:22_

We would need to consider always-setting `provides-extra`, even if it's empty, so we can detect whether the information is available.

---

_Comment by @Gankra on 2025-01-30 13:50_

Depends on #11063 

---

_Comment by @mkniewallner on 2025-02-09 14:46_

Closing in favour of https://github.com/astral-sh/uv/pull/10925.

---

_Closed by @mkniewallner on 2025-02-09 14:46_

---
