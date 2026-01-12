```yaml
number: 7873
title: cp27 wheels selected for psutil on windows
type: issue
state: closed
author: bluss
labels:
  - question
  - tracing
assignees: []
created_at: 2024-10-02T16:33:04Z
updated_at: 2024-10-03T17:45:40Z
url: https://github.com/astral-sh/uv/issues/7873
synced_at: 2026-01-12T15:59:17Z
```

# cp27 wheels selected for psutil on windows

---

_@bluss_

See wheel `psutil-6.0.0-cp27-none-win32.whl` in lockfile. 

```console
uv init -p 3.12 test-psutil
cd test-psutil/
uv add psutil
# Using CPython 3.12.5
# Creating virtual environment at: .venv
# Resolved 2 packages in 1ms
# Installed 1 package in 0.59ms
#  + psutil==6.0.0
rg -u cp27
```

```
uv.lock
10:    { url = "https://files.pythonhosted.org/packages/c5/66/78c9c3020f573c58101dc43a44f6855d01bbbd747e24da2f0c4491200ea3/psutil-6.0.0-cp27-none-win32.whl", hash = "sha256:02b69001f44cc73c1c5279d02b30a817e339ceb258ad75997325e0e6169d8b35", size = 249766 },
11:    { url = "https://files.pythonhosted.org/packages/e1/3f/2403aa9558bea4d3854b0e5e567bc3dd8e9fbc1fc4453c0aa9aafeb75467/psutil-6.0.0-cp27-none-win_amd64.whl", hash = "sha256:21f1fb635deccd510f69f485b87433460a603919b45e2a324ad65b0cc74f8fb1", size = 253024 },
```


uv 0.4.18

For the record, the wheel is selected for install on gh actions platform Microsoft Windows Server 2022 (?)

`2024-10-02T16:53:24.1443510Z DEBUG Selecting: psutil==6.0.0 [preference] (psutil-6.0.0-cp27-none-win32.whl)`

And the functionality of psutil which my test required [worked](https://github.com/bluss/pyproject-local-kernel/actions/runs/11148088268/job/30983917110) without problem.

---

_Comment by @samypr100 on 2024-10-02 23:44_

Interesting, I would expect `psutil-6.0.0-cp37-abi3-win_amd64.whl` to be the one selected given this lock file

```toml
version = 1
requires-python = ">=3.12"

[[package]]
name = "psutil"
version = "6.0.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/18/c7/8c6872f7372eb6a6b2e4708b88419fb46b857f7a2e1892966b851cc79fc9/psutil-6.0.0.tar.gz", hash = "sha256:8faae4f310b6d969fa26ca0545338b21f73c6b15db7c4a8d934a5482faa818f2", size = 508067 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c5/66/78c9c3020f573c58101dc43a44f6855d01bbbd747e24da2f0c4491200ea3/psutil-6.0.0-cp27-none-win32.whl", hash = "sha256:02b69001f44cc73c1c5279d02b30a817e339ceb258ad75997325e0e6169d8b35", size = 249766 },
    { url = "https://files.pythonhosted.org/packages/e1/3f/2403aa9558bea4d3854b0e5e567bc3dd8e9fbc1fc4453c0aa9aafeb75467/psutil-6.0.0-cp27-none-win_amd64.whl", hash = "sha256:21f1fb635deccd510f69f485b87433460a603919b45e2a324ad65b0cc74f8fb1", size = 253024 },
    { url = "https://files.pythonhosted.org/packages/0b/37/f8da2fbd29690b3557cca414c1949f92162981920699cd62095a984983bf/psutil-6.0.0-cp36-abi3-macosx_10_9_x86_64.whl", hash = "sha256:c588a7e9b1173b6e866756dde596fd4cad94f9399daf99ad8c3258b3cb2b47a0", size = 250961 },
    { url = "https://files.pythonhosted.org/packages/35/56/72f86175e81c656a01c4401cd3b1c923f891b31fbcebe98985894176d7c9/psutil-6.0.0-cp36-abi3-manylinux_2_12_i686.manylinux2010_i686.manylinux_2_17_i686.manylinux2014_i686.whl", hash = "sha256:6ed2440ada7ef7d0d608f20ad89a04ec47d2d3ab7190896cd62ca5fc4fe08bf0", size = 287478 },
    { url = "https://files.pythonhosted.org/packages/19/74/f59e7e0d392bc1070e9a70e2f9190d652487ac115bb16e2eff6b22ad1d24/psutil-6.0.0-cp36-abi3-manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:5fd9a97c8e94059b0ef54a7d4baf13b405011176c3b6ff257c247cae0d560ecd", size = 290455 },
    { url = "https://files.pythonhosted.org/packages/cd/5f/60038e277ff0a9cc8f0c9ea3d0c5eb6ee1d2470ea3f9389d776432888e47/psutil-6.0.0-cp36-abi3-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:e2e8d0054fc88153ca0544f5c4d554d42e33df2e009c4ff42284ac9ebdef4132", size = 292046 },
    { url = "https://files.pythonhosted.org/packages/8b/20/2ff69ad9c35c3df1858ac4e094f20bd2374d33c8643cf41da8fd7cdcb78b/psutil-6.0.0-cp37-abi3-win32.whl", hash = "sha256:a495580d6bae27291324fe60cea0b5a7c23fa36a7cd35035a16d93bdcf076b9d", size = 253560 },
    { url = "https://files.pythonhosted.org/packages/73/44/561092313ae925f3acfaace6f9ddc4f6a9c748704317bad9c8c8f8a36a79/psutil-6.0.0-cp37-abi3-win_amd64.whl", hash = "sha256:33ea5e1c975250a720b3a6609c490db40dae5d83a4eb315170c4fe0d8b1f34b3", size = 257399 },
    { url = "https://files.pythonhosted.org/packages/7c/06/63872a64c312a24fb9b4af123ee7007a306617da63ff13bcc1432386ead7/psutil-6.0.0-cp38-abi3-macosx_11_0_arm64.whl", hash = "sha256:ffe7fc9b6b36beadc8c322f84e1caff51e8703b88eee1da46d1e3a6ae11b4fd0", size = 251988 },
]

[[package]]
name = "test-psutil"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "psutil" },
]

[package.metadata]
requires-dist = [{ name = "psutil", specifier = ">=6.0.0" }]
```

---

_Comment by @zanieb on 2024-10-03 02:12_

I see logs for both the cp37 and cp27 wheels getting selected,  e.g.

```
2024-10-02T16:53:42.6934365Z DEBUG Selecting: psutil==6.0.0 [compatible] (psutil-6.0.0-cp37-abi3-win_amd64.whl)
```

I'm confused about what's going on here.

---

_Label `bug` added by @zanieb on 2024-10-03 02:12_

---

_Comment by @charliermarsh on 2024-10-03 10:58_

@zanieb - That message is just from the resolver. The choice of wheel there is arbitrary.

---

_Comment by @charliermarsh on 2024-10-03 13:09_

Are you sure that we're actually using that wheel @bluss? Those `Selecting:` logs are all from the resolver where we choose an arbitrary wheel to determine the version metadata.

---

_Comment by @bluss on 2024-10-03 15:09_

Not sure, I was just misunderstanding the logs, then.

---

_Label `bug` removed by @zanieb on 2024-10-03 15:26_

---

_Label `question` added by @zanieb on 2024-10-03 15:26_

---

_Comment by @charliermarsh on 2024-10-03 16:35_

How can we make this logging clearer? It's not the first time we've had this issue.

---

_Label `tracing` added by @zanieb on 2024-10-03 16:37_

---

_Comment by @zanieb on 2024-10-03 16:37_

Would it be a challenge to stop reading cp27 wheels entirely? I feel like their metadata shouldn't be relevant anymore.

---

_Comment by @charliermarsh on 2024-10-03 16:38_

Probably not but it admittedly doesn't solve the root problem.

---

_Comment by @charliermarsh on 2024-10-03 16:40_

I think we can omit them though, yes.

---

_Comment by @charliermarsh on 2024-10-03 16:41_

#7381 also makes this way better, because we'll prefer compatible wheels here.

---

_Comment by @zanieb on 2024-10-03 16:41_

I guess what's a little confusing to me is we use the "for installation" dist for display

https://github.com/astral-sh/uv/blob/14507a1793f12bf884786be49d716ca8f8322340/crates/uv-resolver/src/resolver/mod.rs#L1094-L1110

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-03 16:41_

---

_Comment by @charliermarsh on 2024-10-03 16:41_

Yeah I can explain why that's the case but I understand why it's confusing.

---

_Comment by @zanieb on 2024-10-03 16:42_

It sort of looks like we should be using the sdist name there?

https://github.com/astral-sh/uv/blob/14507a1793f12bf884786be49d716ca8f8322340/crates/uv-distribution-types/src/prioritized_distribution.rs#L465-L467

---

_Comment by @charliermarsh on 2024-10-03 16:44_

We would do that after https://github.com/astral-sh/uv/pull/7381. Right now we don't match on tags _at all_ in the universal resolver. All wheels are considered compatible.

---

_Closed by @charliermarsh on 2024-10-03 17:35_

---

_Closed by @charliermarsh on 2024-10-03 17:35_

---

_Comment by @bluss on 2024-10-03 17:45_

You could log explicitly when wheels are installed, that's one way to make it more clear? 

> DEBUG: Installing `psutil` from `foo.whl`

I guess the strongest signal for bug here though was the presence of python 2.7 wheels in a python >= 3.8 resolution?

---
