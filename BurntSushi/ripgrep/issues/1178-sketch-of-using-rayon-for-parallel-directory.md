```yaml
number: 1178
title: Sketch of using rayon for parallel directory traversal
type: issue
state: open
author: jessegrosjean
labels:
  - enhancement
  - icebox
assignees: []
created_at: 2019-01-28T18:11:36Z
updated_at: 2019-02-09T17:59:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1178
synced_at: 2026-01-12T16:13:23Z
```

# Sketch of using rayon for parallel directory traversal

---

_@jessegrosjean_

You've mentioned a few times that you'd like to move directory traversal to rayon. Here's a working/incomplete sketch of one approach to doing that:

https://github.com/jessegrosjean/walk

Maybe all obvious, but maybe a useful resource (and was good learning for me) for later development. The basic design is to create a channel to use as work queue. And then use rayon's `par_bridge()` to process that channel of work in parallel.

Performance is comparable to `ignore` crate walking linux source:

```
par_ignore_walk         time:   [72.235 ms 73.115 ms 73.753 ms]                           
                        change: [-3.4721% -2.1156% -0.7586%] (p = 0.01 < 0.05)
                        Change within noise threshold.

rayon_walk              time:   [60.600 ms 60.734 ms 60.917 ms]                      
                        change: [+0.0896% +0.5440% +1.0160%] (p = 0.04 < 0.05)
                        Change within noise threshold.
```

In this run I have ignore crate not doing any filtering, but it's still likely doing more then my rayon_walk, so I expect performance is about the same between the two even though rayon walk is running faster. Another benefit of this design is that it's fairly strait forward to get sorted results also computed in parallel. But that adds some complexity so I figured I would post this without that code for now.

Last you also mentioned wanting to rethink the `ignore` create API. Currently it's callback based. I wonder if it would make more sense (again if you are thinking long term about a redesign) to make it iterator based. So ignore crate would just be responsible for generating an iterator over `DirEntry` results as quickly as possible. If you also wanted to do heavy processing on those entries you could call `par_bridge` again on those results and do your heavy processing (such as perform ripgrep search) there.



---

_Comment by @BurntSushi on 2019-01-28 21:37_

@jessegrosjean Very nice! I didn't know about `par_bridge`. That seems like a very handy function! It will be a while before I can dig into that code, but it looks promising.

With respect to the `ignore` refactor... It's a fairly daunting task with a ton of tiny details, so I haven't really allowed myself to think about it too closely. Briefly, here are my objectives:

* Figure out how to make ignore rules easier to understand and reason about. Both from an API perspective and an implementation perspective.
* Figure out how to de-couple the the parallel directory traverser with ignore rule processing. There is some interesting work to be done here since we need to retain the requirement that every `.gitignore`/`.ignore`/etc file is parsed and built exactly once, and then reused. Currently, this is done by maintaining what is effectively a thread safe linked list.

---

_Comment by @jessegrosjean on 2019-01-29 21:57_

> Figure out how to de-couple the the parallel directory traverser with ignore rule processing. There is some interesting work to be done here since we need to retain the requirement that every .gitignore/.ignore/etc file is parsed and built exactly once, and then reused. Currently, this is done by maintaining what is effectively a thread safe linked list.

Can you describe this problem in a little more detail? I'm working under assumption that .gitignore/.ignore/etc rules only apply to the current directory (and subdirectories). Is that correct?

---

_Comment by @BurntSushi on 2019-01-29 22:01_

I really don't have the bandwidth to go into a lot of detail. If you look at the parallel traverser in the crate today, then the logic for iterating over directory entries is coupled with the logic for filtering file paths. These _should_ be orthogonal concerns and I'm not yet convinced the coupling is necessary.

> I'm working under assumption that .gitignore/.ignore/etc rules only apply to the current directory (and subdirectories). Is that correct?

Yes. With the exception being global gitignore rules or explicitly specified ignore files. But those are easy; they are applied everywhere.

---

_Comment by @jessegrosjean on 2019-01-29 22:30_

> These should be orthogonal concerns and I'm not yet convinced the coupling is necessary.

I'm not sure that I follow. It seems to me that (at least as an optimization) it makes sense for the walk to know about any ignore/filtering logic. That will save lots of work, not having to walk ignored directories.

I do think that the walk shouldn't be coupled with any particular ignore logic. The user of the walk should be able to provide that logic as they see fit, just like they can also provide there own sort logic.

Anyway, I'll post a more concrete implementation to talk about in a few days, then can talk more about the details.

---

_Comment by @jessegrosjean on 2019-01-30 21:52_

Here's a more complete implementation. It follows the same rayon usage pattern, but is more complex because it now supports streamed sorted results. I think it also has the needed API to support .gitignore based filtering, though I haven't implemented that. Performance is about the same as before. 

At the top level is a simple simple WalkDir implementation that shows how to use the more complex `core::walk` function. `core::walk` allows for arbitrary sorting/filtering and maintaining a state value for each directory read. That state value is then cloned and sent on to child directory reads.

https://github.com/jessegrosjean/jwalk

---

_Comment by @BurntSushi on 2019-01-31 12:25_

> I'm not sure that I follow. It seems to me that (at least as an optimization) it makes sense for the walk to know about any ignore/filtering logic. That will save lots of work, not having to walk ignored directories.

It's not clear to me that the coupling is necessary for performance.

Again, I'm just trying to state what my high level goals are. I'm sorry, but I really just do not have the bandwidth to dive into this right now. It's too complex for me to look at this passively, so the best I can do is say what I _think_ my goals are. Whether they are obtainable or not is not something I know right now.

Consider, for example, users that want parallel directory traversal as an alternative to `walkdir` without all the ignore-related logic. The ideal way to provide that is to provide a crate that only cares about parallel directory traversal and another crate that layers ignore logic on top of it. This is exactly how the `ignore` crate works today for the single threaded case, since it uses `walkdir` for directory traversal in that case because `walkdir` has hooks for permitting the caller to skip the traversal of sub-directories. I don't know whether a parallel directory iterator is amenable to the same de-coupling, but it's not immediately obvious to me that it isn't. So that's where I'd start.

I'd just like to say once again that I'm super happy to see work being done on this. However, it could unfortunately be months before I really sink my teeth into this.

---

_Label `enhancement` added by @BurntSushi on 2019-01-31 12:26_

---

_Label `icebox` added by @BurntSushi on 2019-01-31 12:26_

---

_Comment by @jessegrosjean on 2019-01-31 16:26_

No problem and thanks for all the help and feedback so far. I'll likely continue to work on this and publish as a crate if I get far enough. When you get time check back in.

---

_Comment by @jessegrosjean on 2019-02-09 17:59_

When you get a chance I've polished this idea further and published the crate here:

https://crates.io/crates/jwalk

It doesn't support "ignore" features, but I "think" it's possible to add that sort of filtering with a custom `WalkDir::process_entries` function.

---
