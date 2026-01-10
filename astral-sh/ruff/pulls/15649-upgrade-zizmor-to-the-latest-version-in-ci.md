```yaml
number: 15649
title: Upgrade zizmor to the latest version in CI
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/upgrade-zizmor
created_at: 2025-01-21T15:59:53Z
updated_at: 2025-01-22T17:04:25Z
url: https://github.com/astral-sh/ruff/pull/15649
synced_at: 2026-01-10T20:05:43Z
```

# Upgrade zizmor to the latest version in CI

---

_Pull request opened by @AlexWaygood on 2025-01-21 15:59_

## Summary

The latest version of zizmor points out that we have the default permissions set in several of our GitHub workflows that don't really need any permissions at all. These can be made more secure by restricting the permissions for these workflows.

## Test Plan

CI on this PR. All workflows being modified here are fully run as part of normal PR CI.


---

_Label `red-knot` added by @AlexWaygood on 2025-01-21 15:59_

---

_Label `red-knot` removed by @AlexWaygood on 2025-01-21 15:59_

---

_Label `ci` added by @AlexWaygood on 2025-01-21 16:00_

---

_Comment by @github-actions[bot] on 2025-01-21 16:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @AlexWaygood on 2025-01-21 18:01_

---

_@AlexWaygood reviewed on 2025-01-21 18:03_

---

_Review comment by @AlexWaygood on `.github/zizmor.yml`:17 on 2025-01-21 18:03_

I didn't make modifications to these files because they don't run as part of normal PR CI (only during releases, usually) and it wasn't obvious to me how to figure out what permissions each workflow needs. I didn't want to break the release workflow, and it didn't seem worth investing a large amount of time into trying to investigate exactly what permissions each workflow needed, so I left it for now.

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-01-21 18:03_

---

_Review comment by @dhruvmanila on `.github/zizmor.yml`:17 on 2025-01-22 05:05_

The permissions related to jobs in the release workflow are in `Cargo.toml` here:

https://github.com/astral-sh/ruff/blob/4cfa3555199f9377b29c490cbb703beb6b1eb3b4/Cargo.toml#L309-L310

And, can also be referred in the `release.yml`.

So, I think it's just `build-docker.yml` that requires specific permission while the other two jobs don't require any.

But, I'm fine leaving it as it is.

---

_@dhruvmanila approved on 2025-01-22 05:05_

---

_@MichaReiser reviewed on 2025-01-22 07:13_

---

_Review comment by @MichaReiser on `.github/zizmor.yml`:16 on 2025-01-22 07:13_

It might be helpful to add a short comment explaining why they ignored. Ideally, this wouldn't be necessary (maybe add a todo?)

---

_@MichaReiser approved on 2025-01-22 07:13_

---

_@AlexWaygood reviewed on 2025-01-22 11:24_

---

_Review comment by @AlexWaygood on `.github/zizmor.yml`:17 on 2025-01-22 11:24_

Aah, thank you for pointing that out!

> So, I think it's just `build-docker.yml` that requires specific permission while the other two jobs don't require any.

I think if we put `permissions: {}` in e.g. `publish-wasm.yml`, it would override the permissions passed to the workflow by `release.yml` -- the [docs here](https://docs.github.com/en/actions/sharing-automations/reusing-workflows#supported-keywords-for-jobs-that-call-a-reusable-workflow) state:

> The `GITHUB_TOKEN` permissions passed from the caller workflow can be only downgraded (not elevated) by the called workflow.

And the called workflow here would be `publish-wasm.yml`, so that would mean that `publish-wasm.yml`'s more restricted permissions would downgrade the permissions passed to it by `release.yml`, breaking the release workflow.

I think that means that each of these workflows needs to have exactly the same permissions in their individual workflow files as they have passed to them by `release.yml` -- does that sound right to you?

---

_@dhruvmanila reviewed on 2025-01-22 13:33_

---

_Review comment by @dhruvmanila on `.github/zizmor.yml`:17 on 2025-01-22 13:33_

> > The `GITHUB_TOKEN` permissions passed from the caller workflow can be only downgraded (not elevated) by the called workflow.
> 
> And the called workflow here would be `publish-wasm.yml`, so that would mean that `publish-wasm.yml`'s more restricted permissions would downgrade the permissions passed to it by `release.yml`, breaking the release workflow.
> 
> I think that means that each of these workflows needs to have exactly the same permissions in their individual workflow files as they have passed to them by `release.yml` -- does that sound right to you?

Interesting, doesn't this mean that the nested workflow (e.g., `publish-wasm.yml`) _can_ have `permissions: {}` because that's more restrictive as compared to the parent workflow (`release.yml`) ?

Otherwise, we'll need to revert the `build-binaries.yml` update in this PR as that's also a nested workflow called by `release.yml`.

---

_@AlexWaygood reviewed on 2025-01-22 16:56_

---

_Review comment by @AlexWaygood on `.github/zizmor.yml`:16 on 2025-01-22 16:56_

there was already a TODO on line 4, but I added a comment closer to these lines explaining exactly why they are ignored :-)

---

_Review comment by @AlexWaygood on `.github/zizmor.yml`:17 on 2025-01-22 16:57_

> Otherwise, we'll need to revert the `build-binaries.yml` update in this PR as that's also a nested workflow called by `release.yml`.

I'm _pretty_ confident that the `build-binaries.yml` change is okay because, unlike the other workflows, that workflow is run exactly the same way as part of PR CI as it is when it's called from `release.yml`. (And the CI on this PR is green.)

I'm just going to leave this as-is for now, but I'm very happy if somebody else wants to experiment with removing these ignores and actually fixing the issues zizmor is complaining about!

---

_@AlexWaygood reviewed on 2025-01-22 16:57_

---

_Merged by @AlexWaygood on 2025-01-22 17:00_

---

_Closed by @AlexWaygood on 2025-01-22 17:00_

---

_Branch deleted on 2025-01-22 17:00_

---
