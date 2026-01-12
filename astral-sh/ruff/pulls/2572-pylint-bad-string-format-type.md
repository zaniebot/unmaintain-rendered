```yaml
number: 2572
title: "[`pylint`]: bad-string-format-type"
type: pull_request
state: merged
author: colin99d
labels:
  - rule
assignees: []
merged: true
base: main
head: bad-string-format-type
created_at: 2023-02-04T21:07:12Z
updated_at: 2023-02-10T18:07:12Z
url: https://github.com/astral-sh/ruff/pull/2572
synced_at: 2026-01-12T15:55:08Z
```

# [`pylint`]: bad-string-format-type

---

_@colin99d_

ref: #970 

---

_Label `rule` added by @charliermarsh on 2023-02-04 21:58_

---

_Comment by @colin99d on 2023-02-05 03:54_

Do we need to keep the restriction that this only works of the statement is one line. I got this from copying code in UP031, and I am not sure if it still applies here.

---

_Comment by @colin99d on 2023-02-05 16:01_

Also, do we have a way of knowing the type of a variable? For example, pylint can throw a warning here:
```
WORD = "abc"
"%d" % WORD
```

But we cannot, because I am not sure how to tell pylint that the type of WORD is string.

---

_Marked ready for review by @colin99d on 2023-02-05 19:21_

---

_Merged by @charliermarsh on 2023-02-10 01:08_

---

_Closed by @charliermarsh on 2023-02-10 01:08_

---

_Comment by @not-my-profile on 2023-02-10 04:02_

This PR again created `src/registry.rs`.

---

_Comment by @spaceone on 2023-02-10 18:07_

There are a lot of false positives introduced, I started a collection here: https://github.com/charliermarsh/ruff/issues/2724

---
