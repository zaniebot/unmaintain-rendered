```yaml
number: 126
title: win10 crashes
type: issue
state: closed
author: habamax
labels: []
assignees: []
created_at: 2016-09-28T11:38:51Z
updated_at: 2016-09-28T20:05:24Z
url: https://github.com/BurntSushi/ripgrep/issues/126
synced_at: 2026-01-12T18:23:11Z
```

# win10 crashes

---

_@habamax_

Downloaded  v0.2.1

ripgrep-0.2.1-x86_64-pc-windows-msvc.zip

It gives 
![rg_win10_crash](https://cloud.githubusercontent.com/assets/234774/18912069/40cfe2d4-8589-11e6-96f2-10b6f9f95e6d.png)


---

_Comment by @BurntSushi on 2016-09-28 11:48_

Honestly, I have no idea how to debug this. I can download that same file and run it on my new Windows 10 laptop just fine.

Can you try the GNU version?


---

_Comment by @BurntSushi on 2016-09-28 11:49_

Also, for the MSVC version, do you have the [Microsoft VC++ 2015 redistributable](https://www.microsoft.com/en-us/download/details.aspx?id=48145) installed?


---

_Comment by @BurntSushi on 2016-09-28 11:49_

And finally, how are you running `rg.exe`?


---

_Comment by @habamax on 2016-09-28 11:50_

GNU version works


---

_Comment by @habamax on 2016-09-28 11:51_

```
 cmd.exe
```

then

```
 rg
```


---

_Comment by @habamax on 2016-09-28 11:54_

I guess not, I don't have MS redistributable files.


---

_Comment by @BurntSushi on 2016-09-28 20:05_

All right, I'm going to close this. I've heard there's a way to bundle the VC++ redistributable, but I don't quite know how to do that.


---

_Closed by @BurntSushi on 2016-09-28 20:05_

---
