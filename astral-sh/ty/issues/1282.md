```yaml
number: 1282
title: Playground crashes if file name starts with ////
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - playground
assignees: []
created_at: 2025-09-30T00:25:06Z
updated_at: 2025-10-01T13:55:40Z
url: https://github.com/astral-sh/ty/issues/1282
synced_at: 2026-01-10T02:06:25Z
```

# Playground crashes if file name starts with ////

---

_Issue opened by @MeGaGiGaGon on 2025-09-30 00:25_

### Summary

If you make the name of the file in the playground start with `////`, the playground crashes. The error in the console is:
```
uri.ts:40 Uncaught Error: [UriError]: If a URI does not contain an authority component, then the path cannot begin with two slash characters ("//")
```
Video demonstration:

https://github.com/user-attachments/assets/9b95bd58-6d2b-46c6-a257-8af1bc436e7d


### Version

Playground (8664842d0)

---

_Label `bug` added by @MichaReiser on 2025-09-30 06:02_

---

_Label `playground` added by @MichaReiser on 2025-09-30 06:02_

---

_Closed by @MichaReiser on 2025-10-01 13:55_

---
