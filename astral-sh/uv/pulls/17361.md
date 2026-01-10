```yaml
number: 17361
title: "Allow `--bump post` with `--bump dev`"
type: pull_request
state: open
author: zanieb
labels:
  - enhancement
assignees: []
base: main
head: claude/allow-post-prerelease-bump-nfrvn
created_at: 2026-01-08T14:38:30Z
updated_at: 2026-01-08T19:02:57Z
url: https://github.com/astral-sh/uv/pull/17361
synced_at: 2026-01-10T05:49:14Z
```

# Allow `--bump post` with `--bump dev`

---

_Pull request opened by @zanieb on 2026-01-08 14:38_

Closes https://github.com/astral-sh/uv/issues/17359

---

_Renamed from "Allow `--bump post` with pre-release bumps" to "Allow `--bump post` with `--bump dev`" by @zanieb on 2026-01-08 17:51_

---

_Marked ready for review by @zanieb on 2026-01-08 18:01_

---

_@Gankra approved on 2026-01-08 18:03_

---

_@Gankra reviewed on 2026-01-08 18:03_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:222 on 2026-01-08 18:03_

This should be an allow-list, not a deny-list

---

_@Gankra reviewed on 2026-01-08 18:04_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/version.rs`:222 on 2026-01-08 18:04_

(`!matches(VersionBump::Post)`)

---

_Label `bug` added by @konstin on 2026-01-08 18:05_

---

_Comment by @codspeed-hq[bot] on 2026-01-08 18:10_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
### Merging this PR will **not alter performance**




### Summary

`✅ 5` untouched benchmarks  



---

<sub>Comparing <code>zaniebot:claude/allow-post-prerelease-bump-nfrvn</code> (6329a3e) with <code>main</code> (29285db)</sub>

<a href="https://codspeed.io/astral-sh/uv/branches/zaniebot%3Aclaude%2Fallow-post-prerelease-bump-nfrvn?utm_source=github&utm_medium=comment-v2&utm_content=button">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://codspeed.io/pr-report/open-in-codspeed-dark.svg">
    <source media="(prefers-color-scheme: light)" srcset="https://codspeed.io/pr-report/open-in-codspeed-light.svg">
    <img alt="Open in CodSpeed" src="https://codspeed.io/pr-report/open-in-codspeed-light.svg" width="169" height="32">
  </picture>
</a>



---

_Label `bug` removed by @zanieb on 2026-01-08 18:42_

---

_Label `enhancement` added by @zanieb on 2026-01-08 18:42_

---

_Comment by @zanieb on 2026-01-08 18:42_

(@konstin I don't consider this a bug fix, it's definitely working _as intended_ today)


---

_Comment by @zanieb on 2026-01-08 18:42_

I'm still not entirely sure we should merge this.

---
