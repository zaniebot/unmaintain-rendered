```yaml
number: 2238
title: Type check for uv script (PEP 723) is not working
type: issue
state: closed
author: xiaoxiangmoe
labels:
  - uv
assignees: []
created_at: 2025-12-27T18:21:11Z
updated_at: 2025-12-27T20:42:39Z
url: https://github.com/astral-sh/ty/issues/2238
synced_at: 2026-01-12T15:54:26Z
```

# Type check for uv script (PEP 723) is not working

---

_@xiaoxiangmoe_

### Summary

file ./cow-say.py
```
#!/usr/bin/env -S uv run --script
#
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "cowsay>=6.1",
# ]
# ///

import cowsay

cowsay.cow('Hello World')
```

run `chmod a+x cow-say.py && uv lock --script ./cow-say.py && ./cow-say.py`, it puts

```

  ___________
| Hello World |
  ===========
           \
            \
              ^__^
              (oo)\_______
              (__)\       )\/\
                  ||----w |
                  ||     ||
```

Then run `ty check ./cow-say.py`

it puts

```
error[unresolved-import]: Cannot resolve imported module `cowsay`
  --> cow-say.py:10:8
   |
 8 | # ///
 9 |
10 | import cowsay
   |        ^^^^^^
11 |
12 | cowsay.cow('Hello World')
   |
```

### Version

ty 0.0.7

---

_Comment by @sharkdp on 2025-12-27 18:29_

Thank you for reporting this. Have you seen https://docs.astral.sh/ty/reference/typing-faq/#does-ty-support-pep-723-inline-metadata-scripts ?

---

_Label `uv` added by @carljm on 2025-12-27 19:06_

---

_Closed by @MichaReiser on 2025-12-27 19:39_

---
