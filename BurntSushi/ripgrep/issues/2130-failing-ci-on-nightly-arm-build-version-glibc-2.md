```yaml
number: 2130
title: "Failing CI on nightly-arm build, `version 'GLIBC_2.25' not found`"
type: issue
state: closed
author: arcsi42
labels: []
assignees: []
created_at: 2022-01-19T21:09:47Z
updated_at: 2022-03-21T12:59:05Z
url: https://github.com/BurntSushi/ripgrep/issues/2130
synced_at: 2026-01-12T16:13:24Z
```

# Failing CI on nightly-arm build, `version 'GLIBC_2.25' not found`

---

_@arcsi42_

#### What are the steps to reproduce the behavior?

Run [ci.yml](https://github.com/BurntSushi/ripgrep/actions/workflows/ci.yml) workflow.

#### What is the actual behavior?

https://github.com/BurntSushi/ripgrep/runs/4343255639?check_suite_focus=true#step:12:139
```console
/target/arm-unknown-linux-gnueabihf/debug/deps/globset-da336622f9a19f46: /usr/arm-linux-gnueabihf/lib/libc.so.6: version `GLIBC_2.25' not found (required by /target/arm-unknown-linux-gnueabihf/debug/deps/globset-da336622f9a19f46)
error: test failed, to rerun pass '-p globset --lib'
Error: Process completed with exit code 1.
```

#### What is the expected behavior?

Successful CI run.

#### Describe your bug.

Since 2021-Nov-28 a glibc issue consistently fails the ci.yml workflow, https://github.com/BurntSushi/ripgrep/runs/4343255639?check_suite_focus=true#step:12:139, with the following error:
```console
/target/arm-unknown-linux-gnueabihf/debug/deps/globset-da336622f9a19f46: /usr/arm-linux-gnueabihf/lib/libc.so.6: version `GLIBC_2.25' not found (required by /target/arm-unknown-linux-gnueabihf/debug/deps/globset-da336622f9a19f46)
```
The previous successful CI used the same `0b36942` commit. I am not experienced enough to know the cause, maybe something on the dependency tree of the `globset` crate changed. 

But I did check the `glibc` version in the docker image from [`Cross.toml#L11`](https://github.com/BurntSushi/ripgrep/blob/master/Cross.toml#L11), which should be [`burntsushi/cross:arm-unknown-linux-gnueabihf@sha256:e31d41b0555ad4a17197eafdd8945de3574d4789315c7f752fc519fca457c95b`](https://hub.docker.com/layers/burntsushi/cross/arm-unknown-linux-gnueabihf/images/sha256-e31d41b0555ad4a17197eafdd8945de3574d4789315c7f752fc519fca457c95b?context=explore). I think this image should get pulled by the CI runner and used by `cross`.
```console
$ docker run --rm burntsushi/cross:arm-unknown-linux-gnueabihf@sha256:e31d41b0555ad4a17197eafdd8945de3574d4789315c7f752fc519fca457c95b ldd --version
ldd (Ubuntu GLIBC 2.23-0ubuntu11) 2.23
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
Written by Roland McGrath and Ulrich Drepper.
```

#### Possible fix

 - Disable `nightly-arm` from the build matrix for now, so the `ci.yml` workflow doesn't block pull request checks.
 - The nightly-arm tests could also be removed, but this seems wrong?
 - Build a new docker image burntsushi/cross:arm-unknown-linux-gnueabihf and push it to dockerhub



---

_Comment by @arcsi42 on 2022-01-19 21:34_

On dockerhub, `rustembedded/cross:arm-unknown-linux-gnueabihf` has an updated image, which is used as a base for `burntsushi/cross:arm-unknown-linux-gnueabihf`. Therefore we can test a new image, just by rebuilding the image the same way as in https://github.com/BurntSushi/ripgrep/tree/master/ci/docker.

Using this new image, from [arcsi42/cross:arm-unknown-linux-gnueabihf](https://hub.docker.com/layers/arcsi42/cross/arm-unknown-linux-gnueabihf/images/sha256-b53e068270cd833c2d6ec4fdb987fccce5e35472f0680a1194c5bc1df7f2b9c1?context=explore) in my fork, the tests on `nightly-arm` doesn't have an error related to `glibc` version, but there are still 2 failing tests, https://github.com/arcsi42/ripgrep/runs/4874128956?check_suite_focus=true#step:12:1186.
```console
failures:

---- misc::compressed_brotli stdout ----
thread 'main' panicked at '

==========
command failed but expected success!

command: "qemu-arm" "/target/arm-unknown-linux-gnueabihf/debug/deps/../rg" "--path-separator" "/" "-z" "Sherlock" "sherlock.br"

cwd: /tmp/ripgrep-tests/compressed_brotli/109

dir list: ["/tmp/ripgrep-tests/compressed_brotli/109", "/tmp/ripgrep-tests/compressed_brotli/109/sherlock.br"]

status: exit status: 2

stdout: 

stderr: sherlock.br: <stderr is empty>


==========
', tests/util.rs:406:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- misc::compressed_zstd stdout ----
thread 'main' panicked at '

==========
command failed but expected success!

command: "qemu-arm" "/target/arm-unknown-linux-gnueabihf/debug/deps/../rg" "--path-separator" "/" "-z" "Sherlock" "sherlock.zst"

cwd: /tmp/ripgrep-tests/compressed_zstd/117

dir list: ["/tmp/ripgrep-tests/compressed_zstd/117", "/tmp/ripgrep-tests/compressed_zstd/117/sherlock.zst"]

status: exit status: 2

stdout: 

stderr: sherlock.zst: <stderr is empty>


==========
', tests/util.rs:406:13


failures:
    misc::compressed_brotli
    misc::compressed_zstd

test result: FAILED. 271 passed; 2 failed; 0 ignored; 0 measured; 0 filtered out; finished in 190.07s

error: test failed, to rerun pass '-p ripgrep --test integration'
Error: Process completed with exit code 101.
```
I am not quite sure why the `note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace` is there. The `ci` should that environment variable set as far as I know, https://github.com/arcsi42/ripgrep/blob/master/.github/workflows/ci.yml#L21. Maybe it is not propagated to `cross` or `qemu`?

---

_Comment by @arcsi42 on 2022-01-19 22:29_

> but there are still 2 failing tests, https://github.com/arcsi42/ripgrep/runs/4874128956?check_suite_focus=true#step:12:1186.

This can be fixed by installing `brotli` and `zstd` in [`ubuntu-install-packages`](https://github.com/BurntSushi/ripgrep/blob/master/ci/ubuntu-install-packages). 

---

_Closed by @BurntSushi on 2022-03-21 12:59_

---
