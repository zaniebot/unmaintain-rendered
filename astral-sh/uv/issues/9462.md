```yaml
number: 9462
title: "`uv` is not installing the latest version of dependency packages"
type: issue
state: closed
author: Laughing-q
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-11-27T08:49:48Z
updated_at: 2024-11-29T13:22:44Z
url: https://github.com/astral-sh/uv/issues/9462
synced_at: 2026-01-10T04:36:21Z
```

# `uv` is not installing the latest version of dependency packages

---

_Issue opened by @Laughing-q on 2024-11-27 08:49_

Hi team! Really awesome work on this package! 
The installation speed is incredibly fast and we really want to use `uv` package as the python package manager in our github CI tests. However I do noticed it behaves slightly different from the original `pip` manager and it causes our CI tests failing.

We have a dependency `tensorflowjs>=3.9.0` in our repo [ultralytics](https://github.com/ultralytics/ultralytics), https://github.com/ultralytics/ultralytics/blob/7c97ed19372655a8b3d9050fc0f2ffb2e6a31276/pyproject.toml#L104.
- With original `pip` as python package manager it'll install `tensorflow==4.15.0` which works fine with our tests.
- With `uv pip` somehow it'll install `tensorflowjs==3.9.0` and actually this version is not compatible with our tests. (I tried to pin this package to a higher version but `uv` raised another issue to me. see https://github.com/astral-sh/uv/issues/9462#issuecomment-2503274238)

I have all the install log here: 
[3_Benchmarks (ubuntu-latest, 3.11, yolo11n).txt](https://github.com/user-attachments/files/17931564/3_Benchmarks.ubuntu-latest.3.11.yolo11n.txt) 
and from the log it seems `uv` tried a higher version but eventually selected the lowest one, which I don't understand. Can you check the log and tell me why it's selecting the lowest version of the package? Thanks
Because from the docs it should be installing the latest version.
![Image](https://github.com/user-attachments/assets/2445281e-d676-4f8c-b705-6ec347fbd545)



---

_Comment by @Laughing-q on 2024-11-27 08:50_

Also I tried to pin a higher version for `tensorflowjs` but it raised another issue and did not install the packages at all. see https://github.com/ultralytics/ultralytics/pull/17749#issuecomment-2503231199

---

_Renamed from "`uv` is not installing the latest version of package" to "`uv` is not installing the latest version of dependency packages" by @Laughing-q on 2024-11-27 08:52_

---

_Comment by @konstin on 2024-11-27 12:59_

Could it be that the conflict with the tighter bound is caused by packaging and https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes ?

---

_Label `question` added by @konstin on 2024-11-27 13:00_

---

_Label `question` removed by @konstin on 2024-11-27 13:02_

---

_Comment by @charliermarsh on 2024-11-27 13:42_

Yeah, are you using multiple indexes? The version of packaging you want doesn't exist on the PyTorch index: https://download.pytorch.org/whl/packaging/. So you'd need to use `unsafe-first-match` or similar. This looks to be working as intended.

---

_Label `question` added by @charliermarsh on 2024-11-27 13:42_

---

_Label `duplicate` added by @zanieb on 2024-11-27 15:12_

---

_Comment by @Laughing-q on 2024-11-29 13:22_

@konstin @charliermarsh Thank you guys! using `unsafe-first-match` works to our case!

---

_Closed by @Laughing-q on 2024-11-29 13:22_

---
