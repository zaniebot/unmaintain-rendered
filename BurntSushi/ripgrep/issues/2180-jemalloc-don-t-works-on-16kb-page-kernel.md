```yaml
number: 2180
title: "jemalloc don't works on 16KB page kernel"
type: issue
state: closed
author: 12101111
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2022-04-14T15:30:22Z
updated_at: 2023-04-22T16:14:06Z
url: https://github.com/BurntSushi/ripgrep/issues/2180
synced_at: 2026-01-12T16:13:24Z
```

# jemalloc don't works on 16KB page kernel

---

_@12101111_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

Gentoo: emerge sys-apps/ripgrep

#### What operating system are you using ripgrep on?

Gentoo Base System release 2.8 aarch64
Apple MacBook Pro (16-inch, M1 Max, 2021)
Linux 5.17.0-rc7-asahi-next-20220310+

#### Describe your bug.

```
<jemalloc>: Unsupported system page size
<jemalloc>: Unsupported system page size
memory allocation of 5 bytes failed
zsh: abort (core dumped)  rg a
```

#### What are the steps to reproduce the behavior?

Built ripgrep on 4K page kernel, and run it on 16KB page kernel

#### What is the actual behavior?

It crash.

#### What is the expected behavior?

Many other malloc implementation does work on 16KB kernel and is better than jemaloc in code size and performance, such as mimalloc or rpmalloc.

See also: https://github.com/AsahiLinux/docs/wiki/Software-known-to-have-issues-with-16k-page-size

---

_Comment by @BurntSushi on 2022-04-14 15:37_

This is ripgrep's tracker, not jemalloc's. I think you need to report this issue there.

The best work-around for you at the moment is probably to build ripgrep without jemalloc. Currently, jemalloc is only used when building for Linux with musl as the libc. Otherwise, it uses the system allocator.

You could, for example, remove:

https://github.com/BurntSushi/ripgrep/blob/ced5b92aa93eb47e892bd2fd26ab454008721730/Cargo.toml#L59-L60

and remove:

https://github.com/BurntSushi/ripgrep/blob/ced5b92aa93eb47e892bd2fd26ab454008721730/crates/core/main.rs#L42-L44

And then ripgrep should use whatever system allocator you have setup via your glibc. You might have perf regressions though. glibc's allocator has been fine in my experiments, but musl's has had noticeable perf problems.

---

_Closed by @BurntSushi on 2022-04-14 15:37_

---

_Label `enhancement` added by @BurntSushi on 2022-04-14 15:37_

---

_Label `wontfix` added by @BurntSushi on 2022-04-14 15:37_

---

_Comment by @teohhanhui on 2023-04-22 16:14_

Looks like jemalloc just needs to be configured with `--with-lg-page` to support 16K page size.

This can be achieved by setting the environment variable:

`JEMALLOC_SYS_WITH_LG_PAGE=14` (16K page size)

Or to support 64K page size as well:

`JEMALLOC_SYS_WITH_LG_PAGE=16` (64K page size)

In either case, it'd still work for 4K/16K(/64K) page sizes.

---
