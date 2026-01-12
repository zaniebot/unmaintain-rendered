```yaml
number: 17150
title: "[red-knot] Add GitHub PR annotations when mdtests fail in CI"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/mdtest-github-output-2
created_at: 2025-04-02T12:33:44Z
updated_at: 2025-04-02T20:51:54Z
url: https://github.com/astral-sh/ruff/pull/17150
synced_at: 2026-01-12T15:56:00Z
```

# [red-knot] Add GitHub PR annotations when mdtests fail in CI

---

_@AlexWaygood_

## Summary

This PR adds a CI job that causes GitHub to add annotations to a PR diff when mdtest assertions fail. For example:

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/bb2a649b-46ab-429d-a576-b36545940eaf)

</details>

## Motivation

Debugging mdtest failures locally is currently a really nice experience:
- Errors are displayed with pretty colours, which makes them much more readable
- If you run the test from inside an IDE, you can CTRL-click on a path and jump directly to the line that had the failing assertion
- If you use [`mdtest.py`](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/mdtest.py), you don't even need to recompile anything after changing an assertion in an mdtest, amd the test results instantly live-update with each change to the MarkDown file

Debugging mdtest failures in CI is much more unpleasant, however. Sometimes an error message is just

> [static-assert-error] Argument evaluates to `False`

...which doesn't tell you very much unless you navigate to the line in question that has the failing mdtest assertion. The line in question might not even be touched by the PR, and even if it is, it can be hard to find the line if the PR touches many files. Unlike locally, you can't click on the error and jump straight to the line that contains the failing assertion. You also don't get colourised output in CI (https://github.com/astral-sh/ty/issues/235).

GitHub PR annotations should make it really easy to debug why mdtests are failing on PRs, making PR review much easier.

## How it works

A new `mdtest_github_output_format` feature is added to the `red_knot_python_semantic` crate. When the feature is enabled, mdtest failures are printed to the terminal using [a format that causes GitHub to attach annotations to the PR diff](https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/workflow-commands-for-github-actions#setting-an-error-message). If the feature is not enabled, mdtest failures are printed using the same format that they were before, since that's what is best for local development.

A new CI job is added to `ci.yaml` that runs only red-knot's mdtests with this feature flag enabled. The job takes around 1m40s to run to completion on GitHub's `ubuntu-latest` runner. The job only runs on PRs (not pushes to `main` or on `workflow_dispatch` events), and only runs if red-knot-related code changes as part of the PR.

## Test Plan

I opened a PR to my fork [here](https://github.com/AlexWaygood/ruff/pull/11/files) with some bogus changes to an mdtest to show what it looks like when there are failures in CI and this job has been added. Scroll down to `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md` on the "files changed" tab for that PR to see the annotations.

---

_Label `ci` added by @AlexWaygood on 2025-04-02 12:33_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-02 12:33_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-02 12:33_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-02 12:33_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-02 12:33_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-02 12:33_

---

_Comment by @github-actions[bot] on 2025-04-02 12:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-04-02 12:42_

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

_@MichaReiser reviewed on 2025-04-02 12:48_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/Cargo.toml`:67 on 2025-04-02 12:48_

I don't think we should use a feature for this. It seems off. I'd rather just use an environment variable for it.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:80 on 2025-04-02 12:49_

I'd prefer using `Concise` or `Full` or `Cli`. It isn't a format that's consumed by cargo. It's the CLI output

---

_@AlexWaygood reviewed on 2025-04-02 12:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/Cargo.toml`:67 on 2025-04-02 12:50_

That's fine by me! Wasn't sure what would be best here

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:436 on 2025-04-02 12:51_

Is this required to be its own job or could we run this as part of the `linux` test run? I'm worried about extra CI time for the rare cases where an mdtest fails in CI

---

_@MichaReiser reviewed on 2025-04-02 12:51_

This is cool. 

I don't think this should be a cargo feature, I'd use an environment variable instead. It would also be great if we can avoid that it has to run as its own job

---

_@AlexWaygood reviewed on 2025-04-02 12:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:80 on 2025-04-02 12:53_

Yeah, I also considered `Default`. `Cli` works for me!

---

_@AlexWaygood reviewed on 2025-04-02 13:04_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:436 on 2025-04-02 13:04_

I think there's a bunch of things that we'd ideally like, some of which are in competition with each other. The ones I'm already doing on this PR I've marked with ✅; the ones I'm not are marked with ❌:
- ✅ Only run it on PRs, not pushes to `main`
- ✅ Only run it if there are red-knot-related changes in the PR
- ✅ Have the job finish quickly so that annotations show up on the PR diff within a minute or two
- ✅ Have the output of this job clearly separated from the main `cargo test` output in the "checks" list at the bottom of a PR. The GitHub output format is [quite hard to read](https://github.com/AlexWaygood/ruff/actions/runs/14219352254/job/39843390348?pr=11) if you try to just read what's printed directly to the terminal.
  - ➡ This implies that the "annotations" test run should be its own separate job, rather than an additional step in the `linux` test run. Just because its a separate job doesn't mean that it can't reuse the Rust build from the `linux` job, though.
- ❌ Only run the job if the Linux _build succeeds_ (there's no point starting the job if the PR has a compile error)
  - ➡ I think this is possible to accomplish but it might complicate the workflow a bit
- ❌ Reuse the build from the `Linux` CI job.
  - ➡ Should be possible, but depends on the previous item
- ❌ Only run it if the Linux _tests fail_ (if the tests all pass, there's no need to start the separate job to get the annotations).
  - ➡ Doing this would imply we'd have to wait for a full test run to either fail or succeed before starting the mdtest run that adds the annotations. That's going to mean that it will be another 2-3 minutes before the annotations will show up on the PR diff in CI. I think I'd prefer not to do it.

---

_@MichaReiser reviewed on 2025-04-02 13:22_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:436 on 2025-04-02 13:22_

The part I don't understand is why it has to be its own job in the first place

---

_@MichaReiser reviewed on 2025-04-02 13:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2025-04-02 13:24_

Nit: Can we use something like `MDTEST_OUTPUT_FORMAT` or `MDTEST_GITHUB_OUTPUT_FORMAT` so that it's clear where the environment variable is consumed (without having to search)

---

_@MichaReiser reviewed on 2025-04-02 13:24_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:436 on 2025-04-02 13:24_

But I guess my concerns are addressed if we avoid rebuilding the binary and instead reuse it from the linux test run step

---

_@AlexWaygood reviewed on 2025-04-02 13:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/tests/mdtest.rs`:22 on 2025-04-02 13:24_

sure

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/lib.rs`:124 on 2025-04-02 13:24_

```suggestion
    const fn is_cli(self) -> bool {
```

---

_@MichaReiser reviewed on 2025-04-02 13:24_

---

_@AlexWaygood reviewed on 2025-04-02 13:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/lib.rs`:124 on 2025-04-02 13:25_

hmm, maybe I should just derive `is_macro::Is` ;)

---

_@AlexWaygood reviewed on 2025-04-02 13:27_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:436 on 2025-04-02 13:27_

i answered that question in this bullet of my message above:

> ✅ Have the output of this job clearly separated from the main `cargo test` output in the "checks" list at the bottom of a PR

also note that, in addition to the rationale I gave there, I'm not totally sure how to make it so that later steps in the same job are run even if earlier jobs fail. In [this workflow run](https://github.com/astral-sh/ruff/actions/runs/14220037782/job/39845581911?pr=17149), for example, we never get to the `cargo doc --all --no-deps` step. The workflow aborts as soon as `cargo test` fails; later steps in that job are cancelled.

---

_@MichaReiser reviewed on 2025-04-02 13:58_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:436 on 2025-04-02 13:58_

I think the easiest you could do is to not make this a dedicated job but a step that's part of the main test job. You could even go as far as grepping to see if any mdtest failed and only then run this additional step

---

_Converted to draft by @AlexWaygood on 2025-04-02 15:25_

---

_Marked ready for review by @AlexWaygood on 2025-04-02 17:01_

---

_Comment by @AlexWaygood on 2025-04-02 17:04_

https://github.com/astral-sh/ruff/pull/17150/commits/494ef81cd8b2edf9626f4b3f69fd759580c92c1e is my attempt at caching the build and then using it in both the `cargo test` job and the new `GitHub Annotations` job. From what I can tell, it seems to slow CI down quite a bit. Uploading and downloading the contents of `target/debug/deps` seems to take a lot longer than just building the same thing in parallel in two jobs simultaneously. Unless there's a way of limiting the files I'm uploading more?

I'd be inclined to revert https://github.com/astral-sh/ruff/pull/17150/commits/494ef81cd8b2edf9626f4b3f69fd759580c92c1e and have it how it was before that commit. There is the additional advantage that the workflow file was simpler prior to that commit. But I don't feel strongly.

As discussed in [this comment thread](https://github.com/astral-sh/ruff/pull/17150#discussion_r2024763928), I do think the new job has to be a separate job to the regular `cargo test` job. If an earlier step in a workflow job fails, you never get to the later step in the workflow job. That means that if any mdtests failed, GitHub would never run the later step that ran with the new GitHub Annotations format -- defeating the purpose of adding the new format altogether.

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-04-02 17:05_

---

_Comment by @AlexWaygood on 2025-04-02 17:23_

experimenting with another idea from @MichaReiser to see if we can reduce the number of times we do a Rust build in CI...

---

_Comment by @AlexWaygood on 2025-04-02 17:27_

Okay great, annotations are still working on the latest version of the PR!

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/e827ce71-ce38-4df2-8c3e-bd8c8c628369)

</details>

---

_Comment by @AlexWaygood on 2025-04-02 17:30_

The PR is again ready for re-review :-)

---

_@MichaReiser approved on 2025-04-02 19:13_

Thanks, and sorry for the back and forth. 

---

_Comment by @AlexWaygood on 2025-04-02 20:51_

No worries, and thank you for the review. It's much better for your suggestions!

---

_Merged by @AlexWaygood on 2025-04-02 20:51_

---

_Closed by @AlexWaygood on 2025-04-02 20:51_

---

_Branch deleted on 2025-04-02 20:51_

---
