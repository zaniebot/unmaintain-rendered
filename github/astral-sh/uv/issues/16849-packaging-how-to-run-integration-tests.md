---
number: 16849
title: "packaging: How to run integration tests?"
type: issue
state: open
author: mtelka
labels:
  - question
assignees: []
created_at: 2025-11-25T19:16:33Z
updated_at: 2025-12-02T11:19:09Z
url: https://github.com/astral-sh/uv/issues/16849
synced_at: 2026-01-07T13:12:19-06:00
---

# packaging: How to run integration tests?

---

_Issue opened by @mtelka on 2025-11-25 19:16_

I'm packaging `uv-build` for OpenIndiana and I'd like to run integration tests to make sure the package is working properly.  How can I do that?

---

_Comment by @konstin on 2025-11-26 16:17_

uv-build has some tests of its own (`cargo test -p uv-build-backend`), which cover a large fraction of its functional correctness. Other tests are part of the general uv test (to use the shared infrastructure such as for venvs and running Python code) and live in `crates/uv/tests/it/build_backend.rs`. These tests cover the Rust part of the build backend. To also test the Python shims, and as a smoke test, you can build the `uv_build` Python package and build `scripts/packages/built-by-uv` with it.

---

_Label `question` added by @konstin on 2025-11-26 16:17_

---

_Comment by @mtelka on 2025-11-29 06:28_

Thanks @konstin for suggestions.

I tried to run `cargo test -p uv-build-backend` (from sdist) and it failed:
```
failures:

---- tests::built_by_uv_building stdout ----

thread 'tests::built_by_uv_building' panicked at crates/uv-build-backend/src/lib.rs:548:71:
called `Result::unwrap()` on an `Err` value: Custom { kind: NotFound, error: Error { kind: ReadDir, source: Os { code: 2, kind: NotFound, message: "No such file or directory" }, path: "../../scripts/packages/built-by-uv/src" } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- wheel::test::test_prepare_metadata stdout ----

thread 'wheel::test::test_prepare_metadata' panicked at crates/uv-build-backend/src/wheel.rs:834:66:
called `Result::unwrap()` on an `Err` value: Io(Custom { kind: NotFound, error: Error { kind: OpenFile, source: Os { code: 2, kind: NotFound, message: "No such file or directory" }, path: "../../scripts/packages/built-by-uv/pyproject.toml" } })


failures:
    tests::built_by_uv_building
    wheel::test::test_prepare_metadata

test result: FAILED. 32 passed; 2 failed; 0 ignored; 0 measured; 0 filtered out; finished in 0.22s
```

Could you please elaborate a bit how exactly should be tests from `crates/uv/tests/it/build_backend.rs` executed?
Additionally, the `crates/uv/tests/it/build_backend.rs` file is not included in the sdist so it is not possible to use it directly anyway.

Re `scripts/packages/built-by-uv`: It looks like this is done automatically by `cargo test -p uv-build-backend` and so it should not be needed to run this separately (see above).  Or, do I miss something?  The only problem here seems to be that `scripts/packages/built-by-uv` is missing from sdist.

Thank you!

---

_Comment by @konstin on 2025-11-30 17:35_

The sdist is intended for platforms where there's no binary and users need to compile from source, it's rather PyPI-specific. For redistribution, I recommend a git checkout, which will contain all the test data.

---

_Comment by @mtelka on 2025-11-30 21:18_

> The sdist is intended for platforms where there's no binary and users need to compile from source, it's rather PyPI-specific. For redistribution, I recommend a git checkout, which will contain all the test data.

I tried to use https://github.com/astral-sh/uv/releases/download/0.9.13/source.tar.gz (instead of sdist from PyPI), but that does not work as expected because with that the build backend (maturin) tries to build far more crates than the sdist build.  And, unfortunately, the build of the `tikv-jemalloc-sys` crate fails because it does not support illumos.

---

_Comment by @konstin on 2025-12-01 08:22_

> I tried to use [`0.9.13` source.tar.gz (download)](https://github.com/astral-sh/uv/releases/download/0.9.13/source.tar.gz) (instead of sdist from PyPI), but that does not work as expected because with that the build backend (maturin) tries to build far more crates than the sdist build.

What commands did you use for the build?

> And, unfortunately, the build of the `tikv-jemalloc-sys` crate fails because it does not support illumos.

We can exclude tikv from illumos in https://github.com/astral-sh/uv/blob/735b87004ce0df4b4d45958dfc103bd916e67dce/crates/uv-performance-memory-allocator/Cargo.toml#L20 and https://github.com/astral-sh/uv/blob/884b5a3c801f5aad7d5e74d59aa4d84aa7c0b4d6/crates/uv-performance-memory-allocator/src/lib.rs#L8-L17


---

_Comment by @mtelka on 2025-12-02 08:04_

> > I tried to use [`0.9.13` source.tar.gz (download)](https://github.com/astral-sh/uv/releases/download/0.9.13/source.tar.gz) (instead of sdist from PyPI), but that does not work as expected because with that the build backend (maturin) tries to build far more crates than the sdist build.
> 
> What commands did you use for the build?

The exact command used to build is this:

```
/usr/bin/env -i CARGO_HOME="/data/builds/ul-workspace/components/python/uv-build/build/amd64-3.9/.cargo" \
LD_OPTIONS="-M /usr/lib/ld/map.noexstk -M /usr/lib/ld/map.noexdata -M /usr/lib/ld/map.pagealign -Bdirect " \
LD_EXEC_OPTIONS="-z aslr=enable" \
PATH="/usr/gcc/14/bin:/usr/clang/21/bin:/usr/ruby/3.2/bin:/usr/jdk/openjdk21/bin:/usr/postgres/16/bin:/usr/mariadb/10.6/bin:/usr/openssl/3/bin:/usr/bin/amd64:/usr/bin:/usr/gnu/bin:/usr/sbin/amd64:/usr/sbin" \
CC="/usr/gcc/14/bin/gcc" CFLAGS="-m64 -O3 " CXX="/usr/gcc/14/bin/g++" CXXFLAGS="-m64 -O3 " LDFLAGS="-m64" \
PKG_CONFIG_PATH="/usr/mariadb/10.6/lib/amd64/pkgconfig:/usr/openssl/3/lib/amd64/pkgconfig:/usr/lib/amd64/pkgconfig:/usr/lib/pkgconfig" \
CARGO_TARGET_X86_64_UNKNOWN_ILLUMOS_LINKER=/usr/gcc/14/bin/gcc /usr/bin/python3.9 \
-m build --wheel --no-isolation
```

---

_Comment by @konstin on 2025-12-02 11:16_

`-m build --wheel --no-isolation` sounds correct, if you run that in the `uv-build` it builds just the build backend, it's what we're doing in the uv release pipeline. Can you provide a minimal reproducible example for how the build goes wrong?

---
