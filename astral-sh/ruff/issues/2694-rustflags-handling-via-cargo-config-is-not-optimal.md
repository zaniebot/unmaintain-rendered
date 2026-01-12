```yaml
number: 2694
title: RUSTFLAGS handling via cargo config is not optimal.
type: issue
state: closed
author: gyakovlev
labels:
  - internal
assignees: []
created_at: 2023-02-09T20:02:54Z
updated_at: 2024-07-26T02:13:36Z
url: https://github.com/astral-sh/ruff/issues/2694
synced_at: 2026-01-12T15:54:43Z
```

# RUSTFLAGS handling via cargo config is not optimal.

---

_@gyakovlev_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Hi,
https://github.com/charliermarsh/ruff/commit/b5ac93d2eeb2a3af56022c948faa445a577c9ece moved lints to cargo config file.
I believe it's not optimal solution, as it applies to everything and leads to build failures.

it will only get applied if `RUSTFLAGS` envvar is not defined before starting the build.
if a user/CI job has `RUSTFLAGS` defined, this file is skipped altogether.


```
    Running `rustc --crate-name imperative --edition=2021 /var/tmp/portage/dev-util/ruff-0.0.244/work/cargo_home/gentoo/imperative-1.0.4/src/lib.rs --error-format=json --json=diagnostic-rendered-ansi,artifacts
,future-incompat --crate-type lib --emit=dep-info,metadata,link -C opt-level=3 -C panic=abort -C codegen-units=1 -C metadata=c59b923aa5a848a5 -C extra-filename=-c59b923aa5a848a5 --out-dir /var/tmp/portage/dev-u
til/ruff-0.0.244/work/ruff-0.0.244/target/release/deps -L dependency=/var/tmp/portage/dev-util/ruff-0.0.244/work/ruff-0.0.244/target/release/deps --extern phf=/var/tmp/portage/dev-util/ruff-0.0.244/work/ruff-0.
0.244/target/release/deps/libphf-cd1439e7d6a81c48.rmeta --extern rust_stemmers=/var/tmp/portage/dev-util/ruff-0.0.244/work/ruff-0.0.244/target/release/deps/librust_stemmers-3f0bf859cc9852ca.rmeta --cap-lints al
low -C target-cpu=pwr9 -Dunsafe_code '-Wclippy::pedantic' '-Wclippy::char_lit_as_u8' '-Aclippy::collapsible_else_if' '-Aclippy::collapsible_if' '-Aclippy::implicit_hasher' '-Aclippy::match_same_arms' '-Aclippy:
:missing_errors_doc' '-Aclippy::missing_panics_doc' '-Aclippy::module_name_repetitions' '-Aclippy::must_use_candidate' '-Aclippy::similar_names' '-Aclippy::too_many_lines' '-Wclippy::print_stdout' '-Wclippy::pr
int_stderr' '-Wclippy::dbg_macro'`                                                                                                                                                                                
error: usage of an `unsafe` block                                                                                                                                                                                 
   --> /var/tmp/portage/dev-util/ruff-0.0.244/work/RustPython-adc23253e4b58980b407ba2760dbe61681d752fc/common/src/atomic.rs:121:22                                                                                
    |                                                                                                                                                                                                             
121 |                 drop(unsafe { Box::from_raw(ptr) });                                                                                                                                                        
    |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^                                                                                                                                                          
    |                                                                                                                                                                                                             
    = note: requested on the command line with `-D unsafe-code`                                                                                                                                                   
                                                                                                                                                                                                                  
error: usage of an `unsafe` block                                                                                                                                                                                 
   --> /var/tmp/portage/dev-util/ruff-0.0.244/work/RustPython-adc23253e4b58980b407ba2760dbe61681d752fc/common/src/atomic.rs:125:9                                                                                 
    |                                                                                                                                                                                                             
125 |         unsafe { NonNull::new_unchecked(ptr) }                                                                                                                                                              
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^                                                                                                                                                              
                                                                                                                                                                                                                  
error: usage of an `unsafe` block                                                                                                                                                                                 
  --> /var/tmp/portage/dev-util/ruff-0.0.244/work/RustPython-adc23253e4b58980b407ba2760dbe61681d752fc/common/src/boxvec.rs:44:9                                                                                   
   |                                                                                                                                                                                                              
44 | /         unsafe {                                                                                                                                                                                           
45 | |             let layout = match alloc::Layout::array::<T>(n) {                                                                                                                                              
46 | |                 Ok(l) => l,                                                                                                                                                                                
47 | |                 Err(_) => capacity_overflow(),                                                                                                                                                             
...  |                                                                                                                                                                                                            
60 | |             BoxVec { xs, len: 0 }                                                                                                                                                                          
61 | |         }                                                                                                                                                                                                  
   | |_________^                
```
and many many many more.

---

_Comment by @gyakovlev on 2023-02-09 20:04_

it builds fine with `RUSTFLAGS` envvar exported (even empty one).

---

_Comment by @charliermarsh on 2023-02-09 22:57_

Thanks for filing! This was the best pattern I could find for enforcing a standard Clippy configuration across a multi-crate project. Is there another pattern you've seen elsewhere or would suggest?

I could try instead moving to [`cargo cranky`](https://github.com/ericseppanen/cargo-cranky), but there are costs associated with that too.


---

_Label `question` added by @charliermarsh on 2023-02-09 22:57_

---

_Comment by @gyakovlev on 2023-02-10 22:57_

I did some research and looks like no real way of doing it.

https://doc.rust-lang.org/cargo/reference/config.html

~
At present, when being invoked from a workspace, Cargo does not read config files from crates within the workspace. i.e. if a workspace has two crates in it, named /projects/foo/bar/baz/mylib and /projects/foo/bar/baz/mybin, and there are Cargo configs at /projects/foo/bar/baz/mylib/.cargo/config.toml and /projects/foo/bar/baz/mybin/.cargo/config.toml, Cargo does not read those configuration files if it is invoked from the workspace root (/projects/foo/bar/baz/).
~

so it will not work in workspace subdirs unfortunately...

that makes me believe that how it was before is the most compatible way.
And ofc running clippy via CI with specified settings or via an alias for workspace crates only, as you don't really have control over deps.

build time clippy lints for downstream/user/distros are not very valuable, so I think it belongs to CI/dev environment, not release profile builds.

specifying at top level propagates flags to all dependencies and dependencies of dependencies and wreaks total chaos, because not all the packages qualify for those lints.

ofc we (gentoo linux) could simply nuke that file before build, but I'm here to let you know it may be problematic for downstreams/distros and consumers going forward as more distros package ruff.


---

_Comment by @not-my-profile on 2023-02-15 17:23_

I think the very recently (2 hours ago) proposed [RFC 3389](https://github.com/rust-lang/rfcs/pull/3389) would provide a better way.

---

_Comment by @charliermarsh on 2023-02-15 18:29_

Oh yeah, I'd love that.

---

_Label `internal` added by @charliermarsh on 2023-07-10 01:18_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:18_

---

_Comment by @charliermarsh on 2023-07-10 01:19_

(We will fix this as soon as the `Cargo.toml` configuration stabilizes.)

---

_Comment by @charliermarsh on 2024-07-26 02:13_

This got done.

---

_Closed by @charliermarsh on 2024-07-26 02:13_

---
