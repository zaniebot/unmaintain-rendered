```yaml
number: 11322
title: "Ruff formatting \"\\x1E\" to \"\\x1e\""
type: issue
state: closed
author: MarcoForte
labels:
  - question
assignees: []
created_at: 2024-05-07T10:30:37Z
updated_at: 2024-05-07T10:52:33Z
url: https://github.com/astral-sh/ruff/issues/11322
synced_at: 2026-01-12T15:54:51Z
```

# Ruff formatting "\x1E" to "\x1e"

---

_@MarcoForte_

Hello I am trying to understand why ruff is formatting "\x1E" to "\x1e". I'd like to avoid formatting strings since they can be case sensitive. 
Is there a way to see what rule specifically is invoked in this formatting? 
I tried
`ruff format  --check file.py -v`
and see
```
[2024-05-07][10:25:37][ruff::resolve][DEBUG] Using configuration file (via parent) at: pyproject.toml
[2024-05-07][10:25:37][ruff::commands::format][DEBUG] format_path; file.py
[2024-05-07][10:25:37][tracing::span][DEBUG] Printer::print;
[2024-05-07][10:25:37][ruff::commands::format][DEBUG] Formatted 1 files in 937.70µs
Would reformat: file.py
1 file would be reformatted
```

---

_Label `question` added by @MichaReiser on 2024-05-07 10:40_

---

_Comment by @MichaReiser on 2024-05-07 10:45_

What you see here is that Ruff normalizes the casing of hex escape sequences, which are not case-sensitive: 

```python
>>> "\x1E" == "\x1e"
True
```

>  Is there a way to see what rule is invoked in this formatting?

No, the formatter works as a hole, there are no individual rules. 

---

_Comment by @MarcoForte on 2024-05-07 10:52_

Thank you, very clear! 
It was the final hurdle to integrate ruff formatter into our project, we love it at @photoroom. 

---

_Closed by @MarcoForte on 2024-05-07 10:52_

---
