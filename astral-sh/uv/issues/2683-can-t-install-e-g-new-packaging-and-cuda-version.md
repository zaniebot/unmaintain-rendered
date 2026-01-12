```yaml
number: 2683
title: "Can't install e.g. new `packaging` and CUDA version of torch simultaneously"
type: issue
state: closed
author: adzenith
labels: []
assignees: []
created_at: 2024-03-27T03:01:48Z
updated_at: 2024-04-03T23:29:53Z
url: https://github.com/astral-sh/uv/issues/2683
synced_at: 2026-01-12T15:58:39Z
```

# Can't install e.g. new `packaging` and CUDA version of torch simultaneously

---

_@adzenith_

This is related to the "[Packages that exist on multiple indexes](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes)" in the README, to [this closed ticket](https://github.com/astral-sh/uv/issues/2542), and to the idea of [pinning packages to repositories](https://github.com/astral-sh/uv/issues/171).

The pip compatibility readme [says](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#compatibility-with-pip-and-pip-tools) "in most cases, swapping out pip install for uv pip install should 'just work'" unless you "stray from common pip workflows"; I opened this as a new ticket because it's a specific example of not being able to get `uv` to work at all with a `requirements.in` that works with pip, and my impression is that installing a CUDA version of torch is a reasonably common workflow.

The issue I am facing is that `torch` specifies a bunch of packages in its CUDA `extra-index-url`, and you can also install vanilla `torch` from pip. So there's no way as far as I can see to install a new version of any of [the packages here](https://download.pytorch.org/whl/cu118) while also installing `torch==2.1.2+cu118`. Or if there is, please let me know!

Newest `uv` release:
```
❯ uv --version
uv 0.1.24
```

Install python-can in a venv. It depends on packaging>=23.1:
```
❯ uv pip install python-can==4.3.1
Resolved 5 packages in 24ms
Downloaded 4 packages in 42ms
Installed 5 packages in 9ms
 + msgpack==1.0.8
 + packaging==24.0
 + python-can==4.3.1
 + typing-extensions==4.10.0
 + wrapt==1.16.0
```

Install the CUDA version of torch, which requires this extra-index-url:
```
❯ uv pip install torch==2.1.2+cu118 --extra-index-url=https://download.pytorch.org/whl/cu118
Resolved 10 packages in 143ms
Installed 10 packages in 236ms
 + filelock==3.9.0
 + fsspec==2023.4.0
 + jinja2==3.1.2
 + markupsafe==2.1.3
 + mpmath==1.3.0
 + networkx==3.2.1
 + sympy==1.12
 + torch==2.1.2+cu118
 + triton==2.1.0
 - typing-extensions==4.10.0
 + typing-extensions==4.8.0
```

Now try to compile both of these in a requirements file:
```
❯ cat <<EOF >requirements.in
python-can==4.3.1
torch==2.1.2+cu118
EOF
```

It doesn't work with the extra-index-url, because the pytorch url provides only packaging==22.0:
```
❯ uv pip compile requirements.in --extra-index-url=https://download.pytorch.org/whl/cu118
  × No solution found when resolving dependencies:
  ╰─▶ Because only packaging<23.1 is available and python-can==4.3.1 depends on
      packaging>=23.1, we can conclude that python-can==4.3.1 cannot be used.
      And because you require python-can==4.3.1, we can conclude that the requirements are
      unsatisfiable.
```

It doesn't work with the other order either, because now we can't find our CUDA torch version:
```
❯ uv pip compile requirements.in --extra-index-url=https://pypi.org/simple --extra-index-url=https://download.pytorch.org/whl/cu118
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.1.2+cu118 and you require torch==2.1.2+cu118,
      we can conclude that the requirements are unsatisfiable.
```


---

_Comment by @adzenith on 2024-03-27 04:30_

I have figured out a workaround here. I can install `torch` and `torchvision` directly from the whl urls:
```
torch @ https://download.pytorch.org/whl/cu118/torch-2.1.2%2Bcu118-cp310-cp310-linux_x86_64.whl
torchvision @ https://download.pytorch.org/whl/cu118/torchvision-0.16.2%2Bcu118-cp310-cp310-linux_x86_64.whl
```
This leads to `uv` complaining that `torchvision` depends on `torch==2.1.2`, which isn't available, but I can get past that with an override file:
```
torch
```
which allows for any `torch` version.

---

_Comment by @charliermarsh on 2024-03-27 14:59_

Yeah, just confirming that your summary is correct, and this is somewhat unsolved beyond the workaround you described in your second post.

In the next version of uv, you shouldn't need the override file, since #2624 will correctly respect the local version specifier from the URL. So, you'll still need to use the direct wheel URLs, but you won't need the override.


---

_Comment by @charliermarsh on 2024-04-03 23:29_

The next version of uv will include an opt-in flag to allow this: https://github.com/astral-sh/uv/pull/2815

---

_Closed by @charliermarsh on 2024-04-03 23:29_

---
