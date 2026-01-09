---
number: 9972
title: "Use zopfli for packaging `uv` itself?"
type: issue
state: open
author: akx
labels:
  - releases
assignees: []
created_at: 2024-12-17T13:52:01Z
updated_at: 2024-12-17T14:30:48Z
url: https://github.com/astral-sh/uv/issues/9972
synced_at: 2026-01-07T13:12:18-06:00
---

# Use zopfli for packaging `uv` itself?

---

_Issue opened by @akx on 2024-12-17 13:52_

It might be worth it to look at using `zopfli` when creating the tarballs that are uploaded as actual version releases.

It takes significantly longer to compress, of course:

```
$ time zopfli -v --i30 uv-x86_64-unknown-linux-gnu.tar
...
Executed in  107.17 secs
```

and the savings don't seem _very_ great on their own:

```
14983657 Dec 17 15:30 uv-x86_64-unknown-linux-gnu.tar.orig.gz
14405866 Dec 17 15:37 uv-x86_64-unknown-linux-gnu.tar.zopfli.gz
```

– that is, 3.85 %, or 578 KiB – but in the grand scale of things (I'd guess `uv` is downloaded a couple of times every now and then) it could make a dent in time/energy/bandwidth usage.

The extraction time is unaffected (other than that there's less data to read) – they're both just gzip bitstreams afterwards:

```
$ hyperfine --warmup 10 'tar xf uv-x86_64-unknown-linux-gnu.tar.gz.zopfli.gz' 'tar xf uv-x86_64-unknown-linux-gnu.tar.gz.orig.gz'
Benchmark 1: tar xf uv-x86_64-unknown-linux-gnu.tar.gz.zopfli.gz
  Time (mean ± σ):      66.5 ms ±   2.4 ms    [User: 51.6 ms, System: 13.3 ms]
  Range (min … max):    62.0 ms …  76.1 ms    43 runs

Benchmark 2: tar xf uv-x86_64-unknown-linux-gnu.tar.gz.orig.gz
  Time (mean ± σ):      68.1 ms ±   2.9 ms    [User: 52.7 ms, System: 13.7 ms]
  Range (min … max):    63.9 ms …  81.4 ms    42 runs

Summary
  tar xf uv-x86_64-unknown-linux-gnu.tar.gz.zopfli.gz ran
    1.02 ± 0.06 times faster than tar xf uv-x86_64-unknown-linux-gnu.tar.gz.orig.gz
```

---

_Label `releases` added by @zanieb on 2024-12-17 14:30_

---
