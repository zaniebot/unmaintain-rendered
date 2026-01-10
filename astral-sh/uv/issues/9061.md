```yaml
number: 9061
title: Running unit tests with pytest in scripts
type: issue
state: open
author: wrdls
labels: []
assignees: []
created_at: 2024-11-12T15:37:43Z
updated_at: 2025-04-19T16:13:03Z
url: https://github.com/astral-sh/uv/issues/9061
synced_at: 2026-01-10T03:41:46Z
```

# Running unit tests with pytest in scripts

---

_Issue opened by @wrdls on 2024-11-12 15:37_

I use `uv` to run scripts with [inline dependencies](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies).

Sometimes I have a need to add a unit test to scripts. Usually my use case is something like the below example where the `normalize()` function starts off simple and grows in complexity over time (e.g. to handle edge cases) and I want to test these changes.

`my_script.py`
```python
def normalize(paths):
  return [p.rstrip("/") for p in paths]

def test_normalize():
  assert ["foo", "bar"] == normalize(["foo/", "bar"])
```

If the scripts keeps growing in size, it probably makes sense to convert this to a project, but at least initially this feels like overkill.

Before `uv`, I'd execute these like this with the dependencies installed system wide or in a venv:

```shell
python my_script.py  # run the script
pytest my_script.py  # test the script
```

What would be the best way to approach this in `uv`? I feel like I'm missing something obvious, so please let me know if this is the case.

Ideally I'd be able to run it like 

```shell
uv run my_script.py   # run the script with uv
uv run pytest my_script.py  # test the script with uv??
```

Another option I thought of was converting the inline dependencies to a venv (e.g., `uv venv --from-script my_script.py`). This adds steps and complexity for testing, but avoids it for runtime.

---

_Renamed from "Running unit tests in scripts" to "Running unit tests with pytest in scripts" by @wrdls on 2024-11-12 15:55_

---

_Comment by @dylwil3 on 2024-11-12 16:42_

Yep, both of those should work! Let me know if either does not.
If you don't want to install `pytest` in your project, you can also use `uvx pytest my_script.py`

---

_Comment by @zanieb on 2024-11-12 16:44_

`uv run pytest my_script.py` won't work because we won't read the PEP 723 metadata anymore. Nor will `uvx`. I'm not sure we have a good solution yet. I think we need a new `uv run` option.

---

_Comment by @dylwil3 on 2024-11-12 17:00_

Edit: Ignore this, see OP's comment below. (My examples worked with pytest because I didn't actually import the dependency.)

---------------------

Really? Both of those worked for me (but I'm doing something shady somewhere...?):

```console
➜  uvexamples uv init inline
Initialized project `inline` at `/Users/dmbp/Documents/dev/uvexamples/inline`
➜  uvexamples cd inline
➜  inline git:(main) ✗ hx my_script.py
```
(copied and pasted OP's script)
```console
➜  inline git:(main) ✗ uv add --script my_script.py "rich"
Updated `my_script.py`
➜  inline git:(main) ✗ uv run my_script.py
Reading inline script metadata from `my_script.py`
⠙ Resolving dependencies...                                                             INFO add_decision: root @ 0a0.dev0 without checking dependencies
⠙ rich==13.9.4                                                                          INFO add_decision: rich @ 13.9.4 without checking dependencies
⠙ markdown-it-py==3.0.0                                                                 INFO add_decision: markdown-it-py @ 3.0.0 without checking dependencies
⠙ pygments==2.18.0                                                                      INFO add_decision: pygments @ 2.18.0 without checking dependencies
⠙ mdurl==0.1.2                                                                          INFO add_decision: mdurl @ 0.1.2 without checking dependencies
Installed 4 packages in 10ms
➜  inline git:(main) ✗ uvx pytest my_script.py
⠙ Resolving dependencies...                                                             INFO add_decision: root @ 0a0.dev0 without checking dependencies
⠙ pytest==8.3.3                                                                         INFO add_decision: pytest @ 8.3.3 without checking dependencies
⠙ iniconfig==2.0.0                                                                      INFO add_decision: iniconfig @ 2.0.0 without checking dependencies
⠙ packaging==24.2                                                                       INFO add_decision: packaging @ 24.2 without checking dependencies
⠙ pluggy==1.5.0                                                                         INFO add_decision: pluggy @ 1.5.0 without checking dependencies
================================= test session starts ==================================
platform darwin -- Python 3.11.5, pytest-8.3.3, pluggy-1.5.0
rootdir: /Users/dmbp/Documents/dev/uvexamples/inline
configfile: pyproject.toml
collected 1 item

my_script.py .                                                                   [100%]

================================== 1 passed in 0.00s ===================================
➜  inline git:(main) ✗
```



---

_Comment by @wrdls on 2024-11-12 17:28_

This doesn't work for me with the following script.

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "rich",
# ]
# ///
import rich

def normalize(paths):
  return [p.rstrip("/") for p in paths]

def test_normalize():
  assert ["foo", "bar"] == normalize(["foo/", "bar"])

if __name__ == "__main__":
  rich.print(normalize(["foo/"]))

```

Testing in Docker to get a clean environment:

```shell
❯ docker run --rm -it --entrypoint=bash python:3
root@b441346070f6:/# apt update && apt install -y vim
...
root@b441346070f6:/# pip install uv
Collecting uv
  Downloading uv-0.5.1-py3-none-manylinux_2_28_aarch64.whl.metadata (11 kB)
Downloading uv-0.5.1-py3-none-manylinux_2_28_aarch64.whl (13.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 13.0/13.0 MB 17.7 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.5.1
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.

[notice] A new release of pip is available: 24.2 -> 24.3.1
[notice] To update, run: pip install --upgrade pip
root@b441346070f6:/# vim my_script.py
root@b441346070f6:/# uv run my_script.py
Reading inline script metadata from `my_script.py`
Installed 4 packages in 6ms
['foo']
root@b441346070f6:/# uvx pytest my_script.py
Installed 4 packages in 5ms
========================================================================================== test session starts ===========================================================================================
platform linux -- Python 3.13.0, pytest-8.3.3, pluggy-1.5.0
rootdir: /
collected 0 items / 1 error                                                                                                                                                                              

================================================================================================= ERRORS =================================================================================================
_____________________________________________________________________________________ ERROR collecting my_script.py ______________________________________________________________________________________
ImportError while importing test module '/my_script.py'.
Hint: make sure your test modules/packages have valid Python names.
Traceback:
usr/local/lib/python3.13/importlib/__init__.py:88: in import_module
    return _bootstrap._gcd_import(name[level:], package, level)
my_script.py:7: in <module>
    import rich
E   ModuleNotFoundError: No module named 'rich'
======================================================================================== short test summary info =========================================================================================
ERROR my_script.py
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!! Interrupted: 1 error during collection !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
============================================================================================ 1 error in 0.06s ============================================================================================
root@b441346070f6:/# 
```

---

_Comment by @dylwil3 on 2024-11-12 18:40_

Ah, thank you! That was helpful - I guess I didn't try actually importing the dependency in the previous example.

If there are only a few dependencies then you could get around it for now with `uvx --with rich pytest my_script.py` (but that defeats the purpose of inline dependencies...)

---

_Comment by @zanieb on 2024-11-12 18:59_

Related

- https://github.com/astral-sh/uv/issues/8609
- #8652

---

_Comment by @zanieb on 2024-11-12 19:00_

We've also discussed like `--with-requirements <script-path>`

---

_Comment by @janosh on 2025-04-15 19:19_

maybe `uv run pytest path/to/test_script.py` deserves special meaning? seems like the cleanest user interface to me for running test files but still have `uv` take care of installing test file dependencies

---

_Comment by @klmr on 2025-04-19 16:09_

@janosh This feature request is independent from pytest, it affects any tool which interacts with the code inside the script — even if it doesn’t execute it. In fact, I stumble across this daily when editing such scripts. I’d like to be able to run `uv nvim script.py` and have the LSP inside NeoVim run in the context of the virtual environment of the script (insert your own favourite IDE instead of NeoVim). Of course for LSP support the long-term answer might be that the LSP will itself support PEP 723, i.e. read the metadata, create its own virtual environment and install the script dependencies. But honestly that feels like pretty complex compared to adding script support for arbitrary `uv run` invocations.

---

_Comment by @zanieb on 2025-04-19 16:13_

See https://github.com/astral-sh/uv/pull/12763

---
