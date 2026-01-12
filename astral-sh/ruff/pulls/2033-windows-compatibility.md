```yaml
number: 2033
title: Windows compatibility
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: ci-windows
created_at: 2023-01-20T16:08:20Z
updated_at: 2023-01-25T23:01:53Z
url: https://github.com/astral-sh/ruff/pull/2033
synced_at: 2026-01-12T15:55:07Z
```

# Windows compatibility

---

_@sbrugman_

CI testing for Windows to prevent windows-specific issues in the future (https://github.com/charliermarsh/ruff/issues/2026,  https://github.com/charliermarsh/ruff/issues/1915, https://github.com/charliermarsh/ruff/issues/832)



---

_Comment by @charliermarsh on 2023-01-20 17:58_

I feel like I'm missing something hah - how does `cargo test` pass on Windows if there are failing tests?

---

_Comment by @sbrugman on 2023-01-20 18:10_

Probably something with powershell not passing the exit code to github actions (https://github.com/actions/runner/issues/351). Will look into it on my Windows machine later.

e.g. in this job https://github.com/charliermarsh/ruff/actions/runs/3969891762/jobs/6805182473:
```
failures:
[1018](https://github.com/charliermarsh/ruff/actions/runs/3969891762/jobs/6805182473#step:7:1019)
    source_code::generator::tests::indent
[1019](https://github.com/charliermarsh/ruff/actions/runs/3969891762/jobs/6805182473#step:7:1020)
    source_code::generator::tests::set_indent
```

---

_Comment by @messense on 2023-01-21 01:35_

Maybe just add `shell: bash` to that `run` step.

---

_Converted to draft by @sbrugman on 2023-01-22 00:22_

---

_@charliermarsh reviewed on 2023-01-23 18:03_

---

_Review comment by @charliermarsh on `src/assert_yaml_snapshot.rs`:6 on 2023-01-23 18:03_

Probably the only possible way to make this work but also could be a source of false negatives, maybe? I dunno, it's kind of sending my brain in circles. Since we do try to do things like "Even if on Windows, if the file seems to use CL newlines, then insert CL newlines".

(I think Insta does something like this normalization for the snapshot files themselves... but the errors we're running into are in the stringified fixes _within_ the snapshots, right? Just making sure I understand.)

---

_Comment by @charliermarsh on 2023-01-23 18:04_

(In the interest of clear communication: I'd love to get this working!)

---

_Comment by @sbrugman on 2023-01-23 18:30_

@charliermarsh summary of the findings so far.

Tests are failing on Windows for:
- hardcoded line separators in the fix content in the snap files
- paths that are rendered to string differently per platform
- bugs that only occur on windows (false positives and false negatives)

The last category is impacting users (and has been solved).

`insta` does normalize line endings in the `yaml` files, however this does not apply to line endings within strings (e.g. fix content).
The simplest fix at this moment is to use the `filter` method that insta provides to normalize `\r\n` when writing. This impairs our ability to distinguish files where the CRLFs are intentional. I'll try to solve that with https://insta.rs/docs/redactions/. I'd like to prevent needing a file per platform.

When I'm done I'll rewrite the commit history and mark the PR as ready

---

_@sbrugman reviewed on 2023-01-23 18:31_

---

_Review comment by @sbrugman on `src/assert_yaml_snapshot.rs`:6 on 2023-01-23 18:31_

See comment below

---

_Renamed from "ci: enable windows testing" to "Windows compatibility" by @sbrugman on 2023-01-24 00:48_

---

_Marked ready for review by @sbrugman on 2023-01-24 10:02_

---

_Comment by @sbrugman on 2023-01-24 10:15_

@charliermarsh Many files changed, but the actual changes are manageable (especially if you look per commit)

---

_Comment by @charliermarsh on 2023-01-24 13:09_

@sbrugman - Thank you! Heroic work. I'll try to review today.

---

_Comment by @sbrugman on 2023-01-25 00:04_

@charliermarsh could we try to merge the windows compatibility before any more refactors like https://github.com/charliermarsh/ruff/pull/2138? Otherwise it's hitting a moving target

Current errors on windows are caused by the new EXE features

---

_Comment by @charliermarsh on 2023-01-25 00:32_

Yeah will do. Don't want you to have to keep rebasing here. I'll review this tonight, and hopefully we can merge tomorrow or whenever's convenient.

---

_Comment by @charliermarsh on 2023-01-25 00:42_

> Current errors on windows are caused by the new EXE features.

(Are those real failures?)

---

_Review comment by @andersk on `src/assert_yaml_snapshot.rs`:12 on 2023-01-25 00:45_

For more readable output, we could _split_ on newlines instead, yielding a list.

```suggestion
            insta::internals::Content::Seq(
                value.as_str().unwrap().split(line_sep).map(|line| line.into()).collect()
            )
```

---

_@andersk reviewed on 2023-01-25 00:45_

---

_@sbrugman reviewed on 2023-01-25 00:50_

---

_Review comment by @sbrugman on `src/assert_yaml_snapshot.rs`:12 on 2023-01-25 00:50_

Much better!

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:73 on 2023-01-25 01:41_

Should these be removed?

---

_Review comment by @charliermarsh on `ruff_cli/tests/integration_test.rs`:56 on 2023-01-25 01:42_

For posterity: why does this fail on Windows? Can we add a comment?

---

_Comment by @charliermarsh on 2023-01-25 01:42_

(I'm reviewing this now.)

---

_Review comment by @charliermarsh on `src/rules/flake8_comprehensions/snapshots/ruff__rules__flake8_comprehensions__tests__C400_C400.py.snap`:35 on 2023-01-25 01:47_

This is a clever solution.

---

_Review comment by @charliermarsh on `src/rules/isort/rules/add_required_imports.rs`:101 on 2023-01-25 01:53_

Can we move this down to be just after `Locator`?

---

_Review comment by @charliermarsh on `src/rules/isort/rules/add_required_imports.rs`:164 on 2023-01-25 01:53_

Same here. I'd like to group `Stylist` and `Locator` wherever we can.

---

_Review comment by @charliermarsh on `src/source_code/generator.rs`:1297 on 2023-01-25 01:55_

Let's wrap this in a helper method? So its responsibilities are clear (via the method name) and the logic is self-contained.

---

_Review comment by @charliermarsh on `src/assert_yaml_snapshot.rs`:11 on 2023-01-25 02:28_

What would happen if we used `.lines()` here instead? Apart from losing the trailing newline / empty line.

---

_Review comment by @charliermarsh on `src/rules/isort/mod.rs`:745 on 2023-01-25 02:29_

Nit: the code (`#` onwards) should be on the next line, I think.

---

_Review comment by @charliermarsh on `src/rules/isort/mod.rs`:761 on 2023-01-25 02:30_

Any idea how we could re-enable this? What would it take? (This did catch an error once.)

---

_Review comment by @charliermarsh on `src/assert_yaml_snapshot.rs`:11 on 2023-01-25 02:32_

Dumb question: is this redaction applied to both the snapshot file (loaded from disk) and the generated value? 

---

_@charliermarsh reviewed on 2023-01-25 02:32_

---

_Comment by @charliermarsh on 2023-01-25 02:33_

Looks great! Couple questions in the review. I have no doubt that this will catch a ton of issues.

---

_Comment by @charliermarsh on 2023-01-25 04:52_

(I'll prioritize merging this as soon as the review is addressed, and try to avoid merging any conflicting changes in the interim!)

---

_@sbrugman reviewed on 2023-01-25 09:04_

---

_Review comment by @sbrugman on `.github/workflows/ci.yaml`:73 on 2023-01-25 09:04_

Yes. I left it intentionally before to emphasise that git is automatically changing the line endings which, together with the line ending detection, is causing the isort test to fail (but without it many other tests do not work).

---

_@sbrugman reviewed on 2023-01-25 09:07_

---

_Review comment by @sbrugman on `src/rules/isort/rules/add_required_imports.rs`:101 on 2023-01-25 09:07_

✅ 

---

_@sbrugman reviewed on 2023-01-25 09:12_

---

_Review comment by @sbrugman on `src/assert_yaml_snapshot.rs`:11 on 2023-01-25 09:12_

To start with the second question: the test output is 'redacted' before being compared with the static snapshot file. The snapshot file does not change, so should be platform independent (or in some cases we need different tests when the behaviours are different per platform, such as with EXE). 

`.lines()` seems to split on both `\r\n` and `\n`. This would mean that if a rule outputs the wrong line ending, say `\r\n` on Ubuntu, then `.lines()` would recognize that as line ending and test would pass. With the current setup, the `\r` character is different between the test output and the snapshot file and the test would fail correctly.

---

_@sbrugman reviewed on 2023-01-25 09:16_

---

_Review comment by @sbrugman on `src/rules/isort/mod.rs`:761 on 2023-01-25 09:16_

The test does pass locally. The issue is that when we checkout the repository, git reformats the line endings of the test files. This is expected and required for most tests, but in the case of `line_endings_crlf.py` and `line_endings_lf.py`, git should not touch them.

A workaround would be to not store these two files in a python file but as a testing string in the file.


---

_Comment by @charliermarsh on 2023-01-25 16:47_

@sbrugman - What's the current state of this PR? Is it mergeable? I notice that you've been tweaking the EXE tests a bit, but it seems that the changes are now causing different CI failures.

---

_Comment by @sbrugman on 2023-01-25 17:16_

@charliermarsh Something is going wrong with selecting the right snap files per platform, I believe. 
I don't have the time to work on it today. Perhaps we can ignore these tests on Windows for now (with `cfg`), then merge and then review it later

---

_Comment by @charliermarsh on 2023-01-25 17:17_

Yeah that's fine. Do you mind if I debug a bit, push to this PR, then squash and merge?

---

_Comment by @sbrugman on 2023-01-25 17:40_

> Yeah that's fine. Do you mind if I debug a bit, push to this PR, then squash and merge?

Perfect 🙏 ! Thank you 
(perhaps good to keep the mechanical changes somewhat in separate commits from the rest)

---

_Comment by @charliermarsh on 2023-01-25 17:56_

Okay thanks! I'll try to get this merged today then. And I'll look to preserve a few separate commits, but I may squash some of the intermediary commits.

---

_Comment by @charliermarsh on 2023-01-25 20:37_

@sbrugman - I was thinking of squashing this down into a few commits, by squashing all the "core" and "fix" commits, so that we have that first commit, then all the "refactor" commits. Is that ok?

---

_Comment by @sbrugman on 2023-01-25 21:21_

> @sbrugman - I was thinking of squashing this down into a few commits, by squashing all the "core" and "fix" commits, so that we have that first commit, then all the "refactor" commits. Is that ok?

Sounds good! 

---

_Comment by @charliermarsh on 2023-01-25 22:19_

(I'll merge this tonight, I'm just waiting for the v0.0.234 release to publish.)

---

_Comment by @sbrugman on 2023-01-25 22:19_

@charliermarsh I could rebase and squash now if there are no more tweaks from your side (got 30 mins or so)
(Your time and consideration are much appreciated)

---

_Comment by @charliermarsh on 2023-01-25 22:23_

Go for it!

---

_Comment by @charliermarsh on 2023-01-25 22:24_

After this merges, I'll look into re-enabling those disabled tests.

---

_Comment by @sbrugman on 2023-01-25 22:55_

@charliermarsh These 6 commits seem meaningfully distinct to me. The prefix `feat` is typically the only one that shows up in the changelog. The fixes grouped by issue may be helpful as reference if other contributors run into issues with the windows tests.

---

_Merged by @charliermarsh on 2023-01-25 23:00_

---

_Closed by @charliermarsh on 2023-01-25 23:00_

---

_Comment by @charliermarsh on 2023-01-25 23:00_

Awesome. Thank you @sbrugman! This is great for Ruff, I know it was a lot of work.

---

_Branch deleted on 2023-01-25 23:01_

---
