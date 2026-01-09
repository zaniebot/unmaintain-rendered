---
number: 13740
title: "Revisit and add `SAFETY` annotations to uses of `unsafe` in trampoline code"
type: issue
state: open
author: jtfmumm
labels:
  - documentation
  - enhancement
assignees: []
created_at: 2025-05-30T19:29:06Z
updated_at: 2025-06-02T00:16:19Z
url: https://github.com/astral-sh/uv/issues/13740
synced_at: 2026-01-07T13:12:18-06:00
---

# Revisit and add `SAFETY` annotations to uses of `unsafe` in trampoline code

---

_Issue opened by @jtfmumm on 2025-05-30 19:29_

The `uv-trampoline` crate has numerous uncommented instances of `unsafe`. We should (1) re-evaluate whether we actually need these and (2) add `SAFETY:` comments to any that we keep. 

This code started as a fork of [posy trampolines](https://github.com/njsmith/posy/tree/dda22e6f90f5fefa339b869dd2bbe107f5b48448/src/trampolines/windows-trampolines/posy-trampoline). It used unsafe as a means to keep the binary size very small (by avoiding panicking branches). But we should determine if those choices still make sense.

When adding `SAFETY:` comments, @BurntSushi has recommended that we follow this blueprint: https://jack.wrenn.fyi/blog/safety-hygiene/.

---

_Label `documentation` added by @jtfmumm on 2025-05-30 19:29_

---

_Label `enhancement` added by @jtfmumm on 2025-05-30 19:29_

---

_Label `enhancement` removed by @charliermarsh on 2025-06-02 00:16_

---

_Label `enhancement` added by @charliermarsh on 2025-06-02 00:16_

---
