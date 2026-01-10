---
number: 2233
title: “uv pip install -P PACKAGE“ error prompted
type: issue
state: closed
author: zzm-note
labels:
  - question
assignees: []
created_at: 2024-03-06T08:10:54Z
updated_at: 2024-03-07T21:35:06Z
url: https://github.com/astral-sh/uv/issues/2233
synced_at: 2026-01-10T01:23:14Z
---

# “uv pip install -P PACKAGE“ error prompted

---

_Issue opened by @zzm-note on 2024-03-06 08:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
`uv pip install -P   Appium-Python-Client`
![image](https://github.com/astral-sh/uv/assets/25860330/98d9b407-a0c1-4b38-a812-af4a108cd526)

uv version == uv 0.1.15 (a8ac7b1eb 2024-03-05)

---

_Comment by @charliermarsh on 2024-03-06 15:16_

What is the intended behavior of `-P`? I don't see that flag in `pip`. You can omit it if you intended to run `uv pip install Appium-Python-Client`.

---

_Label `question` added by @charliermarsh on 2024-03-06 15:16_

---

_Comment by @zzm-note on 2024-03-07 01:51_

> What is the intended behavior of `-P`? I don't see that flag in `pip`. You can omit it if you intended to run `uv pip install Appium-Python-Client`. `-P` 的预期行为是什么？我在1#里没有看到那面旗。如果您打算运行 `uv pip install Appium-Python-Client` ，则可以省略它。

```
  -P, --upgrade-package <UPGRADE_PACKAGE>
          Allow upgrade of a specific package
```
Sometimes I only want to upgrade a single library. If I use `uv pip install Appium-Python-Client`, the associated dependencies will also be upgraded.

---

_Comment by @charliermarsh on 2024-03-07 01:56_

This flag exists on `uv pip compile`, maybe that's what you're looking for?

---

_Comment by @zzm-note on 2024-03-07 02:02_

> This flag exists on `uv pip compile`, maybe that's what you're looking for?

Maybe not, `uv pip compile` requires a <SRC_FILE>, but I just want to upgrade a certain library separately

And there is a `-P` flag in `uv pip install -h`

---

_Comment by @zzm-note on 2024-03-07 02:18_

For example
 Current virtual environment:
```
Package      Version
------------ -------
click        7.1.2
colorama     0.4.6
flask        2.0.0
itsdangerous 2.1.2
jinja2       3.1.3
markupsafe   2.1.5
werkzeug     3.0.1
```
I want to upgrade `flask` to the latest, use `uv pip install flask -U`
```
Installed 5 packages in 60ms
 + blinker==1.7.0
 - click==7.1.2
 + click==8.1.7
 - flask==2.0.0
 + flask==3.0.2
 + importlib-metadata==7.0.1
 + zipp==3.17.0
```
But I only want to upgrade `flask`, and I don’t want to upgrade `click` to the latest


---

_Comment by @zanieb on 2024-03-07 17:38_

You must provide `uv pip install flask -P flask` — this matches `pip`'s syntax which requires you to both specify the packages to install and the packages you want to upgrade.

---

_Comment by @charliermarsh on 2024-03-07 21:35_

Ahh right -- `uv pip install flask -P flask` should work!

---

_Closed by @charliermarsh on 2024-03-07 21:35_

---
