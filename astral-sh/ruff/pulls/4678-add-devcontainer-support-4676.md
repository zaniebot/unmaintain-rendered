```yaml
number: 4678
title: "Add devcontainer support (#4676)"
type: pull_request
state: merged
author: vadim-su
labels: []
assignees: []
merged: true
base: main
head: add-devcontainer-support
created_at: 2023-05-27T12:49:52Z
updated_at: 2023-05-30T12:49:52Z
url: https://github.com/astral-sh/ruff/pull/4678
synced_at: 2026-01-12T03:50:03Z
```

# Add devcontainer support (#4676)

---

_Pull request opened by @vadim-su on 2023-05-27 12:49_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Added devcontainers environment support.

Devcontainer environment contains:
* Main utilities needed to develop `Ruff`
* Configurated `pre-commit`
* Additional extensions for vscode

### Known problems:
Test Explorer doesn't work
```
[2023-05-27 11:51:25.922] [INFO] Test Explorer found
[2023-05-27 11:51:25.922] [INFO] Initializing Rust adapter
[2023-05-27 11:51:25.924] [INFO] Loading Rust Tests
[2023-05-27 11:51:26.244] [WARN] Unsupported target type: bench for linter
[2023-05-27 11:51:26.244] [WARN] Unsupported target type: bench for parser
[2023-05-27 11:58:23.366] [DEBUG] Unable to retrieve enumeration of tests. Details: Error: Command failed: cargo test -p ruff_testing_macros --lib -- --list
    Blocking waiting for file lock on package cache
    Blocking waiting for file lock on package cache
    Blocking waiting for file lock on package cache
    Blocking waiting for file lock on package cache
    Blocking waiting for file lock on build directory
   Compiling syn v2.0.15
   Compiling ruff_testing_macros v0.0.0 (/workspaces/ruff/crates/ruff_testing_macros)
error[E0432]: unresolved imports `syn::FnArg`, `syn::ItemFn`, `syn::Pat`
  --> crates/ruff_testing_macros/src/lib.rs:13:61
   |
13 | use syn::{bracketed, parse_macro_input, parse_quote, Error, FnArg, ItemFn, LitStr, Pat, Token};
   |                                                             ^^^^^  ^^^^^^          ^^^
   |                                                             |      |               |
   |                                                             |      |               no `Pat` in the root
   |                                                             |      |               help: a similar name exists in the module: `Path`
   |                                                             |      no `ItemFn` in the root
   |                                                             no `FnArg` in the root

For more information about this error, try `rustc --explain E0432`.
error: could not compile `ruff_testing_macros` due to previous error
```

## Test Plan
<details> 
  <summary>Using vscode `devcontainer` extension</summary>

  1. Open the project in vscode
  2. Reopen in container
  ![image](https://github.com/charliermarsh/ruff/assets/1702003/8661a5fb-ed9e-4cd7-b790-12ac45977ab8)
  ![image](https://github.com/charliermarsh/ruff/assets/1702003/04caca66-892a-4596-9762-5d27ff94d0a4)
</details>


<details> 
  <summary>Using Codespace</summary>

  1. Click on `Code` 
  2. Go to the `Codespaces` tab 
  3. Click the big green :green_square: button
  ![image](https://github.com/charliermarsh/ruff/assets/1702003/cb011663-64a5-423a-84cc-0f79ab9e2379) 
</details>



---

_Comment by @github-actions[bot] on 2023-05-27 13:02_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.1±0.15ms     2.9 MB/sec    1.00     14.0±0.08ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.01    426.3±0.90µs     6.9 MB/sec    1.00    423.5±0.73µs     7.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.3 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     5.9 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1505.7±5.99µs    11.1 MB/sec    1.00   1496.9±2.73µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    170.2±0.20µs    17.3 MB/sec    1.00    170.2±2.77µs    17.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.2 MB/sec    1.00      3.1±0.00ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.00ms     7.8 MB/sec    1.02      5.3±0.02ms     7.7 MB/sec
parser/numpy/ctypeslib.py                  1.00   1026.7±0.93µs    16.2 MB/sec    1.01   1040.5±1.27µs    16.0 MB/sec
parser/numpy/globals.py                    1.00    106.0±0.18µs    27.8 MB/sec    1.02    107.8±0.61µs    27.4 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.01ms    11.4 MB/sec    1.02      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.7±0.29ms     2.4 MB/sec    1.00     16.8±0.23ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.06ms     4.0 MB/sec    1.00      4.2±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    500.8±6.47µs     5.9 MB/sec    1.00    501.0±8.38µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.1±0.10ms     3.6 MB/sec    1.00      7.0±0.06ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.17ms     4.9 MB/sec    1.00      8.2±0.11ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1746.3±23.37µs     9.5 MB/sec    1.02  1776.9±27.51µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.5±4.47µs    14.6 MB/sec    1.01    205.4±4.99µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.8±0.06ms     6.8 MB/sec
parser/large/dataset.py                    1.00      6.4±0.05ms     6.3 MB/sec    1.00      6.4±0.04ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1226.3±13.73µs    13.6 MB/sec    1.00  1227.6±15.80µs    13.6 MB/sec
parser/numpy/globals.py                    1.00    125.5±2.37µs    23.5 MB/sec    1.00    125.3±2.18µs    23.5 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.03ms     9.2 MB/sec    1.00      2.8±0.03ms     9.2 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `.devcontainer/devcontainer.json`:22 on 2023-05-30 07:17_

I would prefer to list the dependencies explicitly rather than including bundles, considering that they get installed for everyone: 

```suggestion
				"rust-lang.rust-analyzer",
				"serayuzgur.crates",
```

---

_Review comment by @MichaReiser on `.devcontainer/devcontainer.json`:27 on 2023-05-30 07:18_

Should we add ruff?

---

_Review comment by @MichaReiser on `.devcontainer/post-create.sh`:10 on 2023-05-30 07:26_

This seems to install `pre-commit` globally on my machine and not just in the devcontainer. The result is that all my git commands now fail, because I don't have pre-commit installed. 

Do you know why it installs the pre-commit hook outside of the devcontainer? Should we just remove it?

Edit: Okay, the reason why is because the pre-commit hook gets added to `.git/hooks`. I would prefer to not adding it for now because it messes with the users' setup. 

---

_@MichaReiser approved on 2023-05-30 07:31_

This is cool. Thank you.

I fixed the issue with Test Explorer in https://github.com/charliermarsh/ruff/pull/4722

For some reason, GitHub errors with a 500 error if I open ruff in code space. That's a pitty (or will it work only after merging this PR?)

I've a few smaller comments but overall this is looking good.

---

_@vadim-su reviewed on 2023-05-30 08:35_

---

_Review comment by @vadim-su on `.devcontainer/post-create.sh`:10 on 2023-05-30 08:35_

Interesting effect :sweat_smile:
I completely forgot that it makes changes not only to the environment

---

_@vadim-su reviewed on 2023-05-30 08:35_

---

_Review comment by @vadim-su on `.devcontainer/post-create.sh`:10 on 2023-05-30 08:35_

Should I keep it on the list of packages to install?

---

_Comment by @vadim-su on 2023-05-30 09:16_

Codespace works very well for me
![image](https://github.com/charliermarsh/ruff/assets/1702003/6cb43b73-3efc-470b-ae10-8f968f76441c)



---

_Comment by @vadim-su on 2023-05-30 09:17_

This is probably due to the fact that the branch is in my repository.

---

_@vadim-su reviewed on 2023-05-30 09:21_

---

_Review comment by @vadim-su on `.devcontainer/devcontainer.json`:22 on 2023-05-30 09:21_

Done

---

_@vadim-su reviewed on 2023-05-30 09:21_

---

_Review comment by @vadim-su on `.devcontainer/devcontainer.json`:27 on 2023-05-30 09:21_

Added

---

_@MichaReiser reviewed on 2023-05-30 12:49_

---

_Review comment by @MichaReiser on `.devcontainer/post-create.sh`:10 on 2023-05-30 12:49_

Yeah, I think installing is fine. User's can then opt-in by running the pre-commit install manually. 

---

_Comment by @MichaReiser on 2023-05-30 12:49_

> Codespace works very well for me ![image](https://user-images.githubusercontent.com/1702003/241911371-6cb43b73-3efc-470b-ae10-8f968f76441c.png)

The VS Code Codespaces works fine for me. It's just that opening codespaces on Github fails. Let's see if it is "fixed" after merging your PR. 

Thank you!

---

_Merged by @MichaReiser on 2023-05-30 12:49_

---

_Closed by @MichaReiser on 2023-05-30 12:49_

---
