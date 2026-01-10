---
number: 3931
title: Specify a custom index for only one package
type: issue
state: closed
author: mariomeissner
labels: []
assignees: []
created_at: 2024-05-31T08:33:16Z
updated_at: 2024-06-01T00:17:19Z
url: https://github.com/astral-sh/uv/issues/3931
synced_at: 2026-01-10T01:23:32Z
---

# Specify a custom index for only one package

---

_Issue opened by @mariomeissner on 2024-05-31 08:33_

I am using `uv pip compile` in a project that has pytorch dependencies. I need to use the custom pytorch index `https://download.pytorch.org/whl/cu117` to install `pytorch==2.13.1+cu117`. I am using the following syntax in my `requirements.in` file:
```
--extra-index-url https://download.pytorch.org/whl/cu117
torch==1.13.1+cu117
torchmetrics==1.3.1
```

However, this fails with the following error:
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torchmetrics==1.3.1 and you require
      torchmetrics==1.3.1, we can conclude that the requirements are
      unsatisfiable.
```

Indeed, `torchmetrics==1.3.1` is not available in the pytorch index, but it should be available in the default pypi index. I am aware of uv's limitation to search across multiple indexes, but I thought that I would be able to apply the custom index flag to only one package (in this case `torch`), and have it search for all other packages in the default index. This doesn't seem to be the case, as shown by the torchmetrics failure.

Is there a way to specify a custom index for just one package?

---

_Comment by @charliermarsh on 2024-05-31 16:36_

There isn't a way to do this with `requirements.txt` but we'll support it in the future via TOML (see: #171). In the meantime, you should be able to use `--index-strategy unsafe-first-match` or `--index-strategy unsafe-any-match` to allow searching over multiple indexes.

---

_Closed by @charliermarsh on 2024-05-31 16:36_

---

_Comment by @mariomeissner on 2024-06-01 00:17_

Thanks Charlie. 

I think one more solution is to use the '@' notation to specify the download URL, probably better and faster than enabling unsafe search.

---
