```yaml
number: 14371
title: "[`flake8-type-checking`] Consider type expressions in list for quoting annotations"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: quote-annots
created_at: 2024-11-15T22:11:08Z
updated_at: 2024-11-18T06:57:53Z
url: https://github.com/astral-sh/ruff/pull/14371
synced_at: 2026-01-10T20:50:57Z
```

# [`flake8-type-checking`] Consider type expressions in list for quoting annotations

---

_Pull request opened by @dylwil3 on 2024-11-15 22:11_

This PR adds corrected handling of  list expressions to the `Visitor` implementation of `QuotedAnnotator` in `flake8_type_checking::helpers`.

Closes #14368


---

_Comment by @dylwil3 on 2024-11-15 22:12_

Notice that the snapshot I updated was for a test that already existed - the fixture contains almost exactly the example from the linked issue. It seems to me like the previous behavior was incorrect and the new behavior is correct - but maybe I'm missing something?

---

_Comment by @github-actions[bot] on 2024-11-15 22:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/python-trio/trio/blob/785dc59309ca4f491562b817836092f54b7da4b1/src/trio/testing/_memory_streams.py#L438'>src/trio/testing/_memory_streams.py:438:1:</a> W293 Blank line contains whitespace
+ <a href='https://github.com/python-trio/trio/blob/785dc59309ca4f491562b817836092f54b7da4b1/src/trio/testing/_memory_streams.py#L438'>src/trio/testing/_memory_streams.py:438:2:</a> W292 No newline at end of file
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| W293 | 1 | 1 | 0 | 0 | 0 |
| W292 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @dylwil3 on 2024-11-16 01:41_

---

_Label `bug` added by @MichaReiser on 2024-11-16 16:57_

---

_@MichaReiser approved on 2024-11-16 17:00_

This looks correct to me. I don't know anything about `helpers.rs` but you're doing the same as is done in other places, so it must be correct 😆 

---

_Merged by @charliermarsh on 2024-11-16 17:58_

---

_Closed by @charliermarsh on 2024-11-16 17:58_

---

_Renamed from "[`flake8-type-checking`] Fix helper function which surrounds annotations in quotes" to "[`flake8-type-checking`] Consider type expressions in list for quoting annotations" by @dhruvmanila on 2024-11-18 06:57_

---
