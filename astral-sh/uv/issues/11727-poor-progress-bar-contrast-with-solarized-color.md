```yaml
number: 11727
title: Poor progress bar contrast with solarized color scheme
type: issue
state: closed
author: chrisrodrigue
labels:
  - bug
  - external
assignees: []
created_at: 2025-02-23T18:39:53Z
updated_at: 2025-02-25T23:12:26Z
url: https://github.com/astral-sh/uv/issues/11727
synced_at: 2026-01-12T16:00:45Z
```

# Poor progress bar contrast with solarized color scheme

---

_@chrisrodrigue_

### Summary

uv's progress bars have poor contrast in Windows Terminal using the solarized dark color scheme.

![Image](https://github.com/user-attachments/assets/85340d63-0f24-4d1c-acae-b2996a36cd34)

Newer versions of Window Terminal have a setting for this, but it doesn't seem to affect the progress bar contrast. I'm not sure if there's a generalized way for uv to read the local terminal color (similar to terminal size) to compensate for contrast across different platforms and terminals.

![Image](https://github.com/user-attachments/assets/0364db50-49e0-4e53-9c8c-22fd5783ea7b)

Feel free to close if this is out of scope or infeasible.

### Platform

Windows 11 x86_64

### Version

uv 0.6.2

### Python version

Python 3.13.2

---

_Label `bug` added by @chrisrodrigue on 2025-02-23 18:39_

---

_Comment by @chrisrodrigue on 2025-02-23 20:26_

Hmmm, it would appear that the `Automatically adjust lightness of indistinguishable text` setting is to blame. 

Turning it on has the opposite effect that one would expect, at least on the uv progress bars. 

The picture below shows the results of disabling this setting.

![Image](https://github.com/user-attachments/assets/b2fd4a27-1714-45a4-9537-e673d6fb1b6a)

This doesn't seem to be an issue with uv?

---

_Comment by @konstin on 2025-02-24 14:26_

uv doesn't set the absolute colors (like RGB values), it uses colors codes instead that the terminal then translates into real colors. These color choices are usually the user's theme colors. It looks like this indeed the setting you showed, which turns colors brighter to contrast with the background, but then they don't contrast with each other anymore.

---

_Comment by @chrisrodrigue on 2025-02-25 23:10_

Closing since it's not an issue with uv.

---

_Closed by @chrisrodrigue on 2025-02-25 23:10_

---

_Label `external` added by @zanieb on 2025-02-25 23:12_

---
