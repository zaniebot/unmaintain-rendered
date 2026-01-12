```yaml
number: 2070
title: "ICN001 import-alias-is-not-conventional should check \"from\" imports"
type: pull_request
state: merged
author: Zeddicus414
labels: []
assignees: []
merged: true
base: main
head: ICN001-check-from-imports
created_at: 2023-01-21T19:43:26Z
updated_at: 2023-01-21T22:00:52Z
url: https://github.com/astral-sh/ruff/pull/2070
synced_at: 2026-01-12T15:55:07Z
```

# ICN001 import-alias-is-not-conventional should check "from" imports

---

_@Zeddicus414_

See https://github.com/charliermarsh/ruff/issues/2047



---

_Comment by @charliermarsh on 2023-01-21 19:44_

Thanks! I'll review this in a bit.

---

_Comment by @Zeddicus414 on 2023-01-21 19:48_

Couple of things. I know my commit history is pretty messy. I'm assuming that's something you can just discard during the PR process?

Also, I modified `resources/test/fixtures/pyproject.toml` and the associated `src/settings/pyproject.rs` tests before realizing that the insta tests set their own config, so those changes were not needed. I can undo these changes if that's preferred.

---

_Comment by @charliermarsh on 2023-01-21 19:49_

1. Yup, commit history is no problem at all. By default, I squash the history, so this ends up as one commit (linked to the PR).
2. Yeah, if you don't mind, I'd prefer to roll back those changes to `pyproject.toml`.

---

_@charliermarsh reviewed on 2023-01-21 20:07_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:1238 on 2023-01-21 20:07_

Nit: let's move this into the `if` below, so that we're not creating this string unless it's needed by the `check_conventional_import` call.

---

_@charliermarsh reviewed on 2023-01-21 20:09_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_import_conventions/from_imports.py`:11 on 2023-01-21 20:09_

Per your comment in the issue -- yeah, not sure what should happen if a user has `"..xml.dom.minidom"` in their `pyproject.toml`. It's obviously a bit weird to support relative imports there ("Relative to what?"), but I think it's fine as-is. There's not a clear right answer. The alternative would be to avoid calling `check_conventional_import` for relative imports at all.

---

_@charliermarsh reviewed on 2023-01-21 20:09_

---

_Review comment by @charliermarsh on `resources/test/fixtures/flake8_import_conventions/from_imports.py`:8 on 2023-01-21 20:09_

Maybe add a test for an `import from` with multiple members, only one of which needs to be flagged?

---

_Review comment by @Zeddicus414 on `resources/test/fixtures/flake8_import_conventions/from_imports.py`:11 on 2023-01-21 20:21_

Leaving as is.

---

_@Zeddicus414 reviewed on 2023-01-21 20:21_

---

_@Zeddicus414 reviewed on 2023-01-21 20:25_

---

_Review comment by @Zeddicus414 on `src/checkers/ast.rs`:1238 on 2023-01-21 20:25_

Done.

---

_Merged by @charliermarsh on 2023-01-21 20:43_

---

_Closed by @charliermarsh on 2023-01-21 20:43_

---

_@Zeddicus414 reviewed on 2023-01-21 20:47_

---

_Review comment by @Zeddicus414 on `resources/test/fixtures/flake8_import_conventions/from_imports.py`:8 on 2023-01-21 20:47_

Adding test cases was a good call.

Currently, it considers this a violation:
```python
import xml.dom.minidom
```

But it doesn't catch these:
```python
from xml.dom import minidom
from xml.dom.minidom import parseString
```

I'll try to track down what's up. I think these examples should all be violations.


---

_Comment by @Zeddicus414 on 2023-01-21 20:50_

Oops, looked like you  merged the PR while I was still trying to add some more test conditions. Would you like me to just create a separate PR for that? Or work on it here?

---

_Comment by @charliermarsh on 2023-01-21 20:51_

Gah sorry, yeah, I jumped the gun. Let’s do it in a separate PR please.

---

_Comment by @Zeddicus414 on 2023-01-21 20:53_

No big deal. I think everything still works fine in main, there is just the slight inconsistency between the `import` and `from...import` cases where no alias is defined. 

https://github.com/charliermarsh/ruff/pull/2070#discussion_r1083333898

---

_@charliermarsh reviewed on 2023-01-21 21:32_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:1233 on 2023-01-21 21:32_

In case you haven't caught it: this block only runs when the `as` is set (e.g. `from foo import bar as baz`, but not `from foo import bar`). I think you can lift all these changes out of this `if`, maybe?

---

_@Zeddicus414 reviewed on 2023-01-21 21:46_

---

_Review comment by @Zeddicus414 on `src/checkers/ast.rs`:1233 on 2023-01-21 21:46_

ohhhhhhh, yeah, I see it now. I put it under that condition because I thought the original call was under the same clause on line 896, but I miss-read it. It's not under the `if let Some(asname) = &alias.node.asname` so mine shouldn't be either. Makes sense.

---

_@charliermarsh reviewed on 2023-01-21 22:00_

---

_Review comment by @charliermarsh on `src/checkers/ast.rs`:1233 on 2023-01-21 22:00_

(Just for clarity in communication: do you plan to fix this, or want me to?)

---
