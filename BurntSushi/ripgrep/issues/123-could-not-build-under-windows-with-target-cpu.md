```yaml
number: 123
title: Could not build under Windows with target-cpu=native
type: issue
state: closed
author: pravic
labels: []
assignees: []
created_at: 2016-09-28T07:07:21Z
updated_at: 2016-09-28T18:21:02Z
url: https://github.com/BurntSushi/ripgrep/issues/123
synced_at: 2026-01-12T18:23:11Z
```

# Could not build under Windows with target-cpu=native

---

_@pravic_

```
$ rustup show
Default host: i686-pc-windows-msvc
nightly-i686-pc-windows-msvc (default)
rustc 1.13.0-nightly (d0623cf7b 2016-09-26)

$ git clone https://github.com/BurntSushi/ripgrep
$ cd ripgrep\
$ set RUSTFLAGS="-C target-cpu=native"
$ cargo build --release --features simd-accel
 Downloading fnv v1.0.5
 Downloading memmap v0.2.3
 Downloading lazy_static v0.2.1
 Downloading term v0.4.4
 Downloading num_cpus v1.1.0
 Downloading deque v0.3.1
 Downloading docopt v0.6.85
 Downloading walkdir v0.1.8
 Downloading fs2 v0.2.5
 Downloading simd v0.1.1
 Downloading strsim v0.5.1
error: failed to run `rustc` to learn about target-specific information

To learn more, run the command again with --verbose.

$ cargo build --release --features simd-accel --verbose
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name _ --print=file-names "-C target-cpu=native" --crate-type bin --crate-type rlib --target i686-pc-windows-msvc` (exit code: 101)
--- stderr
error: multiple input filenames provided
```

Any ideas?


---

_Comment by @BurntSushi on 2016-09-28 09:29_

It may be the case that enabling simd on Windows doesn't work. Can you try
`cargo build --release --verbose`?

On Sep 28, 2016 3:07 AM, "pravic" notifications@github.com wrote:

> $ rustup show
> Default host: i686-pc-windows-msvc
> nightly-i686-pc-windows-msvc (default)
> rustc 1.13.0-nightly (d0623cf7b 2016-09-26)
> 
> $ git clone https://github.com/BurntSushi/ripgrep
> $ cd ripgrep\
> $ set RUSTFLAGS="-C target-cpu=native"
> $ cargo build --release --features simd-accel
>  Downloading fnv v1.0.5
>  Downloading memmap v0.2.3
>  Downloading lazy_static v0.2.1
>  Downloading term v0.4.4
>  Downloading num_cpus v1.1.0
>  Downloading deque v0.3.1
>  Downloading docopt v0.6.85
>  Downloading walkdir v0.1.8
>  Downloading fs2 v0.2.5
>  Downloading simd v0.1.1
>  Downloading strsim v0.5.1
> error: failed to run `rustc` to learn about target-specific information
> 
> To learn more, run the command again with --verbose.
> 
> $ cargo build --release --features simd-accel --verbose
> error: failed to run `rustc` to learn about target-specific information
> 
> Caused by:
>   process didn't exit successfully: `rustc - --crate-name _ --print=file-names "-C target-cpu=native" --crate-type bin --crate-type rlib --target i686-pc-windows-msvc` (exit code: 101)
> --- stderr
> error: multiple input filenames provided
> 
> Any ideas?
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/123, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34kuV98Jy6HX0-GWOasu9r17s9Erkks5quhIqgaJpZM4KIe8R
> .


---

_Comment by @pravic on 2016-09-28 09:36_

However, without RUSTFLAGS it compiles well with simd-accel.


---

_Comment by @BurntSushi on 2016-09-28 11:02_

@pravic Really? That's interesting. What about with `RUSTFLAGS="-C target-feature=+ssse3"`?


---

_Comment by @pravic on 2016-09-28 11:12_

```
$ set RUSTFLAGS="-C target-feature=+ssse3"

$ cargo build --release --features simd-accel --verbose
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name _ --print=file-names "-C target-feature=+ssse3" --crate-type bin --crate-type rlib --target x86_64-pc-windows-msvc` (exit code: 101)
--- stderr
error: multiple input filenames provided



$ cargo build --release  --verbose
error: failed to run `rustc` to learn about target-specific information

Caused by:
  process didn't exit successfully: `rustc - --crate-name _ --print=file-names "-C target-feature=+ssse3" --crate-type bin --crate-type rlib --target x86_64-pc-windows-msvc` (exit code: 101)
--- stderr
error: multiple input filenames provided
```


---

_Comment by @pravic on 2016-09-28 11:15_

And without rustflags:

> $ cargo build --release --features simd-accel --verbose
>    Compiling regex-syntax v0.3.5
>    Compiling simd v0.1.1
>    Compiling libc v0.2.16
>    Compiling fnv v1.0.5
>    Compiling strsim v0.5.1
>    Compiling utf8-ranges v0.1.3
>    Compiling winapi-build v0.1.1
>    Compiling lazy_static v0.2.1
>      Running `rustc d:\usr\local\rust\cargo\registry\src\github.com-1ecc6299db9ec823\regex-syntax-0.3.5\src\lib.rs --color never --crate-name regex_syntax --crate-type lib -C opt-level=3 -g -C metadata=b3fc207b97c83ddf -C extra-filename=-b3fc207b97c83ddf --out-dir t:\rg\target\release\deps --emit=dep-info,link -L dependency=t:\rg\target\release\deps --cap-lints allow`
> ...
>    Compiling ripgrep v0.2.1 (file:///T:/rg)
>      Running `rustc src/main.rs --color never --crate-name rg --crate-type bin -C opt-level=3 -g --cfg "feature=\"simd-accel\"" --cfg "feature=\"regex\"" -C metadata=5ce4a56e140a4234 --out-dir t:\rg\target\release --emit=dep-info,link -L dependency=t:\rg\target\release\deps --extern num_cpus=t:\rg\target\release\deps\libnum_cpus-b4751aee9eea536c.rlib --extern docopt=t:\rg\target\release\deps\libdocopt-50d41646f88f269a.rlib --extern memchr=t:\rg\target\release\deps\libmemchr-c555f740a543880f.rlib --extern rustc_serialize=t:\rg\target\release\deps\librustc_serialize-3561541d79c18212.rlib --extern deque=t:\rg\target\release\deps\libdeque-82214e5f75d78bdf.rlib --extern memmap=t:\rg\target\release\deps\libmemmap-80b0ddf14e635969.rlib --extern env_logger=t:\rg\target\release\deps\libenv_logger-c716af707f2027e1.rlib --extern log=t:\rg\target\release\deps\liblog-bf16bb9a4912b11d.rlib --extern grep=t:\rg\target\release\deps\libgrep.rlib --extern fnv=t:\rg\target\release\deps\libfnv-1364d96376ddfb32.rlib --extern kernel32=t:\rg\target\release\deps\libkernel32-df86a08647459244.rlib --extern term=t:\rg\target\release\deps\libterm-44dfa57762d62876.rlib --extern regex=t:\rg\target\release\deps\libregex-a99351f81f55a22d.rlib --extern winapi=t:\rg\target\release\deps\libwinapi-0889532d327ff4e2.rlib --extern walkdir=t:\rg\target\release\deps\libwalkdir-461415b31922a061.rlib --extern libc=t:\rg\target\release\deps\liblibc-1417726cb94dbc83.rlib --extern lazy_static=t:\rg\target\release\deps\liblazy_static-359f5533c970cd71.rlib`
>     Finished release [optimized + debuginfo] target(s) in 110.77 secs


---

_Comment by @BurntSushi on 2016-09-28 11:20_

Interesting. @alexcrichton @retep998 Can you make sense of this?


---

_Comment by @retep998 on 2016-09-28 17:35_

Use the `[build] rustflags = ["args", "to", "pass"]` thing in your `.cargo/config`. I know I experienced some issues with `RUSTFLAGS` causing issues for me too while the config version worked fine.


---

_Comment by @alexcrichton on 2016-09-28 18:05_

@pravic do you have `.cargo/config` configuration, or is it just `RUSTFLAGS`?


---

_Comment by @pravic on 2016-09-28 18:10_

Just `RUSTFLAGS` env var.


---

_Comment by @alexcrichton on 2016-09-28 18:11_

Oh depending on your shell you may need to do `set RUSTFLAGS=-C target-cpu=native`, the surrounding quotes might be leaking into the literal environment variable value depending on your shell


---

_Comment by @pravic on 2016-09-28 18:21_

@alexcrichton thanks!

`set RUSTFLAGS=-C target-cpu=native` (without quotes) works fine inside CMD shell.


---

_Closed by @pravic on 2016-09-28 18:21_

---
