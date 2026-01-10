```yaml
number: 3928
title: Remove installed packages from preferences
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/pref
created_at: 2024-05-30T19:49:39Z
updated_at: 2024-05-30T20:13:01Z
url: https://github.com/astral-sh/uv/pull/3928
synced_at: 2026-01-10T13:59:34Z
```

# Remove installed packages from preferences

---

_Pull request opened by @charliermarsh on 2024-05-30 19:49_

## Summary

I believe that this is not necessary, as the installer packages are already considered in `CandidateSelector::get_preferred`.

Firstly, note that we never pass both non-empty installed packages _and_ non-empty preferences (the installer routines pass site packages and no preferences; the resolver routines pass no site packages but lockfile preferences).

However, in general, if you look at `CandidateSelector::get_preferred`, and consider what's changing, we now skip the `if let Some(version) = preferences.version(package_name)` case for installed packages. But we then check installed packages within that `if`, and in the `else`. So it seems like we'll still return them in either case?

The only behavior change is in the case that you have multiple versions of a package installed. Previously, we'd respect one of them, because `Preferences` takes the last winner (it's a hash map, so we just replace the package key with the last version we see); but in installed packages, we always ignore distributions with multiple versions, since it's indicative of a broken environment. That's a fine change IMO. We could change `CandidateSelector::get_preferred` to support this if we wanted to.


---

_Label `internal` added by @charliermarsh on 2024-05-30 19:49_

---

_Marked ready for review by @charliermarsh on 2024-05-30 19:49_

---

_Comment by @codspeed-hq[bot] on 2024-05-30 19:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/pref)

### Merging #3928 will **degrade performances by 6.08%**

<sub>Comparing <code>charlie/pref</code> (0eb7e8b) with <code>main</code> (261aa2c)</sub>



### Summary

`⚡ 1` improvements
`❌ 1` regressions
`✅ 11` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/pref)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/pref` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 955.8 ns | 897.5 ns | +6.5% |
| ❌ | `resolve_warm_jupyter` | 85.3 ms | 90.9 ms | -6.08% |


---

_Comment by @charliermarsh on 2024-05-30 20:06_

If this regresses I'm very sorry.

---

_Merged by @charliermarsh on 2024-05-30 20:09_

---

_Closed by @charliermarsh on 2024-05-30 20:09_

---

_Branch deleted on 2024-05-30 20:09_

---
