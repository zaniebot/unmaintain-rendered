```yaml
number: 2898
title: "RET507: Same false positive as flake8-return in case of `elif` + `continue` + `else`"
type: issue
state: closed
author: aberres
labels:
  - bug
assignees: []
created_at: 2023-02-14T18:27:54Z
updated_at: 2023-02-14T20:36:51Z
url: https://github.com/astral-sh/ruff/issues/2898
synced_at: 2026-01-12T15:54:43Z
```

# RET507: Same false positive as flake8-return in case of `elif` + `continue` + `else`

---

_@aberres_

The original issue can be found here: https://github.com/afonasev/flake8-return/issues/127

This one is a shameless copy. 

### Description
 
Invalid error reported for **R507** if an `elif` with `continue` is followed by an `else`. The `else` must not be removed in this case.

### What I Did

We have some `if...elif...` clause like this:

```python
def main():
    for a in range(10):                     # | 0 div 4
        if a % 4 == 0:                      # | 1 div nothing
            print(a, 'div', 4)              # | 2 div 2
        elif a % 3 == 0:                    # | 3 div 3
            print(a, 'div', 3)              # | 4 div 4
            continue                        # | 5 div nothing
        elif a % 2 == 0:                    # | 6 div 3
            print(a, 'div', 2)              # | 7 div nothing
        else:                               # | 8 div 4
            print(a, 'div', 'nothing')      # | 9 div 3
    return 0



if __name__ == '__main__':
    main()
```

For this case we are getting this error:

```shell
myfile.py:5:9:  RET507 Unnecessary `elif` after `continue` statement
```
 
 If we follow the suggestion and fix the issue, then we have different output:
 
```python
def main():                                 # | 0 div 4
    for a in range(10):                     # | 0 div 2
        if a % 4 == 0:                      # | 1 div nothing
            print(a, 'div', 4)              # | 2 div 2
        elif a % 3 == 0:                    # | 3 div 3
            print(a, 'div', 3)              # | 4 div 4
            continue                        # | 4 div 2
        if a % 2 == 0:                      # | 5 div nothing
            print(a, 'div', 2)              # | 6 div 3
        else:                               # | 7 div nothing
            print(a, 'div', 'nothing')      # | 8 div 4
    return 0                                # | 8 div 2
                                            # | 9 div 3

if __name__ == '__main__':
    main()
```



---

_Comment by @charliermarsh on 2023-02-14 18:41_

I actually think I may have just fixed this yesterday via #2881. I'll test to confirm.

---

_Label `bug` added by @charliermarsh on 2023-02-14 18:42_

---

_Comment by @aberres on 2023-02-14 19:20_

You are too fast for me :) I did not check closed issues as I did not expect much to be changed since yesterday's release.

Keep on rocking!

---

_Comment by @charliermarsh on 2023-02-14 20:36_

Thanks :)

Confirmed that this doesn't trigger on `main`! New release will go out today or tomorrow.

---

_Closed by @charliermarsh on 2023-02-14 20:36_

---
