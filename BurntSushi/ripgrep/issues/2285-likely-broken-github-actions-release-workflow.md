```yaml
number: 2285
title: likely broken github actions release workflow
type: issue
state: closed
author: jxs
labels:
  - bug
assignees: []
created_at: 2022-08-17T10:58:54Z
updated_at: 2023-11-24T19:20:09Z
url: https://github.com/BurntSushi/ripgrep/issues/2285
synced_at: 2026-01-12T16:13:24Z
```

# likely broken github actions release workflow

---

_@jxs_

#### Describe your bug.

`cross-rs` changed it's [behavior](https://github.com/cross-rs/cross/issues/969#issuecomment-1193204537), which will break  the [release.yml](https://github.com/BurntSushi/ripgrep/blob/master/.github/workflows/release.yml) workflow.  
I fixed this on my use case by [deprecating `cross-rs` and just building on the native workers](https://github.com/rust-db/refinery/commit/3650a0dcdb9431aca63cf603db85856a59629902), if that works for `ripgrep` I can try submitting a PR similar to what I did.

---

_Renamed from "broken github actions release workflow" to "likely broken github actions release workflow" by @BurntSushi on 2022-08-17 12:37_

---

_Comment by @BurntSushi on 2022-08-17 12:42_

I'm not sure what's going on with Cross to be honest, but the issue you linked seems specific to Windows. So the release workflow might need to be updated to *not* use `cross` in that case. Which is fine. We don't need `cross` there.

But ripgrep builds musl and arm targets. Using cross there is a big advantage. Your CI release doesn't produce arm builds, so you don't care about Cross for that. You do have musl builds, which you make possible by explicitly installing `musl-tools` (ripgrep's CI does not do that). IIRC, the other complication with ripgrep and musl specifically is the use of jemalloc. But I can't remember the details other than that Cross made everything just work.

I'll leave this open since it looks like there's probably some work/testing to be done. And I have contemplated moving off of Cross because of the recent changes, but setting up cross compiler toolchains is no fun either.

---

_Comment by @BurntSushi on 2022-08-17 12:43_

Yeah, the normal CI only uses Cross when `matrix.target` is set: https://github.com/BurntSushi/ripgrep/blob/387df97d85291f75511dc77329395931af348b34/.github/workflows/ci.yml#L106

But the release workflow uses Cross unconditionally. I'm not sure why that difference exists, but I'd guess that the release workflow should also have `if: matrix.target != ''`.

---

_Comment by @BurntSushi on 2023-11-24 19:20_

The release was indeed broken for this reason and others. I ended up doing some fairly extensive surgery, but it should be working again.

---

_Closed by @BurntSushi on 2023-11-24 19:20_

---

_Label `bug` added by @BurntSushi on 2023-11-24 19:20_

---
