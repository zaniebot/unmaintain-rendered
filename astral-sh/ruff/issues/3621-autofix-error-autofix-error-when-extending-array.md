```yaml
number: 3621
title: "[Autofix error] Autofix error when extending array"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-20T08:20:23Z
updated_at: 2023-05-10T21:10:43Z
url: https://github.com/astral-sh/ruff/issues/3621
synced_at: 2026-01-10T11:09:46Z
```

# [Autofix error] Autofix error when extending array

---

_Issue opened by @qarmin on 2023-03-20 08:20_

Ruff  a45753f462c7a53afd7f942ab3c6f9af3871bf1f

```
class StyleGAN2GeneratorClean(nn.Module):
    def __init__(self, out_size, num_style_feat=512, num_mlp=8, channel_multiplier=2, narrow=1):
        for i in range(num_mlp):
            style_mlp_layers.extend(
                [nn.Linear(num_style_feat, num_style_feat, bias=True),
                 nn.LeakyReLU(negative_slope=0.2, inplace=True)])
```
with 
```
ruff file.py --fix
```

```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `Desktop/RunEveryCommand/Ruff/Broken/58869stylegan2_clean_arch0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```


---

_Label `bug` added by @charliermarsh on 2023-03-20 15:11_

---

_Comment by @charliermarsh on 2023-05-10 21:10_

This was fixed via #4286.

---

_Closed by @charliermarsh on 2023-05-10 21:10_

---
