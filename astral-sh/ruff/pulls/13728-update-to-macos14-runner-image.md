```yaml
number: 13728
title: Update to macOS14 runner image
type: pull_request
state: merged
author: diceroll123
labels:
  - ci
assignees: []
merged: true
base: main
head: use-macos13-runner
created_at: 2024-10-13T06:28:16Z
updated_at: 2024-10-24T15:13:30Z
url: https://github.com/astral-sh/ruff/pull/13728
synced_at: 2026-01-12T15:55:45Z
```

# Update to macOS14 runner image

---

_@diceroll123_

## Summary

Updates Github Actions macOS runner images to use macOS 13, since runners using 12 will be unavailable on/around December 3rd of this year.

Closes #13231 

For more info, see that issue, as well as https://github.com/actions/runner-images/issues/10721. There will be a 10-hour brownout on each Monday in November.

## Test Plan

I did not. 😎 (until after the checks passed)

---

_Label `cli` added by @MichaReiser on 2024-10-13 13:24_

---

_Comment by @MichaReiser on 2024-10-13 13:26_

Thanks for this PR and making us aware of the runner deprecation. 

We intentionally downgraded the macOS runner in the past because using newer macOS versions resulted in binaries that are incompatible with macOS 11 (https://github.com/astral-sh/ruff/pull/11146, https://github.com/astral-sh/ruff/pull/9834). However, that means that we'll loose macOS 11 support, unless we find another way to cross compile binaries to macOS11. 

---

_Comment by @MichaReiser on 2024-10-14 14:33_

I did some research and my general reading is that macOS only supports building for the same or newer versions but there doesn't seem to be a way to build for older macOS versions. That means, this PR drops support for macOS12 unless we find another way of running the release job in a macOs12 image. 

I found some references of `MACOSX_DEPLOYMENT_TARGET` but it's unclear to me if that would have any effect, considering that the macOS13 image doesn't have the the macOS12 SDK installed.

---

_Assigned to @MichaReiser by @MichaReiser on 2024-10-14 15:56_

---

_Comment by @MichaReiser on 2024-10-18 09:31_

Okay, good news. I think I narrowed down why Ruff 0.2 was crashing and we should be fine to just upgrade to the macos 14 runners. 

I compared the dynamically linked libraries and they're mostly unchanged (ignoring versions)

```
ruff 0.2
    /System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2202.0.0)
    /System/Library/Frameworks/CoreServices.framework/Versions/A/CoreServices (compatibility version 1.0.0, current version 1226.0.0)
    /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1336.61.1)

ruff 0.6.9
    /System/Library/Frameworks/CoreFoundation.framework/Versions/A/CoreFoundation (compatibility version 150.0.0, current version 2503.1.0)
    /System/Library/Frameworks/CoreServices.framework/Versions/A/CoreServices (compatibility version 1.0.0, current version 1226.0.0)
    /usr/lib/libiconv.2.dylib (compatibility version 7.0.0, current version 7.0.0)
    /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1345.120.2)
```

I also compared the dynamically linked symbols and they are almost identical: https://www.diffchecker.com/x9Nond5g/

That's why I went back to analysing the ruff 0.2 crash stack frame and I stumbled across https://github.com/realm/realm-swift/issues/8369 where one user suggests that this is an XCode bug that [was fixed](https://developer.apple.com/documentation/xcode-release-notes/xcode-15_1-release-notes#Resolved-Issues) in 15.1

> Fixed: Binaries using symbols with a weak definition crash at runtime on iOS 14/macOS 12 or older. This impacts primarily C++ projects due to their extensive use of weak symbols. (114813650) (FB13097713)

---

_Comment by @MichaReiser on 2024-10-18 09:31_

I'll upgrade the runner to macOS 14 to get faster compile times.

---

_Comment by @MichaReiser on 2024-10-18 09:34_

I tested that the binary built on macos 14 runs on a mac 10.15. I built the binary using the CI job in https://github.com/astral-sh/ruff/pull/13785

---

_Renamed from "Update to macOS13 runner image" to "Update to macOS14 runner image" by @MichaReiser on 2024-10-18 09:35_

---

_Comment by @MichaReiser on 2024-10-18 09:43_

I downloaded the macos x86-64 binary and verified that it works on macos 10.15

---

_Merged by @MichaReiser on 2024-10-18 09:43_

---

_Closed by @MichaReiser on 2024-10-18 09:43_

---

_Label `cli` removed by @dhruvmanila on 2024-10-24 15:13_

---

_Label `ci` added by @dhruvmanila on 2024-10-24 15:13_

---
