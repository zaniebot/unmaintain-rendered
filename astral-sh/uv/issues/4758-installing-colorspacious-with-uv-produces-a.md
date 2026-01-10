---
number: 4758
title: "installing `colorspacious` with uv produces a broken env"
type: issue
state: closed
author: neutrinoceros
labels:
  - question
assignees: []
created_at: 2024-07-03T06:47:08Z
updated_at: 2024-07-03T13:11:55Z
url: https://github.com/astral-sh/uv/issues/4758
synced_at: 2026-01-10T01:23:41Z
---

# installing `colorspacious` with uv produces a broken env

---

_Issue opened by @neutrinoceros on 2024-07-03 06:47_

Using uv 0.18.0:

```shell
uv venv
source ./venv/bin/activate
uv pip install colorspacious
python -c "from colorspacious import cspace_converter"
```
I get
```python-traceback
/private/tmp/.venv/lib/python3.12/site-packages/colorspacious/comparison.py:11: SyntaxWarning: invalid escape sequence '\D'
  """Computes the :math:`\Delta E` distance between pairs of colors.
```
Additional notes:
- the error only appears the first time I attempt to import from `colorspacious`
- it doesn't show up at all if I pass `--compile` to `uv pip install`
- installing `colorspacious` with `pip` as follows gives no error (even leaving out `compile`)
- passing ` --only-binary 'colorspacious'` or ` --no-binary 'colorspacious'` doesn't change anything as far as I can tell

```shell
uv venv
source ./venv/bin/activate
uv pip install pip
python -m pip install colorspacious
python -c "from colorspacious import cspace_converter"
```

So it seems to me that bytecode compilation is sufficient to get around the issue, but `python -m pip install` isn't supposed to perform it more than `uv pip install` if `--compile` is omitted, so I don't understand what the difference might be.

system info: this seems to happen consistently on all platforms, as seen in [example logs](https://github.com/yt-project/cmyt/actions/runs/9772843597/job/26977924118?pr=174)

---

_Referenced in [yt-project/cmyt#174](../../yt-project/cmyt/pulls/174.md) on 2024-07-03 06:47_

---

_Comment by @charliermarsh on 2024-07-03 11:56_

I think this is the same as https://github.com/astral-sh/uv/issues/1928#issuecomment-1968857514. (I think the warning is correct?)

---

_Label `question` added by @charliermarsh on 2024-07-03 11:56_

---

_Comment by @neutrinoceros on 2024-07-03 12:15_

oh yes, it's the same, and I get it now: I was under the impression that `pip install` would not compile byte code by default (I knew about `--compile` but not `--no-compile`). Now I'm confused what exactly is pip's default behavior but at least I can agree now that this is not a bug in uv. Thanks Charlie !

---

_Renamed from "BUG: installing `colorspacious` with uv produces a broken env" to "installing `colorspacious` with uv produces a broken env" by @neutrinoceros on 2024-07-03 12:15_

---

_Closed by @neutrinoceros on 2024-07-03 12:15_

---

_Comment by @charliermarsh on 2024-07-03 13:11_

No problem! I’m also not sure, I just remember that we debugged this before :)

---
