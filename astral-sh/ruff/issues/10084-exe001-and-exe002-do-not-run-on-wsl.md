```yaml
number: 10084
title: EXE001 and EXE002 do not run on WSL
type: issue
state: open
author: MusicalNinjaDad
labels:
  - rule
assignees: []
created_at: 2024-02-22T19:36:15Z
updated_at: 2026-01-11T09:21:10Z
url: https://github.com/astral-sh/ruff/issues/10084
synced_at: 2026-01-11T09:56:57Z
```

# EXE001 and EXE002 do not run on WSL

---

_Issue opened by @MusicalNinjaDad on 2024-02-22 19:36_

**Background:**
I have just set up ruff for the first time, and loved it, so fast and so complete! I fixed all the errors, added the github action to my pipeline, pushed the commit and ... boom, pipeline fail due to EXE002 ;(

The opening sentence of Microsoft's documentation on [Working across file systems](https://learn.microsoft.com/en-us/windows/wsl/filesystems#file-storage-and-performance-across-file-systems) is:

> We recommend against working across operating systems with your files, unless you have a specific reason for doing so.

Executing checks for EXE001 and EXE002 should therefore be possible on WSL in most situations. These have been specifically disabled with #5735 

**Suggestion:**
Add a configuration option `lint.ignore-exe-wsl`; ideally defaulting to `false` to allow users to configure this for their situation

I would be happy to support a PR with documentation and/or testing but have zero rust experience and the code-base is huge (I'd need too many pointers on how to add the config option)

---

_Comment by @MichaReiser on 2024-02-22 20:10_

Hey @MusicalNinjaRandInt 

We're glad that you're having a good time with ruff! 

What's the error that you see in your CI pipeline? I'm asking because I'm confused that you see an error. After all, we **aren't** running a rule. Or is it that ruff didn't complain locally but it does when running on CI?

What's your file system layout? Like are the files stored in WSL or on your windows partition?

---

_Comment by @MusicalNinjaDad on 2024-02-22 20:51_

Hi @MichaReiser , thanks. Yes, the issue was that I had no complaints when running locally but did in the CI. And the CI was correct - I had a file with 0755 permissions and no shebang.

The files on my local setup are in the WSL filesystem (ext4). This aligns with the recommendations from MS - there is a huge performance and functionality difference vs. mounting the windows system

---

_Comment by @MichaReiser on 2024-02-22 21:08_

Thanks, that clarifies things. I wonder if there's a way to detect if a file is stored inside WSL or on the Windows disk. 

---

_Comment by @MusicalNinjaDad on 2024-02-24 11:29_

Sorry, @MichaReiser , just saw your comment now.
Maybe the presence or response of https://docs.rs/filesystem/latest/filesystem/trait.UnixFileSystem.html could be used? Then the implementation wouldn't be Windows / Linux / WSL / non-WSL / etc... but based on the FS capabilities, which is probably more future-proof and clear.
The question is - how does this trait respond on a Windows filesystem?

---

_Comment by @mikeleppane on 2024-02-24 12:29_

@MusicalNinjaRandInt I've also had the same issue when running tests locally. I also use WSL and files are stored in WSL file system. Please fix this! A while back, I originally reported this in one PR but it did not get any attention. 

> Suggestion:
Add a configuration option lint.ignore-exe-wsl; ideally defaulting to false to allow users to configure this for their situation

Is this the way to tackle this?



---

_Comment by @mikeleppane on 2024-02-24 12:34_

```bash
failures:

---- rules::flake8_executable::tests::rules::path_new_exe002_1_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━Snapshot file: crates/ruff_linter/src/rules/flake8_executable/snapshots/ruff_linter__rules__flake8_executable__tests__EXE002_1.py.snapSnapshot: EXE002_1.py
Source: crates/ruff_linter/src/rules/flake8_executable/mod.rs:43
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────-old snapshot
+new results
────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────    0       │-EXE002_1.py:1:1: EXE002 The file is executable but no shebang is present
    1       │-  |
    2       │-1 | if __name__ == '__main__':
    3       │-  |  EXE002
    4       │-2 |     print('I should be executable.')
    5       │-  |
────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'rules::flake8_executable::tests::rules::path_new_exe002_1_py_expects' panicked at /home/mikko/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.34.0/src/runtime.rs:563:9:
snapshot assertion for 'EXE002_1.py' failed in line 43
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- rules::flake8_executable::tests::rules::path_new_exe001_1_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━Snapshot file: crates/ruff_linter/src/rules/flake8_executable/snapshots/ruff_linter__rules__flake8_executable__tests__EXE001_1.py.snapSnapshot: EXE001_1.py
Source: crates/ruff_linter/src/rules/flake8_executable/mod.rs:43
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────-old snapshot
+new results
────────────┬─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────    0       │-EXE001_1.py:1:1: EXE001 Shebang is present but file is not executable␊
    1       │-  |␊
    2       │-1 | #!/usr/bin/python␊
    3       │-  | ^^^^^^^^^^^^^^^^^ EXE001␊
    4       │-2 | ␊
    5       │-3 | if __name__ == '__main__':␊
    6       │-  |␊
────────────┴─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────To update snapshots run `cargo insta review`
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
thread 'rules::flake8_executable::tests::rules::path_new_exe001_1_py_expects' panicked at /home/mikko/.cargo/registry/src/index.crates.io-6f17d22bba15001f/insta-1.34.0/src/runtime.rs:563:9:
snapshot assertion for 'EXE001_1.py' failed in line 43
```

---

_Comment by @MusicalNinjaDad on 2024-02-24 13:19_

Thinking about this some more @MichaReiser and reviewing #5735 & #5445 it looks like a WSL "Feature" that fakes *nix fs APIs for NTFS systems - so no way to programatically check. Files on an NTFS filesystem mounted on WSL will have chmod & chown capabilities and automatically have a+x permissions.
I guess that the same issue would happen on any OS if someone were storing their files on a windows filesystem via a CIFS mount?
For info here is the result of `df -F` on my WSL setup:
```
Filesystem     Type           1K-blocks      Used Available Use% Mounted on
none           tmpfs            2007028         4   2007024   1% /mnt/wsl
none           9p             247766012 219503880  28262132  89% /usr/lib/wsl/drivers
/dev/sdc       ext4          1055762868   6064324 995995072   1% /
none           tmpfs            2007028       216   2006812   1% /mnt/wslg
none           overlay          2007028         0   2007028   0% /usr/lib/wsl/lib
rootfs         rootfs           2003728      2060   2001668   1% /init
none           tmpfs            2007028       832   2006196   1% /run
none           tmpfs            2007028         0   2007028   0% /run/lock
none           tmpfs            2007028         0   2007028   0% /run/shm
none           overlay          2007028        76   2006952   1% /mnt/wslg/versions.txt
none           overlay          2007028        76   2006952   1% /mnt/wslg/doc
C:\            9p             247766012 219503880  28262132  89% /mnt/c
snapfuse       fuse.snapfuse        128       128         0 100% /snap/bare/5
snapfuse       fuse.snapfuse      75904     75904         0 100% /snap/core22/1033
snapfuse       fuse.snapfuse      93952     93952         0 100% /snap/gtk-common-themes/1535
snapfuse       fuse.snapfuse      75776     75776         0 100% /snap/core22/864
snapfuse       fuse.snapfuse      41856     41856         0 100% /snap/snapd/20290
snapfuse       fuse.snapfuse      41472     41472         0 100% /snap/snapd/20671
snapfuse       fuse.snapfuse     134272    134272         0 100% /snap/ubuntu-desktop-installer/1276
snapfuse       fuse.snapfuse     134144    134144         0 100% /snap/ubuntu-desktop-installer/1284
```
The main filesystem is:
```
/dev/sdc       ext4          1055762868   6064324 995995072   1% /
```
A mounted NTFS drive:
```
C:\            9p             247766012 219503880  28262132  89% /mnt/c
```
What I don't know is if there is any way to identify the filesystem type that a file is stored on. I couldn't find anything obvious in the std docs or via a quick google search.

Which probably loops back to needing to add a config option and extend the fail message to hint at the config if EXE001/EXE002 fail on a WSL system(?)

---

_Comment by @MichaReiser on 2024-02-24 13:25_

> A while back, I originally reported this in one PR but it did not get any attention.

I'm sorry for this. That was not intentional. I'm happy to take a look now.


> Is this the way to tackle this?

I prefer a solution that doesn't rely on said configuration because it is a setup/machine-specific and not a project-specific setting. What I mean by that is that the setting value needs to be different for different people working on the same project because some prefer working in WSL but store their project files on the Windows partition (not advice, but they might have reasons) and others work in WSL but store their files on the UNIX partition. 

@MusicalNinjaRandInt, thanks for the detailed research! It is interesting that NTFS-mounted partitions automatically have execution permissions. I wonder if we can use this as a way to detect if we're on WSL or not. I don't have WSL setup myself, so it's a bit hard for me to experiment but what we could try is:

* if on WSL
* For the first file only (use `Lazy` or a `OnceCell`?)
* Create a temp file (there's a `tempfile_in` function) in the project root (should be stored in the settings)
* check if the temp has executable permissions. If so, it must be a directory on a NTFS directory

A good start would be to use the shell to verify that the above approach works to identify NTFS or Unix partitions, a second step is then to change the implementation. 

Creating a tempfile is a small perf hint but it should be neglectable in comparison to the rule's logic that tests the permission of every file. 




---

_Comment by @MusicalNinjaDad on 2024-02-24 14:31_

Here's the output from the ext4 filesystem mount:
```console
mike@MiBook:~$ mkdir tmp
mike@MiBook:~$ cd tmp/
mike@MiBook:~/tmp$ touch testfile
mike@MiBook:~/tmp$ ll
total 8
drwxr-xr-x  2 mike mike 4096 Feb 24 15:25 .
drwxr-x--- 20 mike mike 4096 Feb 24 15:25 ..
-rw-r--r--  1 mike mike    0 Feb 24 15:25 testfile
mike@MiBook:~/tmp$ 
```

and from the NTFS mount:
```console
mike@MiBook:/mnt/c/Users/iammi$ mkdir tmp
mike@MiBook:/mnt/c/Users/iammi$ cd tmp/
mike@MiBook:/mnt/c/Users/iammi/tmp$ touch testfile
mike@MiBook:/mnt/c/Users/iammi/tmp$ ll
total 0
drwxrwxrwx 1 mike mike 4096 Feb 24 15:27 .
drwxrwxrwx 1 mike mike 4096 Feb 24 15:27 ..
-rwxrwxrwx 1 mike mike    0 Feb 24 15:27 testfile
```

Looks like a good call @MichaReiser

---

_Comment by @MusicalNinjaDad on 2024-02-24 14:42_

> * Create a temp file (there's a `tempfile_in` function) in the project root (should be stored in the settings)

Maybe better to use the .ruff_cache directory to avoid any IDE-git triggers. In fact, it should be possible to check the permissions of (one of) the files in there - they will (should?) only be executable if *everything* is executable - which is what you'd actually want to test for

---

_Comment by @MichaReiser on 2024-02-24 17:37_

That's an interesting thought. We could create a file in the `.ruff_cache` directory once and reuse that one instead of re-creating a tempfile every time ruff runs. The one issue I see with using `.ruff_cache` is that it isn't guaranteed to be the same directory as your project files, e.g. you can have your projects in `/mnt/c/Users/micha/tmp` but the cache points to `/tmp/ruff_cache` in which case the test result isn't reliable. 



---

_Label `rule` added by @MichaReiser on 2024-02-24 17:37_

---

_Comment by @MusicalNinjaDad on 2024-02-25 15:16_

I guess the question will be a look at performance tradeoff between:

1. use `.ruff_cache` if on same filesystem:
    - check location of `.ruff_cache` is same filesystem as project files (is this possible in rust?)
    - if same system: check if test_file exists in cache (create if it doesn't) & whether it has executable bit set; else: create temporary file in project root, check the bit then remove it
1. use `.ruff_cache` if in same path
    - check location of `.ruff_cache` related to project files
    - if cache is under project root: check if test_file exists in cache (create if it doesn't) & whether it has executable bit set; ; else: create temporary file in project root, check the bit then remove it
1. just use a temporary file in the project root every time

That probably comes down to any additional overhead in creating the temporary file vs using a pre-existing one in relation to the overhead to check the details of the cache. I'd guess the usual case will be "repeat runs" so the caching would pay off very soon if the process is faster once that file exists...

What do you think @MichaReiser ?

---

_Comment by @MichaReiser on 2024-02-25 17:30_

I would go with the temp file for simplicity. I expect the performance overhead for creating the tempfile to be marginal when linting many files where ruff needs to read and test every file's permission. 

---

_Comment by @MusicalNinjaDad on 2024-02-25 18:16_

If you want support with testing or documentation, please let me know (@ tag my new username and I'll get a notification).

---

_Label `help wanted` added by @MichaReiser on 2024-02-25 21:17_

---

_Comment by @vollossy on 2024-05-02 14:41_

Just faced same problem, may be already exists some workaround?

---

_Comment by @MusicalNinjaDad on 2025-04-20 11:54_

I've started taking a look at this ... as a thank-you for getting me interested in rust.

Hope to have a draft PR for review soon.

For now: @vollossy - if you're running in a dev-container then the issue doesn't occur as the container is not detected as WSL (and you surely wouldn't be mad enough to bind-mount a NTFS partition into a dev container and suffer the performance hit ;-) )

---

_Comment by @MusicalNinjaDad on 2025-04-23 15:30_

@MichaReiser  - help given ;) ... Thanks for getting me interested in rust!

---

_Comment by @wu-clan on 2025-11-10 16:06_

Today, I encountered this issue on Windows 11, and I am recording it here.

.pre-commit-config.yaml

```
default_language_version:
    python: '>= 3.10'

repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0
    hooks:
      - id: end-of-file-fixer
      - id: check-json
      - id: check-yaml
      - id: check-toml

  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.14.0
    hooks:
      - id: ruff-check
        args:
          # https://github.com/astral-sh/ruff-pre-commit/issues/64
          - '--fix'
          - '--unsafe-fixes'
      - id: ruff-format

  - repo: https://github.com/astral-sh/uv-pre-commit
    rev: 0.9.0
    hooks:
      - id: uv-lock
      - id: uv-export
        args:
          - '-o'
          - 'requirements.txt'
          - '--no-hashes'
```

Windows:

```
❯ prek run --all-files
fix end of files.........................................................Passed
check json...............................................................Passed
check yaml...............................................................Passed
check toml...............................................................Passed
ruff check...............................................................Passed
ruff format..............................................................Passed
uv-lock..................................................................Passed
uv-export................................................................Passed
```

Linux(Github Actions):

```
Run source .venv/bin/activate
  source .venv/bin/activate
  chmod 755 backend/scripts/lint.sh
  ./backend/scripts/lint.sh
  shell: /usr/bin/bash -e {0}
  env:
    UV_CACHE_DIR: /home/runner/work/_temp/setup-uv-cache
fix end of files.........................................................Passed
check json...............................................................Passed
check yaml...............................................................Passed
check toml...............................................................Passed
ruff check...............................................................Failed
- hook id: ruff-check
- exit code: 1
  EXE001 Shebang is present but file is not executable
   --> backend/app/admin/utils/__init__.py:1:1
    |
  1 | #!/usr/bin/env python3
    | ^^^^^^^^^^^^^^^^^^^^^^
    |

  Found 1 error.
  warning: Invalid `# noqa` directive on backend/plugin/tools.py:318: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
  warning: Invalid `# noqa` directive on backend/plugin/tools.py:330: expected a comma-separated list of codes (e.g., `# noqa: F401, F841`).
ruff format..............................................................Passed
uv-lock..................................................................Passed
uv-export................................................................Passed
Error: Process completed with exit code 1.
```

lint.sh

```
#!/usr/bin/env bash

prek run --all-files
```

---

_Comment by @MusicalNinjaDad on 2025-11-12 15:52_

@wu-clan if you would be willing to link to the repo & commit where you ran into this then I'd be grateful. That way I can check that the changes I've proposed would also work in your case. 
Is your local windows run using WSL or natively on windows?

---

_Comment by @wu-clan on 2025-11-13 01:53_

> [@wu-clan](https://github.com/wu-clan) if you would be willing to link to the repo & commit where you ran into this then I'd be grateful. That way I can check that the changes I've proposed would also work in your case. Is your local windows run using WSL or natively on windows?

This is the GitHub record: https://github.com/fastapi-practices/fastapi_best_architecture/actions/runs/19237543091/job/54991387355

I did not use WSL.

---

_Comment by @ntBre on 2025-11-13 15:39_

@wu-clan Ah, that seems slightly different from this issue to me. I assume you're developing on Windows, but the CI runner is using Ubuntu Linux. I think it's correct for the lint to fire in that situation. From a quick search, I think you can set the executable bit with git like this:

```shell
git update-index --chmod=+x path/to/file
```

Windows should ignore it locally, but CI and any Linux users should be happy then.

---

_Comment by @MusicalNinjaDad on 2025-11-16 14:50_

@wu-clan Thanks. As @ntBre says, this is expected behaviour - the lint is impossible on native windows and fired correctly on linux.


I see you fixed by removing the shebang with [d946262](https://github.com/fastapi-practices/fastapi_best_architecture/pull/901/commits/d946262c251ec4c2b07588c01a81911378c3ba0d) which makes sense.

---

_Label `help wanted` removed by @MichaReiser on 2025-11-24 12:13_

---

_Comment by @MusicalNinjaDad on 2026-01-11 09:21_

Overview of currently proposed solutions:

| | WSL user, WSL FS (#10084) | Linux user USB stick (#12941) | WSL user, Win FS (#3110, #5445) |
| -- | -- | -- | -- |
| Status Quo | :x: (false negatives) | :x: (false positives) | :x: (false negatives) |
| #17548 | :white_check_mark: | :white_check_mark: | :white_check_mark: |
| #21724 | :white_check_mark: | :x: (false positives) | :x: (false positives) |

---
