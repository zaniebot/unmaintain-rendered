```yaml
number: 505
title: "Override `max-age` response header to 0"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2023-11-27T15:56:30Z
updated_at: 2024-03-23T16:03:49Z
url: https://github.com/astral-sh/uv/issues/505
synced_at: 2026-01-12T15:58:23Z
```

# Override `max-age` response header to 0

---

_@konstin_

See https://github.com/pypa/pip/blob/1273c7a9694686e438e886b6382b7d41f430a25c/src/pip/_internal/index/collector.py#L145-L158, the same reasons apply: Otherwise you can see a version/file on pypi, but it won't work in your local client until 10 min later.

This has a massive performance impact, it means that actually need to make http queries in the cached case again. These happen in waves for each new level of transitive dependencies we explore, which makes this even slower. One option to alleviate this would be trying a resolution with cached data only (fast) and then revalidating all responses at once (so all requests run in parallel) and then only if there were changes run the resolution again.


---

_Label `bug` added by @charliermarsh on 2023-11-30 01:54_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-11-30 01:54_

---

_Comment by @charliermarsh on 2024-01-16 18:52_

- `pip`: always refreshes.
- `npm`: always refreshes.
- `pnpm`: `install` never updates, `update` updates everything and re-fetches every time.
- `bun`: same as `pnpm`.

---

_Comment by @charliermarsh on 2024-01-16 18:53_

But Bun doesn't send up a `max-age=0` request, so it still gets cached data from the CDN, right?

```
Request: GET /react
	Accept: application/vnd.npm.install-v1+json; q=1.0, application/json; q=0.8, */*
	Connection: keep-alive
	User-Agent: Bun/1.0.23
	Host: registry.npmjs.org
	Accept-Encoding: gzip, deflate, br

Response: < 200 OK
< 	Date: Tue, 16 Jan 2024 18:51:57 GMT
< 	Content-Type: application/vnd.npm.install-v1+json
< 	Transfer-Encoding: chunked
< 	Connection: keep-alive
< 	CF-Ray: 84688d845bab0f79-EWR
< 	CF-Cache-Status: HIT
< 	Access-Control-Allow-Origin: *
< 	Age: 209
< 	Cache-Control: public, max-age=300
< 	ETag: W/"1d157c6623556ad7a20558460de9d64b"
< 	Last-Modified: Sun, 14 Jan 2024 02:57:39 GMT
< 	Vary: accept-encoding, accept
< 	Server: cloudflare
< 	Content-Encoding: br

    [116.05ms]
 Downloaded react versions
```

---

_Comment by @charliermarsh on 2024-01-16 18:54_

@konstin - The thing I don't understand is: doesn't the CDN cache for 10 minutes too? So couldn't you get stale responses even with `max-age=0` ?

---

_Comment by @charliermarsh on 2024-01-16 19:18_

One option here is to pre-populate the `Index` with metadata for all packages in the lockfile (if it exists), and then always send `max-age=0`...


---

_Comment by @charliermarsh on 2024-01-16 19:22_

The other option is to provide a flag to force this, like `--refresh`.

---

_Comment by @charliermarsh on 2024-01-16 19:23_

I will play around with the options to get a sense for the impact.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-16 19:42_

---

_Comment by @charliermarsh on 2024-01-16 22:42_

Confirmed that Bun caches up to 5 minutes.

---

_Comment by @charliermarsh on 2024-01-17 01:05_

Context:
- `max-age-600` is `main`.
- `max-age-0` is `main` but with `max-age=0` on the simple API requests.
- `prefetch` uses `max-age=0`, but kicks off all the API requests in parallel ahead of the resolution.

For `trio.in`:

```shell
❯ python -m scripts.bench --puffin-path ./target/release/prefetch \
        --puffin-path ./target/release/max-age-0 --puffin-path ./target/release/max-age-600 \
        ./scripts/requirements/trio.in --benchmark resolve-warm --warmup 10 --min-runs 50
Benchmark 1: ./target/release/prefetch (resolve-warm)
  Time (mean ± σ):      90.7 ms ±   6.1 ms    [User: 42.6 ms, System: 50.7 ms]
  Range (min … max):    79.0 ms … 112.9 ms    50 runs

Benchmark 2: ./target/release/max-age-0 (resolve-warm)
  Time (mean ± σ):     165.0 ms ±  70.1 ms    [User: 43.4 ms, System: 36.2 ms]
  Range (min … max):   117.8 ms … 368.4 ms    50 runs

  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet PC without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

Benchmark 3: ./target/release/max-age-600 (resolve-warm)
  Time (mean ± σ):      29.4 ms ±   0.6 ms    [User: 23.4 ms, System: 13.2 ms]
  Range (min … max):    28.3 ms …  30.8 ms    68 runs

Summary
  './target/release/max-age-600 (resolve-warm)' ran
    3.08 ± 0.22 times faster than './target/release/prefetch (resolve-warm)'
    5.61 ± 2.39 times faster than './target/release/max-age-0 (resolve-warm)'
```

---

_Comment by @charliermarsh on 2024-01-17 01:06_

For `black.in`:

```shell
❯ python -m scripts.bench --puffin-path ./target/release/prefetch \
        --puffin-path ./target/release/max-age-0 --puffin-path ./target/release/max-age-600 \
        ./scripts/requirements/black.in --benchmark resolve-warm --warmup 10 --min-runs 25
Benchmark 1: ./target/release/prefetch (resolve-warm)
  Time (mean ± σ):      57.1 ms ±   6.5 ms    [User: 11.1 ms, System: 12.7 ms]
  Range (min … max):    49.3 ms …  81.0 ms    41 runs

Benchmark 2: ./target/release/max-age-0 (resolve-warm)
  Time (mean ± σ):      81.0 ms ±  10.8 ms    [User: 11.0 ms, System: 12.4 ms]
  Range (min … max):    65.0 ms … 115.0 ms    34 runs

Benchmark 3: ./target/release/max-age-600 (resolve-warm)
  Time (mean ± σ):      10.5 ms ±   0.4 ms    [User: 6.0 ms, System: 6.1 ms]
  Range (min … max):     9.6 ms …  11.5 ms    125 runs

Summary
  './target/release/max-age-600 (resolve-warm)' ran
    5.44 ± 0.65 times faster than './target/release/prefetch (resolve-warm)'
    7.72 ± 1.07 times faster than './target/release/max-age-0 (resolve-warm)'
```

---

_Comment by @charliermarsh on 2024-01-17 01:15_

And finally, `home-assistant.in`:

```shell
❯ python -m scripts.bench --puffin-path ./target/release/prefetch \
        --puffin-path ./target/release/max-age-0 --puffin-path ./target/release/max-age-600 \
        ./scripts/requirements/home-assistant.in --benchmark resolve-warm
Benchmark 1: ./target/release/prefetch (resolve-warm)
  Time (mean ± σ):      1.104 s ±  0.060 s    [User: 0.651 s, System: 0.464 s]
  Range (min … max):    1.046 s …  1.233 s    10 runs

Benchmark 2: ./target/release/max-age-0 (resolve-warm)
  Time (mean ± σ):      1.254 s ±  0.051 s    [User: 0.686 s, System: 0.492 s]
  Range (min … max):    1.181 s …  1.331 s    10 runs

Benchmark 3: ./target/release/max-age-600 (resolve-warm)
  Time (mean ± σ):     510.8 ms ±   5.2 ms    [User: 469.4 ms, System: 168.2 ms]
  Range (min … max):   502.9 ms … 521.7 ms    10 runs

Summary
  './target/release/max-age-600 (resolve-warm)' ran
    2.16 ± 0.12 times faster than './target/release/prefetch (resolve-warm)'
    2.46 ± 0.10 times faster than './target/release/max-age-0 (resolve-warm)'
```

---

_Comment by @charliermarsh on 2024-01-17 01:20_

So adding `max-age=0` is something like a 2.5-8x slowdown on cached resolutions.

---

_Comment by @konstin on 2024-01-17 17:40_

> The thing I don't understand is: doesn't the CDN cache for 10 minutes too? So couldn't you get stale responses even with max-age=0 ?

Warehouse contains code to purge fastly cache entries, i assume that updates the CDN response.

---

_Closed by @konstin on 2024-02-02 20:00_

---

_Comment by @bewinsnw on 2024-03-23 16:02_

Commenting not to reopen, but to record another argument against that pip feature (partly because I see someone talking about it again)

Artifactory and AWS Codeartifact do not send the etag headers that pypi does for index files, but do send expiry headers, which pip ignores. This means that pip's cache doesn't work at all, and it will refetch 'simple' indexes every time (often half a second to a second for every single dependency), causing users of libraries to pay forever for a feature that's useful for 10 minutes after publishing.

We think the artifactory behaviour regressed, we saw that change last summer after an upgrade, and builds that took 6 minutes jumped to 12 or more - the result of having dozens and dozens of dependencies refetched again and again for each leg of a multiplatform build. We spoke to them and are hopeful to see this fixed in an upcoming release. Codeartifact is not something we use, but I tested that last year to see if it was a viable alternative, and found it had the same problem.

In summary, uv's `--refresh-package` is the better choice.



---
