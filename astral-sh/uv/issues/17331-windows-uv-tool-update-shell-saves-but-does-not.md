---
number: 17331
title: "Windows: `uv tool update-shell` saves but does not apply PATH change"
type: issue
state: open
author: thedewi
labels:
  - bug
assignees: []
created_at: 2026-01-06T02:35:50Z
updated_at: 2026-01-06T09:04:19Z
url: https://github.com/astral-sh/uv/issues/17331
synced_at: 2026-01-10T01:26:16Z
---

# Windows: `uv tool update-shell` saves but does not apply PATH change

---

_Issue opened by @thedewi on 2026-01-06 02:35_

### Summary

`uv tool update-shell` is updating the `PATH` in registry, but it seems Windows is not aware it needs to reload the change.

Version: uv 0.9.18 (0cee76417 2025-12-16)
Context: Windows 10 22H2

## Steps to reproduce

1. Ensure `%SYSTEMPROFILE%\.local\bin` is not already included in `%PATH%`.
2. Install any uv tool, like `uv tool install jtbl`.
3. Run `uv tool update-shell`.
4. Open a fresh cmd.exe and try to run the tool, like `jtbl`.

## Expected result

The tool runs.

## Current result

The tool is not found.

## Workaround

Before step 4, in Control Panel "System Properties", click "Environment Variables..." then "Ok".

## Explanation

`uv tool update-shell` is updating the `PATH` in registry, but it seems Windows is not aware it needs to reload the change.

One way to do this is to broadcast `WM_SETTINGCHANGE` to the system. Here's a PowerShell example that works for me:

```ps1
$windowBroadcastHandle = [IntPtr]0xffff
$wmSettingChangeMessageId = 0x1A
$environmentChangedParameter = "Environment"
$abortIfHungFlag = 0x0002

Add-Type @"
using System;
using System.Runtime.InteropServices;

public static class NativeMethods
{
    [DllImport("user32.dll", SetLastError = true, CharSet = CharSet.Auto)]
    public static extern IntPtr SendMessageTimeout(
        IntPtr windowHandle,
        int messageId,
        IntPtr wParam,
        string lParam,
        int flags,
        int timeoutMilliseconds,
        out IntPtr result);
}
"@

[NativeMethods]::SendMessageTimeout(
    $windowBroadcastHandle,
    $wmSettingChangeMessageId,
    [IntPtr]::Zero,
    $environmentChangedParameter,
    $abortIfHungFlag,
    5000,
    [ref]([IntPtr]::Zero)
)
```


### Platform

Windows 10 22H2

### Version

uv 0.9.18 (0cee76417 2025-12-16)

### Python version

Python 3.14.2


---

_Label `bug` added by @thedewi on 2026-01-06 02:35_

---

_Renamed from "Windows: `uv tool update-shell` saves but does not apply changes" to "Windows: `uv tool update-shell` saves but does not apply PATH change" by @thedewi on 2026-01-06 02:36_

---

_Comment by @konstin on 2026-01-06 09:04_

CC @Gankra @samypr100 for windows expertise

---
