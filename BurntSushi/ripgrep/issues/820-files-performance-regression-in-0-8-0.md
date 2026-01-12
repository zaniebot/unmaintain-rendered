```yaml
number: 820
title: "--files performance regression in 0.8.0"
type: issue
state: closed
author: chrmarti
labels:
  - bug
assignees: []
created_at: 2018-02-19T15:17:49Z
updated_at: 2018-02-21T00:59:38Z
url: https://github.com/BurntSushi/ripgrep/issues/820
synced_at: 2026-01-12T16:13:22Z
```

# --files performance regression in 0.8.0

---

_@chrmarti_

#### What version of ripgrep are you using?

0.8.0

#### What operating system are you using ripgrep on?

Windows 10

#### If this is a bug, what are the steps to reproduce the behavior?

Using an old commit of the Chromium sources (`git clone https://chromium.googlesource.com/chromium/src`) as corpus (~230k files).

With 0.8.0 `--files` runs about 4x longer:
```
PS C:\devel\testWorkspace> (Measure-Command { & 'C:\devel\ripgrep-0.8.0-x86_64-pc-windows-msvc\rg.exe' --files | Measure-Object –Line | Out-Default }).TotalSeconds

 Lines Words Characters Property
 ----- ----- ---------- --------
234319 [files]

43.309153 [seconds]
``` 

Than with 0.7.1:
```
PS C:\devel\testWorkspace> (Measure-Command { & 'C:\devel\ripgrep-0.7.1-x86_64-pc-windows-msvc\rg.exe' --files | Measure-Object –Line | Out-Default }).TotalSeconds

 Lines Words Characters Property
 ----- ----- ---------- --------
234319 [files]

11.2432563 [seconds]
```

Using Process Monitor it looks like 0.8.0 accesses individual files a lot more:
![image](https://user-images.githubusercontent.com/9205389/36384556-fa76474a-158f-11e8-9c7a-208f270c1b0f.png)

Than 0.7.1, which seems to only access folders:
![image](https://user-images.githubusercontent.com/9205389/36384611-27245f16-1590-11e8-8a2e-b54f6abe0b1c.png)

/cc @roblourens

---

_Comment by @BurntSushi on 2018-02-20 01:55_

@chrmarti Thanks for another great bug report! I can indeed reproduce this. Your tip about ripgrep stat'ing more files is right on the money. Basically, this is fallout from fixing #705. In particular, in order to fix #705, I had to work-around the corresponding bug in Rust's standard library, and I was apparently careless enough that I introduced an extra stat call for each file that ripgrep visits. This should be possible to fix, but I'll need to take a closer look tomorrow.

Assuming I get this fixed, and after I get some of the PR backlog cleared, I can put out another release.

---

_Comment by @BurntSushi on 2018-02-20 01:57_

Note that this regression is *also* in walkdir, so it impacts both single threaded and multi-threaded directory traversal.

---

_Label `bug` added by @BurntSushi on 2018-02-20 12:42_

---

_Closed by @BurntSushi on 2018-02-21 00:50_

---

_Comment by @BurntSushi on 2018-02-21 00:59_

This should be fixed in master. I manually QAed this and confirmed that performance is back to `0.7.1` levels for your specific test case. (Both single and multi threaded.)

If all goes well, I'm going to try and squeak a 0.8.1 release out tonight.

---
