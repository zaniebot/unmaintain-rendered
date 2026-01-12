```yaml
number: 10180
title: "Use `D:` drive for Windows CI"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/dev-drive
created_at: 2024-12-26T19:58:45Z
updated_at: 2025-01-17T19:57:11Z
url: https://github.com/astral-sh/uv/pull/10180
synced_at: 2026-01-12T16:09:10Z
```

# Use `D:` drive for Windows CI

---

_@zanieb_

When using the standard Windows runners (as opposed to the _larger_ GitHub runners), an undocumented `D:` drive is available and performant. We can save some money on by using this on a standard runner instead of a larger runner with an ReFS drive. Switching to the `D:` drive was not acceptable for `cargo test` >25m runtime.

Inspired by https://github.com/pypa/pip/pull/13129
See https://github.com/actions/runner-images/issues/8755

Timings (grain of salt — GitHub is super noisy):

- clippy: 2m 18s -> 2m 11s
- build binary: 2m 3s -> 2m 35s
- trampoline check (x86-64): 2m 32s -> 1m 50s (other architectures similar)
- trampoline test (x86-64): 4m 12s -> 6m 7s
- trampoline test (i686): 6m 44s -> 5m 35s

---

_Label `internal` added by @zanieb on 2024-12-26 19:58_

---

_Comment by @samypr100 on 2025-01-04 00:57_

Interesting, I did a similar benchmark originally (not with TEMP) and found ReFS was much faster with uv than D drive. One thing we can definitely improve is moving the actual dev drive vhdx file to D: rather than C:

We can also try a mix of Dev Drive but keep TEMP in D: rather than in the Dev Drive.

---

_Comment by @zanieb on 2025-01-04 01:26_

> Interesting, I did a similar benchmark originally (not with TEMP) and found ReFS was much faster with uv than D drive. One thing we can definitely improve is moving the actual dev drive vhdx file to D: rather than C:

Interesting, `pip`'s recent benchmarking showed otherwise and the numbers here are honestly pretty promising for some of the jobs?

> We can also try a mix of Dev Drive but keep TEMP in D: rather than in the Dev Drive.

Unfortunately there is no `D:` drive on the larger runners. So it's sort of looking like `D:` drive on a small runner or `ReFS` on a large runner.


---

_Comment by @samypr100 on 2025-01-04 01:38_

> Unfortunately there is no D: drive on the larger runners

Ah, I didn't realize that. I haven't tried the larger runners on my personal runs.

---

_Comment by @zanieb on 2025-01-04 02:14_

It took me a while to figure out too :D

---

_Marked ready for review by @zanieb on 2025-01-15 19:21_

---

_Review requested from @samypr100 by @zanieb on 2025-01-15 21:41_

---

_Comment by @zanieb on 2025-01-15 21:41_

@samypr100 if you're interested, this seems good to go.

---

_@samypr100 reviewed on 2025-01-16 00:41_

---

_Review comment by @samypr100 on `.github/workflows/setup-dev-drive.ps1`:18 on 2025-01-16 00:41_

```suggestion
    $OSVersion = [Environment]::OSVersion.Version
    $Partition = New-VHD -Path C:/uv_dev_drive.vhdx -SizeBytes 20GB |
        Mount-VHD -Passthru |
        Initialize-Disk -Passthru |
        New-Partition -AssignDriveLetter -UseMaximumSize
    if ($OSVersion -ge (New-Object 'Version' 10.0.22621)) {
        $Volume = ($Partition | Format-Volume -DevDrive -Confirm:$false -Force)
    } else {
        $Volume = ($Partition | Format-Volume -FileSystem ReFS -Confirm:$false -Force)
    }
```

Possibly worthwhile, I would add the logic from https://github.com/pypa/pip/commit/483859b240109f9c149d7dbf32f4c7f858821e55 to take advantage of native dev drive functionality on the new 2025 preview runners.

---

_@samypr100 approved on 2025-01-16 00:42_

---

_@zanieb reviewed on 2025-01-16 02:20_

---

_Review comment by @zanieb on `.github/workflows/setup-dev-drive.ps1`:18 on 2025-01-16 02:20_

I'm doing that in #10651 (though not conditional right now) and it doesn't look promising. I haven't tried not using ReFS though 🤔 

---

_Merged by @zanieb on 2025-01-17 19:57_

---

_Closed by @zanieb on 2025-01-17 19:57_

---

_Branch deleted on 2025-01-17 19:57_

---
