---
number: 14112
title: "auto-fixing `UP006` yield in errors for python 3.9 and 3.10"
type: issue
state: closed
author: Borda
labels:
  - question
assignees: []
created_at: 2024-11-05T16:57:37Z
updated_at: 2024-11-07T17:46:10Z
url: https://github.com/astral-sh/ruff/issues/14112
synced_at: 2026-01-10T01:22:54Z
---

# auto-fixing `UP006` yield in errors for python 3.9 and 3.10

---

_Issue opened by @Borda on 2024-11-05 16:57_

not sure how much it is a bug or just bad user expectations.
We are performing a bump of min Python version from Python 3.8 to 3.9 so we have updated the Ruff configuration `target-version = "py39"` and running auto fixing which does not fix almost any `List` or `Dict` annotation and suggests enabling `--unsafe-fixes`... which I did but that covers annotation which is not runnable for example `List[torch.Tensor]` converts to `list[torch.Tensor]` which crashes in runtime with
```
ann = list[torch.Tensor], loc = 

    def ann_to_type(ann, loc):
        the_type = try_ann_to_type(ann, loc)
        if the_type is not None:
            return the_type
>       raise ValueError(f"Unknown type annotation: '{ann}' at {loc.highlight()}")
E       ValueError: Unknown type annotation: 'list[torch.Tensor]' at
```
see the full CI log in https://github.com/Lightning-AI/torchmetrics/actions/runs/11685262564/job/32538417524?pr=2827
I understand that "unsafe fixes" could be unstable but on either hand I guess that some trivial fixes shall be made with the default config like converting `List[Dict]` to `list[dict]`

---

_Referenced in [Lightning-AI/torchmetrics#2827](../../Lightning-AI/torchmetrics/pulls/2827.md) on 2024-11-05 16:58_

---

_Comment by @charliermarsh on 2024-11-05 17:00_

Do you know why that crashes at runtime? It looks like it has to do with your own application code. `list[torch.Tensor]` is totally fine in Python 3.9.

---

_Label `question` added by @charliermarsh on 2024-11-06 18:12_

---

_Comment by @Borda on 2024-11-07 16:46_

strange, can't reproduce it again, so closing for now and eventually will reopen later...

---

_Closed by @Borda on 2024-11-07 16:46_

---

_Comment by @Borda on 2024-11-07 17:46_

Ok, so it is probably `torch` issue,the run is: https://github.com/Lightning-AI/torchmetrics/actions/runs/11727000215/job/32667138807?pr=2827

---
