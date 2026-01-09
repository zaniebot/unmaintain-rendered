---
number: 5315
title: Missing files after uv pip install
type: issue
state: closed
author: rnarma321
labels:
  - external
assignees: []
created_at: 2024-07-22T21:28:34Z
updated_at: 2024-07-24T01:16:47Z
url: https://github.com/astral-sh/uv/issues/5315
synced_at: 2026-01-07T13:12:17-06:00
---

# Missing files after uv pip install

---

_Issue opened by @rnarma321 on 2024-07-22 21:28_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Command: ```uv pip install kivymd==1.1.1```
This package should have .kv files inside of it, but when i use uv pip install vs pip install they are missing.
There are no issues popped up during the install, even in verbose mode

The tar.gz is pulls from has these files, I checked manually, Im not exactly sure what is causing this, but I tried across two version 0.2.27 and 0.2.0, along with two different OSs with mac and debian 12 bookworm and got the same results with every combination.

``` DEBUG Adding direct dependency: kivymd==1.1.1
DEBUG Found fresh response for: https://pypi.org/simple/kivymd/
DEBUG Searching for a compatible version of kivymd (==1.1.1)
DEBUG Selecting: kivymd==1.1.1 (kivymd-1.1.1.tar.gz)
DEBUG Trying to lock if free: /Users/roshan/Library/Caches/uv/built-wheels-v3/pypi/kivymd/1.1.1/.lock
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/35/55/d979644cdc9070a9611ab023db986aa0d540fc8f5fdd2c0f49eaef558751/kivymd-1.1.1.tar.gz
DEBUG Using cached metadata for: kivymd==1.1.1
DEBUG Adding transitive dependency for kivymd==1.1.1: kivy>=2.0.0
DEBUG Adding transitive dependency for kivymd==1.1.1: pillow* ````



---

_Comment by @charliermarsh on 2024-07-22 22:56_

Where are those files supposed to be exactly?

---

_Comment by @rnarma321 on 2024-07-22 23:16_

an example can be found under .venv/lib/python3.10/site-packages/kivymd/uix/button/button.kv. This file will exist under pip but never showed up with uv pip

---

_Comment by @zanieb on 2024-07-22 23:46_

For reference, this is included in the `package_data`: https://github.com/kivymd/KivyMD/blob/b5bc79fb71312868be86580b7b32f676926ceb80/setup.py#L102

---

_Comment by @rnarma321 on 2024-07-23 00:32_

So actually that is interesting, it doesn't have the .pot files or the .po files. But it seems to have the .ttf files and the .pngs. Maybe something about the pathing it generates from the "glob_paths" function it calls breaks with uv but not with pip

---

_Comment by @charliermarsh on 2024-07-23 02:57_

Do you see any more consistency when you run `pip install --use-pep517`, and compare that to uv?

---

_Comment by @rnarma321 on 2024-07-23 03:23_

Nope, the pip install using pep517 flag still gives the .kv files, unlike uv which is missing it

---

_Comment by @charliermarsh on 2024-07-24 00:57_

Ok, I figured this out. Their `setup.py` assumes that there is no directory named `kivymd` in the path apart from the one at the top-level. But in our cache, we nest the build under a directory called `kivymd`

E.g., here's one path in the cache: `Users/crmarsh/workspace/uv/foo/built-wheels-v3/pypi/kivymd/1.1.1/_kyEDLdSRqebDVYQM_lQA/kivymd-1.1.1.tar.gz/kivymd/uix/textfield/textfield.kv`

So it's truncating at the first `kivymd`.

I consider this a bug in the package itself, it's effectively not relocatable. I think they need to change `out_files.append(filepath.split(f"kivymd{os.sep}")[1])` to `out_files.append(filepath.split(f"kivymd{os.sep}")[-1])` in the `setup.py`.


---

_Comment by @charliermarsh on 2024-07-24 01:16_

I created an issue here: https://github.com/kivymd/KivyMD/issues/1722

---

_Closed by @charliermarsh on 2024-07-24 01:16_

---

_Label `upstream` added by @charliermarsh on 2024-07-24 01:16_

---
