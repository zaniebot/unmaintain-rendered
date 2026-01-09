---
number: 2042
title: "Can't compile with 3.0.0-beta.1: \"error: linking with `cc` failed: exit code: 1 [...]\""
type: issue
state: closed
author: bbergeron0
labels:
  - C-bug
assignees: []
created_at: 2020-07-31T16:54:41Z
updated_at: 2020-07-31T20:04:06Z
url: https://github.com/clap-rs/clap/issues/2042
synced_at: 2026-01-07T13:12:19-06:00
---

# Can't compile with 3.0.0-beta.1: "error: linking with `cc` failed: exit code: 1 [...]"

---

_Issue opened by @bbergeron0 on 2020-07-31 16:54_

When trying to build a project using "3.0.0-beta.1", I get the following error:

```
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.0.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.1.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.2.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.3.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.4.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.build_script_build.4g6dr5ko-cgu.5.rcgu.o" "-o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8.30tmyzw1ov0vl2ft.rcgu.o" "-Wl,--gc-sections" "-pie" "-Wl,-zrelro" "-Wl,-znow" "-nodefaultlibs" "-L" "/home/dev-user/Projects/application/target/debug/deps" "-L" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "/home/dev-user/Projects/application/target/debug/deps/libversion_check-6d101f1b9670aa86.rlib" "-Wl,--start-group" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-6640d3868fa846e8.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-48481e446108229f.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libhashbrown-bfdf9e1c331f914a.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_alloc-991e68a3d0300af6.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libbacktrace-74304cfed66bbabf.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libbacktrace_sys-5db30a83f5489d12.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_demangle-a106c3f62654e72c.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-3d6b30695af38106.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcfg_if-d8f11f6bb46ba3ee.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-60c81ab95e289dd1.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-fd1a416f10d6c43d.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_core-4a2bd2b60cccd1fb.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-7ea8ebc630055039.rlib" "-Wl,--end-group" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-f7cd12d3ecd59a89.rlib" "-Wl,-Bdynamic" "-ldl" "-lrt" "-lpthread" "-lgcc_s" "-lc" "-lm" "-lrt" "-lpthread" "-lutil" "-ldl" "-lutil"
  = note: /home/dev-user/Projects/application/target/debug/deps/libversion_check-6d101f1b9670aa86.rlib(version_check-6d101f1b9670aa86.version_check.3hgk8ibb-cgu.5.rcgu.o): In function `version_check::is_max_version':
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:206: undefined reference to `version_check::version::Version::read'
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:206: undefined reference to `version_check::version::Version::parse'
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:207: undefined reference to `<version_check::version::Version as core::cmp::PartialOrd>::le'
          /usr/bin/ld: /home/dev-user/Projects/application/target/debug/build/proc-macro-error-attr-293e3d21843d85e8/build_script_build-293e3d21843d85e8: hidden symbol `_ZN73_$LT$version_check..version..Version$u20$as$u20$core..cmp..PartialOrd$GT$2le17hce324ff59318ace4E' isn't defined
          /usr/bin/ld: final link failed: Bad value
          collect2: error: ld returned 1 exit status
          

error: aborting due to previous error

error: could not compile `proc-macro-error-attr`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.0.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.1.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.2.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.3.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.4.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.5.rcgu.o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.build_script_build.67kbu6v9-cgu.6.rcgu.o" "-o" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12" "/home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12.4zz0kqsavf11fnqn.rcgu.o" "-Wl,--gc-sections" "-pie" "-Wl,-zrelro" "-Wl,-znow" "-nodefaultlibs" "-L" "/home/dev-user/Projects/application/target/debug/deps" "-L" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "/home/dev-user/Projects/application/target/debug/deps/libversion_check-6d101f1b9670aa86.rlib" "-Wl,--start-group" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-6640d3868fa846e8.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-48481e446108229f.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libhashbrown-bfdf9e1c331f914a.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_alloc-991e68a3d0300af6.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libbacktrace-74304cfed66bbabf.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libbacktrace_sys-5db30a83f5489d12.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_demangle-a106c3f62654e72c.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-3d6b30695af38106.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcfg_if-d8f11f6bb46ba3ee.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-60c81ab95e289dd1.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-fd1a416f10d6c43d.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/librustc_std_workspace_core-4a2bd2b60cccd1fb.rlib" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-7ea8ebc630055039.rlib" "-Wl,--end-group" "/home/dev-user/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-f7cd12d3ecd59a89.rlib" "-Wl,-Bdynamic" "-ldl" "-lrt" "-lpthread" "-lgcc_s" "-lc" "-lm" "-lrt" "-lpthread" "-lutil" "-ldl" "-lutil"
  = note: /home/dev-user/Projects/application/target/debug/deps/libversion_check-6d101f1b9670aa86.rlib(version_check-6d101f1b9670aa86.version_check.3hgk8ibb-cgu.5.rcgu.o): In function `version_check::is_max_version':
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:206: undefined reference to `version_check::version::Version::read'
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:206: undefined reference to `version_check::version::Version::parse'
          /home/dev-user/.cargo/registry/src/github.com-1ecc6299db9ec823/version_check-0.9.2/src/lib.rs:207: undefined reference to `<version_check::version::Version as core::cmp::PartialOrd>::le'
          /usr/bin/ld: /home/dev-user/Projects/application/target/debug/build/proc-macro-error-3d01fc0f96fb1a12/build_script_build-3d01fc0f96fb1a12: hidden symbol `_ZN73_$LT$version_check..version..Version$u20$as$u20$core..cmp..PartialOrd$GT$2le17hce324ff59318ace4E' isn't defined
          /usr/bin/ld: final link failed: Bad value
          collect2: error: ld returned 1 exit status
          

error: aborting due to previous error

error: could not compile `proc-macro-error`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: build failed
```

Am I missing any library or config?

## Code relevant to clap
```
#[derive(Clap)]
#[clap(version = APP_VERSION, author = APP_AUTHOR)]
struct Opts {
    #[clap(short, long)]
    config: Option<String>,

    #[clap(short, long)]
    address: Option<String>,

    #[clap(short, long)]
    port: Option<u16>,

    #[clap(short, long)]
    enable_https: Option<bool>,

    #[clap(short, long)]
    log_level: Option<u8>,
}

fn main() {
    let opts: Opts = Opts::parse();
    println!("{:#?}", opts);

    // ...
}
```

## Environments
cargo 1.44.1 (88ba85757 2020-06-11)
rustc 1.44.1 (c7087fe00 2020-06-17)
clap 3.0.0-beta.1

---

_Label `T: bug` added by @bbergeron0 on 2020-07-31 16:54_

---

_Comment by @CreepySkeleton on 2020-07-31 18:12_

That's weird. Could you run` cargo clean` and try again? And what is your OS?

---

_Comment by @bbergeron0 on 2020-07-31 19:51_

@CreepySkeleton `cargo clean` solved the issue, thanks a lot!

---

_Closed by @CreepySkeleton on 2020-07-31 20:04_

---
