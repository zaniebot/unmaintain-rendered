```yaml
number: 1693
title: Bad error message when python query encounters python 2
type: issue
state: closed
author: fervand1
labels:
  - bug
  - error messages
  - windows
assignees: []
created_at: 2024-02-19T14:17:54Z
updated_at: 2024-02-23T10:55:40Z
url: https://github.com/astral-sh/uv/issues/1693
synced_at: 2026-01-12T15:58:31Z
```

# Bad error message when python query encounters python 2

---

_@fervand1_

Steps to reproduce:
* Install uv using command - irm https://astral.sh/uv/install.ps1 | iex
* Run command - uv venv. You will see error shown in the screenshot
![image](https://github.com/astral-sh/uv/assets/44014460/56b00d7f-89d0-4fad-83c8-97c333967b34)

* The current uv platform - windows 10
* uv version - 0.1.5

Note: In the windows path I only have path related to python 2. I also have python 3.10 and 3.12 installed but they can be ran using "py" command and are not added to the path. So something like `py -3.10 -m venv venv` creates python 3.10 env

Since 'py' command is accepted norm on windows , I would expect the tool to run with the same and not consider python 2.7 path as a default python path.
Also preferably during install it should prompt what python version he would expect to run uv from maybe?

---

_Comment by @sbrugman on 2024-02-19 14:24_

For Python2.7 this is to be expected, as `base_prefix` was introduced in 3.3 [1]

What does `py -3.10 -m uv venv` output?

[1]: https://docs.python.org/3/library/sys.html#sys.base_prefix

---

_Comment by @fervand1 on 2024-02-19 14:30_

@sbrugman I did not install the tool for python 3.10, so currently it raises following. I installed it globally with the powershell script mentioned in the pypi docs
![image](https://github.com/astral-sh/uv/assets/44014460/51bebf65-9e60-4dfc-b2ae-51ba38f0927c)


---

_Comment by @fervand1 on 2024-02-19 14:35_

@sbrugman So I installed uv for python 3.10 and tried to create venv as you mentioned. The error is the same. It is taking Python 2 path into consideration.
![image](https://github.com/astral-sh/uv/assets/44014460/8ad8bb9a-68ac-4364-be51-52fd90bc145f)


---

_Comment by @MichaReiser on 2024-02-19 15:00_

Sorry you're experiencing this. I'm currently looking into improving the python discovery on Windows see https://github.com/astral-sh/uv/issues/1310. I hope that this will fix your issue too.

---

_Label `windows` added by @MichaReiser on 2024-02-19 15:01_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-19 15:01_

---

_Comment by @fervand1 on 2024-02-19 15:06_

Thanks @MichaReiser for the quick response

---

_Comment by @fervand1 on 2024-02-19 15:19_

@MichaReiser I created #1698 just as observation. It is possibly a side effect of the issue you are working on already but reported just in case it can help to improve the solution. thanks

---

_Comment by @MichaReiser on 2024-02-22 16:46_

@fervand1 can you give it a try once https://github.com/astral-sh/uv/pull/1711/ is released? 

I suspect that this might solve your problem because it changes the precedence to first query `py` before searching PATH in accordance with `virtualenv`. 

The question that remains is if we should be more lenient when it comes to failures querying the python information and instead continue searching for other "functioning" installation. @konstin what do you think?



---

_Comment by @konstin on 2024-02-22 19:33_

We don't support python 2; We should check for python major != 3 and raise an error in the script we pass to python 

---

_Label `bug` added by @konstin on 2024-02-22 19:33_

---

_Renamed from "Error running uv venv command on windows 10" to "Bad error message when python query encounters python 2" by @konstin on 2024-02-22 19:33_

---

_Label `error messages` added by @konstin on 2024-02-22 19:33_

---

_Comment by @konstin on 2024-02-22 19:35_

> The question that remains is if we should be more lenient when it comes to failures querying the python information and instead continue searching for other "functioning" installation. @konstin what do you think?

We should special case python 2 (e.g. returns a specific json message), but otherwise it think we should at least for now show errors. If we encounter many unrelated errors, we can be more lenient later

---

_Comment by @fervand1 on 2024-02-23 09:18_

@MichaReiser I installed new version of uv via irm and it seems to be resolved.
![image](https://github.com/astral-sh/uv/assets/44014460/e0ec2083-f828-4afe-b489-05446c87e35d)

Also I noticed we can pass `--python` param to specify which python we need. Passing 3.10 produced following
![image](https://github.com/astral-sh/uv/assets/44014460/bab9e77c-7b8c-477b-82bc-e482d0f6d086)



---

_Closed by @konstin on 2024-02-23 10:55_

---
