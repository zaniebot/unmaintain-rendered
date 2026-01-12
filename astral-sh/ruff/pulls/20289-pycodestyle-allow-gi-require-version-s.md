```yaml
number: 20289
title: "[pycodestyle] Allow gi.require_version[s]() interspersed with imports, for PyGObject (E402) (fixes #20288)"
type: pull_request
state: closed
author: ferdnyc
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: gi-version-exception
created_at: 2025-09-07T22:05:27Z
updated_at: 2025-09-16T20:50:46Z
url: https://github.com/astral-sh/ruff/pull/20289
synced_at: 2026-01-12T15:56:58Z
```

# [pycodestyle] Allow gi.require_version[s]() interspersed with imports, for PyGObject (E402) (fixes #20288)

---

_@ferdnyc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->


<!-- What's the purpose of the change? What does it do, and why? -->

## Summary

Exempt `gi.require_version()` and `gi.require_versions()` calls from triggering E402 if they appear interspersed with imports, because they are necessary version-selection calls required before importing from `gi.repository`, when working with PyGObject / GIRepository code.

This places `gi.require_version[s]()` calls in the same category as:

* `sys.path` modifications
* `os.environ` modifications
* `site.addsitedir()` calls

And, most similar in both nature and niche-ness:

* `pytest.importorskip()` calls
* `matplotlib.use()` calls
 
All of which are exempted from E402 in Ruff.

Fixes #20288 

## Test Plan

<!-- How was it tested? -->

The PR contains a new test fixture `crates/ruff_linter/resources/test/fixtures/pycodestyle/E402_6.py`.

The sample code contained within triggers E402 without the changes in this PR.

With the PR changes in place, the corresponding test case (added to `crates/ruff_linter/src/rules/pycodestyle/mod.rs`) and `.snap` file result in a passing test.


---

_Comment by @ferdnyc on 2025-09-07 22:11_

### Notes

1. This PR doesn't currently limit the change to the `preview` feature, although I notice many PRs do. There wasn't any guidance in the contributing guide about that, so I decided I'd wait for guidance.
2. The PR also doesn't add any mention of the new exemption to the documentation. I noticed that the docs mention the "major" exemptions like `sys.path` and `os.environ`, but don't go into detail on the more focused ones like `pytest.importorskip()` and `matplotlib.use()`, so I followed suit.

---

_Renamed from "[pycodestyle] Allow gi.require_version[s]() interspersed with imports, for PyGObject (#20288)" to "[pycodestyle] Allow gi.require_version[s]() interspersed with imports, for PyGObject (E402) (fixes #20288)" by @ferdnyc on 2025-09-07 22:12_

---

_Label `rule` added by @ntBre on 2025-09-08 19:46_

---

_Label `needs-decision` added by @ntBre on 2025-09-08 19:46_

---

_Comment by @MichaReiser on 2025-09-16 11:46_

Thank you for your contribution.

As discussed on the issue. I don't think we want to keep adding more library-specific exceptions. Mainly because it is very slippery terrain, where each exception justifies adding the next exception. 

Let's first decide on a path forward on the issue.

---

_Closed by @MichaReiser on 2025-09-16 11:46_

---

_Branch deleted on 2025-09-16 20:50_

---
