```yaml
number: 17331
title: "Windows: `uv tool update-shell` saves but does not apply PATH change"
type: issue
state: closed
author: thedewi
labels:
  - bug
  - windows
assignees: []
created_at: 2026-01-06T02:35:50Z
updated_at: 2026-01-12T02:12:43Z
url: https://github.com/astral-sh/uv/issues/17331
synced_at: 2026-01-12T16:02:48Z
```

# Windows: `uv tool update-shell` saves but does not apply PATH change

---

_@thedewi_

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

_Comment by @samypr100 on 2026-01-11 03:35_

I'm unable to reproduce on Win 11 25H2 Command Prompt. I also tried PWSH and PowerShell.

I verify its behavior with edits made by uv to `HKEY_CURRENT_USER\Environment` (adding `%USERPROFILE%\.local\bin`) and it picked up the changes.

I don't have a Windows 10 22H2 system around to see if it's a version specific issue.

Does `setx` or `regedit` (via reg add) from the command prompt session work for you?
Is there something special about the type of account you're using (e.g. is it not a regular user account)? or perhaps some group policy in effect?

---

_Comment by @samypr100 on 2026-01-11 04:17_

Following up, I believe command prompt (cmd.exe) is not exactly the issue here.

I was able to reproduce this by using the legacy Console Host (conhost.exe) over the newer Windows Terminal.

I believe it would reasonable to include the broadcast to support conhost in https://github.com/astral-sh/uv/blob/7e7f1656ca361b4d3e6ee4d1f2b1dfad6fe08a64/crates/uv-shell/src/windows.rs#L40-L50

---

_Label `windows` added by @samypr100 on 2026-01-11 04:34_

---

_Closed by @charliermarsh on 2026-01-11 14:10_

---

_Comment by @thedewi on 2026-01-12 02:12_

Thank you!!

---
