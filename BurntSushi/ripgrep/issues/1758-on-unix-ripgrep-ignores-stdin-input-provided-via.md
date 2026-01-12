```yaml
number: 1758
title: On Unix, ripgrep ignores stdin input provided via .NET 5.0 APIs 
type: issue
state: closed
author: mklement0
labels:
  - duplicate
assignees: []
created_at: 2020-12-10T18:30:52Z
updated_at: 2020-12-10T19:03:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1758
synced_at: 2026-01-12T16:13:24Z
```

# On Unix, ripgrep ignores stdin input provided via .NET 5.0 APIs 

---

_@mklement0_

#### What version of ripgrep are you using?

`ripgrep 12.1.1`

#### How did you install ripgrep?

Homebrew on macOS, `snap` on Ubuntu (latest version there is `12.1.0`).

#### What operating system are you using ripgrep on?

macOS 10.15.7
Ubuntu 18.04

#### Describe your bug.

`rg` ignores stdin input provided via .NET 5.0 APIs (no other programs I'm aware of have this problem; e.g., `grep` works fine). Windows is _not_ affected.

#### What are the steps to reproduce the behavior?

PowerShell 7.1:

```powershell
$psi = [System.Diagnostics.ProcessStartInfo] @{
  FileName = 'rg'
  RedirectStandardInput = $true
  Arguments = 'hi'
}

$ps = [System.Diagnostics.Process]::Start($psi)

$ps.StandardInput.WriteLine('hi there');
$ps.StandardInput.Close()

$ps.WaitForExit()
```

C# with .NET 5.0.101 SDK:

```csharp
using System;
using System.Diagnostics;

namespace demo
{
  static class ConsoleApp
  {
    static void Main(string[] args)
    {
      var psi = new ProcessStartInfo { FileName = "rg", RedirectStandardInput = true, Arguments = "hi" };
      var ps = Process.Start(psi);

      ps.StandardInput.WriteLine("hi there");
      ps.StandardInput.Close();

      ps.WaitForExit();

    }

  }
}

```


#### What is the actual behavior?

`rg` behaves as if no stdin input had been specified and searches through the files in the current directory subtree.


#### What is the expected behavior?

The output should be `hi there`, just like when you run `echo 'hi there' | rg hi` from any **POSIX-like shell**.

However, since PowerShell [Core] is built on .NET, **this command does _not_ work in PowerShell v7.1**

---

_Comment by @BurntSushi on 2020-12-10 19:02_

This looks like a duplicate of #1741, which was fixed by #1742. 

Also note that I do not recommend using Snap to install ripgrep. It has resulted in all sorts of weird problems. (Searching the issue tracker should turn up prior discussions.)

---

_Closed by @BurntSushi on 2020-12-10 19:02_

---

_Label `duplicate` added by @BurntSushi on 2020-12-10 19:03_

---
