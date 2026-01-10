```yaml
number: 15358
title: "[red-knot] Property test improvements"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/property-test-updates
created_at: 2025-01-08T20:17:05Z
updated_at: 2025-01-08T21:35:46Z
url: https://github.com/astral-sh/ruff/pull/15358
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Property test improvements

---

_Pull request opened by @sharkdp on 2025-01-08 20:17_

## Summary

- Add a workflow to run property tests on a daily basis (based on `daily_fuzz.yaml`)
- Mark `assignable_to_is_reflexive` as flaky (related to #14899)
- Add new (failing) `intersection_assignable_to_both` test (also related to #14899)

## Test Plan

```bash
export QUICKCHECK_TESTS=100000
while cargo test --release -p red_knot_python_semantic -- \
  --ignored types::property_tests::stable; do :; done
```

---

_Label `testing` added by @sharkdp on 2025-01-08 20:17_

---

_Label `red-knot` added by @sharkdp on 2025-01-08 20:17_

---

_Review requested from @carljm by @sharkdp on 2025-01-08 20:17_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-08 20:17_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-08 20:17_

---

_@sharkdp reviewed on 2025-01-08 20:19_

---

_Review comment by @sharkdp on `.github/workflows/daily_property_tests.yaml`:53 on 2025-01-08 20:19_

Like the rest of this workflow, this is copied over from `daily_fuzz.yaml`. It seems like a reasonable idea (how would we notice otherwise?), but I haven't actually seen any *"Daily parser fuzz failed on …"* in the issue tracker. Did the `daily_fuzz` workflow never run, or is this issue-creation disabled somehow?

---

_Review comment by @carljm on `.github/workflows/daily_property_tests.yaml`:43 on 2025-01-08 20:20_

My reading of this comment would suggest that the next line would do a debug build, but the line below appears to do a release build? (I personally don't have strong feelings either way, just curious if this comment should be updated to explain why you actually chose a release build.)

---

_@carljm approved on 2025-01-08 20:22_

Looks good to me!

I have no idea how to verify that the cron thing actually works, other than to land this and wait and see? (I don't even know where to go look tomorrow to see if it ran, assuming it doesn't fail and notify us.)

---

_Comment by @sharkdp on 2025-01-08 20:26_

> I have no idea how to verify that the cron thing actually works, other than to land this and wait and see?

The fuzzer tests run daily at 00:00 (UTM?), so I decided to run the property test daily at 12:00, to distribute a bit among timezones, and so I will be awake when they run :smile: 

https://crontab.guru/#0_12_*_*_*


> I don't even know where to go look tomorrow to see if it ran

Here: https://github.com/astral-sh/ruff/actions/workflows/daily_property_tests.yaml

Sample run is here: https://github.com/astral-sh/ruff/actions/runs/12678401447/job/35335911931

---

_Comment by @github-actions[bot] on 2025-01-08 20:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2025-01-08 20:36_

---

_Review comment by @sharkdp on `.github/workflows/daily_property_tests.yaml`:43 on 2025-01-08 20:36_

Yes, thanks. Should have left this in draft mode until I gathered all the data. I ran two experiments.

Release mode: https://github.com/astral-sh/ruff/actions/runs/12678401447 (build: 1m 53s, five test iterations: 1m 26s)
Debug mode: https://github.com/astral-sh/ruff/actions/runs/12678489298/job/35336173933 (build: 53s, five test iterations: 14 minutes)

The number of tests (5 iterations × 100000 tests / iteration = 0.5 million tests) is not chosen completely arbitrarily. From local tests, this seems to be the right order of magnitude to uncover some bugs that were not detected at one or two orders of magnitude fewer.

So release mode seems like the way to go. Will update that comment.

---

_Comment by @AlexWaygood on 2025-01-08 21:19_

@sharkdp

> Like the rest of this workflow, this is copied over from `daily_fuzz.yaml`. It seems like a reasonable idea (how would we notice otherwise?), but I haven't actually seen any _"Daily parser fuzz failed on …"_ in the issue tracker. Did the `daily_fuzz` workflow never run, or is this issue-creation disabled somehow?

@carljm

> I have no idea how to verify that the cron thing actually works, other than to land this and wait and see? (I don't even know where to go look tomorrow to see if it ran, assuming it doesn't fail and notify us.)

The py-fuzzer script found a number of bugs in our parser immediately after the big parser rewrite in early 2024, but to my knowledge, the daily cron job that we set up to fuzz the parser shortly after the parser rewrite has never failed. @dhruvmanila just wrote a really good parser! 😃 (And also, we haven't made any major updates since the big rewrite.)

However, I'm pretty confident that the issue-creation logic works, because I've used it to great success in multiple other repos. For examples, see:
- Typeshed:
  - Example issue: https://github.com/python/typeshed/issues/13330
  - Workflow: https://github.com/python/typeshed/blob/main/.github/workflows/daily.yml
- typing_extensions:
  - Example issue: https://github.com/python/typing_extensions/issues/513
  - Workflow: https://github.com/python/typing_extensions/blob/main/.github/workflows/third_party.yml
- typeshed-stats:
  - Example issue: https://github.com/AlexWaygood/typeshed-stats/issues/255
  - Workflow: https://github.com/AlexWaygood/typeshed-stats/blob/main/.github/workflows/test.yml

---

_Merged by @sharkdp on 2025-01-08 21:24_

---

_Closed by @sharkdp on 2025-01-08 21:24_

---

_Branch deleted on 2025-01-08 21:24_

---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yaml`:70 on 2025-01-08 21:26_

this should be

```suggestion
              labels: ["bug", "red-knot", "testing"],
```

(https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20label%3Ared-knot)

---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yaml`:69 on 2025-01-08 21:28_

A typeshed contributor recently made a great improvement to the typeshed version of this workflow so that the issue text links directly to the exact run that failed rather than a list of all the runs that have ever happened: https://github.com/python/typeshed/pull/13210/files

We could probably make the same improvement to this workflow and `daily_fuzz.yaml`!

---

_@AlexWaygood reviewed on 2025-01-08 21:28_

---

_@sharkdp reviewed on 2025-01-08 21:35_

---

_Review comment by @sharkdp on `.github/workflows/daily_property_tests.yaml`:70 on 2025-01-08 21:35_

Thank you: https://github.com/astral-sh/ruff/pull/15361

---

_@sharkdp reviewed on 2025-01-08 21:35_

---

_Review comment by @sharkdp on `.github/workflows/daily_property_tests.yaml`:69 on 2025-01-08 21:35_

https://github.com/astral-sh/ruff/pull/15361

---
