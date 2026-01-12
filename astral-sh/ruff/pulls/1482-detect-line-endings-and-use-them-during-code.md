```yaml
number: 1482
title: Detect line endings and use them during code generation
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: use-proper-line-endings
created_at: 2022-12-30T16:55:16Z
updated_at: 2022-12-30T19:43:37Z
url: https://github.com/astral-sh/ruff/pull/1482
synced_at: 2026-01-12T05:36:31Z
```

# Detect line endings and use them during code generation

---

_Pull request opened by @squiddy on 2022-12-30 16:55_

Closes #1477

---

_Comment by @squiddy on 2022-12-30 17:10_

(Clippy issue is from main)

---

_Comment by @charliermarsh on 2022-12-30 17:40_

Nice, this looks quite good!

---

_Review comment by @charliermarsh on `src/source_code_style.rs`:37 on 2022-12-30 17:40_

Thank you for reusing this abstraction!

---

_@charliermarsh reviewed on 2022-12-30 17:40_

---

_Merged by @charliermarsh on 2022-12-30 17:59_

---

_Closed by @charliermarsh on 2022-12-30 17:59_

---

_Branch deleted on 2022-12-30 18:00_

---

_Comment by @rhkleijn on 2022-12-30 19:10_

Thanks!

---

_Comment by @charliermarsh on 2022-12-30 19:19_

Looking back at this: do we also need to change SourceCodeGenerator? That writes newlines too.

---

_Comment by @squiddy on 2022-12-30 19:27_

> Looking back at this: do we also need to change SourceCodeGenerator? That writes newlines too.

Whoops. I specifically searched for line endings in the code, not sure why the generator slipped through. I can do that follow-up if you want.

---

_Comment by @charliermarsh on 2022-12-30 19:43_

That’d be great! No rush.

---
