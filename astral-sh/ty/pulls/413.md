```yaml
number: 413
title: Add daily property tests
type: pull_request
state: merged
author: MatthewMckee4
labels: []
assignees: []
merged: true
base: main
head: property-tests
created_at: 2025-05-15T20:02:49Z
updated_at: 2025-05-16T07:58:41Z
url: https://github.com/astral-sh/ty/pull/413
synced_at: 2026-01-10T02:34:10Z
```

# Add daily property tests

---

_Pull request opened by @MatthewMckee4 on 2025-05-15 20:02_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #410

## Test Plan

Tested the commands locally


---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yml`:35 on 2025-05-15 20:50_

Can we just checkout the Ruff repo instead of this repo? That way the CI job will use the `main` branch of the Ruff repo for testing, rather than the submodule bundled in this repo (which might not be up to date with the main branch of the Ruff repo)

I think this is the config you need to do that:

```suggestion
      - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
        with:
          persist-credentials: false
          repository: astral-sh/ruff
```

---

_@AlexWaygood reviewed on 2025-05-15 20:51_

Thank you!!

---

_Comment by @AlexWaygood on 2025-05-15 20:51_

We'll also need to make a PR over at Ruff to remove the property-test workflow from that repo, after this is merged -- are you up to do that as well?

---

_@MatthewMckee4 reviewed on 2025-05-15 20:53_

---

_Review comment by @MatthewMckee4 on `.github/workflows/daily_property_tests.yml`:35 on 2025-05-15 20:53_

Yeah this makes sense, my thinking was that it only mattered about what commit hash of ruff was currently used for ty, but this makes more sense

---

_@AlexWaygood reviewed on 2025-05-15 20:56_

---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yml`:49 on 2025-05-15 20:56_

now that we're checking out Ruff rather than ty:

```suggestion
      - name: Build ty
        # A release build takes longer (2 min vs 1 min), but the property tests run much faster in release
        # mode (1.5 min vs 14 min), so the overall time is shorter with a release build.
        run: cargo build --locked --release --package ty_python_semantic --tests
      - name: Run property tests
        shell: bash
```

---

_@MatthewMckee4 reviewed on 2025-05-15 20:57_

---

_Review comment by @MatthewMckee4 on `.github/workflows/daily_property_tests.yml`:49 on 2025-05-15 20:57_

sure, thanks

---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yml`:9 on 2025-05-15 21:02_

```suggestion
      - ".github/workflows/daily_property_tests.yml"
```

---

_@AlexWaygood reviewed on 2025-05-15 21:02_

---

_@AlexWaygood reviewed on 2025-05-15 21:02_

---

_Review comment by @AlexWaygood on `.github/workflows/daily_property_tests.yml`:9 on 2025-05-15 21:02_

it took me a while to figure out that this was why the new workflow wasn't running on this PR 😆

---

_Comment by @danielhollas on 2025-05-15 23:03_

~~Sorry for a drive-by comment but isn't it problematic that the tests will run on an "old" ty version, ie. not from the tip of the ruff main branch?~~

EDIT: Sorry, I am blind, don't mind me.

---

_@AlexWaygood approved on 2025-05-16 00:54_

Thank you!!

---

_Merged by @AlexWaygood on 2025-05-16 00:56_

---

_Closed by @AlexWaygood on 2025-05-16 00:56_

---

_Branch deleted on 2025-05-16 07:58_

---
