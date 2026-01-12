```yaml
number: 396
title: "lots of time is spent in allocation in `puffin-dev resolve-many` for the top PyPI packages"
type: issue
state: closed
author: BurntSushi
labels:
  - performance
assignees: []
created_at: 2023-11-10T17:59:13Z
updated_at: 2024-01-05T16:57:34Z
url: https://github.com/astral-sh/uv/issues/396
synced_at: 2026-01-12T15:58:23Z
```

# lots of time is spent in allocation in `puffin-dev resolve-many` for the top PyPI packages

---

_@BurntSushi_

One thing i'm seeing is that we are spending a lot of time in allocation, around 16% for `malloc` and 11.5% for `free` of total runtime for the top 1K pypi packages:

![malloc](https://github.com/astral-sh/puffin/assets/456674/f0383638-9f7f-49e5-a04f-65ab4539ae31)

That jumps to about 18% for `malloc` and 13% for `free` in the top 4K.

My hypothesis for this is that we are allocating oodles of tiny little objects everywhere. For example, `Version` has two `Vec`s in it and `WheelFilename` adds many more allocs with more indirection. There are probably other places with lots of tiny little objects.

There are a variety of different approaches one can use to attack this problem (if we consider it a problem at all):

* Try container types that use inline storage for small elements and fallback to the heap for bigger elements. I'm thinking about things like [`smallvec`](https://docs.rs/smallvec/latest/smallvec/). This is also known as a "small string optimization" (SSO). This might be a good first thing to try since I'm guessing it's quite relevant. I imagine many of the `Vec`s and `String`s in `WheelFilename` and `Version`, for example, are quite small.
* We could try different allocators. It looks like ruff uses `mimalloc`. My system is using glibc's allocator which is generally pretty fast. So I wouldn't expect an enormous gain here.
* We could try different allocation styles. For example, a bump allocator like [`bumpalo`](https://docs.rs/bumpalo/latest/bumpalo/). This will likely require some refactoring of the code.
* We could try representational changes to our data types to make fewer allocations. For example, a `Version` could probably be represented as a `Vec<u8>` internally. This makes its representation more complex and _usually_ will incur some kind of perf penalty when you try to _use_ a `Vesion`. But it could speed up objection creation enough to make it a worthwhile trade.
* We could use some kind of string interning to exploit cross-package redundancy. It is very likely for example that the total number of unique version strings is far smaller than the total number of version strings. Similarly for other things such as package names. (Since some packages have a lot of releases.)

Anyway, this is just what I have off the top of my head. I'm actually not even certain that we want to solve this problem at all. But it is worth pointing out that some of the above remedies are much easier to try than others (`smallvec` should be quite easy to slot into the existing code and _could_ significantly reduce allocs).

---

_Label `performance` added by @BurntSushi on 2023-11-10 17:59_

---

_Comment by @charliermarsh on 2023-11-10 18:00_

Personal opinion is that I'd love to improve this and these ideas strike me as being in the right direction.

---

_Comment by @konstin on 2023-11-10 18:27_

I'd love to try smallvec for `release`. We can also use `u32` instead of `usize` in there

---

_Comment by @charliermarsh on 2023-11-10 18:29_

We could also use an enum for `release` that captures the overwhelmingly common cases of 1, 2, or 3 segments.

---

_Comment by @charliermarsh on 2023-11-10 18:31_

That might be strictly more work. I recall `smallvec` introduces some overhead since you need to check if an element is on the stack or the heap, but I guess an enum would have the same problem :)

---

_Comment by @konstin on 2023-11-10 19:19_

Perfwise we shouldn't see the difference, but `smallvec` would be more ergonomic with less effort

---

_Comment by @BurntSushi on 2023-11-10 19:50_

#399 gets a ~10% perf win by switching the global allocator. I did attempt a smallvec optimization experiment (as mentioned above) but couldn't observe any measurable win. I'm not 100% convinced that smallvec has no role to play and I'm sure there is more that can still be done here.

---

_Comment by @charliermarsh on 2024-01-05 04:36_

@BurntSushi - Should we close this when merging #789 or is there more that's worth tracking here?

---

_Comment by @BurntSushi on 2024-01-05 12:39_

Ah yeah that's a good point. Yeah, I don't think this specific issue still exists once #789 gets merged. (Or even before it to be honest. I think gratuitous allocation has been removed from the happy path for the most part already.)

---

_Closed by @BurntSushi on 2024-01-05 16:57_

---

_Closed by @BurntSushi on 2024-01-05 16:57_

---
