```yaml
number: 1510
title: "ignore crate: adding a min_modified() method to skip old files"
type: pull_request
state: closed
author: P-E-Meunier
labels: []
assignees: []
base: master
head: master
created_at: 2020-03-07T15:37:23Z
updated_at: 2020-03-07T16:34:35Z
url: https://github.com/BurntSushi/ripgrep/pull/1510
synced_at: 2026-01-12T18:23:14Z
```

# ignore crate: adding a min_modified() method to skip old files

---

_@P-E-Meunier_

In some cases we want to skip older files when walking through a directory. This PR does just that.

---

_Comment by @BurntSushi on 2020-03-07 15:50_

I'm not sure this is a good fit. It's adding a lot of complexity and seems fairly niche. Moreover, this seems like the first of a long line of additional options: `max_modified`, `min_created`, `max_created`, `min_accessed`, `max_accessed`, and so on. I'd rather we have a more cohesive strategy to address those.

In the future, I strongly recommend opening issues before filing PRs, especially when the PRs are non-trivial.

---

_Comment by @P-E-Meunier on 2020-03-07 15:55_

Alright, sorry if I didn't follow the normal contribution process.

---

_Closed by @P-E-Meunier on 2020-03-07 15:55_

---

_Comment by @BurntSushi on 2020-03-07 16:26_

I don't really have a normal documented contribution process. I'm just trying to save you time for next time. Trivial PRs are fine because if they don't end up being merged, then you didn't spend a lot of time on it. Meatier PRs are best to discuss first to make sure the feature is actually desirable.

---

_Comment by @P-E-Meunier on 2020-03-07 16:33_

Oh, alright. So, I really need this feature, which is why I wrote it. Do you think it could be merged with max_filesize, for instance by providing arbitrary filters?
Let's say:

```
builder.filter(|path| std::fs::metadata(path).unwrap().filesize() < 200_000)
```

A perhaps cleaner design would allow stacking up these filters (like std's iterators), but then the parallel walker might be harder to do, since you would need a trait to tell that two stacked filters are a valid walker.

---
