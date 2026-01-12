```yaml
number: 149
title: F821 triggers when referencing a deleted (in an if) variable in an else/elif
type: issue
state: closed
author: Jackenmen
labels:
  - bug
assignees: []
created_at: 2022-09-11T03:20:32Z
updated_at: 2022-09-11T14:28:08Z
url: https://github.com/astral-sh/ruff/issues/149
synced_at: 2026-01-12T15:54:40Z
```

# F821 triggers when referencing a deleted (in an if) variable in an else/elif

---

_@Jackenmen_

Example reproduction code:
```py
def func(seq, arg):
    if seq:
        del arg
    elif arg:  # false-positive F821
        print(arg)  # false-positive F821
    else:
        print(arg)  # false-positive F821

    print(arg)  # correctly reported F821
```

---

*Story time*

While running into this false-positive, I actually found a different issue so it's net-positive :)
```py
def func(seq, show_this_once):
    if seq:
        for elem in seq:
            print(elem, show_this_once)
            del show_this_once
    elif show_this_once:
        print(show_this_once)
```
The `del` in the `for` loop here was problematic because on second iteration the variable no loner exists. Somehow we never tested it with a sequence longer than one so this went undetected. Note that ruff didn't report this directly, I just found it because of the `elif` branch I had :) I suppose there could be a rule for reporting this kind of possibly unbound variable issue but it would probably be prone to false positives so I'm not really feature requesting that :smile: 

---

_Comment by @Jackenmen on 2022-09-11 03:34_

Would be great if early returns were supported as well:
```py
def func(remove, arg1, arg2, arg3):
    if remove & 1:
        del arg1
        return

    print(arg1)

    if remove & 2:
        del arg2
        raise RuntimeError("!!!")

    print(arg2)

    for _ in range(2):
        if remove & 4:
            del arg3
            continue
        print(arg3)
```
as in, all `print`s should be considered valid.

---

_Label `bug` added by @charliermarsh on 2022-09-11 13:43_

---

_Closed by @charliermarsh on 2022-09-11 14:28_

---
