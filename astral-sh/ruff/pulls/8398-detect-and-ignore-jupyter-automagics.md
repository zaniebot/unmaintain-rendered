```yaml
number: 8398
title: Detect and ignore Jupyter automagics
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/magics
created_at: 2023-10-31T23:25:08Z
updated_at: 2023-11-03T01:21:56Z
url: https://github.com/astral-sh/ruff/pull/8398
synced_at: 2026-01-12T15:55:26Z
```

# Detect and ignore Jupyter automagics

---

_@charliermarsh_

## Summary

LangChain is attempting to use Ruff over their Jupyter notebooks (https://github.com/langchain-ai/langchain/pull/12677/files), but running into a bunch of syntax errors, the majority of which come from our inability to recognize automagic.

If you run this in a cell:

```jupyter
pip install requests
```

Jupyter will automatically treat that as:

```jupyter
%pip install requests
```

We need to ignore cells that use these automagics, since the parser doesn't understand them. (I guess we could support it in the parser, but that seems much harder?). The good news is that AFAICT Jupyter doesn't let you mix automagics with code, so by skipping these cells, we don't miss out on analyzing any Python code.

## Test Plan

1. `cargo test`
2. Ran over LangChain and verified that there are no more errors relating to `pip install` automagics.


---

_Review requested from @dhruvmanila by @charliermarsh on 2023-10-31 23:25_

---

_@charliermarsh reviewed on 2023-10-31 23:25_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:196 on 2023-10-31 23:25_

Obviously some risk of this getting stale but I kind of doubt these change dramatically over time.

---

_Label `cli` added by @MichaReiser on 2023-10-31 23:30_

---

_Label `cli` removed by @MichaReiser on 2023-10-31 23:30_

---

_Label `bug` added by @charliermarsh on 2023-10-31 23:30_

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/notebook.rs`:93 on 2023-10-31 23:31_

Nit: Trim python whitespace only?

---

_Review comment by @MichaReiser on `crates/ruff_notebook/src/notebook.rs`:117 on 2023-10-31 23:31_

Nit: Split on whitspace only

---

_@MichaReiser reviewed on 2023-10-31 23:32_

LGTM, leaving it to @dhruvmanila, the IPython expert to approve.

---

_Comment by @github-actions[bot] on 2023-10-31 23:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:93 on 2023-11-01 03:23_

The `trim_start` is required because the magics can be at any indentation level but it seems like the logic is different for auto-magics. For example, the following is invalid:

```python
if True:
	pwd
```

But, then the following is valid:
```python
	pwd
# ^^ unnecessary indentation
```

I think it's fine to go forward with this as it seems difficult to detect this without the surrounding context.

<img width="666" alt="Screenshot 2023-11-01 at 08 52 43" src="https://github.com/astral-sh/ruff/assets/67177269/f93d96b0-f41c-44d7-9925-ad1787be0489">

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:196 on 2023-11-01 03:24_

I agree. There are user defined magics as well but I think that's out of scope 😅

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:596 on 2023-11-01 03:27_

Can we add some additional test cases where there's Python code mixed with auto-magics?

Python code _before_ the auto-magic:
```python
x = 1
pwd
```

Python code _after_ the auto-magic:
```python
pwd
x = 1
```

---

_@dhruvmanila approved on 2023-11-01 03:28_

I think this is a reasonable approach. In the future, we could alter the AST to include the magic command name separately from the rest of the command value. That could be beneficial to have dedicated logic for specific magic command.

---

_@dhruvmanila reviewed on 2023-11-01 03:34_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:120 on 2023-11-01 03:34_

There are shell commands listed here as well. Can we add a comment on the source of the possible shell commands here?

Separately, does this list include _all_ of the line and cell magics in the linked documentation (https://ipython.readthedocs.io/en/stable/interactive/magics.html)?

---

_@charliermarsh reviewed on 2023-11-01 03:35_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:120 on 2023-11-01 03:35_

I believe this only applies to line magics, and not cell magics.

---

_@charliermarsh reviewed on 2023-11-01 03:37_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:120 on 2023-11-01 03:37_

Please correct me if I'm wrong though.

---

_@charliermarsh reviewed on 2023-11-01 03:40_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:93 on 2023-11-01 03:40_

Can we still trim, but only test the first line, rather than all subsequent lines?

---

_@charliermarsh reviewed on 2023-11-01 04:43_

---

_Review comment by @charliermarsh on `crates/ruff_notebook/src/notebook.rs`:93 on 2023-11-01 04:43_

Hmm, we actually run the risk of some false positives here...

For example, I guess we'd now flag `alias + 1` as an automagic, incorrectly.

We may need to try parsing this cell...? And fall back to automagics?

---

_@dhruvmanila reviewed on 2023-11-01 05:08_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:120 on 2023-11-01 05:08_

Yes, you're correct.

---

_@dhruvmanila reviewed on 2023-11-01 05:19_

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/notebook.rs`:93 on 2023-11-01 05:19_

> Can we still trim, but only test the first line, rather than all subsequent lines?

That might risk in not detecting cases where there's Python code _before_ the magic commands.

> For example, I guess we'd now flag `alias + 1` as an automagic, incorrectly.

Are you referring `alias` as a variable name? Yes, that's basically a risk for all escape commands 😅 

> We may need to try parsing this cell...? And fall back to automagics?

How would this help?

---

_Comment by @konstin on 2023-11-02 09:14_

![image](https://github.com/astral-sh/ruff/assets/6826232/202b0284-ee90-4047-b136-1242cdd55241)

:face_with_spiral_eyes: 

---

_Comment by @charliermarsh on 2023-11-03 00:42_

@dhruvmanila - Thought on this a bit more, and it seems like a reasonable approach for now. If a user _does_ have a cell that consists of code, but starts with a standalone automagic, then the behavior of that cell will vary based on the order in which the cells are executed. So I think it's ok to assume it's an automagic. But I'm definitely open to refinement.

---

_Comment by @github-actions[bot] on 2023-11-03 01:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @charliermarsh on 2023-11-03 01:14_

---

_Closed by @charliermarsh on 2023-11-03 01:14_

---

_Branch deleted on 2023-11-03 01:14_

---
