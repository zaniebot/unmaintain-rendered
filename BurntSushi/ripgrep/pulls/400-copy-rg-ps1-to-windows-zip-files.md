```yaml
number: 400
title: Copy _rg.ps1 to windows zip files
type: pull_request
state: closed
author: dstcruz
labels: []
assignees: []
base: master
head: patch-1
created_at: 2017-03-09T13:30:08Z
updated_at: 2017-03-09T14:13:31Z
url: https://github.com/BurntSushi/ripgrep/pull/400
synced_at: 2026-01-12T18:23:13Z
```

# Copy _rg.ps1 to windows zip files

---

_@dstcruz_

Please help me verify that the `_rg.ps1` file in the appveyor script is correct.

---

_Comment by @BurntSushi on 2017-03-09 13:31_

I fear that this may not be quite right. Did you confirm this works in a local build? At least on Darwin and Linux, the generated completion files are nestled a bit further down: https://github.com/BurntSushi/ripgrep/blob/master/ci/before_deploy.sh#L23

---

_Comment by @BurntSushi on 2017-03-09 13:32_

(Unfortunately, CI isn't going to test this for us because this code doesn't trigger until a tagged release. If it's too much trouble, I can probably just take this on when I do the next release and iterate until it works.)

---

_Comment by @dstcruz on 2017-03-09 13:38_

I must admit to being completely ignorant about all things rust. I did install rust/cargo on my machine and ran the `cargo build --release` command that I saw in the AppVeyor script. However, I did not get an `rg.exe` file in `target\release\`. I can see than the shell completion file is in `target\release\build\ripgrep-[some-hash]\out\`. I probably wrongly assumed that they would be at the same level as the `rg.exe`.

I see that the AppVeyor script is [currently running?](https://ci.appveyor.com/project/BurntSushi/ripgrep/build/1.0.313/job/p6714xn58pnfylgh). Can we wait to see what error that might throw? Better yet, is there any way to explore the filesystem from that run?

---

_Comment by @dstcruz on 2017-03-09 13:40_

Ugh. Sorry. Just noticed that the AppVeyor script is only running tests.

---

_Comment by @dstcruz on 2017-03-09 13:43_

Ok, just a sec. I think I got this fixed.

---

_Comment by @dstcruz on 2017-03-09 13:46_

Since I can't commit a fix to this pull request, I will close it and add a corrected version.

---

_Closed by @dstcruz on 2017-03-09 13:46_

---

_Branch deleted on 2017-03-09 13:46_

---

_Comment by @BurntSushi on 2017-03-09 13:53_

@dstcruz You can update existing PRs by either adding commits or just amending your commit and force pushing. (The latter is preferred since it will yield a more serviceable history.)

---

_Comment by @dstcruz on 2017-03-09 14:13_

Thanks. As you can see... I haven't used GitHub much either. I'm getting there...

---
