```yaml
number: 202
title: Enable skipping based on .gitignore
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/gitiginore
created_at: 2022-09-15T15:02:45Z
updated_at: 2023-02-03T05:06:14Z
url: https://github.com/astral-sh/ruff/pull/202
synced_at: 2026-01-12T04:51:59Z
```

# Enable skipping based on .gitignore

---

_Pull request opened by @charliermarsh on 2022-09-15 15:02_

This works but it's kind of slow -- it increases the benchmark time from ~300ms to ~420ms which feels like a lot for a nice-to-have feature, so I'd want to make this opt-in.

Alternatively (or in addition): the .gitignore changes infrequently, so I could probably cache the exclusion checks?

Resolves #174.


---

_Comment by @charliermarsh on 2022-09-15 15:02_

(This still needs a CLI argument before it can be merged.)

---

_Comment by @charliermarsh on 2022-09-15 15:03_

\cc @jack1142 

---

_Comment by @Jackenmen on 2022-09-15 15:26_

I pip installed it from the PR's head branch and it doesn't seem to work.

Contents of each Python file:
```py
def func():
    x = 1
```
Configuration:
- `$PROJECT_ROOT/.gitignore`
```
.folder/
should_not_be_here4.py
```
- `$PROJECT_ROOT/.git/info/exclude`:
```
should_not_be_here2.py
```
- `~/.gitignore_global` (I have `core.excludesFile` set to that file):
```
should_not_be_here3.py
```
`$PROJECT_ROOT` file tree:
```
- .folder/
+-- should_not_be_here1.py
- .git/
+-- files and folders
- .ruff_cache/
+-- files and folders
- .venv/
+-- files and folders
- .gitignore
- should_be_here.py
- should_not_be_here2.py
- should_not_be_here3.py
- should_not_be_here4.py
```

The output of `ruff .` (ran in `$PROJECT_ROOT`):
```
❯ ruff . 
Failed to load pyproject.toml: No such file or directory (os error 2)
Falling back to default configuration...
Found 5 error(s).
./.folder/should_not_be_here1.py:2:5: F841 Local variable `x` is assigned to but never used
./should_be_here.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here2.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here3.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here4.py:2:5: F841 Local variable `x` is assigned to but never used
```

`should_not_be_here*` files are the ones that should be ignored while `should_be_here*` shouldn't.

---

_Comment by @Jackenmen on 2022-09-15 15:30_

Update: now that wheels got uploaded by CI, I tried installing from the built wheel instead but that didn't change the result.

---

_Comment by @Jackenmen on 2022-09-15 15:34_

> (This still needs a CLI argument before it can be merged.)

Okay, I just realized that this only means there's no CLI option but the pyproject.toml setting is there :smile: 

But surprisingly, adding this doesn't change the result (aside from ruff no longer complaining about missing pyproject.toml):
```
[tool.ruff]
skip-gitignore = true
```

Output:
```
❯ ruff .
Found 5 error(s).
./.folder/should_not_be_here1.py:2:5: F841 Local variable `x` is assigned to but never used
./should_be_here.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here2.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here3.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here4.py:2:5: F841 Local variable `x` is assigned to but never used
```

---

_Comment by @Jackenmen on 2022-09-15 15:59_

I made #203 which could be a potential cause for me having trouble with getting anything to match here as well.

---

_Comment by @charliermarsh on 2022-09-15 16:15_

The only files I'd expect to be excluded here with current implementation are `should_not_be_here1.py` and `should_not_be_here4.py` -- this is just looking at the local gitignore, and not at the global ignore or `.git/info/exclude`.

---

_Comment by @charliermarsh on 2022-09-15 16:16_

I had the code setup to log-but-not-actually-ignore files so as to avoid affecting the benchmark while testing. Can you try again? If you run with `--verbose`, it should tell you which files are excluded (and why).

---

_Comment by @Jackenmen on 2022-09-15 16:23_

> I had the code setup to log-but-not-actually-ignore files so as to avoid affecting the benchmark while testing. Can you try again? If you run with `--verbose`, it should tell you which files are excluded (and why).

Nothing was logged (tested with -v on both the build with the commit you added and the one before that):
```
❯ ruff -v .        
[2022-09-15][18:19:39][ruff::pyproject][DEBUG] Found pyproject.toml at: "./pyproject.toml"
[2022-09-15][18:19:39][ruff::fs][DEBUG] Ignored path via `exclude`: "./.venv"
[2022-09-15][18:19:39][ruff::fs][DEBUG] Ignored path via `exclude`: "./.git"
[2022-09-15][18:19:39][ruff][DEBUG] Identified files to lint in: 149.729µs
[2022-09-15][18:19:39][ruff][DEBUG] Checked files in: 2.843897ms
Found 5 error(s).
./.folder/should_not_be_here1.py:2:5: F841 Local variable `x` is assigned to but never used
./should_be_here.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here2.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here3.py:2:5: F841 Local variable `x` is assigned to but never used
./should_not_be_here4.py:2:5: F841 Local variable `x` is assigned to but never used
```

> this is just looking at the local gitignore, and not at the global ignore or .git/info/exclude.

On one hand, I get that this would further decrease the performance, on the other, I really would want at least the global gitignore to work because with how I name my venv folders, it's not reasonable to expect the projects I work on to have it in their gitignore. `.git/info/exclude` also has its uses (I use it on one project I regularly work on to be able to put some scripts I use to make my work a bit easier) but I don't feel as strongly about it.

---

_Comment by @Jackenmen on 2022-09-15 16:44_

Actually one more case is adding `.gitignore` in subfolders, e.g.
`another_directory/.gitignore`:
```
*
```
should cause entire `another_directory` to be ignored (which is taken advantage of by various tools).

That reminds me I need to make an issue for adding `.gitignore` with `*` contents to the `.ruff_cache` folder as that's actually what a lot of tools do (mypy, pytest, virtualenv to name a few) :)

---

_Comment by @charliermarsh on 2022-09-15 16:58_

Cool yeah, adding the `.gitignore` is straightforward :)

---

_Comment by @charliermarsh on 2022-09-15 16:59_

I'm probably going to de-prioritize supporting auto-exclusion based on `.gitignore`, at least until I fix + stablize the `skip` globbing behavior, since (once that works) you can always just use skips to achieve the same effect (less conveniently).

---

_Comment by @Jackenmen on 2022-09-15 17:25_

My personal use case is ignoring directories specific to me (especially the virtual environment folders I create where I don't particularly follow conventions as much, as the conventions don't really allow me to have virtual environments for multiple Python version organized neatly) which is why I suggested a more general request for supporting git ignores since it's more likely to be supported by multiple tools.

But personally, I wouldn't mind much just having a user-specific file for configuring excludes but that has the whole other issue of whether the project config should override or extend such configuration (personally I want it to extend *but* it doesn't work well for other settings in a more general case of user configuration rather than user excludes configuration) so I don't think it would work as well as this could.

Either way, de-prioritizing it makes sense to me, it's not particularly critical while ruff is in early adoption phase since it's somewhat unlikely that I would personally work on a project that uses ruff and is not maintained by me (it could happen since you've already reached some big project maintainers but I'm guessing I won't personally encounter an external project I want to contribute to that uses ruff quite yet) and I can put excludes specific to me in configuration for projects *I* maintain :smile: 

---

_Comment by @charliermarsh on 2022-09-21 15:25_

(Closing for now, can always re-open later on.)

---

_Closed by @charliermarsh on 2022-09-21 15:25_

---

_Branch deleted on 2023-02-03 05:06_

---
