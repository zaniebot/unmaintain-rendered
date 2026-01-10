---
number: 12751
title: uv sync failure when running concurrently
type: issue
state: closed
author: noahrossi
labels:
  - bug
assignees: []
created_at: 2025-04-08T16:54:44Z
updated_at: 2025-06-06T12:16:42Z
url: https://github.com/astral-sh/uv/issues/12751
synced_at: 2026-01-10T01:25:24Z
---

# uv sync failure when running concurrently

---

_Issue opened by @noahrossi on 2025-04-08 16:54_

### Summary

First, thank you for creating such a great tool! My experience with uv has been overall fantastic. Unfortunately I'm running into some issues with concurrent `uv sync`ing. Since the MRE will look contrived (i.e. why am I even doing this?), I'll start with some background on what I am hoping to achieve. I am using uv in a parallel computing environment. As part of this, I am using `uv run` to run tasks inside a uv project. Sometimes these tasks need to be run in parallel, so a bunch of instances of `uv run` will start running at the same time. If the uv project needs to be synced, uv will then run a bunch of syncs in parallel. This is where I'm running into what I think is a race condition.

#### My MRE
```bash
#!/bin/bash

uv init repro
cd repro

# You need a project with a lot of dependencies to reproduce this reliably
uv add boto3 fastapi numba pandas polars protobuf pyarrow pydantic requests urllib3 scikit-learn jupyter

rm -Rf .venv
parallel -n0 "uv sync -q" ::: {1..10}
# Uncomment the below (and comment the above) to verbose log to file
# parallel "uv sync -v &>{}.log" ::: {1..10}
```

#### The error
It's nondeterministic, but here's an example of an error from the above script:
<details>

```
error: Failed to install: jupyterlab_widgets-3.0.13-py3-none-any.whl (jupyterlab-widgets==3.0.13)
  Caused by: failed to rename file from /home/nrossi/repro/.venv/lib/python3.13/site-packages/.tmp7EUD1t/plugin.json to /home/nrossi/repro/.venv/lib/python3.13/site-packages/jupyterlab_widgets-3.0.13.data/data/share/jupyter/labextensions/@jupyter-widgets/jupyterlab-manager/schemas/@jupyter-widgets/jupyterlab-manager/plugin.json: No such file or directory (os error 2)
error: Failed to install: notebook-7.3.3-py3-none-any.whl (notebook==7.3.3)
```

</details>

And here's some verbose logs from both a successful and unsuccessful uv sync that were run at the same time (see the commented part of the MRE script): https://gist.github.com/noahrossi/acbaafa55fef36af904970c0a968bcc4.

#### Other details
I believe that this is a bug based the intended behavior described on the [Cache Safety section](https://docs.astral.sh/uv/concepts/cache/#cache-safety) of the docs, but please let me know if this is out of the scope of the guarantees that uv provides.

A few more notes:
* I've used GNU parallel here for simplicity. I can rewrite with bash loops and sending tasks to the bg if that would be easier for you.
* Since it's a concurrency bug, the results of the MRE are nondeterministic. That said, on my system, I'm able to reproduce the bug ~80% of the time using that script. Hopefully it'll also reproduce on your system, but let me know if not.
* I can avoid this error by ensuring that I `uv sync` before the parallel step or by using `--isolated`, but having `uv run` "just work" in a parallel environment would be most ideal.
* I'm open to other ways of running uv projects in parallel, if you believe that there's a better way of accomplishing this without concurrently syncing.

### Platform

Red Hat Enterprise Linux 8.6

### Version

uv 0.6.11

### Python version

3.13.1

---

_Label `bug` added by @noahrossi on 2025-04-08 16:54_

---

_Comment by @zanieb on 2025-04-08 17:16_

Thanks for the report and reproduction! We _should_ be locking the environment to prevent concurrent mutation and we _should_ be doing atomic replacements, so this does look like a bug.

---

_Assigned to @jtfmumm by @zanieb on 2025-04-11 14:48_

---

_Comment by @rafaelangelucci on 2025-06-05 15:31_

I seem to be suffering from the same issue described in the ticket. Im wondering if there is an expected timeline for a fix?

---

_Comment by @oconnor663 on 2025-06-05 17:02_

> Im wondering if there is an expected timeline for a fix?

There is not.

That said, I'd be interested in looking into this one unless you know for a fact @zanieb that it's going to be a can of worms?

---

_Comment by @zanieb on 2025-06-05 17:07_

We just lost track of this issue, hopefully we can address it soon.

---

_Unassigned @jtfmumm by @konstin on 2025-06-05 17:39_

---

_Referenced in [astral-sh/uv#13869](../../astral-sh/uv/pulls/13869.md) on 2025-06-05 17:48_

---

_Referenced in [astral-sh/uv#13883](../../astral-sh/uv/issues/13883.md) on 2025-06-06 11:58_

---

_Closed by @konstin on 2025-06-06 12:16_

---

_Closed by @konstin on 2025-06-06 12:16_

---

_Referenced in [astral-sh/uv#14153](../../astral-sh/uv/pulls/14153.md) on 2025-06-20 14:07_

---
