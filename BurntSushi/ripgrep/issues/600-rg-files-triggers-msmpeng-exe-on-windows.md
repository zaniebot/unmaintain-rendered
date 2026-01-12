```yaml
number: 600
title: rg --files triggers MsMpEng.exe on Windows
type: issue
state: closed
author: chrmarti
labels: []
assignees: []
created_at: 2017-09-07T04:51:45Z
updated_at: 2017-09-18T15:54:49Z
url: https://github.com/BurntSushi/ripgrep/issues/600
synced_at: 2026-01-12T16:13:22Z
```

# rg --files triggers MsMpEng.exe on Windows

---

_@chrmarti_

Version 0.6.0.

Not sure what might cause that, but MsMpEng.exe hogs the CPU and rg.exe makes only very slow progress.

When I use an alternate implementation to list the files in node.js, MsMpEng.exe remains silent or at least doesn't jam the CPU.

---

_Comment by @BurntSushi on 2017-09-07 07:12_

I don't understand this ticket. Please consider that I am not a Windows
user. I don't understand the significance of MsMpEng.exe.

On Sep 7, 2017 12:51 AM, "Christof Marti" <notifications@github.com> wrote:

Version 0.6.0.

Not sure what might cause that, but MsMpEng.exe hogs the CPU and rg.exe
makes only very slow progress.

When I use an alternate implementation to list the files in node.js,
MsMpEng.exe remains silent or at least doesn't jam the CPU.

—
You are receiving this because you are subscribed to this thread.
Reply to this email directly, view it on GitHub
<https://github.com/BurntSushi/ripgrep/issues/600>, or mute the thread
<https://github.com/notifications/unsubscribe-auth/AAb34jsA03YkjNKW713v9ciU3S6JF5NPks5sf3ZigaJpZM4PPSPa>
.


---

_Comment by @chrmarti on 2017-09-07 14:03_

MsMpEng.exe is part of an antispyware. It looks like ripgrep triggers its activity. I would kind of expect this to happen if ripgrep scanned the contents of the files, but with `--files` that should not be the case.

Since using node.js to list all files does not show this behavior, I guess ripgrep must be using some API that triggers it.

---

_Comment by @BurntSushi on 2017-09-07 14:13_

But `--files` does need to read some files. It has to read, for example, `.gitignore`/`.ignore` files to determine which files to filter.

I don't really know what you expect me to do about this.

---

_Comment by @chrmarti on 2017-09-07 14:52_

I'm using `--no-ignore`, that should skip reading of these files I assume. Interestingly the problem stopped appearing on one machine, but still does on a VM.

---

_Comment by @chrmarti on 2017-09-07 15:20_

I have captured traces of ripgrep and the node implementation using ProcMon. One difference is how they open files, ripgrep uses 'Generic Read' while node uses 'Read Attributes'.

ripgrep:
![image](https://user-images.githubusercontent.com/9205389/30170818-fae05508-93a4-11e7-8e14-5e31d04a66a2.png)

node:
![image](https://user-images.githubusercontent.com/9205389/30170533-33db5c32-93a4-11e7-94bb-a052d091c0a1.png)


---

_Comment by @chrmarti on 2017-09-07 15:51_

@BurntSushi I would like to modify it to use 'Read Attributes' to see if maybe 'Generic Read' triggers MsMpEng.exe's scanning, but can't figure out where that is set. (I'm new to Rust.)

---

_Comment by @BurntSushi on 2017-09-18 13:00_

@chrmarti If you aren't searching with memory maps (force it with `--no-mmap`), then the `File::open` call is here: https://github.com/BurntSushi/ripgrep/blob/beb010d004a79eedb8baa6f4eab21532e83a0ef0/src/worker.rs#L221 You can presumably change the open attributes using [`std::fs::OpenOptions`](https://doc.rust-lang.org/std/fs/struct.OpenOptions.html). You may need to use a platform specific `OpenOptionsExt` trait for Windows to set Windows-specific attributes though.

You could also try forcing the issue with `--mmap` to open files with memory maps.

It occurs to me that this is all for opening files to search. For opening `.gitignore` files, I believe the `File::open` call is here: https://github.com/BurntSushi/ripgrep/blob/0668c74ed49320c4224c4ea526aca07692bf590a/ignore/src/gitignore.rs#L351

---

_Closed by @BurntSushi on 2017-09-18 15:54_

---
