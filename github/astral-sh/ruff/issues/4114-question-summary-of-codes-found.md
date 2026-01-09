---
number: 4114
title: "Question: Summary of codes found"
type: issue
state: closed
author: leon1995
labels:
  - question
assignees: []
created_at: 2023-04-26T07:47:29Z
updated_at: 2023-04-27T11:41:32Z
url: https://github.com/astral-sh/ruff/issues/4114
synced_at: 2026-01-07T13:12:14-06:00
---

# Question: Summary of codes found

---

_Issue opened by @leon1995 on 2023-04-26 07:47_

is there a way to just get the list of codes that were found without duplication?
e.g. if there are multiple errors found of the same code, the code will only be contained in the summary one time. Such that you get an overview of what codes are found.
```python
print('hello world 1')
print('hello world 2')
```
will print out the codes `T201` and `Q000` 2 times, but what I want is something like
```
Found the following errors:
- T201: 2 times
- Q000: 2 times
```
I have a project where ruff found more than 12000 errors and I want to discuss whether the codes are relevant for it or not. Therefore, a list of found codes with the frequency would be nice

---

_Comment by @charliermarsh on 2023-04-26 15:06_

You should be able to run with the `--statistics` flag to get something very similar! Let me know if that doesn't solve your problem :)

---

_Closed by @charliermarsh on 2023-04-26 15:06_

---

_Label `question` added by @charliermarsh on 2023-04-26 15:06_

---

_Comment by @leon1995 on 2023-04-27 10:14_

yes perfect. maybe you add it to [Usage)[https://beta.ruff.rs/docs/usage/] to increase the awareness of it :)

---

_Comment by @calumy on 2023-04-27 11:36_

`--statistics` is currently covered in the [CLI ](https://beta.ruff.rs/docs/configuration/#command-line-interface) section of [Configuring Ruff](https://beta.ruff.rs/docs/configuration/).

---

_Comment by @leon1995 on 2023-04-27 11:41_

ok I completely overlooked it. thanks!

---
