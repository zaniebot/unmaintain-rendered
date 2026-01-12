```yaml
number: 2020
title: "False-positive `unsupported-operator` when adding strings to non-strings"
type: issue
state: closed
author: rodya-mirov
labels:
  - question
assignees: []
created_at: 2025-12-17T16:43:46Z
updated_at: 2025-12-17T17:20:24Z
url: https://github.com/astral-sh/ty/issues/2020
synced_at: 2026-01-12T15:54:26Z
```

# False-positive `unsupported-operator` when adding strings to non-strings

---

_@rodya-mirov_

### Summary

Consider the following output:

```
error[unsupported-operator]: Unsupported `+` operation
  --> [my_files].py:65:24
   |
63 |                 out2 = item.split(n)[1]
64 |             except IndexError:
65 |                 out2 = "_" + counter + ".txt"
   |                        ---^^^-------
   |                        |     |
   |                        |     Has type `Literal[1]`
   |                        Has type `Literal["_"]`
66 |             output = out_path + "/" + out1 + out2
67 |             my_file = Path(output)
   |
info: rule `unsupported-operator` is enabled by default
```

Obviously you can add a string and an int, but it's barfing here.

Still, I'm excited for what this going to do when it's ready. Congrats on the beta release!

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-12-17 16:46_

Hey, thanks for the report! But unfortunately this

> Obviously you can add a string and an int

is not accurate, I don't think 😄 here's what happens at runtime:

```pycon
>>> "_" + 1
Traceback (most recent call last):
  File "<python-input-0>", line 1, in <module>
    "_" + 1
    ~~~~^~~
TypeError: can only concatenate str (not "int") to str
```



---

_Label `question` added by @AlexWaygood on 2025-12-17 16:48_

---

_Comment by @rodya-mirov on 2025-12-17 17:01_

Well I'll be damned. It works in Java lol

Thanks for finding a bug in some weird corner of my project! Love you guys!

---

_Closed by @rodya-mirov on 2025-12-17 17:01_

---

_Comment by @AlexWaygood on 2025-12-17 17:20_

Haha you're very welcome! Thanks for using ty!

---
