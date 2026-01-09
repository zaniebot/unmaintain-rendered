---
number: 6100
title: uv forces unrequested package updates
type: issue
state: closed
author: stas00
labels:
  - compatibility
  - cli
assignees: []
created_at: 2024-08-15T01:19:35Z
updated_at: 2024-10-16T17:23:05Z
url: https://github.com/astral-sh/uv/issues/6100
synced_at: 2026-01-07T13:12:17-06:00
---

# uv forces unrequested package updates

---

_Issue opened by @stas00 on 2024-08-15 01:19_

With uv==0.2.36

numpy > 2 breaks many packages, so we don't want it
```
$ uv pip install numpy==1.26.4
```
but when I update other packages it keeps bringing it back:
```
$ uv pip install datasets -U
Resolved 31 packages in 297ms
Prepared 3 packages in 0.45ms
Uninstalled 3 packages in 37ms
░░░░░░░░░░░░░░░░░░░░ [0/3] Installing wheels...                                                                                          warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance. If this is intentional, use `--link-mode=copy` to suppress this warning.

hint: If the cache and target directories are on different filesystems, hardlinking may not be supported.
Installed 3 packages in 52ms
 - datasets==2.20.0
 + datasets==2.21.0
 - fsspec==2024.5.0
 + fsspec==2024.6.1
 - numpy==1.26.4
 + numpy==2.0.1
```

why is it updating `numpy` again? `datasets` has no such requirement - it requires `numpy>=1.17`. If I do:
```
pip install datasets -U
[...]
Requirement already satisfied: numpy>=1.17 in /mnt/nvme0/anaconda3/envs/py310-pt22/lib/python3.10/site-packages (from datasets) (1.26.4)
```
it doesn't fetch the unwanted `numpy==2.0.1`

So now I have to constantly re-install `numpy` after every second `uv pip install` command.

Do you think `uv` thinks it needs `numpy==2.0.1` through some dependency of `datasets`? And `pip` gets it wrong then?

I'm just tired to be on the watch out for `uv` sneaking in `numpy==2.0.1` since our code breaks with it due to many 3rd party packages failing with it.

Thank you!

---

_Comment by @charliermarsh on 2024-08-15 01:21_

I think the issue is that if you pass `--upgrade`, we try to upgrade everything. If you pass `--upgrade-package datasets` instead, we'll only upgrade `datasets` (or any packages that _must_ be upgraded based on the `datasets` upgrade). There's an open issue to change this behavior, let me find it...

---

_Comment by @charliermarsh on 2024-08-15 01:22_

Here it is: https://github.com/astral-sh/uv/issues/4779

\cc @zanieb 

---

_Comment by @charliermarsh on 2024-08-15 01:27_

Maybe we want `--upgrade-all`? It breaks the symmetry of the interface w/r/t other options, but it's clearly a bit confusing for users.

---

_Label `needs-decision` added by @charliermarsh on 2024-08-15 01:27_

---

_Label `cli` added by @charliermarsh on 2024-08-15 01:27_

---

_Comment by @stas00 on 2024-08-15 01:28_

Hmm, but if the intention is to be pip-BC `-U` == update only listed packages - is there a reason to break that well-established pattern?


---

_Comment by @charliermarsh on 2024-08-15 01:37_

I think it's reasonable to change this in the `uv pip` interface. We just probably still want a way to say "Upgrade all packages".

---

_Comment by @charliermarsh on 2024-08-15 01:37_

I also need to look at what happens with `pip install -U -r requirements.txt` -- does it upgrade all the listed packages?

---

_Label `needs-decision` removed by @charliermarsh on 2024-08-15 01:37_

---

_Label `compatibility` added by @charliermarsh on 2024-08-15 01:37_

---

_Comment by @stas00 on 2024-08-15 01:47_

I suppose most of the time we don't care if `-U` updates all packages, except when there is a situation where some package is either broken or leads to a breakage in other packages like in the current situation of `numpy` which is not backward compatible.

So that's perhaps why there were no objections when it was first introduced. That is my guess is that users are oblivious to the fact that `uv pip install xyz -U` doesn't mean the same as `pip install xyz -U` - I'm not sure if it's a good thing.

Looking at pip's manpage:
```
       -U, --upgrade
              Upgrade all packages to the newest available version.  
              This process is recursive regardless of whether a dependency  is
              already satisfied.
```

So it's sort of ambiguous as it probably implies that "packages" are those that were listed on the command line, but one could easily interpret it as "upgrade all installed packages".

Not sure what is the best solution here. I suppose the problem is that you are using `pip` in your interface. If you were to say `uv install` and not `uv pip install` then really choose whatever options you think are the best, but when it pretends to be a drop in replacement - and I have just `s/pip/uv pip/` in my scripts - this is a very unexpected outcome. Not following the principle of the least surprise here.

---

_Comment by @stas00 on 2024-08-15 01:52_

> I also need to look at what happens with `pip install -U -r requirements.txt` -- does it upgrade all the listed packages?

it appears to be so:

```
$ uv pip install datasets==2.20
$ echo "datasets" > r.txt
$ pip install -U -r r.txt
[...]
Downloading datasets-2.21.0-py3-none-any.whl (527 kB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 527.3/527.3 kB 8.3 MB/s eta 0:00:00
Installing collected packages: datasets
  Attempting uninstall: datasets
    Found existing installation: datasets 2.20.0
    Uninstalling datasets-2.20.0:
      Successfully uninstalled datasets-2.20.0
```


---

_Comment by @bluss on 2024-08-15 14:58_

@stas00 Since pip says "This process is recursive regardless" I would think it updates all named packages and their dependencies. But this is *not* the case, if one tries it. So then what does it mean - I think it means that recursive updates are applied if necessary.

---

_Comment by @stas00 on 2024-08-15 17:52_

Indeed, recursive updates related to the listed packages - not all installed packages - we presume that is since the writing is ambiguous, but we can derive their truth/reality from the actions of pip.

But I'm just sharing user's experience feedback - it's your project so you of course can do what makes the most sense to you. 

I'm grateful for this super fast tool and looking forward to having it replace pip completely and rewrite my muscle memory to some of the flags that are different.

---

_Referenced in [mosaicml/ci-testing#28](../../mosaicml/ci-testing/pulls/28.md) on 2024-08-30 22:22_

---

_Referenced in [astral-sh/uv#8260](../../astral-sh/uv/issues/8260.md) on 2024-10-16 17:22_

---

_Comment by @charliermarsh on 2024-10-16 17:23_

I'm going to merge into https://github.com/astral-sh/uv/issues/4779 which I believe is the same.

---

_Closed by @charliermarsh on 2024-10-16 17:23_

---
