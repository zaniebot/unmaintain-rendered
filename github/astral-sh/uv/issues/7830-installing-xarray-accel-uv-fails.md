---
number: 7830
title: "Installing xarray[accel] uv fails"
type: issue
state: closed
author: NathanCummings
labels:
  - needs-mre
assignees: []
created_at: 2024-10-01T08:50:29Z
updated_at: 2024-10-16T20:24:19Z
url: https://github.com/astral-sh/uv/issues/7830
synced_at: 2026-01-07T13:12:17-06:00
---

# Installing xarray[accel] uv fails

---

_Issue opened by @NathanCummings on 2024-10-01 08:50_

Hi,

When installing xarray with the optional "accel" dependencies, this fails with a uv-generated venv. With a venv created with a python install I had on my system (from homebrew in this case) I was able to pip install ok.

Both were python version 3.12.6.


---

_Comment by @charliermarsh on 2024-10-16 18:52_

This runs without issue for me:

```
❯ uv pip install "xarray[accel]"
Resolved 17 packages in 176ms
Prepared 8 packages in 2.12s
Installed 17 packages in 99ms
 + bottleneck==1.4.1
 + flox==0.9.10
 + llvmlite==0.43.0
 + numba==0.60.0
 + numbagg==0.8.2
 + numpy==2.0.2
 + numpy-groupies==0.11.2
 + opt-einsum==3.4.0
 + packaging==24.1
 + pandas==2.2.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.2
 + scipy==1.13.1
 + six==1.16.0
 + toolz==1.0.0
 + tzdata==2024.2
 + xarray==2024.7.0
```

We'd need a reproduction in order to help out here.

---

_Label `needs-mre` added by @charliermarsh on 2024-10-16 18:52_

---

_Comment by @NathanCummings on 2024-10-16 19:50_

Thanks for getting back to me on this.

This is with macOS 15.0.1 (24A348)

I tried with uv-generated venvs using python versions:
- 3.12.7 (I have it on my system via Homebrew so uv used that)
- 3.11.9 (uv downloaded)
- 3.10.15 (uv downloaded)
All of which failed as they tried to use numba==0.53.1 which caused:
```
RuntimeError: Cannot install on Python version 3.10.15; only versions >=3.6,<3.10 are supported.
```

When I created the venv just using the Homebrew-installed python3.12 directly, it used numba==0.60.0 and worked.
```
❯ python3.12 -m venv venv
❯ source venv/bin/activate
❯ pip install "xarray[accel]"
Collecting xarray[accel]
  Using cached xarray-2024.9.0-py3-none-any.whl.metadata (11 kB)
Collecting numpy>=1.24 (from xarray[accel])
  Downloading numpy-2.1.2-cp312-cp312-macosx_14_0_arm64.whl.metadata (60 kB)
Collecting packaging>=23.1 (from xarray[accel])
  Using cached packaging-24.1-py3-none-any.whl.metadata (3.2 kB)
Collecting pandas>=2.1 (from xarray[accel])
  Using cached pandas-2.2.3-cp312-cp312-macosx_11_0_arm64.whl.metadata (89 kB)
Collecting scipy (from xarray[accel])
  Using cached scipy-1.14.1-cp312-cp312-macosx_14_0_arm64.whl.metadata (60 kB)
Collecting bottleneck (from xarray[accel])
  Downloading Bottleneck-1.4.1-cp312-cp312-macosx_11_0_arm64.whl.metadata (7.9 kB)
Collecting numbagg (from xarray[accel])
  Using cached numbagg-0.8.2-py3-none-any.whl.metadata (47 kB)
Collecting flox (from xarray[accel])
  Using cached flox-0.9.13-py3-none-any.whl.metadata (17 kB)
Collecting opt-einsum (from xarray[accel])
  Using cached opt_einsum-3.4.0-py3-none-any.whl.metadata (6.3 kB)
Collecting python-dateutil>=2.8.2 (from pandas>=2.1->xarray[accel])
  Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl.metadata (8.4 kB)
Collecting pytz>=2020.1 (from pandas>=2.1->xarray[accel])
  Using cached pytz-2024.2-py2.py3-none-any.whl.metadata (22 kB)
Collecting tzdata>=2022.7 (from pandas>=2.1->xarray[accel])
  Using cached tzdata-2024.2-py2.py3-none-any.whl.metadata (1.4 kB)
Collecting numpy-groupies>=0.9.19 (from flox->xarray[accel])
  Using cached numpy_groupies-0.11.2-py3-none-any.whl.metadata (18 kB)
Collecting toolz (from flox->xarray[accel])
  Downloading toolz-1.0.0-py3-none-any.whl.metadata (5.1 kB)
Collecting numba (from numbagg->xarray[accel])
  Using cached numba-0.60.0-cp312-cp312-macosx_11_0_arm64.whl.metadata (2.7 kB)
Collecting six>=1.5 (from python-dateutil>=2.8.2->pandas>=2.1->xarray[accel])
  Using cached six-1.16.0-py2.py3-none-any.whl.metadata (1.8 kB)
Collecting llvmlite<0.44,>=0.43.0dev0 (from numba->numbagg->xarray[accel])
  Using cached llvmlite-0.43.0-cp312-cp312-macosx_11_0_arm64.whl.metadata (4.8 kB)
Collecting numpy>=1.24 (from xarray[accel])
  Using cached numpy-2.0.2-cp312-cp312-macosx_14_0_arm64.whl.metadata (60 kB)
Using cached packaging-24.1-py3-none-any.whl (53 kB)
Using cached pandas-2.2.3-cp312-cp312-macosx_11_0_arm64.whl (11.4 MB)
Downloading Bottleneck-1.4.1-cp312-cp312-macosx_11_0_arm64.whl (98 kB)
Using cached flox-0.9.13-py3-none-any.whl (70 kB)
Using cached scipy-1.14.1-cp312-cp312-macosx_14_0_arm64.whl (23.1 MB)
Using cached numbagg-0.8.2-py3-none-any.whl (49 kB)
Using cached opt_einsum-3.4.0-py3-none-any.whl (71 kB)
Using cached xarray-2024.9.0-py3-none-any.whl (1.2 MB)
Using cached numpy_groupies-0.11.2-py3-none-any.whl (40 kB)
Using cached python_dateutil-2.9.0.post0-py2.py3-none-any.whl (229 kB)
Using cached pytz-2024.2-py2.py3-none-any.whl (508 kB)
Using cached tzdata-2024.2-py2.py3-none-any.whl (346 kB)
Using cached numba-0.60.0-cp312-cp312-macosx_11_0_arm64.whl (2.7 MB)
Using cached numpy-2.0.2-cp312-cp312-macosx_14_0_arm64.whl (5.0 MB)
Downloading toolz-1.0.0-py3-none-any.whl (56 kB)
Using cached llvmlite-0.43.0-cp312-cp312-macosx_11_0_arm64.whl (28.8 MB)
Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
Installing collected packages: pytz, tzdata, toolz, six, packaging, opt-einsum, numpy, llvmlite, scipy, python-dateutil, numpy-groupies, numba, bottleneck, pandas, numbagg, xarray, flox
Successfully installed bottleneck-1.4.1 flox-0.9.13 llvmlite-0.43.0 numba-0.60.0 numbagg-0.8.2 numpy-2.0.2 numpy-groupies-0.11.2 opt-einsum-3.4.0 packaging-24.1 pandas-2.2.3 python-dateutil-2.9.0.post0 pytz-2024.2 scipy-1.14.1 six-1.16.0 toolz-1.0.0 tzdata-2024.2 xarray-2024.9.0
```

Is there any more information I can provide?

---

_Comment by @charliermarsh on 2024-10-16 20:02_

I think this is specific to `numba` and `numpy` -- there's a thorough write-up of the problem here: https://github.com/astral-sh/uv/issues/8157. The issue is that Numba sets an upper-bound that excludes the latest NumPy, so if we solve for the latest NumPy, we give you an old version of Numba. It's a completely valid install, but fails at runtime.

I'd suggest adding `numba>=0.60.0` as a constraint the install for now, though we're working on heuristics to get solves that are more intuitive here.

---

_Comment by @charliermarsh on 2024-10-16 20:02_

(Or `numpy<2.1`.)

---

_Comment by @charliermarsh on 2024-10-16 20:05_

I'm going to close this as a case of https://github.com/astral-sh/uv/issues/8157, but I do want to fix that soon. Let me know if you have follow-up questions about the workaround I suggested here.

---

_Closed by @charliermarsh on 2024-10-16 20:05_

---

_Comment by @NathanCummings on 2024-10-16 20:12_

```
❯ uv pip install "xarray[accel]" "numba>=0.60.0"
Resolved 17 packages in 360ms
Prepared 5 packages in 5.41s
Installed 17 packages in 461ms
 + bottleneck==1.4.1
 + flox==0.9.13
 + llvmlite==0.43.0
 + numba==0.60.0
 + numbagg==0.8.2
 + numpy==2.0.2
 + numpy-groupies==0.11.2
 + opt-einsum==3.4.0
 + packaging==24.1
 + pandas==2.2.3
 + python-dateutil==2.9.0.post0
 + pytz==2024.2
 + scipy==1.14.1
 + six==1.16.0
 + toolz==1.0.0
 + tzdata==2024.2
 + xarray==2024.9.0
```

Thank you @charliermarsh , leaving the explicit solution here for future googlers. `uv` is excellent btw and has made things a lot easier!

---

_Comment by @charliermarsh on 2024-10-16 20:24_

Thank you so much! Sorry about this specific pain point. It's been bothering me a lot 😅 But we have a plan to fix it.

---

_Referenced in [astral-sh/uv#8157](../../astral-sh/uv/issues/8157.md) on 2024-10-17 09:55_

---
