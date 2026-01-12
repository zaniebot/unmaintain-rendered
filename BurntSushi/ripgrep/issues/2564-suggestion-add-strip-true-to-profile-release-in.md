```yaml
number: 2564
title: "Suggestion: add strip = true to [profile.release] in Cargo.toml instead of calling `strip` in .github/workflows/release.yml"
type: issue
state: closed
author: jennydaman
labels: []
assignees: []
created_at: 2023-07-22T05:05:00Z
updated_at: 2023-07-22T09:42:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2564
synced_at: 2026-01-12T16:13:24Z
```

# Suggestion: add strip = true to [profile.release] in Cargo.toml instead of calling `strip` in .github/workflows/release.yml

---

_@jennydaman_

Cargo can strip binaries for you, simplifying your CI yamls just a bit.

See https://github.com/johnthagen/min-sized-rust#strip-symbols-from-binary

https://github.com/BurntSushi/ripgrep/blob/304a60e8e9d4b2a42dc3dfb1ba4cef6d7bf92515/.github/workflows/release.yml#L128-L140

---

_Comment by @BurntSushi on 2023-07-22 09:42_

Yes I'm aware. But I don't want stripping happening automatically for every build. Just CI release builds. Probably the answer to that is a new named profile, which is somewhat newer at the time scales that I move at.

Thanks for the suggestion.

---

_Closed by @BurntSushi on 2023-07-22 09:42_

---
