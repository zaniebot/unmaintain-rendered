```yaml
number: 7379
title: "`W191` isn't triggered inside multi-line f-string expression"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
assignees: []
created_at: 2023-09-14T07:46:54Z
updated_at: 2023-09-16T20:25:22Z
url: https://github.com/astral-sh/ruff/issues/7379
synced_at: 2026-01-12T15:54:47Z
```

# `W191` isn't triggered inside multi-line f-string expression

---

_@dhruvmanila_

Code snippet:

```python
f"""test{
	tab_indented_should_be_flagged
}	<- this tab is fine"""
```

Command:

```console
ruff check --no-cache --select=W191 --isolated test.py
```

Rule reference: https://beta.ruff.rs/docs/rules/tab-indentation/

---

_Label `bug` added by @dhruvmanila on 2023-09-14 07:46_

---

_Comment by @dhruvmanila on 2023-09-14 10:22_

(I would wait until PEP 701 support is merged as the implementation might change)

---

_Closed by @dhruvmanila on 2023-09-16 20:25_

---
