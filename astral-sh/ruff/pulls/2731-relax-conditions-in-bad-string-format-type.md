```yaml
number: 2731
title: Relax conditions in bad-string-format-type
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/relax
created_at: 2023-02-10T18:48:23Z
updated_at: 2023-02-10T19:26:00Z
url: https://github.com/astral-sh/ruff/pull/2731
synced_at: 2026-01-12T15:55:10Z
```

# Relax conditions in bad-string-format-type

---

_@charliermarsh_

An oversight on my part. We have to be _way_ more conservative here. If we can't identify the type of an expression, we _have_ to assume that it's ok.

Closes #2724.

---

_Comment by @charliermarsh on 2023-02-10 18:48_

Thanks to @spaceone for testing off `main`!

---

_Comment by @spaceone on 2023-02-10 18:54_

great. Only three leftovers:

1. `'(%r, %r, %r, %r)' % (hostname, address, username, '$PASSWORD')`
2. `'%r' % ({'server_school_roles': server_school_roles, 'is_school_multiserver_domain': is_school_multiserver_domain}, ))`
3. 
```
172 |           return '%s(%r, %r, %r)' % (
    |  ________________^
173 | |             self.__class__.__name__,
174 | |             {},
175 | |             '',
176 | |             self.private(),
177 | |         )
    | |_________^ PLE1307

```

---

_Comment by @charliermarsh on 2023-02-10 18:54_

Thank you

---

_Comment by @charliermarsh on 2023-02-10 18:55_

(That's only two examples, did you mean to include a third?)

---

_Comment by @spaceone on 2023-02-10 18:57_

> (That's only two examples, did you mean to include a third?)

The third is given as ruff-show-source instead of regular code.

I guess the cause for all 3 is the same.
Otherwise here is a 4th:
`"%r" % ([x.printmode for x in settingslist],)`


---

_Comment by @charliermarsh on 2023-02-10 19:07_

If it passes without false-positives on whatever project this is, it'll pass anywhere :D

---

_Comment by @spaceone on 2023-02-10 19:20_

passes now!

---

_Merged by @charliermarsh on 2023-02-10 19:25_

---

_Closed by @charliermarsh on 2023-02-10 19:25_

---

_Branch deleted on 2023-02-10 19:26_

---
