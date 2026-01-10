```yaml
number: 5851
title: "`uv lock --locked` raises false-positive errors with git dependencies"
type: issue
state: closed
author: tpgillam
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-07T12:26:13Z
updated_at: 2024-08-07T16:06:40Z
url: https://github.com/astral-sh/uv/issues/5851
synced_at: 2026-01-10T04:53:49Z
```

# `uv lock --locked` raises false-positive errors with git dependencies

---

_Issue opened by @tpgillam on 2024-08-07 12:26_

I am using uv `0.2.33`.

Suppose I have a git dependency in my `pyproject.toml` (complete listing below):
```toml
[project]
name = "moo"
version = "0.1.0"
dependencies = ["elmer-circuitbuilder"]
requires-python = ">=3.11"

[tool.uv.sources]
elmer-circuitbuilder = { git = "https://github.com/ElmerCSC/elmer_circuitbuilder.git" }

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

In a directoy that is empty other than for the above `pyproject.toml`, I then run:

```bash
uv lock && cp uv.lock ref_uv.lock
```

to generate `uv.lock` and copy it to a reference file.

If I lock again, then `uv.lock` is unchanged:
```bash
uv lock && diff uv.lock ref_uv.lock   # no diff
```

But, if I run `uv lock --locked`, it fails:
```
Using Python 3.11.9
⠙ Resolving dependencies...                                                                                                                      warning: `uv.sources` is experimental and may change without warning
Resolved 2 packages in 15ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

I would expect this to not fail, since `uv.lock` wouldn't actually be changed.

---

<details>
<summary>Output of `uv lock --locked --verbose --verbose`</summary>

```
    0.000068s DEBUG uv uv 0.2.33
warning: `uv lock` is experimental and may change without warning
    0.000934s DEBUG uv_workspace::workspace Found workspace root: `/home/tom/moo`
    0.000988s DEBUG uv_workspace::workspace Adding current workspace member: `/home/tom/moo`
    0.001525s DEBUG uv_python::discovery Searching for Python >=3.11, <3.12 in managed installations or system path
    0.001644s DEBUG uv_python::discovery Searching for managed installations at `/home/tom/.local/share/uv/python`
    0.001752s DEBUG uv_python::discovery Found managed installation `cpython-3.11.9-linux-x86_64-gnu`
    0.001982s DEBUG uv_python::discovery Found `cpython-3.11.9-linux-x86_64-gnu` at `/home/tom/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3` (managed installations)
Using Python 3.11.9
 uv_client::linehaul::linehaul
    0.004138s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
    0.004922s DEBUG uv::commands::project::lock Resolving with existing `uv.lock`
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=moo @ file:///home/tom/moo
    0.007341s DEBUG uv_fs Acquired lock for `/home/tom/.cache/uv/built-wheels-v3/editable/52ff86b5341b58e9`
    0.007972s   1ms DEBUG uv_distribution::source Using cached metadata for: moo @ file:///home/tom/moo
    0.009301s   2ms DEBUG uv_workspace::workspace No workspace root found, using project root
warning: `uv.sources` is experimental and may change without warning
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=elmer-circuitbuilder @ git+https://github.com/ElmerCSC/elmer_circuitbuilder.git
    0.009978s   0ms DEBUG uv_git::resolver Fetching source distribution from Git: https://github.com/ElmerCSC/elmer_circuitbuilder.git
    0.010205s   0ms DEBUG uv_fs Acquired lock for `https://github.com/elmercsc/elmer_circuitbuilder`
 uv_git::source::fetch repository=https://github.com/ElmerCSC/elmer_circuitbuilder.git, rev=Some(GitSha(GitOid { len: 40, bytes: [52, 52, 100, 50, 102, 52, 98, 49, 57, 100, 54, 56, 51, 55, 101, 97, 57, 57, 48, 99, 49, 54, 102, 52, 57, 52, 98, 100, 102, 55, 53, 52, 51, 100, 53, 55, 52, 56, 51, 100] }))
    0.014853s   4ms DEBUG uv_git::source Using existing git source `Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("github.com")), port: None, path: "/ElmerCSC/elmer_circuitbuilder.git", query: None, fragment: None }`
    0.019150s DEBUG uv_fs Acquired lock for `/home/tom/.cache/uv/built-wheels-v3/git/c832992636859332/44d2f4b19d6837ea`
    0.019382s   9ms DEBUG uv_distribution::source Using cached metadata for: elmer-circuitbuilder @ git+https://github.com/ElmerCSC/elmer_circuitbuilder.git
 uv_resolver::resolver::solve
    0.021092s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.9
    0.021129s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.11, <3.12
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.021242s   0ms DEBUG uv_resolver::resolver Adding direct dependency: moo*
   uv_resolver::resolver::choose_version package=moo
      0.021311s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of moo @ file:///home/tom/moo (*)
   uv_resolver::resolver::get_dependencies_forking package=moo, version=0.1.0
     uv_resolver::resolver::get_dependencies package=moo, version=0.1.0
    0.021352s   0ms DEBUG uv_resolver::resolver Adding transitive dependency for moo==0.1.0: elmer-circuitbuilder*
   uv_resolver::resolver::choose_version package=elmer-circuitbuilder
      0.021363s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of elmer-circuitbuilder @ git+https://github.com/ElmerCSC/elmer_circuitbuilder.git (*)
   uv_resolver::resolver::get_dependencies_forking package=elmer-circuitbuilder, version=1.0.0
     uv_resolver::resolver::get_dependencies package=elmer-circuitbuilder, version=1.0.0
    0.021415s   0ms DEBUG uv_resolver::resolver::batch_prefetch Tried 2 versions: elmer-circuitbuilder 1, moo 1
    0.021421s   0ms DEBUG uv_resolver::resolver Split universal resolution took 0.000s
Resolved 2 packages in 16ms
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

</details>


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-07 12:41_

---

_Comment by @charliermarsh on 2024-08-07 12:41_

I’ll take a look, thanks.

---

_Comment by @tpgillam on 2024-08-07 12:42_

Addendum:

After getting my directory into the state above, I thought I'd try adding a specific commit, to see if it was the unbounded tracking of a branch that was the problem. So I amended the `sources` section to:
```toml
[tool.uv.sources]
elmer-circuitbuilder = { git = "https://github.com/ElmerCSC/elmer_circuitbuilder.git", rev = "44d2f4b19d6837ea990c16f494bdf7543d57483d" }
```

I then ran `uv lock`, and got the following, which I guess is unrelated?!
```
warning: `uv lock` is experimental and may change without warning
Using Python 3.11.9
⠙ Resolving dependencies...                                                                                                                      warning: `uv.sources` is experimental and may change without warning
Resolved 2 packages in 7ms
thread 'main' panicked at crates/uv-resolver/src/lock.rs:1490:49:
precise commit
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```
(see below for full output with backtrace)

... anyway, if I delete `uv.lock`, and then go through the steps from my first post (lock, make a reference, then get a diff), then we still get the original failure.


---
<details>
<summary>`RUST_BACKTRACE=full uv lock --verbose --verbose`</summary>

```
    0.000040s DEBUG uv uv 0.2.33
warning: `uv lock` is experimental and may change without warning
    0.000392s DEBUG uv_workspace::workspace Found workspace root: `/home/tom/moo`
    0.000403s DEBUG uv_workspace::workspace Adding current workspace member: `/home/tom/moo`
    0.000654s DEBUG uv_python::discovery Searching for Python >=3.11 in managed installations or system path
    0.000701s DEBUG uv_python::discovery Searching for managed installations at `/home/tom/.local/share/uv/python`
    0.000748s DEBUG uv_python::discovery Found managed installation `cpython-3.11.9-linux-x86_64-gnu`
    0.000822s DEBUG uv_python::discovery Found `cpython-3.11.9-linux-x86_64-gnu` at `/home/tom/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3` (managed installations)
Using Python 3.11.9
 uv_client::linehaul::linehaul 
    0.001236s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries 
    0.001349s DEBUG uv::commands::project::lock Resolving with existing `uv.lock`
 uv_distribution::distribution_database::get_or_build_wheel_metadata dist=moo @ file:///home/tom/moo
    0.002071s DEBUG uv_fs Acquired lock for `/home/tom/.cache/uv/built-wheels-v3/editable/a6b042fec9cfecc7`
   uv_distribution::source::build_metadata dist=moo @ file:///home/tom/moo
      0.002694s   0ms DEBUG uv_distribution::source Preparing metadata for: moo @ file:///home/tom/moo
      0.002897s   0ms DEBUG uv_distribution::source No static `PKG-INFO` available for: moo @ file:///home/tom/moo (MissingPkgInfo)
      0.003007s   0ms DEBUG uv_distribution::source Found static `pyproject.toml` for: moo @ file:///home/tom/moo
    0.003736s   1ms DEBUG uv_workspace::workspace No workspace root found, using project root
warning: `uv.sources` is experimental and may change without warning
 uv_resolver::resolver::solve 
    0.005109s   0ms DEBUG uv_resolver::resolver Solving with installed Python version: 3.11.9
    0.005121s   0ms DEBUG uv_resolver::resolver Solving with target Python version: >=3.11
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies_forking package=root, version=0a0.dev0
     uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
    0.005215s   0ms DEBUG uv_resolver::resolver Adding direct dependency: moo*
   uv_resolver::resolver::choose_version package=moo
      0.005256s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of moo @ file:///home/tom/moo (*)
   uv_resolver::resolver::get_dependencies_forking package=moo, version=0.1.0
     uv_resolver::resolver::get_dependencies package=moo, version=0.1.0
    0.005284s   0ms DEBUG uv_resolver::resolver Adding transitive dependency for moo==0.1.0: elmer-circuitbuilder*
   uv_resolver::resolver::choose_version package=elmer-circuitbuilder
      0.005297s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of elmer-circuitbuilder @ git+https://github.com/ElmerCSC/elmer_circuitbuilder.git@44d2f4b19d6837ea990c16f494bdf7543d57483d (*)
   uv_resolver::resolver::get_dependencies_forking package=elmer-circuitbuilder, version=1.0.0
     uv_resolver::resolver::get_dependencies package=elmer-circuitbuilder, version=1.0.0
    0.005319s   0ms DEBUG uv_resolver::resolver::batch_prefetch Tried 2 versions: elmer-circuitbuilder 1, moo 1
    0.005323s   0ms DEBUG uv_resolver::resolver Split universal resolution took 0.000s
Resolved 2 packages in 4ms
thread 'main' panicked at crates/uv-resolver/src/lock.rs:1490:49:
precise commit
stack backtrace:
   0:     0x55a8732dcc05 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h3692694645b1bb6a
   1:     0x55a87330d16b - core::fmt::write::h5131d80b4c69b88d
   2:     0x55a8732d86ff - std::io::Write::write_fmt::h1fb327a7d8b0eb36
   3:     0x55a8732dc9de - std::sys_common::backtrace::print::h998d75b840f75a73
   4:     0x55a8732de5b9 - std::panicking::default_hook::{{closure}}::h18ec7fe6a38b9da0
   5:     0x55a8732de35a - std::panicking::default_hook::hfb3f22c2e4075a6a
   6:     0x55a8732dea53 - std::panicking::rust_panic_with_hook::h51af00bcb4660c4e
   7:     0x55a8732de934 - std::panicking::begin_panic_handler::{{closure}}::h39f76aa863fbe8ce
   8:     0x55a8732dd0c9 - std::sys_common::backtrace::__rust_end_short_backtrace::h4d10fc2251b89840
   9:     0x55a8732de667 - rust_begin_unwind
  10:     0x55a871ee8843 - core::panicking::panic_fmt::h319840fcbcd912ef
  11:     0x55a871ee880b - core::option::expect_failed::h1726eaf02b540434
  12:     0x55a872b9006b - uv_resolver::lock::DistributionId::from_annotated_dist::h26af7654f057d56e
  13:     0x55a872b80033 - uv_resolver::lock::Lock::from_resolution_graph::h2e065cf20f83fb19
  14:     0x55a872115fd0 - uv::commands::project::lock::do_lock::{{closure}}::ha7fe82dbc74116a7
  15:     0x55a8721121b9 - uv::commands::project::lock::do_safe_lock::{{closure}}::h14d4b716c9b7c02c
  16:     0x55a87209f571 - uv::run_project::{{closure}}::h58ce759ce9400f62
  17:     0x55a8720aedc5 - uv::run::{{closure}}::{{closure}}::h617eb9f4f0c7eb73
  18:     0x55a872162719 - <core::pin::Pin<P> as core::future::future::Future>::poll::h57ddc5583e67df57
  19:     0x55a87239199a - tokio::runtime::scheduler::current_thread::Context::enter::hda21978c0a3d9ab0
  20:     0x55a87267d4cf - tokio::runtime::context::set_scheduler::hee0f9e3cd8d0d8c9
  21:     0x55a872391d5f - tokio::runtime::scheduler::current_thread::CoreGuard::block_on::hba7465fc23e9580e
  22:     0x55a872516687 - tokio::runtime::context::runtime::enter_runtime::h572289c9d09cfe6b
  23:     0x55a8723914e2 - tokio::runtime::scheduler::current_thread::CurrentThread::block_on::hfb3c546a00725d2c
  24:     0x55a87251726d - tokio::runtime::runtime::Runtime::block_on::hf0d4466522304e89
  25:     0x55a8724d271e - uv::main::hb10b32a9b64ee0e3
  26:     0x55a872663189 - uv::main::ha8063c76788bf5fe
  27:     0x55a8723495c3 - std::sys_common::backtrace::__rust_begin_short_backtrace::h815c2009e76677bc
  28:     0x55a872487a39 - std::rt::lang_start::{{closure}}::h14834df7e8326d70
  29:     0x55a8732cdb5d - std::rt::lang_start_internal::h63a185b0ddd212e9
  30:     0x55a8726631b5 - main
  31:     0x7fa41a32bd90 - <unknown>
  32:     0x7fa41a32be40 - __libc_start_main
  33:     0x55a871ee9109 - <unknown>
  34:                0x0 - <unknown>

```

</details>

---

_Comment by @charliermarsh on 2024-08-07 13:02_

Thanks, I can look at that too :)

---

_Label `bug` added by @charliermarsh on 2024-08-07 13:02_

---

_Label `preview` added by @charliermarsh on 2024-08-07 13:02_

---

_Comment by @charliermarsh on 2024-08-07 13:25_

Going to separate into two issues, I've identified them both but they're different.

---

_Closed by @charliermarsh on 2024-08-07 15:47_

---

_Comment by @charliermarsh on 2024-08-07 15:48_

These are great, thank you for testing this stuff out and reporting. We'll probably release today with all these fixes.

---

_Comment by @tpgillam on 2024-08-07 16:06_

Thanks, and I look forward to testing the new release! Very happy to help track down the buglets.

---
