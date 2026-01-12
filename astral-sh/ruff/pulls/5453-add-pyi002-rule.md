```yaml
number: 5453
title: Add PYI002 Rule
type: pull_request
state: closed
author: USER-5
labels:
  - rule
assignees: []
base: main
head: pyi002-Complex-if-Test-in-Stub
created_at: 2023-06-30T12:56:41Z
updated_at: 2023-07-04T00:06:26Z
url: https://github.com/astral-sh/ruff/pull/5453
synced_at: 2026-01-12T15:55:18Z
```

# Add PYI002 Rule

---

_@USER-5_

## Summary

Add rule PYI002 from #848 

## Test Plan

Re-created behaviour from flake8-pyi repo:
![pyi002](https://github.com/astral-sh/ruff/assets/20570261/c500a13c-5d67-4507-a99c-5f47735ef2d8)


---

_Comment by @USER-5 on 2023-06-30 12:57_

~Also, for some reason I can't run `cargo test` successfully at the moment. I feel like it's an apple silicone-ism, but I can't generate the insta snapshots:
![Screenshot 2023-06-30 at 10 23 50 pm](https://github.com/astral-sh/ruff/assets/20570261/afb10eaa-3caf-4f21-ae12-6700ec5049c8)~

Edit: never mind. I didn't re-generate the schema.



---

_Comment by @github-actions[bot] on 2023-06-30 13:28_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+63, -0, 0 error(s))

<details><summary>typeshed (+63, -0)</summary>
<p>

```diff
+ stdlib/_curses.pyi:96:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:107:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:153:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:171:6: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:216:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:229:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:231:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:274:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:286:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:355:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:372:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:405:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:503:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:508:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:516:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:541:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:547:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:61:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_socket.pyi:691:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/_stat.pyi:81:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/errno.pyi:116:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/errno.pyi:122:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/locale.pyi:144:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/mmap.pyi:106:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/mmap.pyi:113:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/mmap.pyi:55:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/mmap.pyi:82:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/os/__init__.pyi:167:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/os/__init__.pyi:602:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/os/__init__.pyi:688:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/os/__init__.pyi:85:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/ossaudiodev.pyi:5:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/pathlib.pyi:182:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/pathlib.pyi:220:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/pathlib.pyi:74:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/select.pyi:142:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/select.pyi:35:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:122:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:137:6: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:145:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:178:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:238:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:252:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:256:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:259:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:384:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:399:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:427:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:429:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:491:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:518:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:522:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:541:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:570:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:575:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:610:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/socket.pyi:643:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/time.pyi:15:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/time.pyi:25:8: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/time.pyi:28:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/time.pyi:31:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stdlib/urllib/request.pyi:78:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
+ stubs/keyboard/keyboard/_mouse_event.pyi:21:4: PYI002 If test must be a simple comparison against sys.platform or sys.version_info
```

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI002 | 63 | 63 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.6±0.20ms     5.3 MB/sec    1.04      7.9±0.20ms     5.1 MB/sec
formatter/numpy/ctypeslib.py               1.02  1760.2±49.45µs     9.5 MB/sec    1.00  1733.8±64.13µs     9.6 MB/sec
formatter/numpy/globals.py                 1.02    203.6±8.43µs    14.5 MB/sec    1.00    200.1±8.48µs    14.7 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.12ms     6.6 MB/sec    1.00      3.8±0.18ms     6.7 MB/sec
linter/all-rules/large/dataset.py          1.00     13.8±0.54ms     2.9 MB/sec    1.01     13.9±0.35ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.20ms     4.7 MB/sec    1.00      3.5±0.14ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   437.6±20.00µs     6.7 MB/sec    1.04   456.8±16.04µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.28ms     4.1 MB/sec    1.00      6.2±0.20ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.7±0.41ms     6.0 MB/sec    1.06      7.1±0.14ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1433.5±52.44µs    11.6 MB/sec    1.06  1519.1±50.44µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.02   180.0±13.76µs    16.4 MB/sec    1.00    176.1±6.80µs    16.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.23ms     8.0 MB/sec    1.00      3.2±0.10ms     8.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.21     14.5±0.67ms     2.8 MB/sec    1.00     12.0±0.57ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.12      2.9±0.16ms     5.8 MB/sec    1.00      2.6±0.12ms     6.5 MB/sec
formatter/numpy/globals.py                 1.05   304.3±19.41µs     9.7 MB/sec    1.00   288.8±19.42µs    10.2 MB/sec
formatter/pydantic/types.py                1.09      6.3±0.36ms     4.1 MB/sec    1.00      5.8±0.32ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.01     20.3±0.74ms     2.0 MB/sec    1.00     20.1±0.68ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.25ms     3.1 MB/sec    1.03      5.5±0.96ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   638.7±31.69µs     4.6 MB/sec    1.00   641.2±40.25µs     4.6 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.0±0.39ms     2.8 MB/sec    1.01      9.1±0.45ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.05     10.8±0.57ms     3.8 MB/sec    1.00     10.2±0.41ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.11      2.4±0.16ms     6.9 MB/sec    1.00      2.2±0.08ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.11   283.6±24.01µs    10.4 MB/sec    1.00   255.8±16.31µs    11.5 MB/sec
linter/default-rules/pydantic/types.py     1.07      4.9±0.22ms     5.2 MB/sec    1.00      4.6±0.21ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @charliermarsh by @charliermarsh on 2023-07-02 19:24_

---

_Label `rule` added by @charliermarsh on 2023-07-02 19:24_

---

_Comment by @charliermarsh on 2023-07-03 03:42_

Dang it, I started to review this but then realized it was implemented as part of https://github.com/astral-sh/ruff/pull/5457. I'm really sorry for the mixup, it looks like a race condition in picking up tasks.

---

_Closed by @charliermarsh on 2023-07-03 03:52_

---

_Comment by @USER-5 on 2023-07-04 00:06_

> Dang it, I started to review this but then realized it was implemented as part of #5457. I'm really sorry for the mixup, it looks like a race condition in picking up tasks.

Lucky me finding a race condition when only writing safe rust

---
