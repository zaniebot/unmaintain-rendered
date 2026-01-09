---
number: 3382
title: "ruff `>=0.0.252` as pycharm File Watcher"
type: issue
state: closed
author: FieryDruid
labels:
  - question
assignees: []
created_at: 2023-03-07T11:29:24Z
updated_at: 2023-03-10T02:56:42Z
url: https://github.com/astral-sh/ruff/issues/3382
synced_at: 2026-01-07T13:12:14-06:00
---

# ruff `>=0.0.252` as pycharm File Watcher

---

_Issue opened by @FieryDruid on 2023-03-07 11:29_

I tried to set up ruff launch in File Watcher. This worked fine on ruff versions prior to `0.0.252` (found out by iterating through versions), but in versions `>=0.0.252` when running File Watcher, my IDE highlights errors momentarily instead of permanently displaying them. On versions `<=0.0.251`, everything works correctly, when a file is changed, the File Watcher is launched, the IDE highlights errors and the highlighting does not disappear until the error is fixed

Maybe someone can tell me what could be the problem? I did not see any "breaking" changes in the changelogs of versions `0.0.252 - 0.0.254`, I noticed similar behavior when updating from `0.0.247` to the current `0.0.254`, the configs did not change, as did the File Watcher settings. Perhaps this is some kind of my local problem, but for some reason I observe different behavior on the old and new versions. Due to this, I thought it might have something to do with changes in versions `>=0.0.252`.

I check on the simplest example - the type of quotes used in strings (in pyproject.toml `inline-quotes` set as `single`):
```python
test = "hello world"
```

* **File Watcher settings**:
  
  ![image](https://user-images.githubusercontent.com/36041629/223405884-bf10e477-5dee-485e-96ad-6addaf4817f1.png)
  * `File type` - `python`
  * `Scope` - `Project Files`
  * `Program` - `$PyInterpreterDirectory$/ruff`
  * `Arguments` - `$FilePath$`
  * `Working directory` - `$ProjectFileDir$`
 
* **ruff version** - `0.0.254`
* **ruff settings**:
  ```toml
  [tool.ruff]
  line-length = 120
  select = ["ALL"]
  target-version = "py37"

  [tool.ruff.flake8-quotes]
  docstring-quotes = "double"
  inline-quotes = "single"
  multiline-quotes = "double"
  ```

Short videos with showing problem. There are no errors when launching in the console (I tried to enable displaying the console when starting File Watcher), all tested versions (`0.0.247`, `0.0.251-0.0.254`) give the same list of remarks

On versions `>=0.0.252`:

https://user-images.githubusercontent.com/36041629/223407218-7938ff1a-9763-46f3-b1cd-a9977d982055.mp4

---

On versions `<=0.0.251`

https://user-images.githubusercontent.com/36041629/223406994-3d499d70-13fd-4686-bf4d-b8d82702fb4b.mp4


---

_Comment by @charliermarsh on 2023-03-08 20:49_

Thank you for the clear issue. It's not immediately obvious to me what's going on, but first step is I need to find a few mins to try and repro on my end.

---

_Comment by @charliermarsh on 2023-03-08 22:00_

I see the same behavior.

---

_Comment by @charliermarsh on 2023-03-08 22:04_

This was somehow caused by #3104.

---

_Comment by @charliermarsh on 2023-03-08 22:18_

I don't quite understand what's going on, but if you change arguments to `$FilePath$ --no-cache` -- at least on my end, that seems to fix it. (Caching here is unnecessary anyways, since you're running on-change, though it's still a "bug" of course.)

---

_Label `question` added by @charliermarsh on 2023-03-08 22:18_

---

_Comment by @charliermarsh on 2023-03-08 22:18_

Do you mind trying that?

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-08 22:21_

---

_Comment by @FieryDruid on 2023-03-09 06:28_

Yes, works correctly with `--no-cache`

---

_Comment by @charliermarsh on 2023-03-10 02:56_

I'm going to mark this as fixed due to a viable workaround. If we start seeing this in other integrations, I'll re-open and try to understand what's going on.

---

_Closed by @charliermarsh on 2023-03-10 02:56_

---
