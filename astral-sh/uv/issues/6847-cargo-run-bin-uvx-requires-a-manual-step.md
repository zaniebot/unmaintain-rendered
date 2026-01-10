---
number: 6847
title: cargo run --bin uvx requires a manual step
type: issue
state: closed
author: dimaqq
labels: []
assignees: []
created_at: 2024-08-30T01:10:52Z
updated_at: 2024-08-30T12:48:23Z
url: https://github.com/astral-sh/uv/issues/6847
synced_at: 2026-01-10T01:24:06Z
---

# cargo run --bin uvx requires a manual step

---

_Issue opened by @dimaqq on 2024-08-30 01:10_

To reproduce:

- cargo clean
- cargo run --bin uvx

fails with
```command
> cargo run --bin uvx
   Compiling proc-macro2 v1.0.86
   Compiling unicode-ident v1.0.12
...[snip]...
   Compiling tikv-jemallocator v0.6.0
   Compiling uv v0.4.0 (/code/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 51.71s
     Running `target/debug/uvx`
error: No such file or directory (os error 2)
```

My understanding is that "uv" (crate? lib?) is compiled but uv binary is not linked:

```command
> ls -lt target/debug/
total 1112
-rw-r--r--@    1 dima  staff   15467 30 Aug 10:08 uvx.d
drwxr-xr-x@ 6082 dima  staff  194624 30 Aug 10:08 deps/
-rwxr-xr-x@    1 dima  staff  551992 30 Aug 10:08 uvx*
drwxr-xr-x@   44 dima  staff    1408 30 Aug 10:08 incremental/
drwxr-xr-x@   82 dima  staff    2624 30 Aug 10:07 build/
drwxr-xr-x@    2 dima  staff      64 30 Aug 10:07 examples/
```

Meanwhile, uvx parses arguments and exec's (I think) uv, which is not there.


To work around:

- [opt] cargo clean
- cargo run -- --help  # builds uv
- cargo run --bin uvx -- --help  # ok!

---

_Referenced in [astral-sh/uv#6848](../../astral-sh/uv/pulls/6848.md) on 2024-08-30 01:17_

---

_Comment by @charliermarsh on 2024-08-30 12:45_

You should just run `cargo run -- tool run` -- `uvx` isn't intended to be run directly during development :)

---

_Closed by @charliermarsh on 2024-08-30 12:45_

---

_Comment by @charliermarsh on 2024-08-30 12:45_

`uvx` depends on a `uv` binary existing alongside it.

---

_Comment by @zanieb on 2024-08-30 12:48_

For what it's worth, I do use `cargo run --bin uvx` sometimes to test things _specific_ to the uvx interface — but yeah you need a compiled `uv` binary.

---
