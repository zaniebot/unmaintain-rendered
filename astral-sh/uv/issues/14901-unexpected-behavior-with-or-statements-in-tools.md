```yaml
number: 14901
title: "Unexpected behavior with OR statements in [tools.uv] environments"
type: issue
state: closed
author: bathsundeep-graticule
labels:
  - enhancement
assignees: []
created_at: 2025-07-25T17:57:05Z
updated_at: 2025-07-30T15:12:23Z
url: https://github.com/astral-sh/uv/issues/14901
synced_at: 2026-01-12T16:01:59Z
```

# Unexpected behavior with OR statements in [tools.uv] environments

---

_@bathsundeep-graticule_

### Question

I'm trying to pare down the list of wheels we pull down when installing packages for users and systems using `pyproject.toml` settings with uv. Right now, users run on macosx with arm architecture, and systems run on linux with x86 arch, so I wanted uv to pull just those wheels. We intend to use uv.lock to control what is uploaded into our private package repository, so a limited list of wheels is the goal.

However, when I put compound OR statements representing both systems in the [tools.uv] environments section, I get some unexpected behavior. I'll go over the cases that worked as I built toward the problem.

### Base case, no environments, works as expected
`uv.lock` has wheels for every OS/system, normal behavior.

### Case 1, simple AND statement on user system, works as expected
Added this to `pyproject.toml`
```
[tool.uv]
environments = [
  "platform_machine=='arm64' and sys_platform=='darwin'"
]
```
Running `uv sync` gives us a nice clean list of wheels in `uv.lock` as expected, only osx arm wheels. We can see `numpy` only has 2 wheels, where before it had 10.
```
wheels = [
    { url = "https://files.pythonhosted.org/packages/6b/6c/a9fbef5fd2f9685212af2a9e47485cde9357c3e303e079ccf85127516f2d/numpy-2.1.1-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:6435c48250c12f001920f0751fe50c0348f5f240852cfddc5e2f97e007544cbe", size = 13493375, upload_time = "2024-09-03T15:08:18.952Z" },
    { url = "https://files.pythonhosted.org/packages/34/f2/1316a6b08ad4c161d793abe81ff7181e9ae2e357a5b06352a383b9f8e800/numpy-2.1.1-cp312-cp312-macosx_14_0_arm64.whl", hash = "sha256:3269c9eb8745e8d975980b3a7411a98976824e1fdef11f0aacf76147f662b15f", size = 5088823, upload_time = "2024-09-03T15:08:27.827Z" },
]
```

So far so good. Now I just want to add wheels for linux x86 systems to the list.

### Case 2, compound OR statement with both systems, unexpected behavior
I modify `pyproject.toml` to the following
```
[tool.uv]
environments = [
  "(sys_platform=='linux' and platform_machine=='x86_64')", 
  "(platform_machine=='arm64' and sys_platform=='darwin')"
]
```

I run `uv sync` and now `uv.lock` has a list that matches as if everything is an OR. It includes linux arm and macos x86 wheels, none of which are intended:
```
wheels = [
    { url = "https://files.pythonhosted.org/packages/36/11/c573ef66c004f991989c2c6218229d9003164525549409aec5ec9afc0285/numpy-2.1.1-cp312-cp312-macosx_10_9_x86_64.whl", hash = "sha256:7c803b7934a7f59563db459292e6aa078bb38b7ab1446ca38dd138646a38203e", size = 20884403, upload_time = "2024-09-03T15:07:57.947Z" },
    { url = "https://files.pythonhosted.org/packages/6b/6c/a9fbef5fd2f9685212af2a9e47485cde9357c3e303e079ccf85127516f2d/numpy-2.1.1-cp312-cp312-macosx_11_0_arm64.whl", hash = "sha256:6435c48250c12f001920f0751fe50c0348f5f240852cfddc5e2f97e007544cbe", size = 13493375, upload_time = "2024-09-03T15:08:18.952Z" },
    { url = "https://files.pythonhosted.org/packages/34/f2/1316a6b08ad4c161d793abe81ff7181e9ae2e357a5b06352a383b9f8e800/numpy-2.1.1-cp312-cp312-macosx_14_0_arm64.whl", hash = "sha256:3269c9eb8745e8d975980b3a7411a98976824e1fdef11f0aacf76147f662b15f", size = 5088823, upload_time = "2024-09-03T15:08:27.827Z" },
    { url = "https://files.pythonhosted.org/packages/be/15/fabf78a6d4a10c250e87daf1cd901af05e71501380532ac508879cc46a7e/numpy-2.1.1-cp312-cp312-macosx_14_0_x86_64.whl", hash = "sha256:fac6e277a41163d27dfab5f4ec1f7a83fac94e170665a4a50191b545721c6521", size = 6619825, upload_time = "2024-09-03T15:08:38.242Z" },
    { url = "https://files.pythonhosted.org/packages/9f/8a/76ddef3e621541ddd6984bc24d256a4e3422d036790cbbe449e6cad439ee/numpy-2.1.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:fcd8f556cdc8cfe35e70efb92463082b7f43dd7e547eb071ffc36abc0ca4699b", size = 13696705, upload_time = "2024-09-03T15:08:58.534Z" },
    { url = "https://files.pythonhosted.org/packages/cb/22/2b840d297183916a95847c11f82ae11e248fa98113490b2357f774651e1d/numpy-2.1.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:d2b9cd92c8f8e7b313b80e93cedc12c0112088541dcedd9197b5dee3738c1201", size = 16041649, upload_time = "2024-09-03T15:09:22.321Z" },
    { url = "https://files.pythonhosted.org/packages/c7/e8/6f4825d8f576cfd5e4d6515b9eec22bd618868bdafc8a8c08b446dcb65f0/numpy-2.1.1-cp312-cp312-musllinux_1_1_x86_64.whl", hash = "sha256:afd9c680df4de71cd58582b51e88a61feed4abcc7530bcd3d48483f20fc76f2a", size = 16409358, upload_time = "2024-09-03T15:09:47.3Z" },
    { url = "https://files.pythonhosted.org/packages/bf/f8/5edf1105b0dc24fd66fc3e9e7f3bca3d920cde571caaa4375ec1566073c3/numpy-2.1.1-cp312-cp312-musllinux_1_2_aarch64.whl", hash = "sha256:8661c94e3aad18e1ea17a11f60f843a4933ccaf1a25a7c6a9182af70610b2313", size = 14172488, upload_time = "2024-09-03T15:10:08.941Z" },
]
```

Windows are the only wheels excluded, so I know the markers are still active in that regard. But I would expect my two statements to be resolved distinctly.

Even if I use a combined statement like below, `uv.lock` still looks the same as above after a sync.
```
"(platform_machine=='arm64' and sys_platform=='darwin') or (sys_platform=='linux' and platform_machine=='x86_64')"
```

So my question is, what am I doing wrong with environments, and is there something else I should be doing to accomplish my goal of a limited list of wheels in uv.lock? 

Here is my full `pyproject.toml` for reference
```
[project]
name = "my-pypi"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12.4"
dependencies = [
  "papermill==2.6.0",
  "jupytext==1.16.7",
  "jupysql==0.10.14",
  "numpy==2.1.1",
  "pandas==2.2.2",
  "ipykernel==6.29.5",
  "prettytable==3.11.0",
  "toml==0.10.2"
]

[tool.uv]
environments = [
  "(sys_platform=='linux' and platform_machine=='x86_64')", 
  "(platform_machine=='arm64' and sys_platform=='darwin')"
]
```

### Platform

macos Sonoma 14.6.1

### Version

0.6.17

---

_Label `question` added by @bathsundeep-graticule on 2025-07-25 17:57_

---

_Renamed from "Unexpected behavior with multiple OR statements in [tools.uv] environments" to "Unexpected behavior with OR statements in [tools.uv] environments" by @bathsundeep-graticule on 2025-07-25 18:04_

---

_Comment by @charliermarsh on 2025-07-28 16:14_

I think we probably check these conditions independently. We could improve the pruning here.

---

_Label `question` removed by @charliermarsh on 2025-07-28 16:14_

---

_Label `enhancement` added by @charliermarsh on 2025-07-28 16:14_

---

_Comment by @bathsundeep-graticule on 2025-07-28 16:57_

@charliermarsh So this is currently expected behavior? This seems like a common use case to have 2 separate environments. Is there something else I could be doing to achieve this?

---

_Comment by @charliermarsh on 2025-07-28 17:00_

I think it’s “expected” in the sense that this behavior matches the existing implementation. It would be nice to make it smarter, but pruning the wheels is only considered an optimization though — it may never be 100% correct, and including extra wheels isn’t considered a bug, strictly speaking.

---

_Comment by @charliermarsh on 2025-07-28 17:04_

Can I ask why it’s a problem for you?

---

_Comment by @bathsundeep-graticule on 2025-07-28 17:10_

We're trying to control the assets (wheels) that get uploaded to our private repository to prevent a developer from pulling down a dependency that hasnt been validated/approved for internal use. That includes other OS and platforms that we dont test on. We plan to build automation around our uv setup to keep our private repo up to date as new versions/packages get approved and added to pyproject.toml

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-30 14:48_

---

_Closed by @charliermarsh on 2025-07-30 15:12_

---
