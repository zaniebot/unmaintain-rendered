```yaml
number: 2300
title: uv hangs on install
type: issue
state: closed
author: hauntsaninja
labels:
  - bug
  - great writeup
assignees: []
created_at: 2024-03-08T10:04:52Z
updated_at: 2024-03-12T06:41:08Z
url: https://github.com/astral-sh/uv/issues/2300
synced_at: 2026-01-10T05:40:32Z
```

# uv hangs on install

---

_Issue opened by @hauntsaninja on 2024-03-08 10:04_

uv hangs on various installs for me. The hangs are weirdly deterministic, except for when I really want them to be deterministic and then they stop being so. Anyway, I've got it down to something small that repros reasonably often. The full unminimised case with like 1000 packages always hangs.

In the official Python 3.11 docker image, with `uv --version` equal to `uv 0.1.16`, run the following:
```
import subprocess
from collections import Counter
def run(cmd):
    try:
        subprocess.check_call(cmd, shell=True, timeout=10)
    except Exception as e:
        return type(e)
    return None
Counter(run("uv cache clean; rm -rf .venv; uv venv; uv pip install --no-cache-dir --no-deps --compile ddtrace==2.6.2 debugpy==1.8.1 decorator==5.1.1 defusedxml==0.7.1 ecdsa==0.18.0 ecos==2.0.13 editorconfig==0.12.4 einops==0.7.0") for _ in range(10))
```

You'll see something like `Counter({subprocess.TimeoutExpired: 6, None: 3, subprocess.CalledProcessError: 1})` (and larger cases fail more reliably). The singleton crash is a spurious bytecode compilation failure, e.g.:
```
error: Failed to bytecode compile /root/tmpv/.venv/lib/python3.11/site-packages
  Caused by: Bytecode compilation failed, expected "/root/tmpv/.venv/lib/python3.11/site-packages/defusedxml/cElementTree.py", received: ""
```

You can't really reduce the set of packages further without the hangs going away, although you can get rid of `--compile` if you want

Anyway, uv making much more progress on installing big work environment from scratch than [last time I tried](https://github.com/astral-sh/uv/issues/1585) :-)

---

_Renamed from "uv hangs, also bytecode compilation fails every now and then" to "uv hangs on install, also sometimes bytecode compilation fails" by @hauntsaninja on 2024-03-08 10:12_

---

_Label `great writeup` added by @konstin on 2024-03-08 10:20_

---

_Comment by @konstin on 2024-03-08 10:24_

The bytecode compilation failures look like #2245/#2278, but those should have been included in 0.1.16.

---

_Comment by @hauntsaninja on 2024-03-08 10:38_

Hm, that output was from a less reproducible environment, it's possible I messed up the uv version (I had a few requirements files that contained uv so probably clobbered myself). I'd seen it a few times and so wanted to include it in the issue.

I haven't seen the bytecode crash in my careful docker repro, which is definitely uv 0.1.16. I'll remove the part about bytecode from the title and let you know if I see it again — thanks for fixing!

---

_Renamed from "uv hangs on install, also sometimes bytecode compilation fails" to "uv hangs on install" by @hauntsaninja on 2024-03-08 10:40_

---

_Label `bug` added by @zanieb on 2024-03-08 15:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-11 22:00_

---

_Comment by @charliermarsh on 2024-03-12 00:12_

I _think_ I'm able to reproduce this.

---

_Comment by @charliermarsh on 2024-03-12 00:15_

I was able to reduce it to `ddtrace==2.6.2 debugpy==1.8.1 ecdsa==0.18.0 editorconfig==0.12.4`.

---

_Comment by @charliermarsh on 2024-03-12 01:39_

Don't worry, I added a bunch of debug logging and it stopped hanging :joy:

---

_Comment by @hauntsaninja on 2024-03-12 01:58_

Yeah, `RUST_LOG=trace` [was a fix for me as well](https://discord.com/channels/1039017663004942429/1207998321562619954/1215455734444204073) :D

---

_Comment by @charliermarsh on 2024-03-12 02:41_

I've identified the problem. I think it's specific to `--no-deps`...

---

_Comment by @charliermarsh on 2024-03-12 02:42_

Actually, to be more specific, I could see it manifesting without `--no-deps`, but `--no-deps` would make it way more likely.

---

_Closed by @charliermarsh on 2024-03-12 03:44_

---

_Comment by @charliermarsh on 2024-03-12 04:08_

Fixed in the next release, thank you!

---

_Comment by @hauntsaninja on 2024-03-12 06:41_

Nice, thanks for solving it!

---
