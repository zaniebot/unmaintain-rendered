```yaml
number: 5132
title: "🐢 `.ruff_cache` directory has a huge size and it slows down PyCharm load"
type: issue
state: closed
author: saippuakauppias
labels:
  - bug
assignees: []
created_at: 2023-06-15T21:19:19Z
updated_at: 2023-06-27T13:17:09Z
url: https://github.com/astral-sh/ruff/issues/5132
synced_at: 2026-01-10T11:09:47Z
```

# 🐢 `.ruff_cache` directory has a huge size and it slows down PyCharm load

---

_Issue opened by @saippuakauppias on 2023-06-15 21:19_

After checking with all linters of a large project - the directory `.ruff_cache` has a large size and a huge number of files inside. The first load of PyCharm slows down at the stage of indexing all the files.

The volume of lines in all files:
```
$ ( find src apps -name '*.py' -print0 | xargs -0 cat ) | wc -l
  979136
```

Cache directory size:
```
$ du -h .ruff_cache
1,5G	.ruff_cache/content
1,5G	.ruff_cache
```

Number of files in the cache directory:
```
ls -1 .ruff_cache/content | wc -l
  319586
```

As a quick fix, I excluded directory `.ruff_cache` using the context menu: «Mark Directory as -> Excluded».

But it doesn't seem to be ok if the cache doesn't clear itself.

PS: MacBook Pro Intel Core i9 with SSD

---

_Comment by @charliermarsh on 2023-06-15 21:28_

Thanks for this. Coincidentally, we're actually working on the cache a bit now (\cc @Thomasdezeeuw). It has a couple significant problems. (You can always run `ruff clean` to remove all caches recursively, though it's not self-cleaning.)

---

_Comment by @charliermarsh on 2023-06-15 22:04_

I believe #5117 would help with this significantly, as it would bound the size of the cache to the size of the project (rather than letting it grow in perpetuity).

#5122 would also help a lot, since we'll move the cache out of the project directory and into the global cache directory.

---

_Comment by @KotlinIsland on 2023-06-16 00:20_

> As a quick fix, I excluded directory .ruff_cache using the context menu: «Mark Directory as -> Excluded».
> 
> But it doesn't seem to be ok if the cache doesn't clear itself.

As far as I know, this is the recommended solution. Can you elaborate on what you mean by "But it doesn't seem to be ok if the cache doesn't clear itself."? 


---

_Assigned to @Thomasdezeeuw by @Thomasdezeeuw on 2023-06-16 08:14_

---

_Label `bug` added by @charliermarsh on 2023-06-16 17:24_

---

_Comment by @saippuakauppias on 2023-06-17 18:46_

I meant that it would be good if ruff itself would periodically check the directory with the cache at the time of the check and automatically delete old files (for example, those that were created earlier than 7 days ago).

Maybe we should rework the caching system itself, because it is not normal to have so many files, don't forget that a disk has inodes.

---

_Comment by @charliermarsh on 2023-06-17 18:47_

#5117 will fix that -- it changes to a single cache file per package.

---

_Comment by @saippuakauppias on 2023-06-17 18:49_

By the way, in principle, if in the directory is formed such a large number of files - it is not normal. The directory listing will be slow.

Usually, if the cache is stored on disk - try to spread it over N directories inside, so that each directory has a limited number of files.

---

_Comment by @Thomasdezeeuw on 2023-06-27 13:17_

Should fixed by https://github.com/astral-sh/ruff/pull/5120.

---

_Closed by @Thomasdezeeuw on 2023-06-27 13:17_

---
