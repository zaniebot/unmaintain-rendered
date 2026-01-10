---
number: 19346
title: E501 (line-too-long) auto-fixing
type: issue
state: closed
author: pwithams
labels:
  - question
assignees: []
created_at: 2025-07-15T06:49:55Z
updated_at: 2025-09-20T20:37:42Z
url: https://github.com/astral-sh/ruff/issues/19346
synced_at: 2026-01-10T01:23:00Z
---

# E501 (line-too-long) auto-fixing

---

_Issue opened by @pwithams on 2025-07-15 06:49_

### Question

I know auto-fixing E501 (line-too-long) (https://github.com/astral-sh/ruff/issues/7414) is tricky for many reasons, such as:
 - comments may include machine-parsable content
 - some multi-line or doc strings may be sensitive to newlines

I recently had to tidy a large codebase and found many E501 follow simple pattern fixes, such as "go to line length, go back to break char, newline", etc.

Is there any appetite for a PR to include some basic unsafe fixing logic for E501? I created a poc and found that most are fairly simple to fix with line-based pattern matching and even just fixing a few cases could be useful. One or two more complex cases seemed more appropriate for token-based parsing which could be out of scope, though I saw somewhere talk of making E501 token aware.

The biggest downside seems to be the risk changing program logic, but the `--unsafe-fixes` option seems to be a good way to opt into this and accept some risk.

### Version

_No response_

---

_Label `question` added by @pwithams on 2025-07-15 06:49_

---

_Comment by @MichaReiser on 2025-07-15 07:10_

Hi

You can use `ruff format` which fixes the majority of `line-too-long` violations. Implementing an auto fix for E501 would have to do very similar work (besides that it would need to be rewritten to work at a logical line level when performing fixes) to the formatter and we don't want to maintain the same logic twice. 

That's why we don't plan on adding or supporting an autofix for E501. But thanks for expressing interested in contributing such a fix.

---

_Closed by @MichaReiser on 2025-07-15 07:10_

---

_Comment by @omarkb93 on 2025-09-20 20:37_

I understand the rationale given here, but there's no "unsafe-fixes" for the format version either, right? It still seems like there's a place for this or something similar. I have a large repo where my team is considering changing our line length requirement from 120 characters to 100 characters and it results in over 2000 E501 errors. Some basic unsafe fixes to automate the majority of this would be very welcome. 

---
