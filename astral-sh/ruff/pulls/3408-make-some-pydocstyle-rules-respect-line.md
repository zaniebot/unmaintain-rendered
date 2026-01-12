```yaml
number: 3408
title: Make some pydocstyle rules respect line continuation character
type: pull_request
state: closed
author: yuxqiu
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2023-03-08T20:49:27Z
updated_at: 2023-05-07T17:26:43Z
url: https://github.com/astral-sh/ruff/pull/3408
synced_at: 2026-01-12T03:56:38Z
```

# Make some pydocstyle rules respect line continuation character

---

_Pull request opened by @yuxqiu on 2023-03-08 20:49_

Currently, pydocstyle rules produce many false positives on code bases that use continuation characters in docstrings. This PR tries to make some pydocstyle rules respect line continuation character. 

Progress so far
- [ ] fix D205 and add one more auto fix
    - Need to respect [stringescapeseq](https://docs.python.org/3/reference/lexical_analysis.html#grammar-token-python-grammar-stringescapeseq)

After a quick look, I plan to investigate the following rules as they may have the same problem.
- [ ] D200 (empty first line + content -> no report)
- [ ] D209
- [ ] D210 (?)
- [ ] D400
- [ ] D415

---

_Comment by @charliermarsh on 2023-03-08 21:14_

Do you know if `pydocstyle` itself treats continuations in this way? As in, would we be intentionally deviating from `pydocstyle` by merging this? Or does Ruff _currently_ differ from `pydocstyle` here?

---

_Comment by @yuxqiu on 2023-03-08 21:20_

> Do you know if `pydocstyle` itself treats continuations in this way? As in, would we be intentionally deviating from `pydocstyle` by merging this? Or does Ruff _currently_ differ from `pydocstyle` here?

As far as I know, `pydocstyle` treats continuations in this way. When I run `pydocstyle crates/ruff/resources/test/fixtures/pydocstyle/D205.py --select D205`, it prints

```
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:13 in public function `f3`:
D205: 1 blank line required between summary line and description (found 0)
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:30 in public function `f5`:
D205: 1 blank line required between summary line and description (found 0)
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:42 in public function `f6`:
D205: 1 blank line required between summary line and description (found 3)
```

But when I run `ruff check rates/ruff/resources/test/fixtures/pydocstyle/D205.py --select D205` (without this PR), it produces 3 false positives:

```
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:2:5: D205 1 blank line required between summary line and description
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:7:5: D205 1 blank line required between summary line and description
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:13:5: D205 1 blank line required between summary line and description
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:19:5: D205 1 blank line required between summary line and description
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:30:5: D205 1 blank line required between summary line and description
crates/ruff/resources/test/fixtures/pydocstyle/D205.py:42:5: D205 1 blank line required between summary line and description
```

---

_Converted to draft by @yuxqiu on 2023-03-08 21:28_

---

_Comment by @yuxqiu on 2023-03-09 09:24_

@charliermarsh Considering [how Python escape string literals](https://docs.python.org/3/reference/lexical_analysis.html#index-23), I think we can provide a function in `crates/ruff_python_ast/src/str.rs` to unescape docstrings. After that, we can easily handle line continuation and newlines. What do you think?

---

_Comment by @charliermarsh on 2023-05-07 17:26_

Going to close in the interest of keeping the PR list up-to-date. Can always re-open if we choose to revisit!

---

_Closed by @charliermarsh on 2023-05-07 17:26_

---
