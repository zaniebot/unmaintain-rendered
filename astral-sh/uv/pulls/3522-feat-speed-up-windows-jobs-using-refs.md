```yaml
number: 3522
title: "feat: speed up windows jobs using ReFS"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: windows-fs-speedup
created_at: 2024-05-11T04:32:00Z
updated_at: 2024-05-15T02:09:07Z
url: https://github.com/astral-sh/uv/pull/3522
synced_at: 2026-01-10T14:37:54Z
```

# feat: speed up windows jobs using ReFS

---

_Pull request opened by @samypr100 on 2024-05-11 04:32_

## Summary

Switch to using Virtual HD + ReFS to speed up windows jobs.

Closes #3508

---

_Comment by @zanieb on 2024-05-11 12:46_

So promising, thanks for exploring! You can push an empty commit to test the cached test time.

---

_Comment by @samypr100 on 2024-05-11 14:15_

Stats from first run...

| Step     | Before       | After        |
|----------|--------------|--------------|
| Install Toolchain   | 1m ~ | 11s~ |
| Install nextest   | 15s ~ | 8s~ |
| Cargo Tests   | 5m 25s ~ (cached) | 5m 44s ~ (uncached) -- TODO check cached speeds     |

Follow up updated run, will do a few more to try get the cached versions of some of these

| Steps     | Before       | After        |
|----------|--------------|--------------|
| Clippy   | 30s~ (cached) | 2m ~ (uncached) |
| Cache Restoration | 1.5m ~ | 12s ~ |
| Installing Toolchain   | 1m ~ | 11s~ |
| Install nextest   | 15s ~ | 8s~ |
| Cargo Tests   | 5m 25s ~ (cached) | 3m 59s ~ (cached)     |
| Trampoline   | 24s ~ (cached) | 25s ~ (uncached)     |
| build binary |  2m ~ (cached) | 2m 30s ~ (uncached)     |

---

_Comment by @samypr100 on 2024-05-11 14:52_

More Step Totals

| Steps     | Before       | After        |
|----------|--------------|--------------|
| Clippy step   | 30s~ (cached) | 30s ~ (cached) |
| Cache Restoration | 1.5m ~ | 12s ~ |
| Installing Toolchain   | 1m ~ | 11s~ |
| Install nextest   | 15s ~ | 8s~ |
| Cargo Tests   | 5m 25s ~ (cached) | 3m 59s ~ (cached)     |
| trampoline build step   | 24s ~ (cached) | 24s ~ (cached)     |
| binary build step|  2m ~ (cached) | 50s ~ (cached)     |

Job Totals (looking at a few runs)

| Job | Before       | After        |
|----------|--------------|--------------|
| Clippy | 1m-1.5m ~ | 1m-1.5m ~ |
| Cargo Tests   | 8m | 4m-5m ~     |
| Trampoline build   | 4m ~ | 2m ~    |
| build binary |  4m  | 1m-2m ~  |

Other notes might be worth adjusting the vhdx default size to speed up it's creation as it can take between 10s to 30s. I'll reduce to 5GB next.

---

_Comment by @samypr100 on 2024-05-11 15:05_

~~Reduced to 5GB clippy & trampoline, left test & build binary 10GB since it ran out of space otherwise.~~ Made no substantial difference, left at 10GB.

---

_Renamed from "feat: speed up windows tests using ReFS" to "feat: speed up windows jobs using ReFS" by @samypr100 on 2024-05-11 15:08_

---

_Comment by @samypr100 on 2024-05-11 15:21_

In summary, migrated windows jobs got 1.5x-2.x ~ speed up except clippy which stayed at 1x. Clippy job can be a bit faster or slower depending on how fast the VHDX gets created. Maybe switching to cargo-xwin as described in #3507 along with VHDX from this PR can unlock faster speeds.

Note, I did not get data for `uncached before`. Only `cached before` and `cached/uncached after`. So it's possible when comparing clippy `before and after uncached` it might be faster.

---

_Marked ready for review by @samypr100 on 2024-05-11 15:22_

---

_Comment by @charliermarsh on 2024-05-11 16:34_

I'll defer to @konstin and @zanieb on the review but based on the numbers, this seems really good! You're awesome!

---

_Review requested from @zanieb by @charliermarsh on 2024-05-11 16:34_

---

_Review requested from @konstin by @charliermarsh on 2024-05-11 16:34_

---

_Comment by @zanieb on 2024-05-13 13:52_

Should we skip it on Clippy for now then? I don't know if it merits the complexity there if it's both slower/faster.

---

_@zanieb approved on 2024-05-13 13:53_

---

_Comment by @charliermarsh on 2024-05-13 13:58_

I don't mind keeping it for consistency but defer to you.

---

_Comment by @zanieb on 2024-05-13 14:01_

I don't know, we need to split the job out of the matrix and maybe we're going to switch to xwin later? 🤷‍♀️ I don't have strong opinions though.

---

_Comment by @charliermarsh on 2024-05-13 14:01_

Sadly the decision falls upon you...

---

_Comment by @samypr100 on 2024-05-13 15:55_

Note clippy might still be faster in purely uncached times, I haven't compared though. I'd say worth trying it out for a bit and we can change later easily.

Agreed, changing to xwin will require a split from the matrix regardless.

---

_Label `testing` added by @zanieb on 2024-05-13 16:15_

---

_Label `internal` added by @zanieb on 2024-05-13 16:15_

---

_Merged by @zanieb on 2024-05-13 16:15_

---

_Closed by @zanieb on 2024-05-13 16:15_

---

_Branch deleted on 2024-05-14 02:11_

---

_Comment by @zooba on 2024-05-14 13:20_

This is awesome to see!

You might need some error handling in case you land on an older system, but there's a `-DevDrive` option to `Format-Volume` that _ought_ to improve things further. It may not be hugely significant since this is already a somewhat stripped back CI system rather than an OS install.

Putting the VHDX in `$env:GITHUB_WORKSPACE` might also help (I don't know exactly how the GHA setup looks, but it's based on Azure Pipelines which puts the OS on a slower disk than the workspace), and passing `-Fixed` to `New-VHD` may also help (but I'm less confident about that one).

---

_Comment by @samypr100 on 2024-05-15 02:07_

Thanks! @zooba 

> there's a -DevDrive option to Format-Volume that ought to improve things further

Great point! I did try out `-DevDrive`, but got a `Format-Volume: A parameter cannot be found that matches parameter name 'DevDrive'.` since it seems it works on windows versions `10.0.22621` or greater and runners seem to be a bit below that `~10.0.20348`.

I did add that check to the [new action I created](https://github.com/samypr100/setup-dev-drive) out of this PR that uses `-DevDrive` when it meets the minimum os version (e.g. self hosted runners) and also uses the same drive `${{ github.workspace }}` uses. Feedback welcome of course :-)

---
