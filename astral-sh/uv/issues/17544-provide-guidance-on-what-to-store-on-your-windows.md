```yaml
number: 17544
title: Provide guidance on what to store on your Windows Dev Drive
type: issue
state: open
author: brettcannon
labels: []
assignees: []
created_at: 2026-01-16T23:51:47Z
updated_at: 2026-01-19T18:02:30Z
url: https://github.com/astral-sh/uv/issues/17544
synced_at: 2026-01-19T18:27:52Z
```

# Provide guidance on what to store on your Windows Dev Drive

---

_@brettcannon_

https://learn.microsoft.com/en-us/windows/dev-drive/#what-should-i-put-on-my-dev-drive provides guidance on what files should be put on one's Dev Drive. I'm not sure what the equivalent guidance would be for uv as there a lot of potential storage paths to set. For example, I assume the package cache would go on the Dev Drive, but should tools as well as they pull directly from the cache?

---

_Comment by @brettcannon on 2026-01-16 23:52_

Forked from https://github.com/astral-sh/uv/issues/7008#issuecomment-3758787115 .

---

_Comment by @zooba on 2026-01-19 18:02_

FTR (as one of the team who designed Dev Drive), the ideal here would be for uv to have it's caches on the same drive as where the "real" environments are. A developer trying to install packages onto a Dev Drive is a good enough signal to keep caches somewhere on that drive, though we never settled on a directory structure for it (POSIXy `x:\.uv` was my suggestion).

The code to check if a path is on a Dev Drive [is here](https://github.com/python/cpython/blob/main/Modules/posixmodule.c#L5391) (in C, not sure if there's a Rust equivalent floating around yet).

Our alternative idea was that tools would recommend the global config/environment settings needed to just move the entire cache onto a Dev Drive, and let the user decide. Users will typically be developers, so it doesn't need to be completely automatic, but they may not recognise the benefit of moving the cache without being prompted. Though if you're relying on hard links that can't cross partitions, then having the cache on the same drive is probably more benefit than always putting it on a Dev Drive.

Also FWIW, ReFS (the file system used for Dev Drives) supports automatic copy-on-write if you're using the native CopyFile APIs, so copies will be basically free. Not as good as a directory link, for sure, but worth knowing about.

---
