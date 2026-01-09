---
number: 14516
title: Clock skew detected when using uv pip install
type: issue
state: open
author: Isfate
labels:
  - question
assignees: []
created_at: 2025-07-09T03:30:11Z
updated_at: 2025-07-09T12:13:19Z
url: https://github.com/astral-sh/uv/issues/14516
synced_at: 2026-01-07T13:12:18-06:00
---

# Clock skew detected when using uv pip install

---

_Issue opened by @Isfate on 2025-07-09 03:30_

### Summary

uv pip install numpy
      Generating multi-targets for "loops_half.dispatch.h"
        Enabled targets: AVX512_SKX, baseline
      WARNING: Project targets '>=1.5.2' but uses feature deprecated since '1.3.0': Source file src/umath/svml/linux/avx512/svml_z0_acos_d_la.s in the 'objects' kwarg is not an object..
      Generating multi-targets for "_simd.dispatch.h"
        Enabled targets: SSE42, AVX2, FMA3, AVX512F, AVX512_SKX, baseline
      Build targets in project: 106
      WARNING: Deprecated features used:
       * 1.3.0: {'Source file src/umath/svml/linux/avx512/svml_z0_acos_d_la.s in the 'objects' kwarg is not an object.'}

      NumPy 2.3.1

        User defined options
          Native files: /data1/melo/zhangyulong/.cache/uv/sdists-v9/pypi/numpy/2.3.1/F93Zo0eBqgLqCbZx6pNaI/src/.mesonpy-vlunxuux/meson-python-native-file.ini
          b_ndebug    : if-release
          b_vscrt     : md
          buildtype   : release

      Found ninja-1.11.1.git.kitware.jobserver-1 at /data1/melo/zhangyulong/.cache/uv/builds-v0/.tmp4xkh5G/bin/ninja

      ERROR: Clock skew detected. File /data1/melo/zhangyulong/.cache/uv/sdists-v9/pypi/numpy/2.3.1/F93Zo0eBqgLqCbZx6pNaI/src/.mesonpy-vlunxuux/meson-python-native-file.ini has a time stamp 170.9665s in
      the future.

      A full log can be found at /data1/melo/zhangyulong/.cache/uv/sdists-v9/pypi/numpy/2.3.1/F93Zo0eBqgLqCbZx6pNaI/src/.mesonpy-vlunxuux/meson-logs/meson-log.txt

      hint: This usually indicates a problem with the package or the build environment.


### Platform

cenos7

### Version

uv=0.7.19

### Python version

3.12.9

---

_Label `bug` added by @Isfate on 2025-07-09 03:30_

---

_Comment by @zanieb on 2025-07-09 12:13_

I'm not sure we can help you here. We're just building the package per the standards. Is your system clock incorrect?

---

_Label `bug` removed by @zanieb on 2025-07-09 12:13_

---

_Label `question` added by @zanieb on 2025-07-09 12:13_

---
