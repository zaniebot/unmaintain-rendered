```yaml
number: 5482
title: "investigate \"remaining universe\" fork and marker construction"
type: issue
state: closed
author: BurntSushi
labels:
  - resolver
assignees: []
created_at: 2024-07-26T14:14:35Z
updated_at: 2024-08-16T17:30:26Z
url: https://github.com/astral-sh/uv/issues/5482
synced_at: 2026-01-10T04:53:49Z
```

# investigate "remaining universe" fork and marker construction

---

_Issue opened by @BurntSushi on 2024-07-26 14:14_

In PR #5163, @konstin [brought up a concern](https://github.com/astral-sh/uv/pull/5163#discussion_r1691703519) that the way I was creating the "remaining universe" fork in the case of incomplete markers was either not correct or, perhaps, was doing more than what was needed. I spent some time trying to come up with a packse scenario that demonstrated why it was needed. My first attempt was this:

```toml
name = "fork-remaining-universe-partitioning"

[resolver_options]
universal = true

[expected]
satisfiable = true

[root]
requires = [
  "a>=2 ; sys_platform == 'windows'",
  "a<2 ; sys_platform == 'illumos'",
]

[packages.z.versions."1.0.0"]
[packages.a.versions."1.0.0"]
requires = [
  "b>=2 ; os_name == 'linux'",
  "b<2 ; os_name == 'darwin'",
  "z ; sys_platform == 'windows'",
]
[packages.a.versions."2.0.0"]
[packages.b.versions."1.0.0"]
[packages.b.versions."2.0.0"]
```

But as @konstin points out, it doesn't actually exercise the code path in question. I was wrong to think about this in terms of "parent" forks, since those are handled at a different point in the code. So I tried to provoke a scenario where the "remaining universe" fork _needed_ to be constructed by starting from the existing forks, rather than creating its own. I _think_ this requires a double sibling split, but I'm not sure. This is where I stopped:

```
name = "fork-remaining-universe-partitioning2"

[resolver_options]
universal = true

[expected]
satisfiable = true

[root]
requires = [
  "a>=2 ; sys_platform == 'windows'",
  "a<2 ; sys_platform == 'illumos'",
  "b>=2 ; os_name == 'linux'",
  "b<2 ; os_name == 'darwin'",
  "y ; sys_platform == 'foo'",
  "z ; os_name == 'bar'",
]

[packages.a.versions."1.0.0"]
[packages.a.versions."2.0.0"]
[packages.b.versions."1.0.0"]
[packages.b.versions."2.0.0"]
[packages.y.versions."1.0.0"]
[packages.z.versions."1.0.0"]
```

For now, we decided to move forward with #5163 in its current state since it fixes known bugs and doesn't introduce any _known_ regressions. But this is something we should circle back to.

---

_Label `resolver` added by @BurntSushi on 2024-07-26 14:14_

---

_Comment by @BurntSushi on 2024-07-26 14:21_

Also, I think that my next step here would be to try @konstin's suggestion in the linked PR and see if it can be knocked down.

---

_Comment by @BurntSushi on 2024-08-16 17:30_

I think this is no longer relevant with #5887 merged. Namely, while we are still dealing with incomplete markers, we are doing it more systematically in a way that deals with overlapping markers as well. So this should be fixed (if it was an issue before).

---

_Closed by @BurntSushi on 2024-08-16 17:30_

---
