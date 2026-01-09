---
number: 5308
title: "Show symbolic links in `uv python list`"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-07-22T20:26:03Z
updated_at: 2024-10-24T09:56:31Z
url: https://github.com/astral-sh/uv/issues/5308
synced_at: 2026-01-07T13:12:17-06:00
---

# Show symbolic links in `uv python list`

---

_Issue opened by @zanieb on 2024-07-22 20:26_

Per comment at https://github.com/astral-sh/uv/issues/5210#issuecomment-2243750947 we should show if a path links to another to help identify duplicates that are the same interpreter.

---

_Label `cli` added by @zanieb on 2024-07-22 20:26_

---

_Referenced in [astral-sh/uv#5210](../../astral-sh/uv/issues/5210.md) on 2024-07-22 20:26_

---

_Comment by @eth3lbert on 2024-07-23 15:08_

Does this mean we want to display the result in a format similar to the following?

canonicalized:

```
:) uv python list
warning: `uv python list` is experimental and may change without warning
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python3.12 -> /nix/store/3swy1vadi125g0c1vxqp8ykdr749803j-python3-3.12.4/bin/python3.12
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python3 -> /nix/store/3swy1vadi125g0c1vxqp8ykdr749803j-python3-3.12.4/bin/python3.12
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python -> /nix/store/3swy1vadi125g0c1vxqp8ykdr749803j-python3-3.12.4/bin/python3.12
cpython-3.12.4-macos-aarch64-none     <download available>
cpython-3.11.9-macos-aarch64-none     <download available>
cpython-3.10.14-macos-aarch64-none    <download available>
cpython-3.9.19-macos-aarch64-none     <download available>
cpython-3.9.6-macos-aarch64-none      /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> /Applications/Xcode.app/Contents/Developer/Library/Frameworks/Python3.framework/Versions/3.9/bin/python3.9
cpython-3.8.19-macos-aarch64-none     <download available>

```

or just read_link:
```
:)  uv python list
warning: `uv python list` is experimental and may change without warning
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python3.12 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3.12
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python3 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3
cpython-3.12.4-macos-aarch64-none     /etc/profiles/per-user/eth/bin/python -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python
cpython-3.12.4-macos-aarch64-none     <download available>
cpython-3.11.9-macos-aarch64-none     <download available>
cpython-3.10.14-macos-aarch64-none    <download available>
cpython-3.9.19-macos-aarch64-none     <download available>
cpython-3.9.6-macos-aarch64-none      /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
cpython-3.8.19-macos-aarch64-none     <download available>
```

---

_Comment by @zanieb on 2024-07-23 15:14_

Yeah that's the format I'm thinking. Probably `read_link`? I'm not sure I have a strong preference.

---

_Comment by @eth3lbert on 2024-07-23 15:30_

Yeah, initially, I thought about using `read_link` to display output similar to `ls -l`. However, after rereading the comment,
> For instance, several of the above items point to the same executable

I'm not sure if `read_link` can effectively indicate that multiple items point to the same executable.

Otherwise, I would also lean towards using `read_link`.

---

_Comment by @zanieb on 2024-07-23 15:37_

It seems nice to match `ls -l` — does it show relative paths in that format when you're in a different working directory?

I don't mind if we don't fully resolve whether or not they refer to the same executable, I think. We could add a `--canonical-paths` flag in the future which canonicalizes everything and collapses duplicates for that use-case.

---

_Comment by @eth3lbert on 2024-07-23 15:52_

> does it show relative paths in that format when you're in a different working directory?

Yes, it should be!

```
/tmp/example
:) command ls -l /Applications/Xcode.app/Contents/Developer/usr/bin/python3
lrwxr-xr-x 1 root wheel 67 Mar 31 14:21 /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
/tmp/example
:)  cd -

/tmp/example/a/b/c/d
:) command ls -l /Applications/Xcode.app/Contents/Developer/usr/bin/python3
lrwxr-xr-x 1 root wheel 67 Mar 31 14:21 /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
```
```
/tmp/example
:)  uv python list --only-installed
warning: `uv python list` is experimental and may change without warning
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python3.12 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3.12
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python3 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python
cpython-3.9.6-macos-aarch64-none     /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
/tmp/example
:)  cd -

/tmp/example/a/b/c/d
:)  uv python list --only-installed
warning: `uv python list` is experimental and may change without warning
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python3.12 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3.12
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python3 -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python3
cpython-3.12.4-macos-aarch64-none    /etc/profiles/per-user/eth/bin/python -> /nix/store/jk3hlbzzklsyxh0cminfqw1mg57yqqs7-home-manager-path/bin/python
cpython-3.9.6-macos-aarch64-none     /Applications/Xcode.app/Contents/Developer/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
```

---

_Referenced in [astral-sh/uv#5343](../../astral-sh/uv/pulls/5343.md) on 2024-07-23 16:13_

---

_Closed by @zanieb on 2024-07-23 16:41_

---

_Closed by @zanieb on 2024-07-23 16:41_

---

_Referenced in [astral-sh/uv#6690](../../astral-sh/uv/issues/6690.md) on 2024-08-27 14:37_

---

_Comment by @cbrnr on 2024-10-24 09:56_

Would it be possible to add an option to hide symlinks? Usually, I just want to find out which Python versions are installed, and adding symlinks to the list makes it more difficult to see the unique versions.

---

_Referenced in [astral-sh/uv#12302](../../astral-sh/uv/issues/12302.md) on 2025-03-18 23:07_

---
