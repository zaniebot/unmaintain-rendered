```yaml
number: 18720
title: Ruff seems to detect git repo above (outside) my current git rep
type: issue
state: closed
author: kaddkaka
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-06-17T11:44:20Z
updated_at: 2025-06-19T11:07:59Z
url: https://github.com/astral-sh/ruff/issues/18720
synced_at: 2026-01-10T11:09:58Z
```

# Ruff seems to detect git repo above (outside) my current git rep

---

_Issue opened by @kaddkaka on 2025-06-17 11:44_

### Summary

I run `ruff check -v --fix-only .` inside my repo at `projects/current_project/` and see these DEBUG prints early on:
```
[ignore::gitignore][DEBUG] opened gitignore file: /projects/.git/info/exclude
[ignore::gitignore][DEBUG] opened gitignore file: /projects/current_project/.gitignore
[ignore::gitignore][DEBUG] opened gitignore file: /projects/current_project/.git/info/exclude
```

I did not expect to see the file `/projects/.git/info/exclude` to be picked up. Is this expected behavior or a bug?

### Version

ruff 0.11.3

---

_Comment by @MichaReiser on 2025-06-17 14:24_

I think this is related to https://github.com/astral-sh/ruff/issues/13692 

What's the content of your `.gitignore` file?

---

_Closed by @MichaReiser on 2025-06-17 14:24_

---

_Reopened by @MichaReiser on 2025-06-17 14:24_

---

_Label `question` added by @MichaReiser on 2025-06-17 14:25_

---

_Comment by @kaddkaka on 2025-06-18 12:00_

`/projects/current_project/.gitignore` contains about 100 different patterns, half of them having a wildcard `*`.

Things like
```
*~
*#*
*.lo
*.obj
*.pyc
```

---

_Comment by @MichaReiser on 2025-06-18 12:44_

I'm trying to understand your setup. It seems that `projects` is a git repository and `current_project`. Do you use gitmodules or similar? 


The behavior where the sub-repository also respects the ignores from the outer repository does seem reasonable to me. But @BurntSushi should know better if that's expected (I couldn't find any documentation around it)

---

_Comment by @BurntSushi on 2025-06-18 14:30_

Yeah the documentation in `ignore` isn't great. I believe the relevant settings here are [`WalkBuilder::parents`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.parents) and [`WalkBuilder::require_git`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#method.require_git).

It looks like `parents` is enabled via enabling `standard_filters`:

https://github.com/astral-sh/ruff/blob/913f136d33fbc1b7d20fc91190431ccef2e2834c/crates/ruff_workspace/src/resolver.rs#L481-L495

And I don't see anywhere that `require_git` is set, which means it should be enabled by default:

https://github.com/BurntSushi/ripgrep/blob/cbc598f245f3c157a872b69102653e2e349b6d92/crates/ignore/src/dir.rs#L587-L605

Which means that `ignore` _should_ stop ascending parent directories to read git-related ignore patterns once it sees a `.git`. So this may be a bug in `ignore`? I think I'd need an MRE.

---

_Label `needs-mre` added by @MichaReiser on 2025-06-18 15:44_

---

_Comment by @kaddkaka on 2025-06-18 20:12_

> I'm trying to understand your setup. It seems that `projects` is a git repository and `current_project`. Do you use gitmodules or similar?
> 
> The behavior where the sub-repository also respects the ignores from the outer repository does seem reasonable to me. But [@BurntSushi](https://github.com/BurntSushi) should know better if that's expected (I couldn't find any documentation around it)

Sorry the example is unclear and misleading.

`projects` and `projects/current_project` are two unrelated git repo. 
It's probably just a mistake from my side to one git repo in a subfolder of another one,and I can't just move it out of there. But nonetheless, is the ruff behavior to be considered a bug? 

---

_Comment by @MichaReiser on 2025-06-19 09:48_

> Yeah the documentation in ignore isn't great.

I was actually refering to git's documentation here because that's what `ignore` implements. But I couldn't find a good reference explaining how ignore's are matched other than the syntax-reference (which doesn't go into details on how the ignore files apply if there are multiple or nested repositories)

>  It's probably just a mistake from my side to one git repo in a subfolder of another one,and I can't just move it out of there. But nonetheless, is the ruff behavior to be considered a bug?

Yes, I think this is a bug. But fixing it might be somewhat involved in the `ignore` crate because it needs to track ignore rules by repository (it can no longer just accumulate all found ignore rules from its ancestor)

---

_Comment by @MichaReiser on 2025-06-19 10:01_

Actually, I tried to repro this and I think it works as expected. It's just the log messages that are confusing. 

Ruff (ignore) correctly visits files that are ignored by the parent directory. Ruff (ignore) does read the ignore files from the outer repository. This is somewhat unnecessary but it doesn't lead to any downstream problems. 

@kaddkaka do you see any unexpected ruff behavior besides the logs? Like, does it not check files that it should.

Here's my repro where ruff correctly flags the undeclared variable `a` even though it is ignored in the outer `gitignore` file.

[ignore2.zip](https://github.com/user-attachments/files/20816813/ignore2.zip)

---

_Comment by @kaddkaka on 2025-06-19 10:08_

This is the reproduction steps:
```console
mkdir -p -v bugreport_ruff_git_parent
cd bugreport_ruff_git_parent/
mkdir -p -v another_proj
cd another_proj/
git init
touch pyproject.toml
touch main.py
ruff check -v
```

The second line of the verbose output confused me. It looks like it visits outside the `another_proj` directory:
```
[2025-06-19][12:05:43][ruff::resolve][DEBUG] Using Ruff default settings
[2025-06-19][12:05:43][ignore::gitignore][DEBUG] opened gitignore file: /tmp/bugreport_ruff_git_parent/.git/info/exclude
[2025-06-19][12:05:43][ignore::gitignore][DEBUG] opened gitignore file: /tmp/bugreport_ruff_git_parent/another_proj/.git/info/exclude
[2025-06-19][12:05:43][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/tmp/bugreport_ruff_git_parent/another_proj/.git"
[2025-06-19][12:05:43][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/bugreport_ruff_git_parent/another_proj/pyproject.toml"
[2025-06-19][12:05:43][ignore::gitignore][DEBUG] opened gitignore file: /tmp/bugreport_ruff_git_parent/another_proj/.ruff_cache/.gitignore
[2025-06-19][12:05:43][ruff_workspace::resolver][DEBUG] Included path via `include`: "/tmp/bugreport_ruff_git_parent/another_proj/main.py"
[2025-06-19][12:05:43][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/tmp/bugreport_ruff_git_parent/another_proj/.ruff_cache"
[2025-06-19][12:05:43][ruff::commands::check][DEBUG] Identified files to lint in: 5.527421ms
[2025-06-19][12:05:44][ruff::diagnostics][DEBUG] Checking: /tmp/bugreport_ruff_git_parent/another_proj/pyproject.toml
[2025-06-19][12:05:44][ruff::commands::check][DEBUG] Checked 2 files in: 192.703µs
All checks passed!
```

---

_Comment by @kaddkaka on 2025-06-19 10:12_

I see the same behavior as you @MichaReiser and no actual issue with ignored source files or reporter warning.

But why is ruff traversing files "outside" my project? Does it go all the way up to `/`?

---

_Comment by @MichaReiser on 2025-06-19 10:42_

Yes, ruff traverses upwards from the current directory to find any `ignore` or `.gitignore` file that it should apply. That doesn't mean that it will apply all of them. Ruff also doesn't traverse the entire `/` directory. It only traverses downwards from the project directory. 

<details><summary>Logs when running with diagnostics</summary>

```
[2025-06-19][11:58:46][ruff::resolve][DEBUG] Using configuration file (via parent) at: /Users/micha/astral/test/pyproject.toml
[2025-06-19][11:58:46][ruff_workspace::pyproject][DEBUG] Detected minimum supported `requires-python` version: 3.10
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test/.gitignore
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test/.git/info/exclude
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test/ignore2/.gitignore
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test/ignore2/.git/info/exclude
[2025-06-19][11:58:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/micha/astral/test/ignore2/.git"
[2025-06-19][11:58:46][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test/ignore2/nested-repository/.git/info/exclude
[2025-06-19][11:58:46][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test/ignore2/nested-repository/main.py"
[2025-06-19][11:58:46][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/micha/astral/test/ignore2/nested-repository/.git"
[2025-06-19][11:58:46][ruff::commands::check][DEBUG] Identified files to lint in: 3.677917ms
[2025-06-19][11:58:46][ruff::commands::check][DEBUG] Checked 1 files in: 120.041µs
nested-repository/main.py:1:1: INP001 File `main.py` is part of an implicit namespace package. Add an `__init__.py`.
nested-repository/main.py:1:1: T201 `print` found
```


</details> 

---

_Comment by @kaddkaka on 2025-06-19 10:50_

Downwards or upwards?

In my repro example, isn't `/tmp/bugreport_ruff_git_parent/another_proj/` the project directory? 

---

_Comment by @MichaReiser on 2025-06-19 10:52_

It mainly traverses upwards to find ignore files. It might need to traverse into some sub directories (e.g. `.git`) to find the ignore files. 

Once Ruff has found all ignore files, it traverses downwards.

---

_Closed by @MichaReiser on 2025-06-19 10:52_

---

_Comment by @kaddkaka on 2025-06-19 11:01_

 But when does it stop? Does it it go all the up to `/`? 

---

_Comment by @MichaReiser on 2025-06-19 11:03_

Yes, it goes all the way up to `/` because you could have a `.ignore` file at `/.ignore`

---

_Comment by @kaddkaka on 2025-06-19 11:07_

I see, I thought it would stay inside the project. Thanks. 

---
