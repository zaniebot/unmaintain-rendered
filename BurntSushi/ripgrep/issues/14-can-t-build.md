```yaml
number: 14
title: "Can't build"
type: issue
state: closed
author: ChrisJefferson
labels: []
assignees: []
created_at: 2016-09-23T15:21:52Z
updated_at: 2016-09-23T16:41:33Z
url: https://github.com/BurntSushi/ripgrep/issues/14
synced_at: 2026-01-12T18:23:11Z
```

# Can't build

---

_@ChrisJefferson_

(This might be my stupidity at rust).

I tried downloading from github on Mac OS X, and then running `cargo install`:

```
~/p/r/ripgrep ❯❯❯ cargo install --verbose                                            ⏎
       Fresh rustc-serialize v0.3.19
       Fresh regex-syntax v0.3.5
       Fresh winapi-build v0.1.1
       Fresh lazy_static v0.2.1
       Fresh log v0.3.6
   Compiling strsim v0.5.1
     Running `rustc /Users/caj/.cargo/registry/src/github.com-1ecc6299db9ec823/strsim-0.5.1/src/lib.rs --crate-name strsim --crate-type lib -C opt-level=3 -C panic=abort -g -C metadata=b42a694875d9a3b0 -C extra-filename=-b42a694875d9a3b0 --out-dir /Users/caj/progs/rust/ripgrep/target/release/deps --emit=dep-info,link -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps --cap-lints allow`
   Compiling kernel32-sys v0.2.2
     Running `rustc /Users/caj/.cargo/registry/src/github.com-1ecc6299db9ec823/kernel32-sys-0.2.2/build.rs --crate-name build_script_build --crate-type bin -g --out-dir /Users/caj/progs/rust/ripgrep/target/release/build/kernel32-sys-d6afa5bd3d7cfaef --emit=dep-info,link -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps --extern build=/Users/caj/progs/rust/ripgrep/target/release/deps/libbuild-493a7b0628804707.rlib --cap-lints allow`
   Compiling utf8-ranges v0.1.3
     Running `rustc /Users/caj/.cargo/registry/src/github.com-1ecc6299db9ec823/utf8-ranges-0.1.3/src/lib.rs --crate-name utf8_ranges --crate-type lib -C opt-level=3 -C panic=abort -g -C metadata=5c6a6dacba3be7ce -C extra-filename=-5c6a6dacba3be7ce --out-dir /Users/caj/progs/rust/ripgrep/target/release/deps --emit=dep-info,link -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps --cap-lints allow`
       Fresh winapi v0.2.8
       Fresh fnv v1.0.5
   Compiling libc v0.2.16
     Running `rustc /Users/caj/.cargo/registry/src/github.com-1ecc6299db9ec823/libc-0.2.16/src/lib.rs --crate-name libc --crate-type lib -C opt-level=3 -C panic=abort -g --cfg feature=\"default\" --cfg feature=\"use_std\" -C metadata=1417726cb94dbc83 -C extra-filename=-1417726cb94dbc83 --out-dir /Users/caj/progs/rust/ripgrep/target/release/deps --emit=dep-info,link -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps --cap-lints allow`
error: the crate `build` is compiled with the panic strategy `abort` which is incompatible with this crate's strategy of `unwind`
error: aborting due to previous error
Build failed, waiting for other jobs to finish...
error: failed to compile `ripgrep v0.1.16 (file:///Users/caj/progs/rust/ripgrep)`, intermediate artifacts can be found at `/Users/caj/progs/rust/ripgrep/target`

Caused by:
  Could not compile `kernel32-sys`.

Caused by:
  Process didn't exit successfully: `rustc /Users/caj/.cargo/registry/src/github.com-1ecc6299db9ec823/kernel32-sys-0.2.2/build.rs --crate-name build_script_build --crate-type bin -g --out-dir /Users/caj/progs/rust/ripgrep/target/release/build/kernel32-sys-d6afa5bd3d7cfaef --emit=dep-info,link -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps -L dependency=/Users/caj/progs/rust/ripgrep/target/release/deps --extern build=/Users/caj/progs/rust/ripgrep/target/release/deps/libbuild-493a7b0628804707.rlib --cap-lints allow` (exit code: 101)
```


---

_Closed by @BurntSushi on 2016-09-23 15:27_

---

_Comment by @BurntSushi on 2016-09-23 15:27_

I think I may have fixed it in the latest release. Could you try again? Thanks!


---

_Comment by @ChrisJefferson on 2016-09-23 16:41_

Fixed, thanks!


---
