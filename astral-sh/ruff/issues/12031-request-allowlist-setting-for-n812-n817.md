```yaml
number: 12031
title: "Request: allowlist setting for `N812`/`N817`"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
assignees: []
created_at: 2024-06-25T17:10:31Z
updated_at: 2024-06-25T22:59:15Z
url: https://github.com/astral-sh/ruff/issues/12031
synced_at: 2026-01-12T15:54:51Z
```

# Request: allowlist setting for `N812`/`N817`

---

_@jamesbraza_

There are certain libraries like [PyTorch's FSDP](https://pytorch.org/docs/stable/fsdp.html) that standardized on being imported with all caps:

```python
from torch.distributed.fsdp import FullyShardedDataParallel as FSDP

...
sharded_module = FSDP(my_module)
```

Currently, with Ruff 0.4.10 you just have to use a `# noqa: N817` for every import line of libraries like FSDP.

Also, a similar PyTorch use case is `from torch.nn import functional as F`, which raises N812.

Ideally, there is a configuration option to build up an allowlist of possible names to not raise [N812](https://docs.astral.sh/ruff/rules/lowercase-imported-as-non-lowercase/)/[N817](https://docs.astral.sh/ruff/rules/camelcase-imported-as-acronym/). I tried using the [`ignore-names`](https://docs.astral.sh/ruff/settings/#lint_pep8-naming_ignore-names) or `extend-ignore-names` settings, but neither worked.

Do you think we can either allow `ignore-names` to apply to N817, or create a new option `ignore-acronyms` that can accommodate this use case?

Thank you for your consideration!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-25 21:15_

---

_Comment by @charliermarsh on 2024-06-25 21:15_

It does look like those rules respect the setting, but confusingly... N812 matches against the `asname` (`FSDP` above) while N817 matches against the `name` (`FullyShardedDataParallel` above). So I'll fix that. Which would you find more intuitive? Maybe we match against... both?

---

_Label `bug` added by @charliermarsh on 2024-06-25 21:16_

---

_Comment by @jamesbraza on 2024-06-25 21:27_

So to write it out, I think:

`from torch.nn import functional as F`:
- Ruff should throw N812 as `functional` is lowercase
    - And one part of this request is to manually allowlist this in `pyproject.toml`
- Ruff should not throw N817 as `functional` is not camel case

`from torch.distributed.fsdp import FullyShardedDataParallel as FSDP`
- Ruff should not throw N812 as `FullyShardedDataParallel` is not lowercase
- Ruff should throw N817 as this is N817's normal use case
    - And the other part of this request is to manually allowlist this in `pyproject.toml`

Does that answer your question?

---

_Comment by @charliermarsh on 2024-06-25 21:34_

Actually what I was asking is: given `from torch.distributed.fsdp import FullyShardedDataParallel as FSDP`, what value would you expect to put in `ignore-names`?

---

_Comment by @jamesbraza on 2024-06-25 21:38_

Oh I see now. Yeah I think I would want to put `FSDP`, as that's the infringing acronym

Open to specifying `FullyShardedDataParallel`, but that's not really the point of infringement so I prefer `FSDP`

---

_Closed by @charliermarsh on 2024-06-25 22:47_

---

_Closed by @charliermarsh on 2024-06-25 22:47_

---

_Comment by @charliermarsh on 2024-06-25 22:47_

I changed it to respect both. The rules were inconsistent as to which they looked at.

---

_Comment by @jamesbraza on 2024-06-25 22:59_

Thank you! Appreciate the fast turnaround 👍 

---
