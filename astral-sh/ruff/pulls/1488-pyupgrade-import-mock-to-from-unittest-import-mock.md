```yaml
number: 1488
title: "Pyupgrade: `import mock` to `from unittest import mock`"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: mockimports
created_at: 2022-12-30T21:42:30Z
updated_at: 2023-01-01T16:11:23Z
url: https://github.com/astral-sh/ruff/pull/1488
synced_at: 2026-01-12T15:55:06Z
```

# Pyupgrade: `import mock` to `from unittest import mock`

---

_@colin99d_

This gets us further on #827. I added a lot of tests not included in the original library. Also, the function to create the changes was a little complicated, so I added a BUNCH of comments to it.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_mock_import.rs`:40 on 2022-12-30 23:22_

Have you looked into using LibCST at all for these sorts of complicated rewrites? We have a few examples, like in `src/flake8_comprehensions/fixes.rs`. It gives you a concrete syntax tree (which includes punctuation, whitespace, etc.), which you can then edit, and generate fixed code from the edited tree. It might give us a lot more safety as these transforms are kind of bespoke, and I worry that they might get tripped up since we're doing so much exact-string processing.

Are you open to seeing if LibCST would be useable here?

---

_@charliermarsh reviewed on 2022-12-30 23:22_

---

_@charliermarsh reviewed on 2022-12-30 23:22_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_mock_import.rs`:59 on 2022-12-30 23:22_

Could this be `contents.lines().count()`?

---

_@charliermarsh reviewed on 2022-12-30 23:22_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_mock_import.rs`:41 on 2022-12-30 23:22_

Can probably be `needed_imports: &[&str]`?

---

_@charliermarsh reviewed on 2022-12-30 23:23_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_mock_import.rs`:66 on 2022-12-30 23:23_

For example, this assumes four-space indent, but LibCST would probably let you preserve whatever indent was already present (unless I'm misunderstanding this).

---

_@charliermarsh reviewed on 2022-12-30 23:24_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pyupgrade/UP026.py`:29 on 2022-12-30 23:24_

(It's also ok, in my opinion, for fixes like this to make some assumptions (e.g., inject a trailing comma) if it greatly simplifies the code.)

---

_Review comment by @colin99d on `src/pyupgrade/plugins/rewrite_mock_import.rs`:66 on 2022-12-31 13:56_

This adds 4 spaces on top of any indent, so it still works. However I did not know about LibCST, and will definitely try it.

---

_@colin99d reviewed on 2022-12-31 13:56_

---

_@colin99d reviewed on 2022-12-31 13:58_

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP026.py`:29 on 2022-12-31 13:58_

Could this lead to issues in further run throughs? For example if the trailing comma was removed, then the next time the list might be folded into one line. This isnt a HUGE deal, but could be a little frustrating.

---

_Review comment by @colin99d on `resources/test/fixtures/pyupgrade/UP026.py`:29 on 2022-12-31 13:59_

Of course its your project and your decisions. Let me know how you want it handled.

---

_@colin99d reviewed on 2022-12-31 13:59_

---

_Comment by @colin99d on 2022-12-31 14:14_

One issue I do need to worry about here is `asname`. Would you like me to preserve them the way they were? The new lib_cst will make that a lot easier.

---

_@charliermarsh reviewed on 2022-12-31 18:43_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/rewrite_mock_import.rs`:66 on 2022-12-31 18:43_

Give LibCST a shot for this change and let me know how it goes. I'm here to answer any questions. There's definitely a learning curve (it uses a "concrete syntax tree" so the representation is a bit different and more detailed), but it's very powerful and could help make a lot of this stuff "safer" and easier to reason about.

---

_Comment by @colin99d on 2022-12-31 19:58_

@charliermarsh definitely a learning curve involved, but I see the value. All the code is updated now.

---

_Merged by @charliermarsh on 2023-01-01 02:25_

---

_Closed by @charliermarsh on 2023-01-01 02:25_

---

_Branch deleted on 2023-01-01 16:11_

---
