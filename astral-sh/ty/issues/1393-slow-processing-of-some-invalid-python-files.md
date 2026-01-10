---
number: 1393
title: Slow processing of some invalid python files
type: issue
state: open
author: qarmin
labels:
  - performance
  - hang
assignees: []
created_at: 2025-10-18T06:22:28Z
updated_at: 2026-01-08T23:58:30Z
url: https://github.com/astral-sh/ty/issues/1393
synced_at: 2026-01-10T01:48:23Z
---

# Slow processing of some invalid python files

---

_Issue opened by @qarmin on 2025-10-18 06:22_

### Summary

I ran the `ty check . command` on each file individually — each filename includes the time it took to scan on my machine.

```
40K   07774379___10.531_seconds___invalid_python.py
40K   13832638___7.077_seconds___invalid_python.py
68K   16059136___14.184_seconds___invalid_python.py
12K   28136638___6.122_seconds___invalid_python.py
11K   35138501___4.671_seconds___invalid_python.py
12K   41326180___5.628_seconds___invalid_python.py
46K   48769042___timeout_20_seconds___invalid_python.py
44K   56867334___timeout_20_seconds___invalid_python.py
2,0K   86099013___14.385_seconds___invalid_python.py
32K   94313466___7.124_seconds___invalid_python.py
5,1K   97114626___timeout_20_seconds___invalid_python.py

```
[files.zip](https://github.com/user-attachments/files/22982706/thunderbird.tmp.zip)

[BBB.zip](https://github.com/user-attachments/files/23373565/BBB.zip)

Most of the larger files probably share the same root cause as https://github.com/astral-sh/ty/issues/71

However, the file `5,1K 97114626___timeout_20_seconds___invalid_python.py` is particularly interesting, as it doesn’t contain even small tuples.

### Version

ty 0.0.1-alpha.23

---

_Label `performance` added by @AlexWaygood on 2025-10-18 11:18_

---

_Label `hang` added by @MichaReiser on 2025-10-19 08:57_

---

_Comment by @MichaReiser on 2025-10-19 09:00_

Thank you. Your computer must be much faster than mine:

```
2025-10-19 10:58:19.922277 DEBUG Module `scipy.linalg` not found in search paths
2025-10-19 10:58:28.551658 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/35138501___4.671_seconds___invalid_python.py` took more than 100ms (8.647666042s)
2025-10-19 10:58:30.10918 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/28136638___6.122_seconds___invalid_python.py` took more than 100ms (10.205207292s)
2025-10-19 10:58:30.138328 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/41326180___5.628_seconds___invalid_python.py` took more than 100ms (10.234164167s)
2025-10-19 10:58:31.972721 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/13832638___7.077_seconds___invalid_python.py` took more than 100ms (12.068145083s)
2025-10-19 10:58:32.600115 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/94313466___7.124_seconds___invalid_python.py` took more than 100ms (12.695962833s)
2025-10-19 10:58:36.491941 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/07774379___10.531_seconds___invalid_python.py` took more than 100ms (16.587057333s)
2025-10-19 10:58:39.552515 DEBUG Skipping argument type expansion as it would exceed the maximum number of expansions (512)
2025-10-19 10:58:39.578064 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/86099013___14.385_seconds___invalid_python.py` took more than 100ms (19.674333542s)
2025-10-19 10:58:44.335118 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/16059136___14.184_seconds___invalid_python.py` took more than 100ms (24.43005825s)
2025-10-19 10:58:58.478216 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/56867334___timeout_20_seconds___invalid_python.py` took more than 100ms (38.5736185s)
2025-10-19 10:59:38.813459 INFO Checking file `/Users/micha/Downloads/thunderbird.tmp/48769042___timeout_20_seconds___invalid_python.py` took more than 100ms (78.908021917s)
```

and there's one file that never seems to complete?


---

_Added to milestone `Stable` by @carljm on 2026-01-08 23:58_

---
