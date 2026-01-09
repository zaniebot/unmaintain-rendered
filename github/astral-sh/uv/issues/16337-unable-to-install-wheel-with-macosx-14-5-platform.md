---
number: 16337
title: Unable to install wheel with macosx_14_5 platform tag on macOS 15.6.1
type: issue
state: closed
author: anuraaga
labels:
  - question
assignees: []
created_at: 2025-10-17T06:10:11Z
updated_at: 2025-10-17T15:24:14Z
url: https://github.com/astral-sh/uv/issues/16337
synced_at: 2026-01-07T13:12:19-06:00
---

# Unable to install wheel with macosx_14_5 platform tag on macOS 15.6.1

---

_Issue opened by @anuraaga on 2025-10-17 06:10_

### Summary

I am publishing a python extension and having trouble with the tags for the macOS wheel. The project is [pyvoy](https://pypi.org/project/pyvoy).

I initially tried setting the platform tag to 14_5 as that is the actual minimum macOS version for my package, but it fails to install with uv.

https://pypi.org/project/pyvoy/0.0.2/#files

```
❯ uvx --refresh-package pyvoy pyvoy==0.0.2 -h
  × No solution found when resolving tool dependencies:
  ╰─▶ Because pyvoy==0.0.2 has no wheels with a matching platform tag (e.g., `macosx_15_0_arm64`) and you require pyvoy==0.0.2, we can conclude that your requirements are unsatisfiable.

      hint: Wheels are available for `pyvoy` (v0.0.2) on the following platforms: `manylinux_2_31_aarch64`, `manylinux_2_31_x86_64`, `macosx_14_5_arm64`
```

After setting it to 15_0, it is fine

https://pypi.org/project/pyvoy/0.0.3/#files

```
❯ uvx --refresh-package pyvoy pyvoy==0.0.3 -h
usage: pyvoy [-h] [--address ADDRESS] [--port PORT] [--tls-port TLS_PORT] [--tls-key TLS_KEY] [--tls-cert TLS_CERT] [--tls-ca-cert TLS_CA_CERT] [--tls-disable-http3] [--interface {asgi,wsgi}]
             [--print-envoy-config]
             app
```

From my understanding, 14_5 should work too though as it indicates macOS 14.5+, including macOS 15.6.1. 

Many packages work fine, for example ruff sets `macosx_11_0`. The main difference with it I can see is this is actually a cpython wheel so cp313, etc, instead of py3. But I can't find any information about this affecting macOS version compatibility, such as on pypa.

https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/

pypi also renders a tag macOS 14.5+, not 14.5+,<15 or such

<img width="548" height="70" alt="Image" src="https://github.com/user-attachments/assets/9975c454-371f-4896-b9fd-f0cd5276265b" />

Is it expected for 14_5 wheel to not work in this situation?

### Platform

Darwin 24.6.0 arm64 (macos 15.6.1)

### Version

uv 0.9.3 (Homebrew 2025-10-15)

### Python version

Python 3.12.6

---

_Label `bug` added by @anuraaga on 2025-10-17 06:10_

---

_Comment by @konstin on 2025-10-17 09:05_

For macOS starting with macOS 11, only minor versions of 0 are supported in tags, i.e. `macosx_11_0`, `macosx_12_0`, `macosx_13_0`, `macosx_14_0`, `macosx_15_0`, the midyear updates can't be used in tags.

---

_Label `bug` removed by @konstin on 2025-10-17 09:05_

---

_Label `question` added by @konstin on 2025-10-17 09:05_

---

_Referenced in [pypa/packaging.python.org#1933](../../pypa/packaging.python.org/issues/1933.md) on 2025-10-17 09:24_

---

_Comment by @anuraaga on 2025-10-17 09:25_

Thanks a lot for the explanation! I couldn't find it anywhere in the docs but it makes sense.

I filed https://github.com/pypa/packaging.python.org/issues/1933#issue-3525235478 to see if the pypa docs can be updated with the guidance.

---

_Closed by @anuraaga on 2025-10-17 09:26_

---

_Reopened by @anuraaga on 2025-10-17 13:48_

---

_Comment by @anuraaga on 2025-10-17 13:50_

Sorry @konstin , from the comment on my pypa issue it sounds like this was never actually decided. Would you be able to chime in on that issue? Or otherwise maybe uv should support the tags more flexibly? Appreciate any help. If the latter, I'm happy to try a PR for uv.

---

_Comment by @konstin on 2025-10-17 15:01_

I don't think there's a spec around this, it's effectively implementation defined by `packaging`: https://github.com/pypa/packaging/blob/a85e63daecba56bbb829492624a844d306053504/src/packaging/tags.py#L452-L460

iirc it's done this way so we don't have to know which macOS version had how many minor releases, so we're only tracking the major version. If we want to change that, we should discuss it in https://discuss.python.org/c/packaging/14 and then change it in uv and packaging. I know it's a but of an awkward situation where there's an implementation-defined standard by packaging, it's something that's mainly defined by apple changing its release cycle.

---

_Comment by @anuraaga on 2025-10-17 15:24_

Thanks for the context, and for what it's worth I know well the pains of working on de-facto vs written standards in observability.

While I don't have any authority in the packaging ecosystem, as the author of this issue I will go ahead and close it again since I agree the only two meaningful solutions are

- Clarify the docs based on the clear version scheme change of macOS
- Leave things in limbo (status quo)

Thanks for the help on the other issue, I don't know if I would have the will to try out DPO for the first time to avoid the second option, but I don't think uv should have to worry about this either. Few will have as bad luck as me for putting envoy, which targets 14.5 for whatever reason, in a wheel ;)

---

_Closed by @anuraaga on 2025-10-17 15:24_

---

_Referenced in [pypa/packaging#435](../../pypa/packaging/issues/435.md) on 2025-10-17 15:57_

---

_Referenced in [pypi/warehouse#18959](../../pypi/warehouse/pulls/18959.md) on 2025-10-29 10:15_

---
