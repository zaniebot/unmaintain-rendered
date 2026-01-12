```yaml
number: 3998
title: Use musl in ecosystem docker
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: use_musl_in_ecosystem_docker
created_at: 2023-04-17T16:23:10Z
updated_at: 2023-04-26T03:54:54Z
url: https://github.com/astral-sh/ruff/pull/3998
synced_at: 2026-01-12T04:28:19Z
```

# Use musl in ecosystem docker

---

_Pull request opened by @konstin on 2023-04-17 16:23_

This prevents errors when the host glibc is newer than the one in the docker container

---

_Review requested from @MichaReiser by @konstin on 2023-04-17 16:23_

---

_Review requested from @charliermarsh by @konstin on 2023-04-17 16:23_

---

_@konstin reviewed on 2023-04-17 16:23_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:68 on 2023-04-17 16:23_

Saw the deprecation warning when i looked at the release ci job

---

_@charliermarsh reviewed on 2023-04-17 16:24_

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:24_

I feel like this was necessary for some reason.

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:25_

yes, but it's now moved to pyproject.toml: https://github.com/PyO3/maturin/blob/main/Changelog.md#01416---2023-03-26

---

_@konstin reviewed on 2023-04-17 16:25_

---

_@charliermarsh reviewed on 2023-04-17 16:25_

---

_Review comment by @charliermarsh on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:25_

Mmm, maybe not?

---

_@Skylion007 reviewed on 2023-04-17 16:26_

---

_Review comment by @Skylion007 on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:26_

Can confirm, saw this warning when building locally with pip too.

---

_@Skylion007 reviewed on 2023-04-17 16:28_

---

_Review comment by @Skylion007 on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:28_

@konstin Don't we need to update the `maturin` version in the `[build-system] requires` section of the pyproject for pip building though?

---

_@konstin reviewed on 2023-04-17 16:28_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:28_

wdym? the deprecation warning comes from https://github.com/charliermarsh/ruff/actions/runs/4614388337/jobs/8157296671#step:5:531 and i've built the wheel and it looks correct

---

_@MichaReiser reviewed on 2023-04-17 16:29_

---

_Review comment by @MichaReiser on `scripts/Dockerfile.ecosystem`:12 on 2023-04-17 16:29_

Nice... do we need to disable jemalloc because I run into:

error: failed to run custom build command for `tikv-jemalloc-sys v0.5.3+5.3.0-patched`


---

_@Skylion007 reviewed on 2023-04-17 16:29_

---

_Review comment by @Skylion007 on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:29_

@konstin Referring to this line: https://github.com/charliermarsh/ruff/blob/280dffb5e161acb9deab97c9a2158055e4174d66/pyproject.toml#L2

---

_Review comment by @Skylion007 on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:30_

It needs to be bumped to `0.14.16` at least, right?

---

_@Skylion007 reviewed on 2023-04-17 16:30_

---

_@konstin reviewed on 2023-04-17 16:34_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:34_

hmm i actually always get the correct wheel with just `maturin build` and no name explicitly set now, but that also seems correct since we're not shipping a python package but only a script which already has the correct name set: https://github.com/charliermarsh/ruff/blob/07759652c68a85efa9eebdff600c34a5d72fa588/crates/ruff_cli/Cargo.toml#L15-L16

---

_@konstin reviewed on 2023-04-17 16:34_

---

_Review comment by @konstin on `scripts/Dockerfile.ecosystem`:12 on 2023-04-17 16:34_

huh what os are you building on?

---

_@konstin reviewed on 2023-04-17 16:35_

---

_Review comment by @konstin on `crates/ruff_cli/Cargo.toml`:69 on 2023-04-17 16:35_

i've bumped it to 0.14.17 now, just to avoid testing different combinations of versions

---

_Comment by @github-actions[bot] on 2023-04-17 16:35_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     15.1±0.03ms     2.7 MB/sec    1.00     15.0±0.02ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.8±0.00ms     4.4 MB/sec    1.00      3.8±0.01ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.00    416.3±1.11µs     7.1 MB/sec    1.00    414.8±1.87µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.01ms     3.9 MB/sec    1.00      6.4±0.01ms     4.0 MB/sec
linter/default-rules/large/dataset.py      1.00      7.8±0.01ms     5.2 MB/sec    1.00      7.8±0.01ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1746.4±2.35µs     9.5 MB/sec    1.00   1727.7±2.43µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    180.7±0.41µs    16.3 MB/sec    1.00    180.7±0.51µs    16.3 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.6±0.00ms     7.0 MB/sec    1.00      3.6±0.00ms     7.1 MB/sec
parser/large/dataset.py                    1.00      6.2±0.00ms     6.5 MB/sec    1.00      6.3±0.01ms     6.5 MB/sec
parser/numpy/ctypeslib.py                  1.00   1232.5±1.40µs    13.5 MB/sec    1.01   1240.9±1.03µs    13.4 MB/sec
parser/numpy/globals.py                    1.00    124.1±0.44µs    23.8 MB/sec    1.01    125.3±0.49µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.7±0.00ms     9.4 MB/sec    1.00      2.7±0.00ms     9.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.3±0.22ms     2.5 MB/sec    1.01     16.4±0.23ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.13ms     3.9 MB/sec    1.00      4.3±0.10ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   439.1±12.36µs     6.7 MB/sec    1.01   442.0±10.75µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.09ms     3.7 MB/sec    1.01      7.0±0.16ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.07ms     5.0 MB/sec    1.02      8.3±0.13ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1787.9±25.84µs     9.3 MB/sec    1.01  1802.2±29.30µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    186.6±4.95µs    15.8 MB/sec    1.01    187.9±4.48µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.8 MB/sec    1.01      3.8±0.05ms     6.7 MB/sec
parser/large/dataset.py                    1.01      6.7±0.12ms     6.1 MB/sec    1.00      6.6±0.07ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.00  1270.4±16.20µs    13.1 MB/sec    1.01  1277.1±23.52µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    127.6±1.50µs    23.1 MB/sec    1.00    128.0±2.62µs    23.0 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.05ms     9.0 MB/sec    1.00      2.8±0.05ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @konstin on 2023-04-17 16:46_

I've removed the maturin changes here and opened #3999 separately

---

_@MichaReiser reviewed on 2023-04-18 08:39_

---

_Review comment by @MichaReiser on `scripts/Dockerfile.ecosystem`:12 on 2023-04-18 08:39_

Manjaro (arch) x86.... 

It seems that some files are missing

```
  --- stderr
  running "musl-gcc" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-m64" "-I" "include" "-Wall" "-Wextra" "-pedantic" "-pedantic-errors" "-Wall" "-Wextra" "-Wcast-align" "-Wcast-qual" "-Wconversion" "-Wenum-compare" "-Wfloat-equal" "-Wformat=2" "-Winline" "-Winvalid-pch" "-Wmissing-field-initializers" "-Wmissing-include-dirs" "-Wredundant-decls" "-Wshadow" "-Wsign-compare" "-Wsign-conversion" "-Wundef" "-Wuninitialized" "-Wwrite-strings" "-fno-strict-aliasing" "-fvisibility=hidden" "-fstack-protector" "-g3" "-U_FORTIFY_SOURCE" "-DNDEBUG" "-c" "-o/home/micha/astral/ruff/target/x86_64-unknown-linux-musl/release/build/ring-6202016dbf886143/out/aesni-x86_64-elf.o" "/home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/ring-0.16.20/pregenerated/aesni-x86_64-elf.S"
  thread 'main' panicked at 'failed to execute ["musl-gcc" "-O3" "-ffunction-sections" "-fdata-sections" "-fPIC" "-m64" "-I" "include" "-Wall" "-Wextra" "-pedantic" "-pedantic-errors" "-Wall" "-Wextra" "-Wcast-align" "-Wcast-qual" "-Wconversion" "-Wenum-compare" "-Wfloat-equal" "-Wformat=2" "-Winline" "-Winvalid-pch" "-Wmissing-field-initializers" "-Wmissing-include-dirs" "-Wredundant-decls" "-Wshadow" "-Wsign-compare" "-Wsign-conversion" "-Wundef" "-Wuninitialized" "-Wwrite-strings" "-fno-strict-aliasing" "-fvisibility=hidden" "-fstack-protector" "-g3" "-U_FORTIFY_SOURCE" "-DNDEBUG" "-c" "-o/home/micha/astral/ruff/target/x86_64-unknown-linux-musl/release/build/ring-6202016dbf886143/out/aesni-x86_64-elf.o" "/home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/ring-0.16.20/pregenerated/aesni-x86_64-elf.S"]: No such file or directory (os error 2)', /home/micha/.cargo/registry/src/github.com-1ecc6299db9ec823/ring-0.16.20/build.rs:653:9
  note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

```

Running `cargo clean` nor building with `--release` helped

Edit: https://github.com/briansmith/ring/issues/1414#issuecomment-1055177218

---

_@MichaReiser reviewed on 2023-04-18 08:42_

---

_Review comment by @MichaReiser on `scripts/Dockerfile.ecosystem`:12 on 2023-04-18 08:42_

Solution:

I had to install `musl` (`yay musl`). For debian `apt-get install musl-tools`. 

---

_@MichaReiser approved on 2023-04-18 08:43_

---

_@konstin reviewed on 2023-04-18 08:47_

---

_Review comment by @konstin on `scripts/Dockerfile.ecosystem`:12 on 2023-04-18 08:47_

how does this compare to the experience with zig? i'm trying to figure out which one to recommend 

---

_@MichaReiser reviewed on 2023-04-18 08:58_

---

_Review comment by @MichaReiser on `scripts/Dockerfile.ecosystem`:12 on 2023-04-18 08:58_

It's very similar. With Zig, I had to install `zig` and I had to install `musl` with this approach.

I'm a complete docker noob but is there an image (even if slightly larger in size) that uses a more recent `glibc` version to avoid cross-compiling?

---

_@konstin reviewed on 2023-04-18 09:12_

---

_Review comment by @konstin on `scripts/Dockerfile.ecosystem`:12 on 2023-04-18 09:12_

thanks, added it to the documentation

---

_Merged by @konstin on 2023-04-26 03:54_

---

_Closed by @konstin on 2023-04-26 03:54_

---

_Branch deleted on 2023-04-26 03:54_

---
