```yaml
number: 705
title: Ripgrep not working in OneDrive folders using Files On Demand
type: issue
state: closed
author: roblourens
labels: []
assignees: []
created_at: 2017-12-04T02:58:08Z
updated_at: 2018-02-06T05:12:24Z
url: https://github.com/BurntSushi/ripgrep/issues/705
synced_at: 2026-01-12T16:13:22Z
```

# Ripgrep not working in OneDrive folders using Files On Demand

---

_@roblourens_

Ref: https://github.com/Microsoft/vscode/issues/35433

Files On Demand is a new OneDrive feature that only downloads files to the local disk when needed. It has caused some issues in Node/Electron land - https://github.com/nodejs/node/issues/12737, https://github.com/Microsoft/vscode/issues/27285.

When running ripgrep in a folder with this feature enabled, I just get `Access is denied. (os error 5)` immediately. Running with `--debug` doesn't produce any clues. I will investigate if you can point me in the right place to start looking?

---

_Comment by @BurntSushi on 2017-12-04 03:04_

I'm on mobile and headed to bed at the moment, but the first thing that
springs to mind is memory maps. If you run rg with --no-mmap (--mmap does
the opposite) then it will avoid using memory maps and instead use standard
file APIs. Linux, for example, has an issue where virtual files can't be
memory mapped. ripgrep detects this by looking at file size. If the size is
zero (which is what virtual files like /proc/cpuinfo report), then it
forcefully disables use of memory maps and everything works.

If memory maps are indeed the problem, and if there is a way to tell that
memory maps won't work with a stat call or similar then we can change
ripgrep's heuristics on Windows.

If this isn't the problem, then I personally would probably try to
reproduce the problem using a minimal Rust program, and then start
diagnosing from there. If it comes to that, how easy would it be for me to
reproduce the bug on my own Windows machine?

On Dec 3, 2017 9:58 PM, "Rob Lourens" <notifications@github.com> wrote:

> Ref: Microsoft/vscode#35433
> <https://github.com/Microsoft/vscode/issues/35433>
>
> Files On Demand is a new OneDrive feature that only downloads files to the
> local disk when needed. It has caused some issues in Node/Electron land -
> nodejs/node#12737 <https://github.com/nodejs/node/issues/12737>,
> Microsoft/vscode#27285 <https://github.com/Microsoft/vscode/issues/27285>.
>
> When running ripgrep in a folder with this feature enabled, I just get Access
> is denied. (os error 5) immediately. Running with --debug doesn't produce
> any clues. I will investigate if you can point me in the right place to
> start looking?
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/705>, or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oBLIpawThhCJmvauqV1SEgWmK2Uks5s81_BgaJpZM4Q0C1B>
> .
>


---

_Comment by @roblourens on 2017-12-04 03:14_

I get the same error with `--no-mmap`. Here is a fix in libuv for this onedrive feature: https://github.com/libuv/libuv/pull/1522/files, if this is a similar problem, it's probably in Rust's `fs`.

Repro steps are basically 

- Install the latest updates for Windows and OneDrive
- Open OneDrive settings, enable "Files On-Demand"
- Wait a couple minutes for OneDrive to do it's thing
- Invoke ripgrep in a OneDrive folder

---

_Comment by @BurntSushi on 2017-12-04 03:32_

Thanks!

cc @retep998 Do you have any insight on this with respect to std?

---

_Comment by @roblourens on 2017-12-04 06:05_

Filed https://github.com/rust-lang/rust/issues/46484

---

_Comment by @roblourens on 2018-01-22 02:47_

Since there isn't much progress on the upstream issue, I'm trying to work around it in the ripgrep build for vscode. Would you be interested in a PR? The workaround is fairly simple, just need to check the `file_attributes` of a directory for the reparse point and directory flags: https://msdn.microsoft.com/en-us/library/windows/desktop/gg258117.aspx. And it needs to be done in a few places in the walkdir and ripgrep repos.

---

_Comment by @BurntSushi on 2018-01-29 18:58_

@roblourens Sorry for the late response, but yes, I would definitely be interested in a patch to ripgrep proper. If I had more Windows expertise, then I'd be happy to push this through upstream too, but I just don't have the bandwidth at the moment, so I'm happy to work around it.

---

_Comment by @BurntSushi on 2018-02-02 01:00_

@roblourens I am working on a fix to this in `walkdir` and `ignore`. I may have convinced someone to fix it in `std` as well, but even if the latter were fixed and merged tonight, ripgrep wouldn't see it for months.

---

_Comment by @roblourens on 2018-02-02 01:02_

Awesome! Sorry I never delivered on my drive-by PR offer, it's been on my list but I was too busy in January.

---

_Comment by @BurntSushi on 2018-02-02 01:08_

@roblourens No worries! The Windows side of things is pretty straight-forward, but fixing the code itself is a little gnarly!

---

_Closed by @BurntSushi on 2018-02-02 02:15_

---

_Comment by @BurntSushi on 2018-02-02 02:16_

Got it! I manually tested the fix. I can search directories on OneDrive using ripgrep master.

---

_Comment by @roblourens on 2018-02-06 05:12_

This works great. Thanks! Will update in vscode ASAP.

---
