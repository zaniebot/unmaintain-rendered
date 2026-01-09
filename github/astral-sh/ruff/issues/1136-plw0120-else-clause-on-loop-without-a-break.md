---
number: 1136
title: PLW0120 (else clause on loop without a break statement) incorrectly flagged
type: issue
state: closed
author: eddyg
labels:
  - bug
assignees: []
created_at: 2022-12-08T02:12:58Z
updated_at: 2022-12-08T17:07:28Z
url: https://github.com/astral-sh/ruff/issues/1136
synced_at: 2026-01-07T13:12:14-06:00
---

# PLW0120 (else clause on loop without a break statement) incorrectly flagged

---

_Issue opened by @eddyg on 2022-12-08 02:12_

```shell
❯ ruff --version
ruff 0.0.169
```

Trying to understand why PLW0120 _("Else clause on loop without a break statement, remove the else and de-indent all the code inside it")_ is being raised.

Here's a simple example which causes PLW0120, even though it _does_ have a `break`:

```python
for page in range(1, 10):
    if page < 5:
        print(page)
    else:
        print("error")
        break
else:
    print("OK")
```

When the `break` is moved inside the `if`, PLW0120 goes away.

---

_Comment by @eddyg on 2022-12-08 02:30_

Also noticed it improperly flags code like this (which doesn't have any `break`s) as well:

```python
def demo(end):
    for page in range(1, end):
        if page < 6:
            print(page)
        else:
            print("OK")
            return
    else:
        print("error")
```


---

_Comment by @charliermarsh on 2022-12-08 03:59_

It's _probably_ just missing the nested detection somehow.

---

_Label `bug` added by @charliermarsh on 2022-12-08 03:59_

---

_Comment by @charliermarsh on 2022-12-08 03:59_

(But, definitely looks like a bug.)

---

_Comment by @charliermarsh on 2022-12-08 15:20_

(Will take a look at this today.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-08 16:38_

---

_Referenced in [astral-sh/ruff#1143](../../astral-sh/ruff/pulls/1143.md) on 2022-12-08 16:53_

---

_Closed by @charliermarsh on 2022-12-08 16:53_

---

_Comment by @charliermarsh on 2022-12-08 16:54_

Fixed in #1143.

I think the second example you included is actually correct (or, at least, consistent with Pylint), since if the loop exits early with a `return`, you'll never hit the `else` block code anyway -- so, IIUC, it's identical to removing the `else`.


---

_Comment by @eddyg on 2022-12-08 17:07_

Thanks for being so responsive! 🎉 

And thanks for pointing out my second example is bogus... I was playing around with the minimal example I came up with to illustrate the issue and didn't think to verify with Pylint. 😬 

---
