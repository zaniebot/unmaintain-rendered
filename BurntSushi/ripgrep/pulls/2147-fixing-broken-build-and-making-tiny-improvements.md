```yaml
number: 2147
title: Fixing broken build and making tiny improvements
type: pull_request
state: closed
author: ink-splatters
labels: []
assignees: []
base: master
head: feature/improvements
created_at: 2022-02-19T02:34:53Z
updated_at: 2023-09-22T00:08:47Z
url: https://github.com/BurntSushi/ripgrep/pull/2147
synced_at: 2026-01-12T18:23:14Z
```

# Fixing broken build and making tiny improvements

---

_@ink-splatters_

Hi there,

being long-term `ripgrep` admirer and everyday user, I've come up with those ultra-tiny improvements, besides fixing blocker build issue caused by `packed_simd_2` which had used now removed feature.

Update, however, has been done using `cargo update` hence rip previous `Cargo.lock`

Another thing is enabling lto: I've not profiled your tool yet, but basing on my recent experience with custom build of Python (with which I observed perf improvements, at least, in some synthetic benchmarks)[1], I consider this potentially to be
beneficial.

New profile has been created as lto badly slows down the linking, which may not be desirable by default.


Finally - rip warnings :) ( related to code considered dead by rust )

---
[1] FYI Python build details:
```shell
    PYTHON_CONFIGURE_OPTS="--enable-framework" CONFIGURE_OPTS="--enable-optimizations --with-lto" \
    CPPFLAGS="-I$(brew --prefix openssl)/include -I$(brew --prefix libffi)/include" \
    LDFLAGS="-L$(brew --prefix openssl)/lib -L$(brew --prefix libffi)/lib" \
pyenv install 3.9.2
```

---

_Comment by @BurntSushi on 2023-07-08 14:04_

Thanks, but I think I'm going to pass on this for now.

I also generally prefer to make all `Cargo.lock` changes myself manually. And putting such changes in PRs like this makes them a nightmare to merge in my workflow. (Which is that I have bursts of activity where PRs tend to sit for a while and get stale.)

---

_Closed by @BurntSushi on 2023-07-08 14:04_

---

_Branch deleted on 2023-09-22 00:08_

---
