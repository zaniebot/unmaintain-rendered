```yaml
number: 14611
title: Bump version to 0.7.21
type: pull_request
state: merged
author: geofft
labels: []
assignees: []
merged: true
base: main
head: geofft/release-0.7.21
created_at: 2025-07-14T16:48:57Z
updated_at: 2025-07-14T18:01:30Z
url: https://github.com/astral-sh/uv/pull/14611
synced_at: 2026-01-12T16:11:18Z
```

# Bump version to 0.7.21

---

_@geofft_

_No description provided._

---

_Review requested from @zanieb by @geofft on 2025-07-14 16:48_

---

_@zanieb reviewed on 2025-07-14 16:50_

---

_Review comment by @zanieb on `CHANGELOG.md`:10 on 2025-07-14 16:50_

```suggestion
- Restore the SQLite "fts4", "fts5", "rtree", and "geopoly" extensions on Unix
```

If you want to retain both, then I'd say "macOS and Linux"

---

_@zanieb reviewed on 2025-07-14 16:51_

---

_Review comment by @zanieb on `CHANGELOG.md`:19 on 2025-07-14 16:51_

This should be in a `### Preview` section

---

_@zanieb reviewed on 2025-07-14 16:51_

---

_Review comment by @zanieb on `CHANGELOG.md`:21 on 2025-07-14 16:51_

This actually includes `uv run`, might as well just shorten this
```suggestion
- Add `-w` shorthand for `--with` ([#14530](https://github.com/astral-sh/uv/pull/14530))
```

---

_Review comment by @zanieb on `CHANGELOG.md`:22 on 2025-07-14 16:51_

```suggestion
- Add an exception handler on Windows to display information on crash ([#14582](https://github.com/astral-sh/uv/pull/14582))
```

---

_@zanieb reviewed on 2025-07-14 16:51_

---

_@zanieb reviewed on 2025-07-14 16:55_

---

_Review comment by @zanieb on `CHANGELOG.md`:32 on 2025-07-14 16:55_

We might want to clarify the symlink behavior. We only resolve terminal symlinks in globs, I think? See https://github.com/astral-sh/uv/pull/13438#issuecomment-3059477520. I'd consider it an enhancement rather than a bug since this was not allowed by design before.

We might want to just split https://github.com/astral-sh/uv/pull/13469 into two notes and link to it twice. One enhancement to support parent paths in cache keys and another to improve the performance of cache keys.

---

_Review comment by @zanieb on `CHANGELOG.md`:38 on 2025-07-14 16:55_

```suggestion
- Update CONTRIBUTING.md with instructions to format markdown files via Docker ([#14246](https://github.com/astral-sh/uv/pull/14246))
```

---

_@zanieb reviewed on 2025-07-14 16:55_

---

_@geofft reviewed on 2025-07-14 17:02_

---

_Review comment by @geofft on `CHANGELOG.md`:32 on 2025-07-14 17:02_

Like this?

```md
- Follow leaf symlinks matched by globs in `cache-key` ([#13438](https://github.com/astral-sh/uv/pull/13438))
- Support parent path components (`..`) in globs in `cache-key` ([#13469](https://github.com/astral-sh/uv/pull/13469))
- Improve `cache-key` performance ([#13469](https://github.com/astral-sh/uv/pull/13469))
```

I would be inclined to say the `..` thing is a bugfix if I'm reading #13469 correctly - it silently was ignoring changes to those files without telling you it wasn't supported, right? (Actually isn't that also true of the symlinks thing?)

---

_Review requested from @zanieb by @geofft on 2025-07-14 17:29_

---

_@zanieb reviewed on 2025-07-14 17:32_

---

_Review comment by @zanieb on `CHANGELOG.md`:32 on 2025-07-14 17:32_

Yeah that seems fine.

I guess I'm not sure what our previous behavior was. Is it "Support parent paths in `cache-keys`" or specifically "Support parent paths in `cache-key` globs"? It sounds more like a bug fix if it's just the rather.

I don't really feel that strongly about whether or not it's classified as a bug. My impression is that it was intentionally not allowed. A bug fix would be "Error on invalid `cache-key` entries", but actually making it work is more of a feature. Do what you want with that though :)

---

_@zanieb approved on 2025-07-14 17:32_

---

_@BurntSushi reviewed on 2025-07-14 17:38_

---

_Review comment by @BurntSushi on `CHANGELOG.md`:32 on 2025-07-14 17:38_

I think it could go either way. I'd lean toward enhancement personally, but only weakly so.

---

_Merged by @geofft on 2025-07-14 18:01_

---

_Closed by @geofft on 2025-07-14 18:01_

---

_Branch deleted on 2025-07-14 18:01_

---
