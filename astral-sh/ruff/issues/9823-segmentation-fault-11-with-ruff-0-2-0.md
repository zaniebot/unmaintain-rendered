---
number: 9823
title: "Segmentation fault: 11 with ruff 0.2.0"
type: issue
state: closed
author: jetli
labels:
  - bug
  - cli
assignees: []
created_at: 2024-02-05T06:55:41Z
updated_at: 2024-02-05T16:24:52Z
url: https://github.com/astral-sh/ruff/issues/9823
synced_at: 2026-01-10T01:22:49Z
---

# Segmentation fault: 11 with ruff 0.2.0

---

_Issue opened by @jetli on 2024-02-05 06:55_

when run ruff 0.2.0, it always return an error msg: Segmentation fault: 11, but ruff 0.1.15 is ok.
```bash
$ ruff --version
Segmentation fault: 11
```

OS: macOS 11.7.10
python: 3.8.9 in virtual env

---

_Comment by @qarmin on 2024-02-05 06:58_

Can you run it under gdb?
```
gdb ruff
run --version
bt
```


---

_Comment by @MichaReiser on 2024-02-05 13:28_

@jetli, what shell are you using? Is it ZSH? https://github.com/astral-sh/ruff/discussions/9824

---

_Comment by @charliermarsh on 2024-02-05 13:55_

How strange. Does running with `--no-cache` change anything? (I assume not, but worth a try.)

---

_Comment by @jetli on 2024-02-05 14:07_

not zsh, just bash.  it crashes even with no options.
```
$ ruff
Segmentation fault: 11
```

installation log:
```
Collecting ruff
  Using cached ruff-0.2.0-py3-none-macosx_10_12_x86_64.whl (7.3 MB)
Installing collected packages: ruff
  Attempting uninstall: ruff
    Found existing installation: ruff 0.1.15
    Uninstalling ruff-0.1.15:
      Successfully uninstalled ruff-0.1.15
Successfully installed ruff-0.2.0
```

---

_Comment by @charliermarsh on 2024-02-05 14:17_

Do you know what version of macOS you’re on? Perhaps this is related to our upgrade to use the Apple Silicon runners.

---

_Comment by @charliermarsh on 2024-02-05 14:17_

Oh sorry, you said macOS 11 above. I’m guessing there’s something incompatible there.

---

_Comment by @zanieb on 2024-02-05 14:23_

Could you share the output of the `uname -a` command?

---

_Comment by @Gankra on 2024-02-05 14:52_

I can reproduce this on my old EOL x64 macbook (macos 11 has been eol for 4 months, but i think this issue can plausibly manifest on 12/13).

The crash stack includes lots of things like `ImageLoaderMachO::doModInitFunctions(ImageLoader::LinkContext const&)` which is a pretty clear sign the crash is during the dynamic linking of the executable on load. This is *probably* a symptom of binaries compiled against the macos 14 sdk (the default for github's new apple silicon runners) being run on an older OS.

---

_Comment by @charliermarsh on 2024-02-05 14:54_

@Gankra - Do you think this is an issue for ARM macbooks on macos 11, 12, 13? Or just x86? Or can't say at this point?

---

_Comment by @Gankra on 2024-02-05 14:58_

I would expect the issue to happen equally on both platforms, with OS version being the primary issue -- but that's purely a guess, and the only thing I can think to do is to find people with macbooks running those versions to try (I guess you could write some github CI jobs to spawn some of those OSes and try to run the binary?).

I can send you the full crash log but it doesn't obviously indicate what symbol was trying to be loaded (only skimmed it).

---

_Label `bug` added by @MichaReiser on 2024-02-05 15:14_

---

_Label `cli` added by @MichaReiser on 2024-02-05 15:14_

---

_Referenced in [astral-sh/ruff#9834](../../astral-sh/ruff/pulls/9834.md) on 2024-02-05 15:53_

---

_Comment by @charliermarsh on 2024-02-05 16:12_

@Gankra - FYI, it looks like these binaries work okay on macOS 12 and macOS 13: https://github.com/astral-sh/ruff/pull/9835

---

_Comment by @charliermarsh on 2024-02-05 16:13_

Now I'm more tempted _not_ to change this given that macOS 11 is EOL :(

---

_Closed by @charliermarsh on 2024-02-05 16:24_

---
