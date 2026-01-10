```yaml
number: 20038
title: "Similar flag as `pylint --fail-under <score>`?"
type: issue
state: closed
author: soderluk
labels:
  - question
assignees: []
created_at: 2025-08-22T06:42:17Z
updated_at: 2025-09-15T13:11:36Z
url: https://github.com/astral-sh/ruff/issues/20038
synced_at: 2026-01-10T11:09:59Z
```

# Similar flag as `pylint --fail-under <score>`?

---

_Issue opened by @soderluk on 2025-08-22 06:42_

### Question

Are there any plans on implementing a similar flag for `ruff check` like `pylint --fail-under <score>`? E.g. let the `ruff check` pre-commit hook pass if the score is over 90%. 
There are cases where we need to make changes to an old code base, and we can't fix all the linter issues, or add a `# noqa: {}` on all the failing issues. Having a threshold that could still pass the linting would be a great addition.
Or is there a way to simulate this behavior with Ruff?

### Version

ruff 0.12.9

---

_Label `question` added by @soderluk on 2025-08-22 06:42_

---

_Comment by @ntBre on 2025-08-22 18:39_

Ah, that's interesting! I think the way we're likely to handle this is either with baselines (#1149) or a warning severity (#1256, not sure that would help here exactly).

The closest thing we have right now is probably the GitLab output format, which has a `fingerprint` for each diagnostic, intended for matching up diagnostics over time for GitLab's [Code Quality](https://docs.gitlab.com/ci/testing/code_quality/#code-quality-report-format) reports. You might be able to hack something together with that.

---

_Comment by @soderluk on 2025-08-23 10:22_

@ntBre the gitlab output format looks like it could be hacked into something usable. One question though, ruff check --output-format marks _all_ issues with severity `major`. Can this be configured somehow with pyproject? 

---

_Comment by @ntBre on 2025-08-25 14:52_

Ah no, I think that's hard-coded at the moment:

https://github.com/astral-sh/ruff/blob/a50b389d626528d55c22979dac50c48387a5e1e0/crates/ruff_linter/src/message/gitlab.rs#L99

We were planning to update that once we had real severity levels for our diagnostics in general.

---

_Comment by @MichaReiser on 2025-09-15 13:11_

I'll close this as I think the question is answered and the 90% request is tracked as part of the baselines feature

---

_Closed by @MichaReiser on 2025-09-15 13:11_

---
