---
number: 3576
title: Mismatch with flake8 
type: issue
state: closed
author: Mofef
labels: []
assignees: []
created_at: 2023-03-17T13:02:04Z
updated_at: 2023-03-18T09:07:48Z
url: https://github.com/astral-sh/ruff/issues/3576
synced_at: 2026-01-10T01:22:42Z
---

# Mismatch with flake8 

---

_Issue opened by @Mofef on 2023-03-17 13:02_

I never really spend any thoughts on linting, I rather accepted that it was there, only since Charlies webinar with jetbrains I became interested and since I suggested to give ruff a try in our projects. Just as a disclaimer, most likely i'm just making a stupid mistake....

With this aside, what is the issue? As far as I understand flake8 and ruff should be pretty much consistent, but we found this:
```
(venv) moritz@VirtualBox:~/PycharmProjects/b.series$ flake8 bseries/
bseries/.....py:57:21: E225 missing whitespace around operator
bseries/.....py:170:41: E231 missing whitespace after ':'
bseries/.....py:7:1: E302 expected 2 blank lines, found 1
bseries/.....py:19:22: E127 continuation line over-indented for visual indent
(venv) moritz@VirtualBox:~/PycharmProjects/b.series$ ruff bseries/
(venv) moritz@VirtualBox:~/PycharmProjects/b.series$ 
```

are those just not implemented yet? 

here is the snippet regarding the first flake8 error: (last line)
```python
    def __getitem__(self, item):
        if not isinstance(item, slice):
            if isinstance(item, str):
                item=[item] 
```

```
(venv) moritz@VirtualBox:~/PycharmProjects/b.series$ ruff --version
ruff 0.0.246
```

pyproject.toml:
```toml
[tool.ruff]
select = ["E", "F", "I", "W"]
line-length = 120
```

setup.cfg
```ini
[flake8]
max-line-length = 120
extend-ignore = E203
```


---

_Comment by @JonathanPlasse on 2023-03-17 13:36_

These checks from flake8 are not yet available in Ruff.
It was not implemented as Black can fix these errors automatically.

---

_Comment by @charliermarsh on 2023-03-17 14:44_

Yup that's right! We haven't finished implementing those "stylistic" rules from Flake8, but we've made a bunch of progress on them.

If you try with a snippet like:

```py
import os

def f():
  x = 1
```

You should see unused import and unused variable errors in both Flake8 and Ruff.

Thanks for giving Ruff a try :)

---

_Closed by @charliermarsh on 2023-03-17 14:44_

---

_Comment by @Mofef on 2023-03-17 21:25_

Awesome, thx for the heads up! Is there an easy way to check if rules are / are not implemented yet?

---

_Comment by @charliermarsh on 2023-03-17 21:33_

We're missing 13 rules (https://github.com/charliermarsh/ruff/issues/2402), however some of the rules that are checked off in that issue are still behind a flag and not yet available in the release (coming soon).

So the place to look would be the [`E`](https://beta.ruff.rs/docs/rules/#error-e) list in the docs, which you can compare to [pycodestyle's own list](https://pycodestyle.pycqa.org/en/latest/intro.html#error-codes). Sorry, I know it's not "easy" to compare those two sources!

---

_Comment by @Mofef on 2023-03-18 09:07_

Easy enough! 🚀  Thank you so much!

---
