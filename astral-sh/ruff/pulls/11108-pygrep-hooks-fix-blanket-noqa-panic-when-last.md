```yaml
number: 11108
title: "[`pygrep_hooks`] Fix `blanket-noqa` panic when last line has noqa with no newline (`PGH004`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
assignees: []
merged: true
base: main
head: pgh004-bug
created_at: 2024-04-23T17:05:00Z
updated_at: 2024-04-25T19:41:27Z
url: https://github.com/astral-sh/ruff/pull/11108
synced_at: 2026-01-10T22:37:01Z
```

# [`pygrep_hooks`] Fix `blanket-noqa` panic when last line has noqa with no newline (`PGH004`)

---

_Pull request opened by @augustelalande on 2024-04-23 17:05_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #11102

The error stems from these lines
https://github.com/astral-sh/ruff/blob/f5c7a62aa65decb9e286bd65ba17f1a3bd1f91e6/crates/ruff_linter/src/noqa.rs#L697-L702
I don't really understand the purpose of incrementing the last index, but it makes the resulting range invalid for indexing into `contents`.

For now I just detect if the index is too high in `blanket_noqa` and adjust it if necessary.

## Test Plan

Created fixture from issue example.


---

_Comment by @github-actions[bot] on 2024-04-23 17:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`pygreb_hooks`] Fix `blanket-noqa` panic when last line has noqa with no newline (`PGH004`)" to "[`pygrep_hooks`] Fix `blanket-noqa` panic when last line has noqa with no newline (`PGH004`)" by @AlexWaygood on 2024-04-23 18:03_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:91 on 2024-04-23 19:08_

This seems like a bug, if the `.end()` is greater than the contents... Have you looked into how that happens?

---

_@charliermarsh reviewed on 2024-04-23 19:08_

---

_@charliermarsh reviewed on 2024-04-23 19:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:91 on 2024-04-23 19:09_

I see the PR summary, but what happens if we remove it?

---

_@augustelalande reviewed on 2024-04-23 19:44_

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:91 on 2024-04-23 19:44_

Ya it generates a fixture failure with W292_1.py

---

_Review comment by @augustelalande on `crates/ruff_linter/src/rules/pygrep_hooks/rules/blanket_noqa.rs`:91 on 2024-04-23 19:45_

It seems the code mentioned in the PR summary was added by @MichaReiser so maybe he can shed some light.

---

_@augustelalande reviewed on 2024-04-23 19:45_

---

_Comment by @MichaReiser on 2024-04-24 07:10_

Uff, I wrote this code like a year ago. But what I did is uncomment that specific code in the `noqa` file and run all tests and the following test now fails

https://github.com/astral-sh/ruff/blob/5849a752235f5384bfef733e64dbfc6a01e30be9/crates/ruff_linter/resources/test/fixtures/pycodestyle/W292_1.py#L1-L2


The problem is that we have fixes reported at the very end of the file, and there must be a way to suppress them. 

The way the noqa lines are found is by searching by a given offset:

https://github.com/astral-sh/ruff/blob/de46a36bbcdc1df4a56fc486ae5c7bcf2c4bbba0/crates/ruff_linter/src/noqa.rs#L722-L734

If we have two lines, than the `previous_line.end() == next_line.start()`. That's why the above methods searches the line using `directive.end() < offset` to avoid that a noqa comment suppresses a violation on the next lines.

I must admit, my "solution" is a hack and it would be nice if we could support end-of-file noqa comments in a better way.

---

_Comment by @augustelalande on 2024-04-24 18:08_

@MichaReiser @charliermarsh I implemented a fix which addresses the issue at its source. Let me know if you like it better.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:707 on 2024-04-25 07:11_

Nit: I think I would rename this to `includes_end` to make it clear that the comparison should include the end range.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:746 on 2024-04-25 07:20_

The search then becomes

```
self inner.binary_search_by(|directive| {
	if directive.range.end() < offset {
		std::cmp::Ordering::Less
	} else if directive.range.start() >  offset {
		std::cmp::Ordering::Greater
	} 
	// At this point, end >= offset, start <= offset
	else if !directive.includes_end && directive.range.end() == offset  {
		std::cmp::Ordering::Less // I think or should it be greater?
	} else {
		std::cmp::Ordering::Equal
	}

---

_@MichaReiser reviewed on 2024-04-25 07:20_

---

_Comment by @augustelalande on 2024-04-25 18:21_

 Done. I don't know what that wasm test failure is about.

---

_Comment by @charliermarsh on 2024-04-25 18:24_

I think it's spurious, I'll re-run it.

---

_Merged by @charliermarsh on 2024-04-25 18:39_

---

_Closed by @charliermarsh on 2024-04-25 18:39_

---

_Label `bug` added by @charliermarsh on 2024-04-25 18:39_

---

_Branch deleted on 2024-04-25 19:41_

---
