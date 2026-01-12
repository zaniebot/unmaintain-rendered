```yaml
number: 2793
title: cursor disappearing
type: issue
state: closed
author: FedericoMuciaccia
labels: []
assignees: []
created_at: 2024-04-30T23:39:19Z
updated_at: 2025-10-11T01:37:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2793
synced_at: 2026-01-12T16:13:24Z
```

# cursor disappearing

---

_@FedericoMuciaccia_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.


### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Debian 12

### Describe your bug.

the terminal cursor in the prompt  is strangely disappearing after ripgrep produces a lot of output.

### What are the steps to reproduce the behavior?

```
cd ~
rg :q # this will produce a small output and will not give problems
rg --hidden :q # this will produce a lot of output
```
from now on the cursor in the terminal prompt is invisible (I've tried different terminal emulators: xfce-terminal & alacritty)


### What is the actual behavior?

a lot of lines similar to this:

```
rg: DEBUG|grep_printer::standard|/home/federico/.cargo/registry/src/index.crates.io-6f17d22bba15001f/grep-printer-0.2.1/src/standard.rs:892: ignoring .local/share/containers/storage/vfs/dir/d9f93c86bb90b5af46b15fcb63a1e3d06a06d044c86ed255b706f537940c464d/usr/share/locale/sv/LC_MESSAGES/apt.mo: found binary data at offset 4
```

### What is the expected behavior?

the terminal cursor after the ripgrep invocation should behave normally (be visible and movable)

---

_Comment by @BurntSushi on 2024-05-01 00:07_

Please provide a reproduction. A reproduction includes all of the steps and data necessary for someone else to see the same problem that you see. You've also emitted a heap of relevant debug output. For example, whether a config file is being used and any additional options it is setting.

(It is likely that some of your files are emitting terminal control sequences. ripgrep doesn't and will not filter those out.)

---

_Closed by @BurntSushi on 2025-10-11 01:37_

---
