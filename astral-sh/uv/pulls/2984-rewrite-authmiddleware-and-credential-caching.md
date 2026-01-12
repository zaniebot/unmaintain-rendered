```yaml
number: 2984
title: "Rewrite `AuthMiddleware` and credential caching"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: zb/refactor-auth-store
head: zb/test-middleware
created_at: 2024-04-11T00:09:34Z
updated_at: 2024-04-15T19:54:43Z
url: https://github.com/astral-sh/uv/pull/2984
synced_at: 2026-01-12T16:05:21Z
```

# Rewrite `AuthMiddleware` and credential caching

---

_@zanieb_

Further work based on #2976 with a focus on improving the correctness of our `AuthMiddleware`.

I ended up needing to make significant changes to ensure we properly populate passwords on requests that already have a username attached.

The behavior can be summarized as follows:

- Credentials are always parsed from the request, including inspection of the Authorization header contents
   - This is necessary to extract the username
   - We only support HTTP Basic Authentication at this time, I can't think of a way other Authorization headers would show up
- Fully authenticated (username and password) request credentials are respected over all other methods
   - Cache lookups are avoided in this case
- The cache is keyed on realm and username instead of just realm
- Subsequent attempts to fetch credentials from the netrc file and keyring in the middleware are no longer prevented
   - Previously we would use the cache to indicate if we should attempt to fetch credentials
   - The netrc file is in-memory and it seems fine to do a lookup per unauthenticated request
   - The keyring provider includes an internal cache to avoid repeated subprocess calls
- If the credentials do not include a password, we will attempt to find a password in the cache, netrc, and keyring
- When performing lookups, we will respect the username attached to the request (fixes [#2563](https://github.com/astral-sh/uv/pull/2563))
- Credentials are only added to the cache if a request is successful
    - Cache entries are always overwritten on insertion
    - When we insert a cache entry for credentials with a username, we also fill the cache entry for a null username
- A bunch of logs are added and some are changed to `trace` levels

We also add some changes to keyring authentication:

- We attempt a keyring lookup for the full URL _and_ the host
    - Previously, we only attempted the URL 
- We still cache by host, so we _could_ use the wrong credentials if the keyring is inconsistent per host

Closes https://github.com/astral-sh/uv/pull/2570
Closes https://github.com/astral-sh/uv/pull/2563

Partially address:
- https://github.com/astral-sh/uv/issues/2465
- https://github.com/astral-sh/uv/issues/2464

---

_Label `internal` added by @zanieb on 2024-04-11 00:16_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:82 on 2024-04-11 00:17_

This should mean we do not need to "pre-populate" the store from the URLs beforehand. I wonder if we still should?

This should be the only behavioral change here. I don't think it's user-facing, but we'll extract and store authentication from the headers.

---

_@zanieb reviewed on 2024-04-11 00:17_

---

_Renamed from "Refactor `AuthMiddleware` and improve authentication coverage" to "Refactor `AuthMiddleware` and credential caching" by @zanieb on 2024-04-11 20:22_

---

_Renamed from "Refactor `AuthMiddleware` and credential caching" to "Rewrite `AuthMiddleware` and credential caching" by @zanieb on 2024-04-12 18:23_

---

_Comment by @zanieb on 2024-04-12 18:36_

I think I should bench this for performance regressions in request-heavy workloads.

---

_Review requested from @charliermarsh by @zanieb on 2024-04-12 18:40_

---

_Comment by @zanieb on 2024-04-12 21:19_

Benchmarks not showing change when there's not authentication involved

```
❯ python -m scripts.bench \
    --uv-path ./target/release/uv-main \
    --uv-path ./target/release/uv-branch-ii \
    ./scripts/requirements/home-assistant.in --min-runs 15
Benchmark 1: ./target/release/uv-main (resolve-cold)
  Time (mean ± σ):     17.239 s ±  0.432 s    [User: 74.618 s, System: 28.660 s]
  Range (min … max):   16.588 s … 18.125 s    15 runs
 
Benchmark 2: ./target/release/uv-branch-ii (resolve-cold)
  Time (mean ± σ):     17.266 s ±  0.511 s    [User: 74.233 s, System: 28.381 s]
  Range (min … max):   16.875 s … 19.033 s    15 runs

Summary
  ./target/release/uv-main (resolve-cold) ran
    1.00 ± 0.04 times faster than ./target/release/uv-branch-ii (resolve-cold)

Benchmark 1: ./target/release/uv-main (resolve-warm)
  Time (mean ± σ):     166.7 ms ±   2.5 ms    [User: 156.2 ms, System: 248.2 ms]
  Range (min … max):   163.4 ms … 173.9 ms    17 runs
 
Benchmark 2: ./target/release/uv-branch-ii (resolve-warm)
  Time (mean ± σ):     166.5 ms ±   2.6 ms    [User: 155.2 ms, System: 253.2 ms]
  Range (min … max):   162.3 ms … 172.6 ms    17 runs
 
Summary
  ./target/release/uv-branch-ii (resolve-warm) ran
    1.00 ± 0.02 times faster than ./target/release/uv-main (resolve-warm)

❯ python -m scripts.bench \
    --uv-path ./target/release/uv-main \
    --uv-path ./target/release/uv-branch \
    ./scripts/requirements/trio.in
Benchmark 1: ./target/release/uv-main (resolve-cold)
  Time (mean ± σ):     708.4 ms ±  52.7 ms    [User: 140.5 ms, System: 103.9 ms]
  Range (min … max):   629.5 ms … 793.2 ms    10 runs
 
Benchmark 2: ./target/release/uv-branch (resolve-cold)
  Time (mean ± σ):     727.3 ms ±  50.1 ms    [User: 145.7 ms, System: 108.8 ms]
  Range (min … max):   673.0 ms … 808.1 ms    10 runs
 
Summary
  ./target/release/uv-main (resolve-cold) ran
    1.03 ± 0.10 times faster than ./target/release/uv-branch (resolve-cold)

❯ python -m scripts.bench \
    --uv-path ./target/release/uv-main \
    --uv-path ./target/release/uv-branch \
    --min-runs 30 \
    ./scripts/requirements/boto3.in

Benchmark 1: ./target/release/uv-main (resolve-cold)
  Time (mean ± σ):      4.154 s ±  0.793 s    [User: 1.279 s, System: 0.938 s]
  Range (min … max):    2.657 s …  5.486 s    30 runs
 
Benchmark 2: ./target/release/uv-branch (resolve-cold)
  Time (mean ± σ):      4.007 s ±  0.886 s    [User: 1.230 s, System: 0.913 s]
  Range (min … max):    2.606 s …  6.562 s    30 runs
 
Summary
  ./target/release/uv-branch (resolve-cold) ran
    1.04 ± 0.30 times faster than ./target/release/uv-main (resolve-cold)

❯ python -m scripts.bench \
    --uv-path ./target/release/uv-main \
    --uv-path ./target/release/uv-branch-ii \
    ./scripts/requirements/trio.in \
  --benchmark install-cold \
   --min-runs 30
Benchmark 1: ./target/release/uv-main (install-cold)
  Time (mean ± σ):      1.843 s ±  0.505 s    [User: 0.244 s, System: 0.466 s]
  Range (min … max):    0.781 s …  3.285 s    30 runs
 
Benchmark 2: ./target/release/uv-branch-ii (install-cold)
  Time (mean ± σ):      1.995 s ±  0.507 s    [User: 0.269 s, System: 0.531 s]
  Range (min … max):    0.872 s …  2.945 s    30 runs
 
Summary
  ./target/release/uv-main (install-cold) ran
    1.08 ± 0.40 times faster than ./target/release/uv-branch-ii (install-cold)
```

I want to craft a bench that uses the keyring so we can ensure we're good there.

---

_@charliermarsh reviewed on 2024-04-13 23:14_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:87 on 2024-04-13 23:14_

I think we should avoid putting effects in optional chains like this. I'd suggest something like:

```rust
if let Some(username) = credentials.as_ref().and_then(...) {
  let _ = url.set_username(username);
}
```

It took me a bit to see that this was mutating the URL.

---

_@charliermarsh reviewed on 2024-04-13 23:16_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:81 on 2024-04-13 23:16_

Not a huge fan of code that only runs in debug. Maybe we just do this every time (or never)?

---

_@charliermarsh reviewed on 2024-04-13 23:18_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:134 on 2024-04-13 23:18_

Just confirming: if we have both netrc and keyring enabled, and the URL isn't in the netrc, we subsequently check the keyring, right? (That's my read of the code. Confirming my understanding + that it's intended.)

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:19 on 2024-04-13 23:19_

You could probably make this `Option<Box<str>>` if you were excited about shrinking the struct size :joy:

---

_@charliermarsh reviewed on 2024-04-13 23:19_

---

_@charliermarsh reviewed on 2024-04-13 23:20_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-13 23:20_

Should we `.expect` these, like below?

---

_@charliermarsh reviewed on 2024-04-13 23:20_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:105 on 2024-04-13 23:20_

Wonder if this should `.expect`?

---

_@charliermarsh reviewed on 2024-04-13 23:22_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/middleware.rs`:128 on 2024-04-13 23:22_

What is this case exactly -- you provided a username but no password, and we want to lookup in the netrc based on the username?

---

_@zanieb reviewed on 2024-04-14 15:50_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:81 on 2024-04-14 15:50_

It's just for display, I wanted to avoid the extra clone on every request. It's really helpful for seeing what username we're processing and if there's a password. I think it's worth having for debugging and I'd want it on-always rather than removed.

---

_@zanieb reviewed on 2024-04-14 15:50_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:134 on 2024-04-14 15:50_

Yep! and that's the same as the behavior on `main`.

---

_@zanieb reviewed on 2024-04-14 15:51_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:19 on 2024-04-14 15:51_

I made it `Option` for clarity because it's passed around as an empty string (`""`) a lot in reqwest and it simplified the logic to encode that as `None`.

---

_@zanieb reviewed on 2024-04-14 15:55_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:128 on 2024-04-14 15:55_

If you provide a username, it needs to match the one in the netrc. If you don't, the full credential can be retrieve from the netrc (username and password).

https://github.com/astral-sh/uv/blob/65ac2607b6b0f26a9d88022832e43807b9d533e2/crates/uv-auth/src/credentials.rs#L44-L47

---

_@zanieb reviewed on 2024-04-14 15:56_

---

_Review comment by @zanieb on `crates/uv-auth/src/middleware.rs`:87 on 2024-04-14 15:56_

Okay no problem, just felt verbose.

---

_@zanieb reviewed on 2024-04-14 15:57_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-14 15:57_

No it's valid for `username` to be `None` and default to an empty string.

---

_@zanieb reviewed on 2024-04-14 15:58_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:105 on 2024-04-14 15:58_

Right now if we see anything we don't recognize we just give up. I'm not sure if basic authentication is _guaranteed_ to always be base64 encoded, I could look that up.

---

_@charliermarsh reviewed on 2024-04-14 16:10_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:19 on 2024-04-14 16:10_

Yeah `Option` is cool, this makes sense. (Maybe document that it's guaranteed to be non-empty?) I was commenting on `String` vs. `Box<str>` but it doesn't really matter.

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-14 16:10_

I meant `.expect` on the result of `write!`.

---

_@charliermarsh reviewed on 2024-04-14 16:10_

---

_@charliermarsh reviewed on 2024-04-14 16:10_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-14 16:10_

Right now it's being dropped.

---

_@charliermarsh reviewed on 2024-04-14 16:11_

---

_Review comment by @charliermarsh on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-14 16:11_

But maybe that's fine? `write!` can return an error and `let _ = ...` just drops the error.

---

_@zanieb reviewed on 2024-04-14 18:47_

---

_Review comment by @zanieb on `crates/uv-auth/src/credentials.rs`:126 on 2024-04-14 18:47_

Oh I see what you mean. It does seem weird to drop the error here, I think we to know if we failed to write the credentials. I guess I'll add `expect`.

---

_Merged by @zanieb on 2024-04-15 19:54_

---

_Closed by @zanieb on 2024-04-15 19:54_

---

_Branch deleted on 2024-04-15 19:54_

---
