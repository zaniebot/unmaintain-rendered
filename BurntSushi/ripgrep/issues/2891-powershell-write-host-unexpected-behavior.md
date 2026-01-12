```yaml
number: 2891
title: PowerShell Write-Host unexpected behavior
type: issue
state: closed
author: Saya47
labels: []
assignees: []
created_at: 2024-09-13T22:38:34Z
updated_at: 2024-09-14T13:36:26Z
url: https://github.com/BurntSushi/ripgrep/issues/2891
synced_at: 2026-01-12T16:13:25Z
```

# PowerShell Write-Host unexpected behavior

---

_@Saya47_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1 (rev 4649aa9700)

### How did you install ripgrep?

scoop

### What operating system are you using ripgrep on?

Windows 11, PowerShell 7.4.2

### Describe your bug.

ripgrep does not read stdin from Write-Host

https://github.com/BurntSushi/ripgrep/issues/643

### What are the steps to reproduce the behavior?

02:05 ~
❯ echo hello | rg y

02:06 ~
✖  echo hello | rg he
hello

02:06 ~
❯ write-host hello | rg y
hello

02:06 ~
✖  write-host hello | rg he
hello

02:07 ~
✖

### What is the actual behavior?

ripgrep does not read stdin from Write-Host

### What is the expected behavior?

ripgrep should work on Write-Host

---

_Comment by @BurntSushi on 2024-09-13 23:00_

What happens if you do `rg y -`?

My guess is that Write-Host isn't writing to stdout. Which means this is expected behavior. But I'm not familiar with Windows too much and I don't know what Write-Host is. Maybe you can do some debugging by comparing with other tools.

---

_Comment by @ltrzesniewski on 2024-09-14 13:03_

- [Write-Host](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-host) writes directly to PowerShell (the host), skipping the pipeline (stdin/stdout).

  > The Write-Host cmdlet's primary purpose is to produce for-(host)-display-only output, such as printing colored text like when prompting the user for input in conjunction with [Read-Host](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/read-host). Write-Host uses the [ToString()](https://learn.microsoft.com/en-us/dotnet/api/system.object.tostring) method to write the output. **By contrast, to output data to the pipeline, use [Write-Output](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output) or implicit output.**

- [Write-Output](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output) writes to the pipeline (stdout), and `echo` is an alias to it.

  > Write-Output sends objects to the primary pipeline, also known as the success stream. To send error objects to the error stream, use Write-Error.

=> ripgrep *can't* work with `Write-Host`, as it doesn't send its value to stdout. You can see this behavior with how ripgrep adds color to its output:

![image](https://github.com/user-attachments/assets/1e7f1a0c-a76e-44ad-947b-cd628884c1ae)


---

_Locked by @ghost on 2024-09-14 13:36_

---
