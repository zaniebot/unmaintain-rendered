```yaml
number: 2645
title: "Append `--pcre2-version` info to `--version`"
type: issue
state: closed
author: ltrzesniewski
labels:
  - rollup
assignees: []
created_at: 2023-11-05T22:20:37Z
updated_at: 2023-11-25T20:03:57Z
url: https://github.com/BurntSushi/ripgrep/issues/2645
synced_at: 2026-01-12T16:13:24Z
```

# Append `--pcre2-version` info to `--version`

---

_@ltrzesniewski_

This is a really minor "feature" request, but I figured it's the right time to ask it since you're already moving stuff around in #2626 🙂

It would be nice to have *all* the info about ripgrep's build be output when `--version` is called, including the PCRE2 info without having to call `--pcre2-version` in a second step.

This may require changing the *"JIT is available"* message a bit to make it clear it's about the PCRE2 JIT, or just put everything in a single line such as: `PCRE2 10.42 +JIT`

I wondered why there's a separate `--pcre2-version` option in the first place, then I noticed it returns an error code when PCRE2 is unavailable, so I suppose this option will need to be kept as-is.

I can send a PR if you'd like after you're done with #2626.

---

_Comment by @BurntSushi on 2023-11-22 00:18_

Can you say why you want this?

---

_Comment by @ltrzesniewski on 2023-11-22 19:50_

Mostly because I feel this info is missing in `--version`: IMO it should provide all the important info about the build. It currently tells you if you enabled the `simd-accel` feature, but it doesn't tell you about `pcre2`, which is inconsistent. And I wanted an easy way to make sure I didn't forget to add `--features pcre2` in a custom build.

Also, I'm familiar with PCRE2 and knowing which version is shipped with ripgrep and if the JIT is enabled is a useful info.


---

_Comment by @BurntSushi on 2023-11-22 21:22_

All righty. I don't feel too strongly about this, and I do mildly fancy having `--version` be more complete.

This is what the new `--version` output will look like in ripgrep 14 when PCRE2 is enabled:

```
$ rg --version
ripgrep 13.0.0 (rev 204a79f168)

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```

And when PCRE2 is not enabled:

```
$ rg --version
ripgrep 13.0.0 (rev 204a79f168)

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

---

_Label `rollup` added by @BurntSushi on 2023-11-22 21:25_

---

_Comment by @ltrzesniewski on 2023-11-22 21:42_

Thanks! 🙂

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
