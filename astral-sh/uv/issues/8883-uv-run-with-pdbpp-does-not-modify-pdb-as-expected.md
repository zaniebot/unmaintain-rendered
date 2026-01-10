---
number: 8883
title: uv run --with=pdbpp does not modify pdb as expected
type: issue
state: closed
author: adamtheturtle
labels:
  - bug
assignees: []
created_at: 2024-11-07T08:35:54Z
updated_at: 2025-06-09T06:25:04Z
url: https://github.com/astral-sh/uv/issues/8883
synced_at: 2026-01-10T01:24:33Z
---

# uv run --with=pdbpp does not modify pdb as expected

---

_Issue opened by @adamtheturtle on 2024-11-07 08:35_

[`pdbpp`](https://github.com/pdbpp/pdbpp) is a replacement for `pdb` which is automatically used when it is installed and a [`pdb`](https://docs.python.org/3/library/pdb.html) breakpoint is set.

Create a Python file with a breakpoint:

```python
# example.py

breakpoint()
```

Run this (with `echo "c"` to exit the breakpoint) with `pdbpp`:

```sh
$ echo "c" | uv run --with=pdbpp example.py
--Return--
> /Users/adam/Desktop/uv-example/example.py(2)<module>()->None
-> breakpoint()
(Pdb) ⏎ 
```

We can see `(Pdb)` on the last line, which tells us that `pdbpp` is not being used.

To demonstrate what I expect, install `pdbpp` and then run the script:

```sh
$ uv pip install pdbpp
Resolved 6 packages in 15ms
Installed 1 package in 2ms
 + pdbpp==0.10.3
$ echo "c" | uv run example.py
--Return--
[0] > /Users/adam/Desktop/uv-example/example.py(2)<module>()->None
-> breakpoint()
(Pdb++) ⏎    
```

We can see `(Pdb++)` on the last line, which tells us that `pdbpp` is being used.

```
uv 0.4.30 (Homebrew 2024-11-05)
```

---

_Comment by @konstin on 2024-11-07 09:13_

This is probably because pdbpp uses a `.pth` file, CC @zanieb.

---

_Label `bug` added by @charliermarsh on 2024-11-07 13:21_

---

_Comment by @zanieb on 2024-11-07 20:29_

Interesting. Yeah it'd be good to support this, we'll probably need to dig into our virtual environment layering to fix it though.

---

_Comment by @jgehrcke on 2024-11-09 14:24_

@adamtheturtle :turtle: exploring the pdb uv fun :heart: long time no see -- Hello!

(sorry for being offtopic here -- but this is too romantic to not say anything)

---

_Comment by @hynek on 2025-06-08 10:17_

I believe this is not an issue anymore! 🎉

```
~/tmp/8883 via 🐍 v3.13.4
❯ ls
 example.py
~/tmp/8883 via 🐍 v3.13.4 took 3s
❯ cat example.py
import pdb; pdb.set_trace()  # breakpoint() works too

~/tmp/8883 via 🐍 v3.13.4
❯ uv run example.py
> /Users/hynek/tmp/8883/example.py(1)<module>()
-> import pdb; pdb.set_trace()
(Pdb) c

~/tmp/8883 via 🐍 v3.13.4
❯ uv run --with pdbpp example.py
[0] > /Users/hynek/tmp/8883/example.py(1)<module>()

1  -> import pdb; pdb.set_trace()
(Pdb++) c

~/tmp/8883 via 🐍 v3.13.4
```

For Google: not to be confused with #13327 where running `uv run --with pdbpp pytest` doesn't work (but `uv run --with pdbpp -m pytest` does).

---

_Comment by @adamtheturtle on 2025-06-08 11:44_

Thanks @hynek , closing my biggest `uv` bugbear!

---

_Closed by @adamtheturtle on 2025-06-08 11:44_

---
