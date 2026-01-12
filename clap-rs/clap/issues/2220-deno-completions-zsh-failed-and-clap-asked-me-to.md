```yaml
number: 2220
title: deno completions zsh failed and clap asked me to post here
type: issue
state: closed
author: 06kellyjac
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2020-11-23T16:02:52Z
updated_at: 2021-03-12T08:21:42Z
url: https://github.com/clap-rs/clap/issues/2220
synced_at: 2026-01-12T16:14:12Z
```

# deno completions zsh failed and clap asked me to post here

---

_@06kellyjac_

### Make sure you completed the following tasks

- [X] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] Searched the closed issues

### Code

I don't have much familiarity with the changes in the latest release of `deno`
I've created an issue on the `deno` repo too: https://github.com/denoland/deno/issues/8472

### Steps to reproduce the issue

1. clone `git@github.com:06kellyjac/nixpkgs.git`
2. checkout `deno` branch
3. nix-build -A deno
4. see the error
5. run `./result/bin/deno completions zsh` and see the error

If you'd like to run some commands feel free to add to `preBuild` e.g:

```
  preBuild = ''
    _rusty_v8_setup() {
      for v in "$@"; do
        dir="target/$v/gn_out/obj"
        mkdir -p "$dir" && cp "${rustyV8Lib}" "$dir/librusty_v8.a"
      done
    }

    # Copy over the `librusty_v8.a` file inside target/XYZ/gn_out/obj, symlink not allowed
    _rusty_v8_setup "debug" "release" "${arch}/release"

    rustc -v # <- added
  '';
```

or

Fresh debian container
```
$ apt update
$ apt install -y wget unzip
$ wget https://github.com/denoland/deno/releases/download/v1.5.4/deno-x86_64-unknown-linux-gnu.zip
$ unzip deno-x86_64-unknown-linux-gnu.zip
$ ./deno --help
$ ./deno completions bash
$ ./deno completions zsh
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /home/runner/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.3/src/completions/zsh.rs:407:29
```

### Version

* **Rust**: I used `rustc 1.47.0`, deno ci is set to [`rustc 1.48`](https://github.com/denoland/deno/blob/v1.5.4/.github/workflows/ci.yml#L69)
* **Clap**: [2.33.3](https://github.com/denoland/deno/blob/v1.5.4/Cargo.lock#L276)

### Actual Behavior Summary

When I do like *this*, *that* is happening and I think it shouldn't.

**If a project of yours is blocked due to this bug, please, mention it explicitly.**

```
RUST_BACKTRACE=full ./result/bin/deno completions zsh
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/clap-rs/clap/issues', /build/deno-1.5.4-vendor.tar.gz/clap/src/completions/zsh.rs:407:29
stack backtrace:
   0:     0x56290a91e5d0 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h6f05017bae98a16f
   1:     0x56290a590cdd - core::fmt::write::h4e423e63ef6952a5
   2:     0x56290a91a8e4 - std::io::Write::write_fmt::h87a2ee7a0f0be87a
   3:     0x56290a906bb0 - std::panicking::default_hook::{{closure}}::hcb146b886903f1b9
   4:     0x56290a9073b3 - std::panicking::rust_panic_with_hook::h71db2d7d0653eadb
   5:     0x56290a91e978 - std::panicking::begin_panic_handler::{{closure}}::h59f65871c4ccd803
   6:     0x56290a91e944 - std::sys_common::backtrace::__rust_end_short_backtrace::hedd2b2fd0e04e8bb
   7:     0x56290a9036dd - rust_begin_unwind
   8:     0x56290a58e770 - core::panicking::panic_fmt::h810a06353f831f07
   9:     0x56290a59adc2 - core::option::expect_failed::hc27aa597e04252ad
  10:     0x56290a57d142 - core::option::Option<T>::expect::hb10c98441ed0f7c3
  11:     0x56290a57b510 - clap::completions::zsh::get_args_of::h9fa43a798c96bf9f
  12:     0x56290a57c5ca - clap::completions::zsh::get_subcommands_of::h2e84f2d0f0109a99
  13:     0x56290a4df83f - deno::flags::flags_from_vec_safe::h7b3ab19fae9f68ab
  14:     0x56290a53896b - deno::main::he187ab4575e95215
  15:     0x56290a3b3563 - std::sys_common::backtrace::__rust_begin_short_backtrace::h3b2ba86a5b5f293f
  16:     0x56290a542bdc - main
  17:     0x7f6f2ecc4dbd - __libc_start_main
  18:     0x56290a320efa - _start
  19:                0x0 - <unknown>
```

### Expected Behavior Summary

I expected the zsh completions to load

### Additional context

Worked fine in all previous releases

### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

Paste Debug Output Here

</code>
</pre>
</details>


---

_Label `T: bug` added by @06kellyjac on 2020-11-23 16:02_

---

_Referenced in [denoland/deno#8472](../../denoland/deno/issues/8472.md) on 2020-11-23 16:04_

---

_Comment by @pksunkara on 2020-11-23 16:05_

#2191 would fix this, I think.

---

_Comment by @06kellyjac on 2020-11-23 16:07_

ooh that's great, thanks for the swift reply.
Until then would the best bet be rolling back a couple versions of clap?

---

_Comment by @pksunkara on 2020-11-23 16:08_

Not sure. You can try. But we stopped developing v2 long ago.

---

_Comment by @06kellyjac on 2020-11-23 16:10_

Another good point. Thanks :slightly_smiling_face: 

---

_Label `C: generator` added by @pksunkara on 2020-11-26 06:50_

---

_Referenced in [clap-rs/clap#2191](../../clap-rs/clap/pulls/2191.md) on 2020-11-26 07:12_

---

_Comment by @ahkrr on 2020-11-26 18:01_

I wrote the PR #2191 . 
The problem that I was finding was, when a cli argument was declared as global and also as having conflicts with other arguments which weren't present in all the subcommands that the global argument was present in, then the completion generation would error out. 
This would happen because a conflict was declared for which the conflicting argument didn't exist in the command ( or subcommand) that the argument that had the conflict declared on was in. 
I looked at the deno cli and couldn't find the problem that I ran into but commenting out these two lines
``` rust
//deno/cli/flags.rs
/*1398:*/fn watch_arg<'a, 'b>() -> Arg<'a, 'b> {
  Arg::with_name("watch")
    .requires("unstable")
    .long("watch")
    // .conflicts_with("inspect")
    // .conflicts_with("inspect-brk")
    .help("Watch for file changes and restart process automatically")
    .long_help(
      "Watch for file changes and restart process automatically.
Only local files from entry point module graph are watched.",
    )
}
```

prevented the crash from happening. Maybe the conflicts that are declared here don't exist the command ( or subcommand) that watch is part of?

---

_Comment by @pksunkara on 2020-11-26 18:03_

Please note that these bad `conlifcts_with` names will be give better errors starting in v3.

---

_Referenced in [mocyuto/ec2-search#18](../../mocyuto/ec2-search/pulls/18.md) on 2020-12-16 13:04_

---

_Added to milestone `3.0` by @pksunkara on 2021-02-21 12:57_

---

_Comment by @pksunkara on 2021-03-12 08:21_

I am closing this because the fix needs to be in deno and we did everything else from our side.

---

_Closed by @pksunkara on 2021-03-12 08:21_

---

_Referenced in [flamegraph-rs/flamegraph#158](../../flamegraph-rs/flamegraph/issues/158.md) on 2021-10-07 17:44_

---
