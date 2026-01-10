---
number: 8761
title: Rule SIM401 should only trigger for immutable literals and variables
type: issue
state: closed
author: ofek
labels: []
assignees: []
created_at: 2023-11-18T23:40:37Z
updated_at: 2023-11-20T21:18:23Z
url: https://github.com/astral-sh/ruff/issues/8761
synced_at: 2026-01-10T01:22:48Z
---

# Rule SIM401 should only trigger for immutable literals and variables

---

_Issue opened by @ofek on 2023-11-18 23:40_

If I were to make this change then the dictionary would be created regardless of key membership. This would be particularly costly if the default was the result of a function call.

```
src\hatch\template\files_default.py:117:13: SIM401 Use `plugin_config.get('project_urls', {'Documentation': 'https://github.com/unknown/{project_name_normalized}#readme', 'Issues': 'https://github.com/unknown/{project_name_normalized}/issues', 'Source': 'https://github.com/unknown/{project_name_normalized}'})` instead of an `if` block
```

```python
project_urls = (
    plugin_config['project_urls']
    if 'project_urls' in plugin_config
    else {
        'Documentation': 'https://github.com/unknown/{project_name_normalized}#readme',
        'Issues': 'https://github.com/unknown/{project_name_normalized}/issues',
        'Source': 'https://github.com/unknown/{project_name_normalized}',
    }
)
```

---

_Comment by @charliermarsh on 2023-11-18 23:43_

We don't trigger SIM401 if the default value contains an effectful expression (like a function call) -- you can verify it in the playground: https://play.ruff.rs/79791de1-e3ff-452e-b0a0-fab819b58e1c

---

_Comment by @ofek on 2023-11-18 23:47_

Nice, I didn't test that one! Do you think creation of data structures should be avoided here?

---

_Comment by @flying-sheep on 2023-11-20 20:48_

I don’t think so, creating a hierarchy of dicts, lists, etc. is cheap. I bet other parts of the code absolutely dominate benchmarks if you make some. That being said, performance issues should be measured, not assumed. So if you need things to be faster, you should measure first.

Regarding this rule, you don’t even need an `if` branch *if* you do things lazily if your values aren’t falsy:

```py
foo = bar.get('key') or default_expression()
```

---

_Comment by @zanieb on 2023-11-20 21:18_

I think we already have a suitable heuristic here. If you have a real-world example with costly object creation we should avoid suggesting, please let us know.

---

_Closed by @zanieb on 2023-11-20 21:18_

---
