```yaml
number: 17570
title: "[`airflow`] apply Replacement::AutoImport to `AIR302`"
type: pull_request
state: merged
author: Lee-W
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: auto-import-AIR312
created_at: 2025-04-23T02:35:45Z
updated_at: 2025-05-02T08:09:27Z
url: https://github.com/astral-sh/ruff/pull/17570
synced_at: 2026-01-10T18:57:02Z
```

# [`airflow`] apply Replacement::AutoImport to `AIR302`

---

_Pull request opened by @Lee-W on 2025-04-23 02:35_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This is not yet fixing anything as the names are not changed, but it lays down the foundation for fixing.

## Test Plan

<!-- How was it tested? -->

the existing test fixture should already cover this change


---

_Renamed from "Auto import air312" to "[`airflow`] apply Replacement::AutoImport to `AIR312`" by @Lee-W on 2025-04-23 02:36_

---

_Comment by @github-actions[bot] on 2025-04-23 02:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @Lee-W on 2025-04-29 01:19_

---

_Comment by @Lee-W on 2025-04-30 06:42_

Hey @ntBre , this is also ready to be reviewed 🙂 

btw if we would like to mark AIR3XX as stable, what would be the requirement? are we allow to gradually improve the rules afterward?

---

_Comment by @ntBre on 2025-04-30 19:20_

> btw if we would like to mark AIR3XX as stable, what would be the requirement? are we allow to gradually improve the rules afterward?

Going by the [rule stabilization](https://docs.astral.sh/ruff/versioning/#rule-stabilization) docs, the main normal requirement is that a rule has been in preview for one minor release cycle. So since these are going into 0.11.x, I think we usually wouldn't consider stabilizing them until 0.13.0, or at least 0.12.0.

That page also says:

> Stable rule behaviors are not changed significantly in patch versions

So we wouldn't want to change them drastically after stabilization, but if you mean something more like adding a few new deprecated imports, I think that would be okay.

These rules feel like a bit of an exception to the general guidelines to me, so we could possibly consider stabilizing them sooner, but I'd defer to @MichaReiser or @dhruvmanila on that decision.

Were you just hoping that users could access these without enabling preview mode?


---

_Label `rule` added by @ntBre on 2025-04-30 19:22_

---

_Label `preview` added by @ntBre on 2025-04-30 19:22_

---

_@ntBre approved on 2025-04-30 19:52_

Looks good, thanks!

---

_Merged by @ntBre on 2025-04-30 19:53_

---

_Closed by @ntBre on 2025-04-30 19:53_

---

_Comment by @ntBre on 2025-04-30 19:55_

I noticed this a bit late, but is the PR title supposed to say `AIR302`? I'll try to remember to update this in the changelog tomorrow if so.

---

_Comment by @dhruvmanila on 2025-04-30 20:46_

> I noticed this a bit late, but is the PR title supposed to say `AIR302`? I'll try to remember to update this in the changelog tomorrow if so.

You can update it right now as, I think, rooster will fetch the latest PR title :)

---

_Comment by @ntBre on 2025-04-30 20:48_

> You can update it right now as, I think, rooster will fetch the latest PR title :)

Ahh good to know, I assumed that it took the commit message. Thanks!

---

_Renamed from "[`airflow`] apply Replacement::AutoImport to `AIR312`" to "[`airflow`] apply Replacement::AutoImport to `AIR302`" by @ntBre on 2025-04-30 20:48_

---

_Comment by @dhruvmanila on 2025-04-30 21:08_

Everything that @ntBre said is what we usually follow. In this case we could possibly stabilize it in the next minor release but that isn't planned in the near future (possibly in June). Do you think there would be any issue in recommending users to use the `--preview` flag when migrating? It can be clubbed with `--select` flag to only select certain rules so that other preview rules are not selected. I've opened https://github.com/astral-sh/ruff/issues/17749 to track this.

---

_Comment by @Lee-W on 2025-05-02 08:09_

> Everything that @ntBre said is what we usually follow. In this case we could possibly stabilize it in the next minor release but that isn't planned in the near future (possibly in June). Do you think there would be any issue in recommending users to use the `--preview` flag when migrating? It can be clubbed with `--select` flag to only select certain rules so that other preview rules are not selected. I've opened #17749 to track this.

Got it. The only concern we have is the `--preview` flag, but it doesn't sound like a major concern. Thanks for letting us know! I'll bring the message back to the community!

---
