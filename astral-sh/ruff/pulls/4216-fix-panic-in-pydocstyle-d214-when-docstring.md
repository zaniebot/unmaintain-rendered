```yaml
number: 4216
title: Fix panic in pydocstyle D214 when docstring indentation is empty
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: bug/4194
created_at: 2023-05-04T05:41:22Z
updated_at: 2023-05-04T18:42:40Z
url: https://github.com/astral-sh/ruff/pull/4216
synced_at: 2026-01-12T04:28:19Z
```

# Fix panic in pydocstyle D214 when docstring indentation is empty

---

_Pull request opened by @zanieb on 2023-05-04 05:41_

Closes https://github.com/charliermarsh/ruff/issues/4194


---

_Comment by @zanieb on 2023-05-04 05:44_

Fix confirmed with regression test

```
$ git checkout main
$ git cherry-pick 76ff675
$ cargo test
...
thread 'rules::pydocstyle::tests::rules::d214_module' panicked at 'Prefer `Fix::deletion`', crates/ruff_diagnostics/src/edit.rs:41:9
```

---

_@zanieb reviewed on 2023-05-04 05:47_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pydocstyle/mod.rs`:71 on 2023-05-04 05:47_

What's up with the naming nomenclature for these files/tests? It's kind of chaotic and I did not see clear guidelines in the contributing docs.

---

_Comment by @github-actions[bot] on 2023-05-04 05:52_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:631 on 2023-05-04 07:21_

Nit, we can use the same range for both fixes
```suggestion
                let content = whitespace::clean(docstring.indentation);
                let fix_range = TextRange::at(context.range().start(), leading_space.text_len());
                
                diagnostic.set_fix(if content.is_empty() {
                	Edit::range_deletion(fix_range)
                } else {
	                Edit::range_replacement(content, fix_range)
                });
```

---

_@MichaReiser approved on 2023-05-04 07:21_

---

_@zanieb reviewed on 2023-05-04 15:25_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pydocstyle/rules/sections.rs`:631 on 2023-05-04 15:25_

Ah `range_deletion` makes sense here! Thanks :)

---

_@charliermarsh reviewed on 2023-05-04 15:32_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/mod.rs`:71 on 2023-05-04 15:32_

It's mostly path dependence. We started with _just_ `D.py`, then added `sections.py` at some point when we needed some section-specific tests, and then we added a few cases that are specific to individual rules (like `D104/__init__.py`, which requires multiple files to test). Maybe we just rename this file to `module.py` to indicate that it's a test for module-level docstring behavior?

---

_@zanieb reviewed on 2023-05-04 15:37_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pydocstyle/mod.rs`:71 on 2023-05-04 15:37_

🤷‍♀️ I don't have a strong preference. I named it this because it only contains D214 violations — maybe if we add more module-level docstring errors in it in the future it'd be renamed?

Does the string argument after the path need to be unique?

---

_@charliermarsh reviewed on 2023-05-04 15:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pydocstyle/mod.rs`:71 on 2023-05-04 15:42_

Yeah, IIRC the string argument has to be unique.

---

_Merged by @charliermarsh on 2023-05-04 18:42_

---

_Closed by @charliermarsh on 2023-05-04 18:42_

---

_Label `bug` added by @charliermarsh on 2023-05-04 18:42_

---
