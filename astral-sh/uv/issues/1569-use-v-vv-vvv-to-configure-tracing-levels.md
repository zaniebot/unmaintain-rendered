```yaml
number: 1569
title: "Use `-v`, `-vv`, `-vvv` to configure tracing levels"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - help wanted
  - tracing
assignees: []
created_at: 2024-02-17T04:07:20Z
updated_at: 2025-02-28T23:49:28Z
url: https://github.com/astral-sh/uv/issues/1569
synced_at: 2026-01-12T15:58:29Z
```

# Use `-v`, `-vv`, `-vvv` to configure tracing levels

---

_@zanieb_

Right now, as mentioned in #1567, we show a _lot_ of output in `-v`.

We should have `-v` be more user friendly and use `-vv` and `-vvv` to dump more verbose tracing information.

---

_Label `enhancement` added by @zanieb on 2024-02-17 04:07_

---

_Label `tracing` added by @zanieb on 2024-02-17 04:07_

---

_Comment by @olivierlefloch on 2024-02-18 23:46_

I may be missing something, but related to this, it would be great if the `-vvv` level also displayed calls to `trace`, for example to debug

https://github.com/astral-sh/uv/issues/1458

I'd like to be able to see the `method` that `fresh_request` invoked. I'm not familiar enough with `ruff`'s tracing/logging, but perhaps

https://github.com/astral-sh/uv/blob/3b70b42f160eee8a0dbb74436cccb0f6e8ea8979/crates/uv-client/src/cached_client.rs#L389

could be logged?

---

_Comment by @zanieb on 2024-02-18 23:53_

You can control the log level during verbose mode with e.g. `RUST_LOG=trace` for now.

I totally agree though.

---

_Comment by @olivierlefloch on 2024-02-19 00:18_

Thanks for the tip! Actually, that failed when running `uv` with `RUST_LOG=trace python -m uv …` but worked with `RUST_LOG=trace uv …` 👍 

I'd suggest adding it to `CONTRIBUTING.md` as that's where I imagined I might be able to find this information: https://github.com/astral-sh/uv/pull/1670

---

_Comment by @zanieb on 2024-02-19 00:22_

The `python -m ...` issue should be fixed by #1667 

---

_Label `help wanted` added by @zanieb on 2024-02-19 18:32_

---

_Comment by @Gankra on 2025-02-24 17:20_

Ok so, for context, the tracing log filter levels are:

    none < error < warn < info < debug < trace

Currently:

* `<no flag> => none`
* `-v => uv=debug` (enable debug logs, but only from uv crates, e.g. *not* reqwest)
* `-vv` is just more formatting/details on `uv=debug`?
* `RUST_LOG="..."` is available to override these more fine-grain

In our code, we use `warn`, `debug`, and `trace`.
We basically don't use `error` and `info` at all.

Not using `error` is basically fine, but we want to expand our use of `info` (migrating some existing debugs/traces to higher log levels).

Naively/greenfield I would want to do `-v => error`, `-vv => warn`, etc. But I don't think that's worthwhile.

I think `-v => info`, `-vv = debug`, `-vvv => trace` would be reasonable.

I think starting at `-v => warn` might be reasonable though?

Unclear if we still want the filter to only be "uv" (I guess sure?).

Unclear if we want this "extra formatting" mode to still be a thing (I'm ambivalent).


---

_Comment by @zanieb on 2025-02-24 17:56_

I think `-v => info` etc. makes sense in the long run, but I think we need to start with:

`-v => debug (uv-only)`, `-vv => trace (uv-only)`, `-vvv => trace (all crates)`

We have a lot of work to do before there's a proper `info` layer and I'm not sure we should block this work on accomplishing that. `-v` needs to continue to get us logs that are usable for resolving issue reports.

Once we elevate more logs to info, we can shift it all over one?

Regarding the nested formatting, I'd move that to an environment variable. It feels weird in `-v` (to me).

---

_Comment by @Gankra on 2025-02-24 18:19_

Sounds good to me!

---

_Closed by @Gankra on 2025-02-28 23:49_

---
