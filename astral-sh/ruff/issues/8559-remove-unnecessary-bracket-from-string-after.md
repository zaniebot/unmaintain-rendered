```yaml
number: 8559
title: remove unnecessary bracket from string after ISC001 
type: issue
state: open
author: spaceone
labels:
  - rule
  - wish
assignees: []
created_at: 2023-11-08T13:55:08Z
updated_at: 2023-11-09T02:14:28Z
url: https://github.com/astral-sh/ruff/issues/8559
synced_at: 2026-01-12T15:54:48Z
```

# remove unnecessary bracket from string after ISC001 

---

_@spaceone_

`ruff format --diff --isolated --line-length 140 foo.py` turns:
```python
(
    "<html><body>"
    "<h3>{heading}</h3>"
    "{version_text}<br/><br/>"
    "{copyright}<br/>"
    "<p>{gnu_license}</p>"
    "<p>{icon}</p>"
).format(**kwargs)
```
into:
```python
("<html><body>" "<h3>{heading}</h3>" "{version_text}<br/><br/>" "{copyright}<br/>" "<p>{gnu_license}</p>" "<p>{icon}</p>").format(**kwargs)
```

Afterwards `ruff --isolated --line-length 140 --select ISC001 --fix foo.py ` turns it into:

```python
("<html><body><h3>{heading}</h3>{version_text}<br/><br/>{copyright}<br/><p>{gnu_license}</p><p>{icon}</p>").format(**kwargs)
```

I want the following result:
```python
"<html><body><h3>{heading}</h3>{version_text}<br/><br/>{copyright}<br/><p>{gnu_license}</p><p>{icon}</p>".format(**kwargs)
```

---

_Label `rule` added by @charliermarsh on 2023-11-09 02:14_

---

_Label `wish` added by @charliermarsh on 2023-11-09 02:14_

---
