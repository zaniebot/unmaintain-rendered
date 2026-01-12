```yaml
number: 731
title: No output when --after-context is added
type: issue
state: closed
author: wezm
labels: []
assignees: []
created_at: 2018-01-05T01:03:16Z
updated_at: 2018-02-01T00:24:07Z
url: https://github.com/BurntSushi/ripgrep/issues/731
synced_at: 2026-01-12T16:13:22Z
```

# No output when --after-context is added

---

_@wezm_

I'm grepping a log file as follows, which produces 313 matching lines:

```
rg 'WARN.*59082' log/server.log
```

When `--after-context` (or `-A`) is added no results are shown and `rg` exits with a status code of 1. It doesn't seem to matter what number is supplied to `--after context`.

```
rg --after-context=2 'WARN.*59082' log/server.log
```

A smaller log file I created to try to supply a test case works properly. The original log file is 742Mb. Performing the same operation (with `--after`) in `ag` yield results as expected.

Fresh debug and release builds from master (`14779ed`) yields the same result.

**OS:** Arch Linux (`4.14.10-1-ARCH #1 SMP PREEMPT Fri Dec 29 20:17:35 UTC 2017 x86_64 GNU/Linux`)
**ripgrep:** Installed via Arch package, version 0.7.1-1

```
$ rg --version
ripgrep 0.7.1
-AVX -SIMD
```

---

_Comment by @BurntSushi on 2018-01-05 01:12_

Thanks for reporting this! Unfortunately it is going to be hard to debug without a reproducible test case. Any chance you can try to wittle down your log file? 

---

_Comment by @wezm on 2018-01-05 01:17_

I'm working on it. I have it down to 256Mb 🙂

---

_Comment by @wezm on 2018-01-05 01:41_

Ok I've found the source. There was a bunch of NUL bytes buried in the file. Not sure if this a case ripgrep should handle but reduced test case is attached.

[nul-bytes.log](https://github.com/BurntSushi/ripgrep/files/1605278/nul-bytes.log)


---

_Comment by @okdana on 2018-01-05 02:32_

It doesn't print your test case because it thinks the file is a binary. If you add `-a` it should work.

It is a bit hard to troubleshoot what it's doing when it just silently ignores it though. Maybe there should be a debug message?

Could be noisy if you have a lot of binaries in your repo or whatever, but that's what `ag` does:

```
DEBUG: File nul-bytes.log is binary. Skipping...
```

(At least, it does that with files on the file system — i don't think it can detect binary data on `stdin`.)

---

_Comment by @wezm on 2018-01-05 02:42_

Using `-a` does in fact fix it. I think some kind of message would have been super helpful. I figured there was a reason but there was nothing to go on to work out what it was. Extra confusion was caused by getting results without `-A` then adding it and getting none.

---

_Comment by @BurntSushi on 2018-01-05 04:22_

I suspect that #306 might be a duplicate then.

However, the presence or absence of `-A` shouldn't impact binary file detection... So that's worth looking into.

---

_Comment by @BurntSushi on 2018-02-01 00:24_

I can confirm that this is a dupe of #306. I couldn't see any problem with using `-A`.

---

_Closed by @BurntSushi on 2018-02-01 00:24_

---
