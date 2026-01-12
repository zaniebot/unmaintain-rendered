```yaml
number: 17923
title: Cannot checkout ruff repository on windows due to long file paths
type: issue
state: closed
author: jvllmr
labels:
  - windows
assignees: []
created_at: 2025-04-28T18:55:55Z
updated_at: 2025-07-17T14:09:55Z
url: https://github.com/astral-sh/ruff/issues/17923
synced_at: 2026-01-12T15:54:56Z
```

# Cannot checkout ruff repository on windows due to long file paths

---

_@jvllmr_

### Summary

```
PS C:\Users\zunde\Documents\DEV> git clone https://github.com/astral-sh/ruff.git
Cloning into 'ruff'...
remote: Enumerating objects: 205831, done.
remote: Counting objects: 100% (1736/1736), done.
remote: Compressing objects: 100% (751/751), done.
remote: Total 205831 (delta 1417), reused 1022 (delta 985), pack-reused 204095 (from 2)
Receiving objects: 100% (205831/205831), 67.28 MiB | 43.41 MiB/s, done.
Resolving deltas: 100% (151291/151291), done.
error: unable to create file crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`unsupported-bool-conversion`_can_occur_-_Part_of_a_union_where_at_least_one_member_has_incorrect_`__bool__`_method.snap: Filename too long
Updating files: 100% (8829/8829), done.
fatal: unable to checkout working tree
warning: Clone succeeded, but checkout failed.
You can inspect what was checked out with 'git status'
and retry with 'git restore --source=HEAD :/'
```

I don't know if developing ruff on windows is even possible, but I first faced the issue in my GitHub Actions:
https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78
```
Run cargo vendor
  cargo vendor
  shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
  env:
    CARGO_HOME: D:\a\flay\flay/.c
    pythonLocation: C:\hostedtoolcache\windows\Python\3.10.11\x64
    PKG_CONFIG_PATH: C:\hostedtoolcache\windows\Python\3.10.11\x64/lib/pkgconfig
    Python_ROOT_DIR: C:\hostedtoolcache\windows\Python\3.10.11\x64
    Python[2](https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78#step:5:2)_ROOT_DIR: C:\hostedtoolcache\windows\Python\[3](https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78#step:5:3).10.11\x64
    Python3_ROOT_DIR: C:\hostedtoolcache\windows\Python\3.10.11\x6[4](https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78#step:5:4)
    Updating crates.io index
    Updating git repository `https://github.com/astral-sh/ruff.git`
error: failed to sync

Caused by:
  failed to load pkg lockfile

Caused by:
  failed to get `ruff_python_ast` as a dependency of package `flay_rs v0.1.0 (D:\a\flay\flay\rust)`

Caused by:
  failed to load source for dependency `ruff_python_ast`

Caused by:
  Unable to update https://github.com/astral-sh/ruff.git?tag=0.11.7#f7b48[5](https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78#step:5:5)10

Caused by:
  path too long: 'D:/a/flay/flay/.c/git/checkouts/ruff-b18f[6](https://github.com/jvllmr/flay/actions/runs/14660665969/job/41144210179?pr=78#step:5:6)9e2b025fac7/f7b4851/crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`unsupported-bool-conversion`_can_occur_-_Has_a_`__bool__`_attribute,_but_it's_not_callable.snap'; class=Filesystem (30)
Error: Process completed with exit code 1.
```

I could work around that issue by allowing longer file paths on my local windows installation, but this is not possible for GitHub Action Runners,

### Version

tag 0.11.5 or later

---

_Label `windows` added by @ntBre on 2025-04-28 19:00_

---

_Comment by @ntBre on 2025-04-28 19:10_

Interesting, thanks for the report! As a quick fix we could just shorten the name of that `mdtest` snapshot, but maybe we should also add a check for this or update our snapshot naming scheme more generally.

Quoted snapshot path in question:

```
'crates/red_knot_python_semantic/resources/mdtest/snapshots/unsupported_bool_conversion.md_-_Different_ways_that_`unsupported-bool-conversion`_can_occur_-_Has_a_`__bool__`_attribute,_but_it'\''s_not_callable.snap'
```

---

_Comment by @jvllmr on 2025-04-28 19:44_

Sounds great! Please note that I faced this issue 3 weeks ago with another file, but I was able to work around it by chaning `CARGO_HOME` to `.c` and missed out on reporting it here then. Therefore, I think creating a new naming scheme would be the best approach in the long run. The idea might create some overhead in the management of the files, but what about using numeric / very short file names for machines and creating a `README` in the directory which contains an index of the files for humans?


Here comes the other example where my GitHub Actions failed:
File path: `'crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Test_calling_a_function_whose_type_is_vendored_from_`typeshed`.snap'`
GitHub Actions link: https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68


```
Run pdm install --frozen-lock
  pdm install --frozen-lock
  shell: C:\Program Files\PowerShell\7\pwsh.EXE -command ". '{0}'"
  env:
    pythonLocation: C:\hostedtoolcache\windows\Python\3.1[2](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:2).9\x64
    PKG_CONFIG_PATH: C:\hostedtoolcache\windows\Python\[3](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:3).12.9\x64/lib/pkgconfig
    Python_ROOT_DIR: C:\hostedtoolcache\windows\Python\3.12.9\x6[4](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:4)
    Python2_ROOT_DIR: C:\hostedtoolcache\windows\Python\3.12.9\x64
    Python3_ROOT_DIR: C:\hostedtoolcache\windows\Python\3.12.9\x64
INFO: Virtualenv D:\a\flay\flay\.venv is reused.
STATUS: Resolving packages from lockfile...
All packages are synced to date, nothing to do.
  x Update flay 0.1.0 -> None failed

  0:00:00 Installing the project as an editable package... 0/0
See C:\Users\runneradmin\AppData\Local\pdm\pdm\Logs\pdm-install-ok84uz34.log for detailed debug log.
[BuildError]: Build backend raised error: Showing the last 10 lines of the build output:

Caused by:
  Unable to update https://github.com/astral-sh/ruff.git?tag=0.11.[5](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:5)#7186d5e9

Caused by:
  path too long: 'C:/Users/runneradmin/.cargo/git/checkouts/ruff-b18f[6](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:6)9e2b025fac7/7186d5e/crates/red_knot_python_semantic/resources/mdtest/snapshots/invalid_argument_type.md_-_Invalid_argument_type_diagnostics_-_Test_calling_a_function_whose_type_is_vendored_from_`typeshed`.snap'; class=Filesystem (30)
\U0001f4a5 maturin failed
  Caused by: Cargo metadata failed. Does your crate compile with `cargo build`?
  Caused by: `cargo metadata` exited with an error: 
Error: command ['maturin', 'pep51[7](https://github.com/jvllmr/flay/actions/runs/14388456142/job/40349557876?pr=68#step:4:7)', 'build-wheel', '-i', 'D:\\a\\flay\\flay\\.venv\\Scripts\\python.exe', '--compatibility', 'off', '--editable'] returned non-zero exit status 1
WARNING: Add '-v' to see the detailed traceback
Error: Process completed with exit code 1.
```

---

_Comment by @MichaReiser on 2025-04-29 06:24_

Have you tried git's longpath feature ([link](https://stackoverflow.com/questions/22575662/filename-too-long-in-git-for-windows))

We do run windows builds in GitHub actions and we haven't run into the same longpath issues. 

https://github.com/astral-sh/ruff/blob/53a9448fb52a4339d4e0d80113b174cf44ec57b5/.github/workflows/ci.yaml#L309-L334

Any chance you use a different (older runner)?

I haven't used my windows machine in a while but can try out if it reproduces for me.

---

_Comment by @jvllmr on 2025-04-29 07:04_

Thanks for the hint. Did not occur to me that windows-latest could point to an older version. As of right now `windows-latest` actually points to `windows-2022` (https://github.com/actions/runner-images/blob/0e37973a015a27a4a410a4ae37c0aa99bdd63ea8/README.md?plain=1#L36). I will try the `windows-2025` runner for now and tell you if that changed anything. If not I will try the runner you are using in your pipeline.

---

_Comment by @jvllmr on 2025-04-29 08:44_

Using `windows-2025` did not bring the desired outcome, but running `git config --global core.longpaths true` before running cargo related commands fixed this problem for me. I assume the `github-windows-2025-x86_64-16` runner has this enabled by default.

---

_Label `windows` added by @MichaReiser on 2025-05-07 15:18_

---

_Comment by @ntBre on 2025-07-17 13:59_

I think this should also be helped by https://github.com/astral-sh/ruff/pull/18039. 

---

_Comment by @MichaReiser on 2025-07-17 14:09_

I'll close this. @jvllmr let us know if you're still runing into this and we can reopen the issue

---

_Closed by @MichaReiser on 2025-07-17 14:09_

---
