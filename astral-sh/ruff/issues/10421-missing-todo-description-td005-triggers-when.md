---
number: 10421
title: "`missing-todo-description` (`TD005`) triggers when description begins on following line"
type: issue
state: open
author: tjkuson
labels:
  - bug
assignees: []
created_at: 2024-03-15T13:27:58Z
updated_at: 2024-09-05T05:18:05Z
url: https://github.com/astral-sh/ruff/issues/10421
synced_at: 2026-01-10T01:22:50Z
---

# `missing-todo-description` (`TD005`) triggers when description begins on following line

---

_Issue opened by @tjkuson on 2024-03-15 13:27_

`missing-todo-description` (`TD005`) triggers on the following code.

```python
# TODO(tom):
#   something
#   something
```

You can reproduce with `ruff check /tmp/todo.py --select TD005 --isolated` using version 0.3.2.

I would expect it to understand that the following lines are a part of the TODO description.

Search terms: `TD005`, `todo`

PS: I created the issue with a nonsense title and missing description (pressed enter accidentally). Sorry for the noise. 

---

_Renamed from "# dfjslfkjs" to "`missing-todo-description` (`TD005`) triggers when description begins on following line" by @tjkuson on 2024-03-15 13:28_

---

_Label `bug` added by @charliermarsh on 2024-03-15 13:54_

---

_Comment by @luxedo on 2024-09-04 01:51_

I have a couple issues with this issue (lol)
1. The source [flake8-todos](https://github.com/orsinium-labs/flake8-todos) trigger this as an error. I think that the author expected to have a simple description in the same line.
2. How could we know that the following line is related to the previous one? It is possible to do so but I think it would be too expensive.

---

_Comment by @dhruvmanila on 2024-09-04 04:31_

@tjkuson I'm curious to know why the description starts on the second line and is this a common scenario in projects? Is there a prior art for this format?

> 2\. How could we know that the following line is related to the previous one? It is possible to do so but I think it would be too expensive.

I think it's the indent that should signal that the description is part of the todo comment although not sure.

---

_Comment by @tjkuson on 2024-09-05 05:18_

@dhruvmanila It's not very common, but it's used: https://grep.app/search?q=%23%20TODO%28%5C%28%5B%5E%29%5D%2A%5C%29%29%3F%3A%5Cn&regexp=true&filter[lang][0]=Python

I think the motivation is readability (e.g., if the TODO comment consists of a list or is long). That's how I discovered the issue.

> I think it's the indent that should signal that the description is part of the todo comment although not sure.

I added the indentation as that's what I was used to coming from JetBrains IDEs (which require indentation for the syntax highlighting to apply to the entire TODO text). Not all the examples I found include indentation.

---
