```yaml
number: 2775
title: UV pip compile fails with extra-index-url
type: issue
state: closed
author: RobMcH
labels: []
assignees: []
created_at: 2024-04-02T13:35:35Z
updated_at: 2024-05-24T13:25:45Z
url: https://github.com/astral-sh/uv/issues/2775
synced_at: 2026-01-10T05:31:37Z
```

# UV pip compile fails with extra-index-url

---

_Issue opened by @RobMcH on 2024-04-02 13:35_

I am trying to use uv pip compile as a drop-in replacement for pip-compile and am running into issues with how extra-index-url is handled. A minimal working example would be the following requirements.in
```
networkx==2.5
torch==2.1.0+cu118
```

running without extra-index-url will error out as it cannot find torch (as expected):
```
uv pip compile --generate-hashes  requirements.in -o requirements.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.1.0+cu118 and you require torch==2.1.0+cu118, we can conclude that the requirements are unsatisfiable.
```
and running with extra-index-url will error out as it seemingly cannot find networkx on the extra-index (it seems not to check the original index?):
```
uv pip compile --generate-hashes --extra-index-url "https://download.pytorch.org/whl/cu118" requirements.in -o requirements.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of networkx==2.5 and you require networkx==2.5, we can conclude that the requirements are unsatisfiable.
```

I am using the latest uv version (0.1.27). This might be related to existing PyTorch issues here, but it seems those are all related to installing PyTorch, rather than creating a `requirements.txt` using `uv pip compile`.

---

_Comment by @charliermarsh on 2024-04-02 13:42_

For security purposes, we don’t currently allow packages to be looked up on multiple indexes. You can read more on it here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#packages-that-exist-on-multiple-indexes

In this case my guess is that networkx exists on the PyTorch index, but not at that version, so uv can’t find it.

---

_Comment by @RobMcH on 2024-04-02 13:53_

Thanks! So until uv supports specifying registries for each package individually, torch is essentially incompatible with uv in certain configurations (when one of the other requirements also lives on the torch registry but not all versions are there)?

---

_Comment by @charliermarsh on 2024-04-02 14:00_

Yeah. There are a few workarounds: you can use direct URLs for the Torch dependencies, and omit the extra index altogether. Or you should be able to use —find-links instead of an extra index, and that would also work around the issue.

I’m considering adding an opt-in flag to allow this though. It’s not a technical limitation, but rather a security limitation.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 18:25_

---

_Closed by @charliermarsh on 2024-04-03 23:23_

---

_Comment by @korhojoa on 2024-05-24 07:59_

uv.toml doesn't seem to respect/support this:

```
venv/bin/python -m uv pip install --verbose --upgrade pip-tools
error: Failed to parse `uv.toml`
  Caused by: TOML parse error at line 2, column 18
  |
2 | index-strategy = "unsafe-any-match"
  |                  ^^^^^^^^^^^^^^^^^^
unknown variant `unsafe-any-match`, expected one of `first-index`, `unsafe-first-match`, `unsafe-best-match`
```


---

_Comment by @charliermarsh on 2024-05-24 13:25_

@korhojoa - you want `unsafe-first-match` -- `unsafe-any-match` is a deprecated alias. I will add backwards-compatible support for it though.

---
