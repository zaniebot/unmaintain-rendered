```yaml
number: 13724
title: disfavor aarch64 windows in its own house
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/deprio-arm64-windows
created_at: 2025-05-29T22:17:37Z
updated_at: 2025-06-30T21:42:02Z
url: https://github.com/astral-sh/uv/pull/13724
synced_at: 2026-01-12T16:10:49Z
```

# disfavor aarch64 windows in its own house

---

_@Gankra_

and prefer emulated x64 windows in its stead.

This is preparatory work for shipping support for uv downloading and installing aarch64 (arm64) windows Pythons. We've [had builds for this platform ready for a while](https://github.com/astral-sh/python-build-standalone/pull/387), but have held back on shipping them due to a fundamental problem:

**The Python packaging ecosystem does not have strong support for aarch64 windows**, e.g., not many projects build aarch64 wheels yet. The net effect of this is that, if we handed you an aarch64 python interpreter on windows, you would have to build a lot more sdists, and there's a high chance you will simply fail to build that sdist and be sad.

Yes unfortunately, in this case a non-native Python interpreter simply *works better* than the native one... in terms of working at all, today. Of course, if the native interpreter works for your project, it should presumably have better performance and platform compatibility.

We do not want to stand in the way of progress, as ideally this situation is a temporary state of affairs as the ecosystem grows to support aarch64 windows. To enable progress, on aarch64 Windows builds of uv:

* We will still use a native python interpreter, e.g., if it's at the front of your `PATH` or the only installed version.
* If we are choosing between equally good interpreters that differ in architecture, x64 will be preferred.
* We will emit a diagnostic on installation, and show the python request to pass to uv to force aarch64 windows to be used.
* Will be shipping [aarch64 Windows Python downloads](https://github.com/astral-sh/python-build-standalone/pull/387)
* Will probably add some kind of global override setting/env-var to disable this behaviour.
* Will be shipping this behaviour in [astral-sh/setup-uv](https://github.com/astral-sh/setup-uv)

We're coordinating with Microsoft, GitHub (for the `setup-python` action), and the CPython team (for the `python.org` installers), to ensure we're aligned on this default and the timing of toggling to prefer native distributions in the future.

See discussion in 

- https://github.com/astral-sh/uv/issues/12906

---

This is an alternative to 

* #13719 

which uses sorting rather than filtering, as discussed in 

* #13721

---

_Label `internal` added by @Gankra on 2025-05-29 22:17_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:488 on 2025-05-29 22:24_

Why not implement this in https://github.com/astral-sh/uv/blob/546a6d017d6294712a9796630397862ba86d77ff/crates/uv-python/src/platform.rs#L40-L59 instead under a Windows cfg? Do we need the `self.os` check here?

---

_@zanieb reviewed on 2025-05-29 22:24_

---

_@zanieb reviewed on 2025-05-29 22:24_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:488 on 2025-05-29 22:24_

(this just looks a lot like the logic already in `Arch::cmp` and I think it'd be easier to reason about in a single location)

---

_Comment by @codspeed-hq[bot] on 2025-05-29 22:32_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/gankra%2Fdeprio-arm64-windows)

### Merging #13724 will **not alter performance**

<sub>Comparing <code>gankra/deprio-arm64-windows</code> (1777186) with <code>main</code> (d9b76b9)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_@Gankra reviewed on 2025-05-29 22:34_

---

_Review comment by @Gankra on `crates/uv-python/src/installation.rs`:488 on 2025-05-29 22:34_

Yeah it's because of the os check. Not that it's super useful now that I think about it harder, but it feels... wrong... that we would resolve a request for `python-3.11-darwin-any` to intel on arm64 windows.

---

_@zanieb reviewed on 2025-05-29 23:00_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:488 on 2025-05-29 23:00_

Hm... it does feel wrong... but like... not a real use-case today. I worry about this being brittle. I'll ponder this overnight.

It does suggest that long-term we need a dedicated `Platform` type to encapsulate both the OS and the Arch and implement `Ord` on that instead? 


---

_Comment by @konstin on 2025-05-30 07:21_

When applying this, we should make sure that we signal to the user clearly that we prefer x64_86 over aarch64 and how to revert that, both on automatic installation and on `uv python list`. (This is different from the Apple case where there was an explicit transition with a big announcement on the transparent emulation and users expected software from Intel Macs to keep working.)

---

_Assigned to @zanieb by @zanieb on 2025-05-30 13:00_

---

_Comment by @Gankra on 2025-05-30 13:40_

> When applying this, we should make sure that we signal to the user clearly that we prefer x64_86 over aarch64 and how to revert that, both on automatic installation and on uv python list

As in uv should print messages whenever it detected it was doing this, or a public marketing campaign?

---

_Comment by @Gankra on 2025-05-30 13:42_

Also wanna note, we don't really need to land this until we go to update to a PBS that ships arm64 windows builds, so there's no rush to land this. I don't think we have any reports of users installing system intel and arm64 pythons on windows and getting mad uv isn't picking the intel one (but this PR *would* change that).

---

_Comment by @konstin on 2025-05-30 15:15_

We should print a note when uv installs a managed Python interpreter that is not native and not explicitly requested as non-native by the user.

---

_Comment by @Gankra on 2025-06-13 17:51_

Pushed up a fixed impl with the requested diagnostic (zanie wanted a note, not a warning).

---

_@zanieb reviewed on 2025-06-13 18:02_

---

_Review comment by @zanieb on `crates/uv-python/src/platform.rs`:65 on 2025-06-13 18:02_

It's a _little_ weird that we do this by assigning a "wrong" value to `native`, maybe we should rename to `preferred`? 
```suggestion
        let preferred = if cfg!(all(windows, target_arch = "aarch64")) {
            Arch {
                family: target_lexicon::Architecture::X86_64,
                variant: None,
            }
        } else {
            // By default, prefer the native architecture (i.e., the one matching the uv binary)
            Arch::from_env()
        };
```

---

_@zanieb approved on 2025-06-13 18:02_

---

_Comment by @zanieb on 2025-06-13 18:05_

Can we add a test case for this? Like the macOS emulated one? We don't run the test suite on arm Windows though, so that's kind of annoying. Maybe better as a follow-up, though we could at least write a janky integration test with a bash assertion in CI? Like `integration test | uv python on arm windows`?

---

_Comment by @Gankra on 2025-06-13 18:08_

> Can we add a test case for this? Like the macOS emulated one? We don't run the test suite on arm Windows though, so that's kind of annoying. Maybe better as a follow-up, though we could at least write a janky integration test with a bash assertion in CI? Like integration test | uv python on arm windows?

I was planning to explore testing more when we PR the PBS update that adds windows releases.

---

_Comment by @zanieb on 2025-06-13 18:10_

That makes sense, though I'd double check we have existing test coverage to ensure this doesn't "break" anything (we do have a couple CI jobs covering aarch64 windows)

---

_Comment by @Gankra on 2025-06-30 17:43_

Added a "aarch64 on aarch64 windows" smoketest to go with the "x64 on aarch64" one

---

_Merged by @Gankra on 2025-06-30 21:42_

---

_Closed by @Gankra on 2025-06-30 21:42_

---

_Branch deleted on 2025-06-30 21:42_

---
