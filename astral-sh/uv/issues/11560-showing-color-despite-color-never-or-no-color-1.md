```yaml
number: 11560
title: "Showing color despite `--color never` or `NO_COLOR=1` on `uv python install`"
type: issue
state: closed
author: Choudhry18
labels:
  - bug
  - tracing
assignees: []
created_at: 2025-02-16T18:15:36Z
updated_at: 2025-02-18T20:39:30Z
url: https://github.com/astral-sh/uv/issues/11560
synced_at: 2026-01-12T16:00:40Z
```

# Showing color despite `--color never` or `NO_COLOR=1` on `uv python install`

---

_@Choudhry18_

### Summary

I am trying to log the output from uv invocation and as my text editor doesn't support ansi I am using `NO_COLOR=1 `or `--color never` but I am still getting color in the following debug statements:

```
DEBUG Found `cpython-3.13.0-macos-aarch64-none` for request `Python 3.13.0`
DEBUG Found `cpython-3.12.6-macos-aarch64-none` for request `Python 3.12.6`
DEBUG Found `cpython-3.11.10-macos-aarch64-none` for request `Python 3.11.10`
DEBUG Found `cpython-3.10.15-macos-aarch64-none` for request `Python 3.10.15`
DEBUG Found `cpython-3.9.20-macos-aarch64-none` for request `Python 3.9.20`
DEBUG Found `cpython-3.8.18-macos-aarch64-none` for request `Python 3.8.18`
DEBUG Found `cpython-3.8.12-macos-aarch64-none` for request `Python 3.8.12`
```
on a non-ansi support text editor it looks like:

```
DEBUG Found `^[[32mcpython-3.13.0-macos-aarch64-none^[[39m` for request `^[[36mPython 3.13.0^[[39m`
DEBUG Found `^[[32mcpython-3.12.6-macos-aarch64-none^[[39m` for request `^[[36mPython 3.12.6^[[39m`
DEBUG Found `^[[32mcpython-3.11.10-macos-aarch64-none^[[39m` for request `^[[36mPython 3.11.10^[[39m`
DEBUG Found `^[[32mcpython-3.10.15-macos-aarch64-none^[[39m` for request `^[[36mPython 3.10.15^[[39m`
DEBUG Found `^[[32mcpython-3.9.20-macos-aarch64-none^[[39m` for request `^[[36mPython 3.9.20^[[39m`
DEBUG Found `^[[32mcpython-3.8.18-macos-aarch64-none^[[39m` for request `^[[36mPython 3.8.18^[[39m`
```

### Minimum Recreation Steps

For contributors if you have python the python versions install needed for testing in .python-versions and you run: 
`NO_COLOR=1 RUST_LOG=uv=debug uv python install` 

you should be able to recreate. But the issue can be recreated by any variation of color off and trying to install and python version already installed. 



### Platform

macOS Darwin 24.3.0 arm64

### Version

uv 0.5.24 

### Python version

_No response_

---

_Label `bug` added by @Choudhry18 on 2025-02-16 18:15_

---

_Comment by @charliermarsh on 2025-02-16 18:16_

Ah this is a good catch. This is a tracing log, and the tracing logs don't go through `anstream`. We should either avoid using colors in the tracing logs, or figure out a way to wrap these with `anstream`.

---

_Label `tracing` added by @charliermarsh on 2025-02-16 18:17_

---

_Comment by @zanieb on 2025-02-16 18:34_

cc @Gankra for the more holistic changes needed to our output system

---

_Comment by @konstin on 2025-02-17 11:32_

Using anstream inside tracing would be great, I ran into trait typing problems last time I tried that. 

---

_Comment by @Gankra on 2025-02-18 17:51_

Weird we actually do factor in anstream here when setting the colors for tracing, I'll look into why it's buggy.

---

_Comment by @charliermarsh on 2025-02-18 17:57_

Oops, thanks. Is it possible that filtering only applies to the tracing-emitted fields, and not the message itself?

---

_Comment by @Gankra on 2025-02-18 17:59_

Ah yep that's the issue.

---

_Assigned to @Gankra by @Gankra on 2025-02-18 18:41_

---

_Closed by @Gankra on 2025-02-18 20:39_

---

_Closed by @Gankra on 2025-02-18 20:39_

---
