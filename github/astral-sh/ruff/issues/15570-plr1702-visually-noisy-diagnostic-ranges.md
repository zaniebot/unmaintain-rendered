---
number: 15570
title: "PLR1702: Visually noisy diagnostic ranges"
type: issue
state: open
author: beauxq
labels:
  - breaking
  - needs-design
  - diagnostics
assignees: []
created_at: 2025-01-18T15:10:54Z
updated_at: 2025-01-20T17:00:40Z
url: https://github.com/astral-sh/ruff/issues/15570
synced_at: 2026-01-07T13:12:16-06:00
---

# PLR1702: Visually noisy diagnostic ranges

---

_Issue opened by @beauxq on 2025-01-18 15:10_

If a function has nested blocks to violate PLR1702, almost the entire function is marked for it, including lines that are not violations.

Even lines that are only nested 2 or 3 deep are marked with squigglies.

Even when there are multiple lines in a block nested 9 deep, I think it would be enough to only mark the first line of that deeply nested block.

![Image](https://github.com/user-attachments/assets/8b03eee8-e446-4218-b73a-d68970e701f4)


---

_Comment by @beauxq on 2025-01-18 15:23_

PLR1702 is currently marking every line inside this function:
```python
def foo():
    while a:  # should not be marked - not a violation
        if b:  # should not be marked - not a violation
            for c in range(3):  # should not be marked - not a violation
                if d:  # should not be marked - not a violation
                    while e:  # should not be marked - not a violation
                        if f:  # should not be marked - not a violation
                            for g in z:  # not bad to mark this line, but not needed if only marking the max
                                print(p)  # this line should be marked
                                pass  # this line doesn't need to be marked, because the previous line is marked
        else:  # should not be marked - not a violation
            print(q)  # should not be marked - not a violation
```

---

_Label `diagnostics` added by @dylwil3 on 2025-01-18 15:52_

---

_Label `needs-mre` added by @dylwil3 on 2025-01-18 15:56_

---

_Comment by @dylwil3 on 2025-01-18 15:58_

That sounds annoying, I'm sorry! When I run this locally or in the playground, I only see a single diagnostic, though the whole range is highlighted. Similarly, when I use the VSCode extension, I see the entire block highlighted but the pop-up only displays a single diagnostic no matter where I hover.

Could you help me reproduce what you're seeing? What's your setup - how are you calling Ruff, and what version are you using?

[Playground link](https://play.ruff.rs/1207724a-cad2-45c5-a82e-a6ce79d0cb3b)

VSCode:

<img width="799" alt="Image" src="https://github.com/user-attachments/assets/3883185b-2a9f-4ce2-b5e5-8160eedbd819" />

Terminal:

```console
❯ cat tmp.py
def foo():
    while a:  # should not be marked - not a violation
        if b:  # should not be marked - not a violation
            for c in range(3):  # should not be marked - not a violation
                if d:  # should not be marked - not a violation
                    while e:  # should not be marked - not a violation
                        if f:  # should not be marked - not a violation
                            for g in z:  # not bad to mark this line, but not needed if only marking the max
                                print(p)  # this line should be marked
                                pass  # this line doesn't need to be marked, because the previous line is marked
        else:  # should not be marked - not a violation
            print(q)  # should not be marked - not a violation

❯ uvx ruff check tmp.py --isolated --select PLR --preview --no-cache
tmp.py:2:5: PLR1702 Too many nested blocks (7 > 5)
   |
 1 |   def foo():
 2 | /     while a:  # should not be marked - not a violation
 3 | |         if b:  # should not be marked - not a violation
 4 | |             for c in range(3):  # should not be marked - not a violation
 5 | |                 if d:  # should not be marked - not a violation
 6 | |                     while e:  # should not be marked - not a violation
 7 | |                         if f:  # should not be marked - not a violation
 8 | |                             for g in z:  # not bad to mark this line, but not needed if only marking the max
 9 | |                                 print(p)  # this line should be marked
10 | |                                 pass  # this line doesn't need to be marked, because the previous line is marked
11 | |         else:  # should not be marked - not a violation
12 | |             print(q)  # should not be marked - not a violation
   | |____________________^ PLR1702
   |

Found 1 error.
```

---

_Comment by @beauxq on 2025-01-18 16:04_

Here's an example with multiple diagnostics.
But I think the multiple diagnostics is not as big of a problem as everything being highlighted.
```python
def foo():
    while a:  # should not be marked - not a violation
        if b:  # should not be marked - not a violation
            for c in range(3):  # should not be marked - not a violation
                if d:  # should not be marked - not a violation
                    while e:  # should not be marked - not a violation
                        if f:  # should not be marked - not a violation
                            for g in z:  # not bad to mark this line, but not needed if only marking the max
                                print(p)  # this line should be marked
                                pass  # this line doesn't need to be marked, because the previous line is marked
                        else:
                            if h:
                                print(r)
        else:  # should not be marked - not a violation
            print(q)  # should not be marked - not a violation
```

---

_Comment by @beauxq on 2025-01-18 16:16_

In the original example of the first screenshot, there 250 lines all completely highlighted, making it difficult to see other diagnostics.
That's not helpful highlighting.

If there are 5 violations, highlighting 5 lines is enough.

---

_Label `needs-mre` removed by @dylwil3 on 2025-01-18 16:18_

---

_Comment by @dylwil3 on 2025-01-18 16:19_

Thank you, that's helpful!

I think there's a small design question about how to handle this situation, and we should be conscious of whether we are creating a breaking change by modifying where a suppression comment can go. But I agree that we can do better here.

---

_Label `needs-design` added by @dylwil3 on 2025-01-18 16:20_

---

_Referenced in [astral-sh/ruff#15578](../../astral-sh/ruff/pulls/15578.md) on 2025-01-19 00:19_

---

_Comment by @InSyncWithFoo on 2025-01-19 13:06_

I whipped up [a draft PR](https://github.com/astral-sh/ruff/pull/15578) for this. It doesn't quite work yet, but here's how I envision it:

```python
| def foo():
|     while a:                              # \
|         if b:                             # |
|             for c in range(3):            # | These should not be reported,
|                 if d:                     # | as they don't exceed the max depth.
|                     while e:              # |
|                         if f:             # /
# 
|                             for g in z:   # This statement is the first to exceed the limit.
# ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report range
|                                 print(p)  # Thus, it is reported but not any of its substatements.
|                                 pass      #
# 
|                             with y:       # The former statement was already reported.
|                                 print(x)  # Thus, reporting these is redundant.
|                             print(u)      #
# 
|         else:                             # Other blocks of an ancestor statement
|             print(q)                      # are also not reported.
```

```python
| def foo():
|     while a:                              # \
|         if b:                             # |
|             for c in range(3):            # | These should not be reported,
|                 if d:                     # | as they don't exceed the max depth.
|                     while e:              # |
|                         if f:             # /
# 
|                             if x == y:    # This statement is the first to exceed the limit.
# ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Report range
|                                 print(p)  # It is therefore reported.
# 
|                             elif y > x:   # This block belongs to the same statement,
|                                 print(p)  # and so it is not reported on its own.
```

Indentation seems to be the most sensible place to report an overnesting.


---

_Renamed from "PLR1702 spam" to "PLR1702: Visually noisy diagnostic ranges" by @MichaReiser on 2025-01-19 14:51_

---

_Label `breaking` added by @MichaReiser on 2025-01-19 14:51_

---

_Comment by @InSyncWithFoo on 2025-01-19 14:55_

A refined explanation can be found at [this test file](https://github.com/astral-sh/ruff/blob/da0aae8ac33ac88487e4649d5cac74aa95ed395a/crates/ruff_linter/resources/test/fixtures/pylint/too_many_nested_blocks_2.py).

I'm torn between this current approach and one that simply reports the first block that exceeds the limit:

```python
def foo():
	if a:
		if b:
			if c:
				if d:
					if e:
						if f:
							if g:             # PLR1702: Too many nested blocks (only n allowed)
								if h:
									print(i)
```

But that would require some recursion to find the max depth.


---

_Comment by @MichaReiser on 2025-01-20 08:44_

I'm torn on this.

Both too-many-branches and complex-structure report the entire function (and I expect so do other too-many-* rules). I also think that highlighting the function itself sort of makes sense, considering:

> Checks for functions or methods with too many nested blocks.

There's a Ruff helper somewhere (I wasn't able to find it just now) that only extracts the range of the function's name. This would reduce the highlighting range but doesn't point the user to the relevant code. 

That's where highlighting the first "violating" block would definitely be an improvement. But this is a breaking change (it's still a preview rule so it might be fine). 

I'm not convinced that highlighting the indent is the right solution. It incorrectly suggests that something is wrong with the indent, which there isn't. 

Has anyone looked into what the upstream rule odes or what other tools do in this situation?

Side note: The rule should probably be updated to support `match` statements

---

_Comment by @dhruvmanila on 2025-01-20 09:14_

(This is a scenario where related diagnostics might help where the main diagnostic will highlight the function name but related diagnostics will also highlight the block which makes Ruff consider this as exceeding the limit.)

---

_Comment by @InSyncWithFoo on 2025-01-20 14:10_

> I'm not convinced that highlighting the indent is the right solution. It incorrectly suggests that something is wrong with the indent, which there isn't.

I <em>do</em> think deep indentation is the problem in this case. Not character-wise, no, but semantically.

---

_Comment by @MichaReiser on 2025-01-20 14:25_

> I do think deep indentation is the problem in this case. Not character-wise, no, but semantically.

Ideally the same approach could be taken for all `too-*` rules and most of them aren't about the indent. 

---

_Comment by @InSyncWithFoo on 2025-01-20 14:50_

> Ideally the same approach could be taken for all `too-*` rules and most of them aren't about the indent.

I take it that you are suggesting other rules should be updated as well?

If so, here's my take: The reported range is that of the first that exceeds the limit.

* [`PLR0904` (`too-many-public-methods`)](https://docs.astral.sh/ruff/rules/too-many-public-methods/): The $(n + 1)$-th public method is reported.
* [`PLR0911` (`too-many-return-statements`)](https://docs.astral.sh/ruff/rules/too-many-return-statements/): The $(n + 1)$-th `return` statement is reported.
* [`PLR0912` (`too-many-branches`)](https://docs.astral.sh/ruff/rules/too-many-branches/): The $(n + 1)$-th branch is reported.
* [`PLR0913` (`too-many-arguments`)](https://docs.astral.sh/ruff/rules/too-many-arguments/): The $(n + 1)$-th <em>parameter</em> is reported.
* [`PLR0914` (`too-many-locals`)](https://docs.astral.sh/ruff/rules/too-many-locals/): The $(n + 1)$-th local variable is reported.
* [`PLR0915` (`too-many-statements`)](https://docs.astral.sh/ruff/rules/too-many-statements/): The $(n + 1)$-th statement is reported.
* [`PLR0916` (`too-many-boolean-expressions`)](https://docs.astral.sh/ruff/rules/too-many-boolean-expressions/): The $(n + 1)$-th expression is reported.
* [`PLR0917` (`too-many-positional-arguments`)](https://docs.astral.sh/ruff/rules/too-many-positional-arguments/): The $(n + 1)$-th positional <em>parameter</em> is reported.

This aligns with, say, [`E501`](https://docs.astral.sh/ruff/rules/line-too-long/), which reports all characters from the $(n + 1)$-th onward. The collective range for all exceeders would be just as bad as the name of the function/class, if not worse, so the first of them would perhaps be representative enough. 

(Off-topic: `PLR0913` and `PLR0917` will benefit from a rename and documentation update. They are currently not very distinct.)

---

_Comment by @InSyncWithFoo on 2025-01-20 17:00_

Reporting the keywords is a solid choice too:

```python
if a:
	...
	if b:
#   ^^ PLR1702
		...
	for d in e:
#   ^^^ PLR1702
		...
	while f < g:
#   ^^^^^ PLR1702
		...
	match h:
#   ^^^^^ PLR1702
		case I(j=k):
			...
```


---

_Referenced in [astral-sh/ruff#14900](../../astral-sh/ruff/issues/14900.md) on 2025-11-22 18:11_

---
