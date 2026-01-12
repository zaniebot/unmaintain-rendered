```yaml
number: 2943
title: possibility of Providing binaries for Windows arm 64?
type: issue
state: closed
author: mzanm
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2024-12-02T14:47:42Z
updated_at: 2025-09-20T01:08:34Z
url: https://github.com/BurntSushi/ripgrep/issues/2943
synced_at: 2026-01-12T16:13:25Z
```

# possibility of Providing binaries for Windows arm 64?

---

_@mzanm_

Hi,
It would be great to have native Windows ARM64 binaries available. While Windows on ARM can emulate x86 and x64 binaries, this comes with a performance overhead.
Thanks so much for considering this!

---

_Label `enhancement` added by @BurntSushi on 2024-12-02 14:48_

---

_Comment by @BurntSushi on 2024-12-02 14:49_

I guess I'm open to this, but someone will likely need to do the leg work to add it to the release pipeline _and test that it works_.

---

_Comment by @chadbaldwin on 2025-01-09 19:28_

I was hoping I might be able to give this a shot, but it seems Github only offers their Windows ARM64 hosted runners under their Teams and Enterprise plans.

I do, however, see that rust has an arm64 (emulated) target (`arm64ec-pc-windows-msvc`), so that's at least half way there...

Given that there's currently no free-tier Windows ARM64 hosted instance, I assume this probably isn't on the horizon and would have to be something done locally or maintained separately by someone else until it can be baked into the official test/release pipeline?

---

_Comment by @BurntSushi on 2025-01-09 19:30_

That sounds about right assuming that background research is right. And for this to be something I'm willing to accept, I need to 1) be able to maintain it and 2) rely on it to work somewhat reliably. Debugging CI on release failures is a big time sink and probably why I don't do releases more often. So for example, "hey let's depend on this GitHub Action that was just published and has 5 commits" probably won't fly.

---

_Comment by @jcotton42 on 2025-04-21 23:27_

There is now a free Windows arm64 runner for GitHub Actions https://blogs.windows.com/windowsdeveloper/2025/04/14/github-actions-now-supports-windows-on-arm-runners-for-all-public-repos/

---

_Comment by @jcotton42 on 2025-04-23 08:16_

As a followup, I'd be willing to take a shot at this.

It will be potentially a few weeks before I can get my hands on an arm64 Windows machine for full testing though. 

---

_Comment by @jcotton42 on 2025-04-24 05:20_

Found a blocker: the rustup action used by ripgrep fails on Windows ARM https://github.com/dtolnay/rust-toolchain/issues/143. Seems that the ARM image doesn't bundle rustup, unlike the x64 image.

---

_Comment by @jcotton42 on 2025-04-29 20:11_

That action has now been updated to work on arm64 Windows.

---

_Label `rollup` added by @BurntSushi on 2025-07-27 16:51_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
