```yaml
number: 2902
title: "Recursive search doesn't work on /tmp files"
type: issue
state: closed
author: thepragmaticmero
labels: []
assignees: []
created_at: 2024-09-25T15:23:15Z
updated_at: 2024-09-25T15:31:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2902
synced_at: 2026-01-12T16:13:25Z
```

# Recursive search doesn't work on /tmp files

---

_@thepragmaticmero_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

DNF / Pacman

### What operating system are you using ripgrep on?

Fedora Silverblue 40 / Arch Linux

### Describe your bug.

Ripgrep fails to give ANYTHING inside the /tmp/(gibberish) files. `grep -R` works tho, but `grep -r`. Why? Is grep better than ripgrep? It seems so to me. One works, the other doesn't, even with `rg -e` flags.

### What are the steps to reproduce the behavior?

I want to search a certain string inside a pkg build made by paru (arch linux build). The string is "ld -z", thing is. RipGrep can't search even basic text like "Build" (there's a lot of build text inside the folder). `grep -r` doesn't work either. But `grep -R` does. What's the matter with the /tmp files? even with sudo it doesn't find anything.

### What is the actual behavior?

`$ rg -e "ld -z" /tmp/aurmkulYq/` and I get no output.
`$ rg -e "Build" /tmp/aurmkulYq/` still no output.
```
$ grep -R "Build" /tmp/aurmkulYq/
/tmp/aurmkulYq/blender-git/src/blender/CMakeLists.txt:Build and link against address sanitizer (only for Debug & 10:21:16 [1886/1886]
ts)."
/tmp/aurmkulYq/blender-git/src/blender/CMakeLists.txt:Build `extern` dependencies with address sanitizer when WITH_COMPILER_ASAN is o
n. \
/tmp/aurmkulYq/blender-git/src/blender/CMakeLists.txt:Build and link with code coverage support (only for Debug targets)."
/tmp/aurmkulYq/blender-git/src/blender/CMakeLists.txt:      # Build type is not known for multi-config generator, so don't add object
-size sanitizer.
/tmp/aurmkulYq/blender-git/src/blender/CMakeLists.txt:  info_cfg_text("Build Options:")
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * debug:         Build a debug binary.
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * headless:      Build without an interface (renderfarm or server automation).
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * cycles:        Build Cycles standalone only, without Blender.
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * bpy:           Build as a python module which can be loaded from python direc
tly.
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * deps:          Build library dependencies (intended only for platform maintai
ners).
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:   * package_archive:   Build an archive package.
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:# Source and Build DIR's
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:# Build Blender
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:     @echo Building Blender ...
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:# Build dependencies
/tmp/aurmkulYq/blender-git/src/blender/GNUmakefile:     @echo Building dependencies ...
/tmp/aurmkulYq/blender-git/src/blender/README.md:- [Build Instructions](https://developer.blender.org/docs/handbook/building_blender/
)
... This is too long, but you get the idea.
```
So... is gnu grep better than ripgrep? One works, the other doesn't. Huh~

### What is the expected behavior?

At least with `rg -e "Build" /tmp/aurmkulYq` it should output _something_

---

_Locked by @ghost on 2024-09-25 15:31_

---
