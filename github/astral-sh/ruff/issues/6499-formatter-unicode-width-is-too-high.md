---
number: 6499
title: "Formatter: Unicode width is too high"
type: issue
state: closed
author: konstin
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-08-11T11:45:22Z
updated_at: 2023-08-22T15:01:10Z
url: https://github.com/astral-sh/ruff/issues/6499
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Unicode width is too high

---

_Issue opened by @konstin on 2023-08-11 11:45_

The following snippet fits visually in pycharm and is kept in this layout by black
```python
function_call(
    "aaaaaaaaaaaaaaaaaaaa", "我隻氣墊船裝滿晒鱔.txt", "मेरी मँडराने वाली नाव सर्पमीनों से भरी ह"
)
```
ruff formats it as
```python
function_call(
    "aaaaaaaaaaaaaaaaaaaa",
    "我隻氣墊船裝滿晒鱔.txt",
    "मेरी मँडराने वाली नाव सर्पमीनों से भरी ह",
)
```
We likely compute the unicode width of the string too high so that we assume the line is too long

---

_Label `bug` added by @konstin on 2023-08-11 11:45_

---

_Label `formatter` added by @konstin on 2023-08-11 11:45_

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-08-11 11:45_

---

_Comment by @MichaReiser on 2023-08-11 12:09_

How did you measure the width in Pycharm? I understand that Pycharms cursor position (row:column) is character based and not width based.

---

_Comment by @konstin on 2023-08-11 13:47_

Really just visually:

![Untitled](https://github.com/astral-sh/ruff/assets/6826232/9fc1626f-b581-400f-9d03-83a87c4052e9)

The chinese characters line up with two latin characters, the hindi doesn't.

I don't know by which rules which tool measures, but we should try to match what black does

---

_Comment by @MichaReiser on 2023-08-11 14:03_

Black migrates to use unicode-width in preview style, but only for strings (not identifiers). 

Edit: But we should look into this. 



---

_Comment by @konstin on 2023-08-11 15:25_

black does also break in preview mode! I guess it makes more sense by the unicode text width algorithm but it's also odd because it's different from the actual text rendering

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-17 09:43_

---

_Label `bug` removed by @konstin on 2023-08-21 05:58_

---

_Label `needs-decision` added by @konstin on 2023-08-21 05:58_

---

_Assigned to @konstin by @MichaReiser on 2023-08-22 14:44_

---

_Assigned to @MichaReiser by @konstin on 2023-08-22 14:45_

---

_Unassigned @konstin by @konstin on 2023-08-22 14:45_

---

_Comment by @MichaReiser on 2023-08-22 14:52_

@konstin and I talked about this and we must admit, we're confused. I will close this issue because I believe we're doing the right thing. Please open a new issue and link this issue if you're more familiar with CJK characters and/or have reasons to believe that the implementation is incorrect. We'll hopefully be able to figure out the correct behavior together. Reasons for closing:

* According to @konstin. Our implementation matches black preview style, which is good
* The [Unicode spec recommends](http://www.unicode.org/reports/tr11/#Recommendations) to tread the ambiguous characters as half-width when displaying but the context cannot be inferred (e.g. by the used font). 
  > Ambiguous characters behave like wide or narrow characters depending on the context (language tag, script identification, associated font, source of data, or explicit markup; all can provide the context). If the context cannot be established reliably, they should be treated as narrow characters by default.
* The formatter uses [`width`](https://docs.rs/unicode-width/latest/unicode_width/trait.UnicodeWidthChar.html#tymethod.width) which treads ambiguous characters as half-width. 


I also went ahead and pasted the example into my IDE and it seems it exceeds the column width for my configuration (the `xx...` is exactly 88 characters wide)
![image](https://github.com/astral-sh/ruff/assets/1203881/9ef32f89-68a5-4cbc-bfc9-e68cb8a7cdd4)

---

_Closed by @MichaReiser on 2023-08-22 14:52_

---
