---
number: 4967
title: "`--override`ing a package from a `--extra-index-url` index is not registered in the cache properly"
type: issue
state: closed
author: aabtop
labels:
  - needs-decision
assignees: []
created_at: 2024-07-10T16:35:02Z
updated_at: 2024-07-28T00:38:35Z
url: https://github.com/astral-sh/uv/issues/4967
synced_at: 2026-01-10T01:23:43Z
---

# `--override`ing a package from a `--extra-index-url` index is not registered in the cache properly

---

_Issue opened by @aabtop on 2024-07-10 16:35_

When the package is not registered properly in the cache, subsequent installations with the `--offline` fail.

Minimal repro:

`override.txt`:
```
torch==2.0.0
```

Then run:
```
uv cache clean
uv pip uninstall torch

uv pip install --extra-index-url https://download.pytorch.org/whl/cpu torch==2.0.1 --override override.txt
uv pip uninstall torch
uv pip install --extra-index-url https://download.pytorch.org/whl/cpu torch==2.0.1 --override override.txt --offline
```

The expected output here is that it successfully installs torch, since we're requesting to install the exact same package again, which should be in uv's cache after the first install. However we instead get this output:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because torch was not found in the cache and you require torch==2.0.0, we can conclude that the requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled
```

I'm on Linux aarch64 (but fairly certain I've tried it on other platforms and the issue is the same there)

My UV version is 0.2.23:
```
uv --version
uv 0.2.23
```


---

_Comment by @charliermarsh on 2024-07-10 16:36_

Interesting, thanks. I would also expect this to work. Not obvious why it's failing on first glance.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-10 16:40_

---

_Label `bug` added by @charliermarsh on 2024-07-10 16:43_

---

_Comment by @charliermarsh on 2024-07-10 16:45_

The problem is that the PyTorch index returns cache headers that don't permit caching the responses (`cache-control: no-cache,no-store,must-revalidate`), so we don't save them. It's unclear if we should cache them, to allow them to be used with `--offline`.

What do you think @BurntSushi?

---

_Comment by @BurntSushi on 2024-07-10 16:58_

A `cache-control: no-cache,no-store,must-revalidate` header is very weird. That effectively reduces to `cache-control: no-store` as I understand it, and indeed, this means no caching. I'm not sure we should go against what the HTTP headers are explicitly telling us not to do. Maybe this is a bug that needs to be reported to the PyTorch folks since I'm not sure I see an issue with caching things here? Like, `no-store` is a very definitive "DO NOT CACHE THIS" indicator.

---

_Comment by @charliermarsh on 2024-07-10 16:59_

Yeah... Though `--offline` is explicitly opting in to "stale cache is ok" in our semantics, IIRC, so like... arguably it's ok to return data that you shouldn't? 

---

_Label `bug` removed by @charliermarsh on 2024-07-10 17:01_

---

_Label `needs-decision` added by @charliermarsh on 2024-07-10 17:01_

---

_Comment by @aabtop on 2024-07-10 17:15_

Thanks for the quick response. It looks like you have sussed this out already, but I realize now the `--override` is not relevant and a more minimal repro is:

```
uv cache clean
uv pip uninstall torch

uv pip install --extra-index-url https://download.pytorch.org/whl/cpu torch==2.0.1
uv pip uninstall torch
uv pip install --extra-index-url https://download.pytorch.org/whl/cpu torch==2.0.1 --offline
```

---

_Comment by @charliermarsh on 2024-07-10 17:19_

👍 Yeah, I did notice that too. I ended up using Jinja for a smaller repro :)

---

_Comment by @BurntSushi on 2024-07-10 17:36_

> Yeah... Though `--offline` is explicitly opting in to "stale cache is ok" in our semantics, IIRC, so like... arguably it's ok to return data that you shouldn't?

I agree that `--offline` is opting into "stale cache is ok," and I think that would mean that we could ignore things like `must-revalidate` where you're supposed to send a revalidation request to the origin. I could see that if `--offline` is given, we could say, "nah we don't have to do that because the client told us that stale data was cool." But in this particular case, PyTorch is telling us not to even _store_ a response in the first place. The question of staleness shouldn't even come up in this case because we shouldn't even be caching in the first place.

I'd be really curious if this is intended behavior on the PyTorch side.

---

_Referenced in [pytorch/pytorch#130571](../../pytorch/pytorch/issues/130571.md) on 2024-07-11 20:52_

---

_Comment by @charliermarsh on 2024-07-12 00:47_

Yeah, I generally agree with you @BurntSushi. I would be fine returning a stale response, but `no-store` is a bit of a stretch.

---

_Comment by @charliermarsh on 2024-07-28 00:38_

Ok, I think the issue here is with the PyTorch index so going to close.

---

_Closed by @charliermarsh on 2024-07-28 00:38_

---
