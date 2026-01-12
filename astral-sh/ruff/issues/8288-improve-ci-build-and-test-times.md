```yaml
number: 8288
title: Improve CI build and test times
type: issue
state: open
author: zanieb
labels:
  - ci
assignees: []
created_at: 2023-10-27T20:50:19Z
updated_at: 2023-11-27T23:13:11Z
url: https://github.com/astral-sh/ruff/issues/8288
synced_at: 2026-01-12T15:54:48Z
```

# Improve CI build and test times

---

_@zanieb_

**Without cache hit** [source](https://github.com/astral-sh/ruff/actions/runs/6670980068)

Compile time for Linux tests 7m 23s
Compile time for Windows tests 8m 56s
Runtime for docs check 2m 33s

**With cache hit** [source](https://github.com/astral-sh/ruff/actions/runs/6671183579)

Compile time for Linux tests 3m 13s
Compile time for Windows tests 4m 48s
Runtime for docs check 35s 

CI takes an additional 2 minutes to run the tests in both environments

We only check documentation on Linux.

It'd be great if these were faster. I'm not sure it's possible but I'd love to hear ideas.

---

_Comment by @charliermarsh on 2023-10-27 20:58_

Is that the documentation check, or doc tests?

---

_Comment by @zanieb on 2023-10-27 21:01_

It's the `cargo doc` command (and most of the time is spent compiling) so it should just be documentation checks.

Considering how slow doctests are generally though, an action item here may be to investigate the time of doctests :)

---

_Label `internal` added by @zanieb on 2023-10-27 21:02_

---

_Comment by @zanieb on 2023-10-27 21:03_

Related exploration of nextest 
- https://github.com/astral-sh/ruff/issues/3752
- #6979 

---

_Comment by @zanieb on 2023-10-27 22:49_

We could consider paying for runners with more CPUs (https://docs.github.com/en/actions/using-github-hosted-runners/about-larger-runners/about-larger-runners) although I would like to chase free options first :)

---

_Comment by @zanieb on 2023-10-30 21:02_

Related:
- https://github.com/astral-sh/ruff/issues/1820

---

_Comment by @BurntSushi on 2023-11-07 16:33_

I haven't looked at CI and the build in detail, but if any of it is building in release mode with fat LTO enabled, then that is likely taking some sizeable chunk of time. Fat LTO is notorious for being horrendously slow. #8544 switches over to thin LTO. The other thing potentially worth experimenting with is bumping up the `codegen-units` from `1` to something a little higher. It lets compilation use more parallelization at the expense of potentially giving up some code optimizations.

In addition to the other ideas mentioned, particularly in the non-cache case, a more involved approach might be to reduce the number of dependencies overall. Ruff has quite a lot of them. However, it's not clear to me how much the non-cache case truly matters. (I imagine most folks are installing Ruff as a wheel of some kind? Apologies for the deployment ignorance here on my part.)

---

_Comment by @charliermarsh on 2023-11-07 16:39_

> (I imagine most folks are installing Ruff as a wheel of some kind? Apologies for the deployment ignorance here on my part.)

No worries at all. That's right, the vast majority of downloads are pre-compiled wheels that we upload to PyPI. (These are basically compiled binaries that get wrapped up with some other metadata in a zip archive.) I would say slow release builds are mostly a problem for _development_ (especially profiling). And slow debug builds are _definitely_ a problem for development.


---

_Comment by @konstin on 2023-11-08 10:57_

I believe the most impactful - but also most effort - change would be splitting up the `ruff_linter` crate (#1820), the crate is very slow to compile compared to other crates i've used, even in debug mode.

---

_Label `ci` added by @MichaReiser on 2023-11-27 23:13_

---

_Label `internal` removed by @MichaReiser on 2023-11-27 23:13_

---
