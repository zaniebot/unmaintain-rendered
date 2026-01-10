---
number: 14878
title: Ability to output junit format to a file and retain default output to standard out.
type: issue
state: closed
author: martinka
labels: []
assignees: []
created_at: 2024-12-09T15:40:04Z
updated_at: 2024-12-09T15:47:57Z
url: https://github.com/astral-sh/ruff/issues/14878
synced_at: 2026-01-10T01:22:55Z
---

# Ability to output junit format to a file and retain default output to standard out.

---

_Issue opened by @martinka on 2024-12-09 15:40_

We attempt to use the same Makefile for both our dev environments and our CI pipelines.  We would like junit reporting for the CI pipelines but its not particularly human readable.  We have been able to configure other linters  to emit a file in junit while having the stdout still be human readable.  Effective this would change the output-format setting to a list and allow setting a file with the formatter.  An Example of how this is handled in pylint is `output-format="text,junit:build/junit-pylint.xml` 

I am new to rust but it looks like a bulk of the change would be in printer.rs

This would make our ci pipeline a lot cleaner, the only alternative right now seems to be to run ruff twice.





---

_Comment by @MichaReiser on 2024-12-09 15:46_

Hi @martinka 
I think this is the same as https://github.com/astral-sh/ruff/issues/2388



---

_Closed by @MichaReiser on 2024-12-09 15:46_

---

_Comment by @martinka on 2024-12-09 15:47_

Yes it is.. I missed that when looking at the issues.  you can close this I will follow that.


---
