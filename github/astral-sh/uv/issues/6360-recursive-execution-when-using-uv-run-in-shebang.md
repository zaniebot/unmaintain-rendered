---
number: 6360
title: "Recursive execution when using `uv run` in shebang line of script without `.py` extension"
type: issue
state: closed
author: ngnpope
labels:
  - bug
assignees: []
created_at: 2024-08-21T17:37:43Z
updated_at: 2024-10-31T14:52:19Z
url: https://github.com/astral-sh/uv/issues/6360
synced_at: 2026-01-07T13:12:17-06:00
---

# Recursive execution when using `uv run` in shebang line of script without `.py` extension

---

_Issue opened by @ngnpope on 2024-08-21 17:37_

Related to https://github.com/astral-sh/uv/pull/6313 and https://github.com/astral-sh/ruff/issues/13021#issuecomment-2302585295.

Using the example from [here](https://astral.sh/blog/uv-unified-python-packaging#single-file-scripts) with the shebang line added and saved to a file named `list-peps`:

```python
#!/usr/bin/env -S uv run --verbose

# /// script
# requires-python = ">=3.12"
# dependencies = ["requests<3", "rich"]
# ///

import requests
from rich.pretty import pprint

resp = requests.get("https://peps.python.org/api/peps.json")
data = resp.json()
pprint([(k, v["title"]) for k, v in data.items()][:10])
```

On execution, it keeps attempting to execute itself:

```console
$ ./list-peps
DEBUG uv 0.3.0
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/nick/.pyenv/shims/python3` (search path)
DEBUG Using Python 3.12.4 interpreter at: /usr/bin/python3
DEBUG Running `./list-peps`
DEBUG uv 0.3.0
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Using Python 3.12.4 interpreter at: /usr/bin/python3
DEBUG Running `./list-peps`
...
```

If executing without an extension using `uv run`:

```console
$ uv run list-peps
error: Failed to spawn: `list-peps`
  Caused by: No such file or directory (os error 2)
```

For comparison, if the script has the extension:

```console
$ ./list-peps.py 
DEBUG uv 0.3.0
Reading inline script metadata from: ./list-peps.py
DEBUG Searching for Python >=3.12 in managed installations or system path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/nick/.pyenv/shims/python3` (search path)
DEBUG Caching via interpreter: `/usr/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.4
DEBUG Adding direct dependency: requests<3
DEBUG Adding direct dependency: rich*
DEBUG Found fresh response for: https://pypi.org/simple/requests/
DEBUG Searching for a compatible version of requests (<3)
DEBUG Selecting: requests==2.32.3 [compatible] (requests-2.32.3-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f9/9b/335f9764261e915ed497fcdeb11df5dfd6f7bf257d4a6a2a686d80da4d54/requests-2.32.3-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://pypi.org/simple/rich/
DEBUG Adding transitive dependency for requests==2.32.3: charset-normalizer>=2, <4
DEBUG Adding transitive dependency for requests==2.32.3: idna>=2.5, <4
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
DEBUG Adding transitive dependency for requests==2.32.3: certifi>=2017.4.17
DEBUG Searching for a compatible version of rich (*)
DEBUG Selecting: rich==13.7.1 [compatible] (rich-13.7.1-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/87/67/a37f6214d0e9fe57f6ae54b2956d550ca8365857f42a1ce0392bb21d9410/rich-13.7.1-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for rich==13.7.1: markdown-it-py>=2.2.0
DEBUG Adding transitive dependency for rich==13.7.1: pygments>=2.13.0, <3.0.0
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
DEBUG Found fresh response for: https://pypi.org/simple/charset-normalizer/
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
DEBUG Found fresh response for: https://pypi.org/simple/idna/
DEBUG Searching for a compatible version of charset-normalizer (>=2, <4)
DEBUG Found fresh response for: https://pypi.org/simple/markdown-it-py/
DEBUG Found fresh response for: https://pypi.org/simple/pygments/
DEBUG Selecting: charset-normalizer==3.3.2 [compatible] (charset_normalizer-3.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ee/fb/14d30eb4956408ee3ae09ad34299131fb383c47df355ddb428a7331cfa1e/charset_normalizer-3.3.2-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/ca/1c/89ffc63a9605b583d5df2be791a27bc1a42b7c32bab68d3c8f2f73a98cd4/urllib3-2.2.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of idna (>=2.5, <4)
DEBUG Selecting: idna==3.7 [compatible] (idna-3.7-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e5/3e/741d8c82801c347547f8a2a06aa57dbb1992be9e948df2ea0eda2c8b79e8/idna-3.7-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/42/d7/1ec15b46af6af88f19b8e5ffea08fa375d433c998b8a7639e76935c14f1f/markdown_it_py-3.0.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Selecting: urllib3==2.2.2 [compatible] (urllib3-2.2.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f7/3f/01c8b82017c199075f8f788d0d906b9ffbbc5a47dc9918a945e13d5a2bda/pygments-2.18.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of certifi (>=2017.4.17)
DEBUG Selecting: certifi==2024.7.4 [compatible] (certifi-2024.7.4-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/1c/d5/c84e1a17bf61d4df64ca866a1c9a913874b4e9bdc131ec689a0ad013fb36/certifi-2024.7.4-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of markdown-it-py (>=2.2.0)
DEBUG Selecting: markdown-it-py==3.0.0 [compatible] (markdown_it_py-3.0.0-py3-none-any.whl)
DEBUG Adding transitive dependency for markdown-it-py==3.0.0: mdurl>=0.1, <1.dev0
DEBUG Searching for a compatible version of pygments (>=2.13.0, <3.0.0)
DEBUG Selecting: pygments==2.18.0 [compatible] (pygments-2.18.0-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/mdurl/
DEBUG Searching for a compatible version of mdurl (>=0.1, <1.dev0)
DEBUG Selecting: mdurl==0.1.2 [compatible] (mdurl-0.1.2-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b3/38/89ba8ad64ae25be8de66a6d463314cf1eb366222074cfda9ee839c56a4b4/mdurl-0.1.2-py3-none-any.whl.metadata
DEBUG Tried 9 versions: certifi 1, charset-normalizer 1, idna 1, markdown-it-py 1, mdurl 1, pygments 1, requests 1, rich 1, urllib3 1
DEBUG Split specific environment resolution took 0.004s
Resolved 9 packages in 5ms
DEBUG Using Python 3.12.4 interpreter at: /home/nick/.cache/uv/archive-v0/YE732SMtA0aLqaQ-9zyiT/bin/python3
DEBUG Running `python ./list-peps.py`
[
│   ('1', 'PEP Purpose and Guidelines'),
│   ('2', 'Procedure for Adding New Modules'),
│   ('3', 'Guidelines for Handling Bug Reports'),
│   ('4', 'Deprecation of Standard Modules'),
│   ('5', 'Guidelines for Language Evolution'),
│   ('6', 'Bug Fix Releases'),
│   ('7', 'Style Guide for C Code'),
│   ('8', 'Style Guide for Python Code'),
│   ('9', 'Sample Plaintext PEP Template'),
│   ('10', 'Voting Guidelines')
]
```

---

_Label `bug` added by @charliermarsh on 2024-08-21 17:54_

---

_Comment by @charliermarsh on 2024-08-21 17:54_

Thanks!

---

_Comment by @charliermarsh on 2024-08-21 18:58_

```
$ uv run list-peps
error: Failed to spawn: `list-peps`
  Caused by: No such file or directory (os error 2)
```

You can avoid this with `uv run ./list-peps`. I'm not sure whether that's a bug or working as intended, given that running `list-peps` in the terminal is interpreted the same way:

```
❯ list-peps
zsh: command not found: list-peps
```

---

_Comment by @charliermarsh on 2024-08-21 18:58_

(But the recursion is definitely a bug.)

---

_Comment by @PaarthShah on 2024-08-22 08:14_

Hah, I wondered what would happen if I tried this, didn't get around to it!

---

_Comment by @ngnpope on 2024-08-22 09:02_

> You can avoid this with `uv run ./list-peps`. I'm not sure whether that's a bug or working as intended, given that running `list-peps` in the terminal is interpreted the same way

Hmm. I'm not sure that "command not found" is really the same as the "failed to spawn" case. The former is basically because the current directory is not on `$PATH` (a good thing) so you have to specify a path where the executable is found, hence `./list-peps`.

On the other hand, we should probably expect `uv run list-peps` to work in a similar vein to `python list-peps` where we don't need to do `python ./list-peps`. I've not attempted to look at what `uv run` does, but it seems as though it's treating files ending in `.py` as something to be run with a Python interpreter and anything else as execute directly, irrespective of whether it has the executable bit or not. I realise that fixing this might require some sort of file type detection instead of only relying on the extension.

Some further things of interest that seem to confirm the above...

<details>
<summary>Python not needing the <tt>./</tt> prefix</summary>

```console
$ python list-peps
Traceback (most recent call last):
  File "/home/nick/list-peps", line 9, in <module>
    from rich.pretty import pprint
ModuleNotFoundError: No module named 'rich'
```

</details>
<details>
<summary>Removing the executable bit and using <tt>uv run</tt></summary>

```console
$ uv run ./list-peps
error: Failed to spawn: `./list-peps`
  Caused by: Permission denied (os error 13)
```

</details>
<details>
<summary>Attempting to execute a <tt>.pyc</tt> (which is something that Python can do)</summary>

_This is something that is unlikely to support the PEP 723 stuff, so of much less use._

```console
$ python -m compileall list-peps.py
$ python __pycache__/list-peps.cpython-312.pyc 
Traceback (most recent call last):
  File "list-peps.py", line 9, in <module>
    from rich.pretty import pprint
ModuleNotFoundError: No module named 'rich'
$ uv run __pycache__/list-peps.cpython-312.pyc 
error: Failed to spawn: `__pycache__/list-peps.cpython-312.pyc`
  Caused by: Permission denied (os error 13)
```

</details>

---

_Referenced in [astral-sh/uv#6313](../../astral-sh/uv/pulls/6313.md) on 2024-08-22 14:14_

---

_Comment by @charliermarsh on 2024-08-25 20:56_

I'll try to fix this for tomorrow's release.

---

_Referenced in [astral-sh/uv#6635](../../astral-sh/uv/issues/6635.md) on 2024-08-26 00:18_

---

_Referenced in [astral-sh/uv#6634](../../astral-sh/uv/issues/6634.md) on 2024-08-26 00:18_

---

_Assigned to @konstin by @zanieb on 2024-08-26 17:07_

---

_Referenced in [astral-sh/uv#6711](../../astral-sh/uv/pulls/6711.md) on 2024-08-27 18:10_

---

_Comment by @teroshan on 2024-08-28 14:53_

I didn't know that `uv run` could also run binaries, my expectation for its behaviour was actually in line with what I found on [this documentation page](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies): 

As in I was lead to believe that `uv run example.py` or `uv run --python 3.12 example.py` would work like `python example.py` or `python3.12 example.py`

---

_Comment by @zanieb on 2024-08-28 14:58_

That is the case (we use `python` for `.py` files), but it can run any command in the environment context e.g. `uv add ruff && uv run ruff` — this is really important.

---

_Comment by @zanieb on 2024-08-28 14:59_

As a minor note, Hatch and Poetry also recursive infinitely when used this way.

---

_Comment by @teroshan on 2024-08-28 15:02_

> As a minor note, Hatch and Poetry also recursive infinitely when used this way.

`pipx run` doesn't for me

---

_Comment by @plobsing on 2024-09-07 12:43_

A perhaps surprising consequence to the current behaviour is that symlinks to working scripts might themselves not work, depending on the symlink name. For example, if I start with the working version of `./list-peps.py` in the original report, and create `./list-peps` as a symlink to it, I also get the recursion bug but only when invoking through the symlink. I think if the script filename was canonicalized prior to choosing whether to invoke it as a Python source or a binary, this would be less of a problem and perhaps serve as a viable workaround.

---

_Unassigned @konstin by @konstin on 2024-09-09 15:08_

---

_Comment by @dracos on 2024-09-10 12:13_

Not sure if linked, sorry if not, couldn't find another issue and sorry again if I've got confused - the documentation says "uv run file.py is equivalent to uv run python file.py", but it doesn't appear to be, and it gets confused with/without extension:

```
~/test $ cat noextension
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "click",
# ]
# ///
import click
~/test $ cat extension.py
#!/usr/bin/env python3
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "click",
# ]
# ///
import click
~/test $ uv run noextension  # This error is expected
error: Failed to spawn: `noextension`
  Caused by: No such file or directory (os error 2)
~/test $ uv run ./noextension
Traceback (most recent call last):
  File "/Users/m/test/./noextension", line 8, in <module>
    import click
ModuleNotFoundError: No module named 'click'
~/test $ uv run python3 ./noextension
Traceback (most recent call last):
  File "/Users/m/test/./noextension", line 8, in <module>
    import click
ModuleNotFoundError: No module named 'click'
~/test $ uv run extension.py
Reading inline script metadata from: extension.py
~/test $ uv run ./extension.py
Reading inline script metadata from: ./extension.py
~/test $ uv run python3 ./extension.py
Traceback (most recent call last):
  File "/Users/m/test/./extension.py", line 8, in <module>
    import click
ModuleNotFoundError: No module named 'click'
```

Hope that's helpful.

---

_Comment by @bo5o on 2024-10-31 13:43_

The following fixed the recursion bug for me:

```python
#!/usr/bin/env -S uv run -s
```

From `uv help run`:

```help
-s, --script
        Run the given path as a Python script.

        Using `--script` will attempt to parse the path as a PEP 723 script, irrespective of its extension.
```

With this, I can call just `list-peps` (no `./` or `.py`).

---

_Comment by @ngnpope on 2024-10-31 14:52_

Yes. The `--script` flag was added after I opened this and solves this issue. I forgot to come back and close this out.

---

_Closed by @ngnpope on 2024-10-31 14:52_

---

_Referenced in [astral-sh/uv#10003](../../astral-sh/uv/issues/10003.md) on 2024-12-18 15:30_

---

_Referenced in [astral-sh/uv#11220](../../astral-sh/uv/issues/11220.md) on 2025-02-04 17:05_

---
