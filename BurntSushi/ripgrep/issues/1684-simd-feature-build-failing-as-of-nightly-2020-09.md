```yaml
number: 1684
title: SIMD Feature Build Failing as of nightly-2020-09-15 (on x86_64)
type: issue
state: closed
author: danieldjewell
labels:
  - wontfix
assignees: []
created_at: 2020-09-19T21:51:28Z
updated_at: 2020-09-19T22:53:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1684
synced_at: 2026-01-12T16:13:24Z
```

# SIMD Feature Build Failing as of nightly-2020-09-15 (on x86_64)

---

_@danieldjewell_

Opening issue here after tracking it upstream:

Build of ripgrep failing as of nightly-2020-09-15 because of a build failure in packed_simd: https://github.com/rust-lang/packed_simd/issues/288 

This, in turn, appears to have been caused by https://github.com/rust-lang/stdarch/pull/890


---

_Comment by @danieldjewell on 2020-09-19 21:55_

Also see https://github.com/rust-lang/rust/pull/76295

---

_Comment by @BurntSushi on 2020-09-19 22:34_

Thanks, but as you've helpfully linked, this isn't really an issue with ripgrep and the `simd` feature is nightly only for exactly this reason.

In any case, I'd suggest not using the `simd` feature unless you know you need it. The only benefit you get is when searching UTF-16 files. (ripgrep uses lots of SIMD in other places, but decoding UTF-16 is maintained by others and refuses to use the non-portable but stable SIMD APIs.)

---

_Closed by @BurntSushi on 2020-09-19 22:34_

---

_Label `wontfix` added by @BurntSushi on 2020-09-19 22:34_

---

_Comment by @danieldjewell on 2020-09-19 22:42_

>In any case, I'd suggest not using the simd feature unless you know you need it. The only benefit you get is when searching UTF-16 files.

This is great info! Thank you for the clarification. 

Perhaps this would be good to add into the readme? Maybe somewhere in here: :grin:

https://github.com/BurntSushi/ripgrep/blob/f511849c818024817df9760ef5cd6709a5e28fe3/README.md#L369-L390

---

_Comment by @BurntSushi on 2020-09-19 22:53_

It is already, right in the section you quoted:

> The simd-accel feature enables SIMD support in certain ripgrep dependencies (responsible for transcoding). They are not necessary to get SIMD optimizations for search; those are enabled automatically. Hopefully, some day, the simd-accel feature will similarly become unnecessary.

---

_Comment by @BurntSushi on 2020-09-19 22:53_

If you think it could be clearer, please submit a PR with a clarification. :)

---
