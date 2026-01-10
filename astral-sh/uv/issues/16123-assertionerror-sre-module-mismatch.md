---
number: 16123
title: "AssertionError: SRE module mismatch"
type: issue
state: open
author: adaaaaaa
labels:
  - question
assignees: []
created_at: 2025-10-04T11:13:51Z
updated_at: 2025-10-06T08:51:46Z
url: https://github.com/astral-sh/uv/issues/16123
synced_at: 2026-01-10T01:26:03Z
---

# AssertionError: SRE module mismatch

---

_Issue opened by @adaaaaaa on 2025-10-04 11:13_

### Summary

uv run main.py                                                                                                                                                                 ─╯
Traceback (most recent call last):
  File "/data/MediaCrawler/main.py", line 12, in <module>
    import asyncio
  File "/usr/lib/python3.13/asyncio/__init__.py", line 8, in <module>
    from .base_events import *
  File "/usr/lib/python3.13/asyncio/base_events.py", line 18, in <module>
    import concurrent.futures
  File "/usr/lib/python3.13/concurrent/futures/__init__.py", line 8, in <module>
    from concurrent.futures._base import (FIRST_COMPLETED,
  File "/usr/lib/python3.13/concurrent/futures/_base.py", line 7, in <module>
    import logging
  File "/usr/lib/python3.13/logging/__init__.py", line 26, in <module>
    import sys, os, time, io, re, traceback, warnings, weakref, collections.abc
  File "/usr/lib/python3.13/re/__init__.py", line 126, in <module>
    from . import _compiler, _parser
  File "/usr/lib/python3.13/re/_compiler.py", line 18, in <module>
    assert _sre.MAGIC == MAGIC, "SRE module mismatch"
           ^^^^^^^^^^^^^^^^^^^
AssertionError: SRE module mismatch


### Platform

archlinux

### Version

0.8.22

### Python version

3.11.13
3.13.5
3.13.7

'uv python pin' = 3.11 while above message python is in 3.13 why is the different?


---

_Label `bug` added by @adaaaaaa on 2025-10-04 11:13_

---

_Label `bug` removed by @konstin on 2025-10-06 08:50_

---

_Label `question` added by @konstin on 2025-10-06 08:50_

---

_Comment by @konstin on 2025-10-06 08:51_

Are you sure that this is a bug in uv, and not in your code or your Python distribution? If you think it's a uv bug, can you share a minimal reproducible example (https://github.com/astral-sh/uv/issues/9452)?

---
