---
number: 73
title: Support file substitutions
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: support-substitutions
created_at: 2025-08-12T01:29:18Z
updated_at: 2025-08-26T16:22:20Z
url: https://github.com/zanieb/rooster/pull/73
synced_at: 2026-01-09T23:57:07Z
---

# Support file substitutions

---

_Pull request opened by @samypr100 on 2025-08-12 01:29_

In order to support https://github.com/astral-sh/uv/pull/15196

This adds initial support to rooster for file substitutions.

Replace old version with new version in README.md using the special substitution string `rooster.previous`.

```toml
[tool.rooster]
...
substitution_files = { "README.md"  = ["rooster.previous"] }
```

or

Replace FOO and BAR with the new version on all md files.

```toml
[tool.rooster]
...
substitution_files = { "**/*.md"  = ["FOO", "BAR"] }
```

---

_Comment by @zanieb on 2025-08-12 02:26_

I might prefer something like

```
substitution-files = ["README.md", {file = "**/*.md", replace = "foo"}]
```

So we don't need to add the special `rooster.previous` string? wdyt?

---

_Comment by @samypr100 on 2025-08-12 02:45_

> I might prefer something like
> 
> ```
> substitution-files = ["README.md", {file = "**/*.md", replace = "foo"}]
> ```
> 
> So we don't need to add the special `rooster.previous` string? wdyt?

We can also assume the behavior of "rooster.previous" if the substitution array is empty? Since right now this also works:

```toml
[tool.rooster]
...
substitution_files = { "**/*.md"  = ["rooster.previous", "FOO", "BAR"], "README.md"  = ["BAZ"] }
```

I didn't add single file support because I feel that's covered by `version_files`, or is your intent to supersede it by this?

---

_Comment by @samypr100 on 2025-08-12 02:48_

I do get `rooster.previous` may feel weird though 😅 

---

_Comment by @zanieb on 2025-08-12 02:53_

Oh I see — sorry I don't know my own APIs here. Yeah, I think I'd replace `version_files`? Or at least, I wouldn't have two settings that do such similar things. I think if we did a generic substitution I'd expect more like..

```
substitutions = [ {find = "rooster.previous", replace = "rooster.next", files = "**/*.md"} ]
```

I don't know if I really want that yet though, so maybe we should just add some functionality to `version_files`?

---

_Comment by @samypr100 on 2025-08-12 03:19_

I ended up going ahead and adding support to replacements to the existing `version_files` like you proposed originally.

This will work now:

```toml
version_files = [ 
    "README.md", # Existing functionality
    { target = "**/*.md" },  # Replaces all prev versions with new version on all matches
    { target = "**/*.md", replace = "FOO" } # Replaces all instances of FOO with new version on all matches
]
```

---

_Comment by @samypr100 on 2025-08-13 02:31_

Note, I chose `target` over `file` for the glob as it felt weird to name a glob pattern parameter `file` but I can switch to `file` if that's preferred.

---

_Comment by @samypr100 on 2025-08-26 16:05_

Sorry for the ping as you're very busy; thoughts here? 😄

---

_@zanieb approved on 2025-08-26 16:09_

---

_Merged by @zanieb on 2025-08-26 16:14_

---

_Closed by @zanieb on 2025-08-26 16:14_

---

_Branch deleted on 2025-08-26 16:22_

---
