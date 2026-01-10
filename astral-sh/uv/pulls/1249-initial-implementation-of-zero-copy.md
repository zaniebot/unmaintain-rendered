```yaml
number: 1249
title: initial implementation of zero-copy deserialization for SimpleMetadata
type: pull_request
state: merged
author: BurntSushi
labels:
  - performance
  - internal
assignees: []
merged: true
base: main
head: ag/rkyv-cache
created_at: 2024-02-05T02:36:26Z
updated_at: 2024-02-05T21:47:54Z
url: https://github.com/astral-sh/uv/pull/1249
synced_at: 2026-01-10T15:33:24Z
```

# initial implementation of zero-copy deserialization for SimpleMetadata

---

_Pull request opened by @BurntSushi on 2024-02-05 02:36_

(Please review this PR commit by commit.)

This PR closes an initial loop on zero-copy deserialization. That
is, provides a way to get a `Archived<SimpleMetadata>` (spelled
`OwnedArchive<SimpleMetadata>` in the code) from a `CachedClient`. The
main benefit of zero-copy deserialization is that we can read bytes
from a file, cast those bytes to a structured representation without
cost, and then start using that type as any other Rust type. The
"catch" is that the structured representation is not the actual type
you started with, but the "archived" version of it.

In order to make all this work, we ended up needing to shave a rather
large yak: we had to re-implement HTTP cache semantics. Previously,
we were using the `http-cache-semantics` crate. While it does support
Serde, it doesn't support `rkyv`. Moreover, even simple support for
`rkyv` wouldn't be enough. What we actually want is for the HTTP cache
semantics to be implemented on the *archived* type so that we can
decide whether our cached response is stale or not without needing to
do a full deserialization into the unarchived type. This is why, in
this PR, you'll see `impl ArchivedCachePolicy { ... }` instead of
`impl CachePolicy { ... }`. (The `derive(rkyv::Archive)` macro
automatically introduces the `ArchivedCachePolicy` type into the
current namespace.)

Unfortunately, this PR does not fully realize the dream that is
zero-copy deserialization. Namely, while a `CachedClient` can now
provide an `OwnedArchive<SimpleMetadata>`, the rest of our code
doesn't really make use of it. Indeed, as soon as we go to build a
`VersionMap`, we eagerly convert our archived metadata into an owned
`SimpleMetadata` via deserialization (that *isn't* zero-copy). After
this change, a lot of the work now shifts to `rkyv` deserialization
and `VersionMap` construction. More precisely, the main thing we drop
here is `CachePolicy` deserialization (which is now truly zero-copy)
and the parsing of the MessagePack format for `SimpleMetadata`. But we
are still paying for deserialization. We're just paying for it in a
different place.

This PR does seem to bring a speed-up, but it is somewhat underwhelming.
My measurements have been pretty noisy, but I get a 1.1x speedup fairly
often:

```
$ hyperfine -w5 "puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A kang
Benchmark 1: puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     164.4 ms ±  18.8 ms    [User: 427.1 ms, System: 348.6 ms]
  Range (min … max):   131.1 ms … 190.5 ms    18 runs

Benchmark 2: puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     148.3 ms ±  10.2 ms    [User: 357.1 ms, System: 319.4 ms]
  Range (min … max):   136.8 ms … 184.4 ms    19 runs

Summary
  puffin-test pip compile --cache-dir ~/astral/tmp/cache-test ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.11 ± 0.15 times faster than puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

One downside is that this does increase cache size (`rkyv`'s
serialization format is not as compact as MessagePack). On disk size
increases by about 1.8x for our `simple-v0` cache.

```
$ sort-filesize cache-main
4.0K    cache-main/CACHEDIR.TAG
4.0K    cache-main/.gitignore
8.0K    cache-main/interpreter-v0
8.7M    cache-main/wheels-v0
18M     cache-main/archive-v0
59M     cache-main/simple-v0
109M    cache-main/built-wheels-v0
193M    cache-main
193M    total

$ sort-filesize cache-test
4.0K    cache-test/CACHEDIR.TAG
4.0K    cache-test/.gitignore
8.0K    cache-test/interpreter-v0
8.7M    cache-test/wheels-v0
18M     cache-test/archive-v0
107M    cache-test/simple-v0
109M    cache-test/built-wheels-v0
242M    cache-test
242M    total
```

Also, while I initially intended to do a simplistic implementation of HTTP cache semantics, I found that everything was somewhat inter-connected. I could have wrote code that _specifically_ only worked with the present behavior of PyPI, but then it would need to be special cased and everything else would need to continue to use `http-cache-sematics`. By implementing what we need based on what Puffin actually is (which is still less than what `http-cache-semantics` does), we can avoid special casing and use zero-copy deserialization for our cache policy in _all_ cases.

---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-05 02:36_

---

_Review requested from @konstin by @BurntSushi on 2024-02-05 02:36_

---

_Label `performance` added by @BurntSushi on 2024-02-05 02:37_

---

_Label `internal` added by @BurntSushi on 2024-02-05 02:37_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:219 on 2024-02-05 15:15_

Is it still the case that we avoid sending revalidation requests for wheels? PyPI will return a 200 (not a 304) if you send a revalidation request for an immutable resource.

---

_@charliermarsh reviewed on 2024-02-05 15:15_

---

_Comment by @MichaReiser on 2024-02-05 15:21_

Do you expect some performance improvement by simply switching the caching implementation, or does switching the caching layer help reduce binary size, compile times, or cache size? 

I'm asking because I wonder if we should look at changing the caching separately from enabling zero copy deserialization for it as it in itself might already be a win (or it's slower than what we had before and could be the culprit why we don't see a bigger performance win)

---

_@charliermarsh reviewed on 2024-02-05 15:22_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:219 on 2024-02-05 15:22_

This was previously a problem with `--refresh`, since setting `max-age=0` meant we sent revalidation requests and therefore _always_ re-downloaded wheels.

---

_@charliermarsh reviewed on 2024-02-05 15:23_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/cached_client.rs`:463 on 2024-02-05 15:23_

Is this comment still true?

---

_@charliermarsh reviewed on 2024-02-05 15:25_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/httpcache/mod.rs`:7 on 2024-02-05 15:25_

How closely does the actual code and implementation match `http-cache-semantics`? My guess is that it's actually quite different.

---

_@charliermarsh reviewed on 2024-02-05 15:26_

---

_Review comment by @charliermarsh on `crates/puffin-client/src/httpcache/mod.rs`:896 on 2024-02-05 15:26_

Nit: "no indicators"

---

_@charliermarsh approved on 2024-02-05 15:27_

I generally view this as an improvement even ignoring the zero-copy stuff since (1) we may want fine-grained control over the cache anyway, and (2) we now have actual tests for the caching semantics.

---

_Review comment by @BurntSushi on `crates/puffin-client/src/cached_client.rs`:219 on 2024-02-05 21:13_

Oof, good catch. You're right. PyPI returns a non-304 response. Good catch.

---

_Review comment by @BurntSushi on `crates/puffin-client/src/cached_client.rs`:219 on 2024-02-05 21:22_

I've fixed this in the HTTP cache semantics layer by making the existence of an `immutable` directive on the response result in completing ignoring any cache-control directives on the incoming request that would otherwise force a revalidation request.

---

_@BurntSushi reviewed on 2024-02-05 21:27_

---

_Review comment by @BurntSushi on `crates/puffin-client/src/cached_client.rs`:463 on 2024-02-05 21:27_

The gist of it is (it's 352 bytes now), but it's in the wrong place. Good catch. I've moved it and improved the docs. :)

---

_@BurntSushi reviewed on 2024-02-05 21:29_

---

_Review comment by @BurntSushi on `crates/puffin-client/src/httpcache/mod.rs`:7 on 2024-02-05 21:29_

The API is pretty similar, but the implementation is pretty different in a number of respects. But it was still a source of inspiration. I updated the comment to say this:

```
* The `http-cache-semantics` crate. (The implementation here is completely
different, but the source of `http-cache-semantics` helped guide the
implementation here and understanding of HTTP caching.)
```

---

_Comment by @BurntSushi on 2024-02-05 21:40_

> Do you expect some performance improvement by simply switching the caching
> implementation, or does switching the caching layer help reduce binary size,
> compile times, or cache size?

Nah. I didn't see one either in my testing. Perf remained the same.

The only real point of the custom caching semantics implementation (at least currently) is to facilitate zero-copy.

> I'm asking because I wonder if we should look at changing the caching
> separately from enabling zero copy deserialization for it as it in itself
> might already be a win (or it's slower than what we had before and could be
> the culprit why we don't see a bigger performance win)

The commits are structured such that we can pretty easily measure this. `puffin-main` is current main, `puffin-newcache` is `main` with the new caching implementation and `puffin-zero` is the new caching implementation with zero-copy deserialization:

```
$ hyperfine -w5 --runs 30 \
    "puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" \
    "puffin-newcache pip compile --cache-dir ~/astral/tmp/cache-newcache ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" \
    "puffin-zero pip compile --cache-dir ~/astral/tmp/cache-zero ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null"
Benchmark 1: puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     166.6 ms ±  16.5 ms    [User: 426.8 ms, System: 340.2 ms]
  Range (min … max):   137.2 ms … 204.2 ms    30 runs

Benchmark 2: puffin-newcache pip compile --cache-dir ~/astral/tmp/cache-newcache ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     162.9 ms ±  21.3 ms    [User: 394.4 ms, System: 379.9 ms]
  Range (min … max):   125.5 ms … 191.1 ms    30 runs

Benchmark 3: puffin-zero pip compile --cache-dir ~/astral/tmp/cache-zero ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     146.7 ms ±   4.8 ms    [User: 357.9 ms, System: 308.6 ms]
  Range (min … max):   137.0 ms … 157.5 ms    30 runs

Summary
  puffin-zero pip compile --cache-dir ~/astral/tmp/cache-zero ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.11 ± 0.15 times faster than puffin-newcache pip compile --cache-dir ~/astral/tmp/cache-newcache ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
    1.14 ± 0.12 times faster than puffin-main pip compile --cache-dir ~/astral/tmp/cache-main ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

There isn't much of a difference between `puffin-main` and `puffin-newcache` (I don't expect there to be), but `puffin-zero` is a bit faster.

I don't expect the new caching semantics to make a difference because I didn't identify any obvious bugs in how `http-cache-semantics` worked for our specific use case. Like, there wasn't a case where we weren't using a cached response when we should have been.

---

_Merged by @BurntSushi on 2024-02-05 21:47_

---

_Closed by @BurntSushi on 2024-02-05 21:47_

---

_Branch deleted on 2024-02-05 21:47_

---
