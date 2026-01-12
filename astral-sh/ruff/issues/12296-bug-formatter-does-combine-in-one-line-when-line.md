```yaml
number: 12296
title: "[Bug] [Formatter] Does combine in one line when line-length is not exceeded"
type: issue
state: closed
author: LoicRiegel
labels:
  - question
assignees: []
created_at: 2024-07-12T09:15:30Z
updated_at: 2024-07-12T12:01:49Z
url: https://github.com/astral-sh/ruff/issues/12296
synced_at: 2026-01-12T15:54:51Z
```

# [Bug] [Formatter] Does combine in one line when line-length is not exceeded

---

_@LoicRiegel_

Hi, I find this behavior strange in the formatter, and I think this should be fixed.

Tested on **ruff 0.5.1**.

### Steps to reproduce
1. Activate any environment where ruff is installed
2. Create a new python file and paste this content into it:
  ```py
  def test_1():
      return do_stuff(100, "some quite long string")
  
  
  def test_2():
      return do_stuff(
          100,
          "some quite long string",
      )
  
  
  def test_3():
      raise SomeCustomError(100, "some quite long string")
  
  
  def test_4():
      raise SomeCustomError(
          100,
          "some quite long string",
      )
  
  ```
3. run `ruff format test_formatter.py --no-cache`

### Observed behavior

File is valid for the ruff formatter.
The command output is `1 file left unchanged`

### Expected behavior

* `test_2` function is reformatted so it is identical to`test_1`
* `test_4` function is reformatted so it is identical to `test_3`

### Why I think this should be fixed

A line-spitting occurs if the code exceeds the line-length. So it seems logical to me that a rule for the opposite behavior would be in place. Since the line-length is not exceeded, the calls should not be split into separate lines.


---

_Label `question` added by @MichaReiser on 2024-07-12 09:23_

---

_Comment by @MichaReiser on 2024-07-12 09:24_

The formatter, by default, keeps lines separated if the last item in a sequence has a trailing comma. You can enable the [`skip-magic-trailing-comma` ](https://docs.astral.sh/ruff/settings/#format_skip-magic-trailing-comma) to disable this behavior (in which case `test_2` and `test_4` should be collapsed)

---

_Comment by @LoicRiegel on 2024-07-12 11:49_

Thank you for your answer @MichaReiser, maybe this was all I needed. But shouldn't ``true`` be the default?

---

_Comment by @MichaReiser on 2024-07-12 11:52_

That's great to hear. 

People like to have some control over when lines break; that's why `false` is the default. It also ensures that Black and Ruff have the same default. 

---

_Closed by @MichaReiser on 2024-07-12 11:52_

---

_Comment by @LoicRiegel on 2024-07-12 11:56_

Okay, well, I tested and applied that in my entire project, and actually I'm not sure I like it better... This for instance 
![image](https://github.com/user-attachments/assets/45dd82fc-6875-4f72-bbe0-1a7c80cf6442)

Thanks for your help anyway, at least now I know that it this configuration exists :)

---

_Comment by @MichaReiser on 2024-07-12 12:01_

I don't think I understand "this for instance". I would need a screenshot with some more context.

> Thanks for your help anyway, at least now I know that it this configuration exists :)

You're welcome. Happy to help

---
