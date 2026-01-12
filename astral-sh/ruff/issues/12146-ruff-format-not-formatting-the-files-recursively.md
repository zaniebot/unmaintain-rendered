```yaml
number: 12146
title: "`ruff format` not formatting the files recursively but `ruff format folder/folder1/folder2/folder3` works"
type: issue
state: closed
author: vigneshmanick
labels:
  - question
assignees: []
created_at: 2024-07-02T08:38:51Z
updated_at: 2024-07-04T05:27:54Z
url: https://github.com/astral-sh/ruff/issues/12146
synced_at: 2026-01-12T15:54:51Z
```

# `ruff format` not formatting the files recursively but `ruff format folder/folder1/folder2/folder3` works

---

_@vigneshmanick_

This is a weird issue that i have encountered. I cannot also reproduce it in a similar folder structure outside the current project. I'll try to describe what happens.

I have a python project setup using `pyproject.toml` and the ruff configuration is as follows

```toml

[tool.pytest.ini_options]
testpaths = [
  "testSuite",
]

[tool.ruff]
preview = false

exclude = [
  "src/mymodule/somefolder/anotherfolder",
]

[tool.ruff.lint]
select = ["E", "F", "W", "NPY", "RUF", "I"]
ignore = ["E722", "RUF012", "E111", "E114", "E117", "E501", "W191", "E203"]

[tool.ruff.lint.per-file-ignores]
# Ignore `E402` (import violations) in all `__init__.py` files
"__init__.py" = ["E402"]

```

#### Issue

The files inside `testSuite` are not fomatted when running `ruff format` from the top level folder. 

If i run `ruff format testSuite/path/to/folder/inside/` then it works but only if the path supplied is the last level in the tree. See output below. Note how the last command formats the files but the first 2 don't.

```bash
(myenv) user@node123 project>ruff format .
1001 files left unchanged
(myenv) user@node123 project>ruff format testSuite/testCases/abcutTests/
67 files left unchanged
(myenv) user@node123 project>ruff format testSuite/testCases/abcutTests/HamstrTest/
6 files reformatted, 20 files left unchanged
```

### System info
* ruff version: 0.4.4
* python: 3.11.4
* system: Redhat-7.9

Any pointers here on what exactly i am doing wrong would be helpful. 

Thanks!


---

_Renamed from "`ruff format` command not formatting the files recursively but `ruff format folder/folder1/folder2/folder3` works" to "`ruff format` not formatting the files recursively but `ruff format folder/folder1/folder2/folder3` works" by @vigneshmanick on 2024-07-02 08:39_

---

_Comment by @MichaReiser on 2024-07-02 09:36_

Hmm that sounds strange. Any chance that you have a `.gitignore` or any other ignore file that excludes the `testSuite` directory?

---

_Comment by @vigneshmanick on 2024-07-02 09:41_

there are `.gitignore` files but none of them exclude the `testSuite` directory itself. The existing `.gitignore` only have entries for test generated files that need to be ignored.  

None of the `.py` files inside the `testSuite` folder are in the `.gitignore`

---

_Comment by @vigneshmanick on 2024-07-02 09:55_

thanks for the hint, i think i might have found out what the issue is.

In one `.gitignore` an entry was set to `**/Hamstr*` and i guess this is causing ruff to ignore `HamstrTest` even though the `.py` files inside themselves were not in the `.gitignore`. ?

---

_Comment by @MichaReiser on 2024-07-02 14:15_

> In one .gitignore an entry was set to **/Hamstr* and i guess this is causing ruff to ignore HamstrTest even though the .py files inside themselves were not in the .gitignore. ?

Yes, I think that's correct and should also match Git's behavior, as far as I understand, unless you added the files explicitly (or they were tracked before adding the ignore). We use the [`ignore`](https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#ignore-rules) library that handles this for us.

---

_Comment by @vigneshmanick on 2024-07-02 14:35_

is there a way to see if ruff formatter s ignoring a file due to `gitignore` ? 

---

_Comment by @MichaReiser on 2024-07-02 14:49_

You can try `ruff format -v` but I'm not sure if it logs git ignores. Otherwise, `ruff format --no-respect-gitignore` might do the trick.

---

_Comment by @vigneshmanick on 2024-07-03 12:33_

ok, i have a reproducible example now

#### Folder structure
```
├── src
│   └── ruff_test
│       ├── __init__.py
│       └── classA.py
├── tests
│   └── NewTests
│       ├── __init__.py
│       └── test_func.py
├── .gitattributes
├── .gitignore
└── pyproject.toml
```

#### contents of .gitignore 
```
**/New*/
```

#### Git
force add the `NewTests` folder `git add -f NewTests`

#### Ruff
Now run `ruff -v format .` at the same level as `pyproject.toml` The output is as follows

```
warning: `ruff <path>` is deprecated. Use `ruff check <path>` instead.
[2024-07-03][14:20:25][ruff::resolve][DEBUG] Using Ruff default settings
[2024-07-03][14:20:25][ignore::walk][DEBUG] ignoring /scratch/vm/testDir/ruff_test/tests/NewTests: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/scratch/vm/testDir/ruff_test/.gitignore"), original: "**/New*/", actual: "**/New*", is_whitelist: false, is_only_dir: true })))
[2024-07-03][14:20:25][ruff_workspace::resolver][DEBUG] Included path via `include`: "/scratch/vm/testDir/ruff_test/pyproject.toml"
[2024-07-03][14:20:25][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/scratch/vm/testDir/ruff_test/.ruff_cache"
[2024-07-03][14:20:25][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/scratch/vm/testDir/ruff_test/.git"
[2024-07-03][14:20:25][ruff_workspace::resolver][DEBUG] Included path via `include`: "/scratch/vm/testDir/ruff_test/src/ruff_test/__init__.py"
[2024-07-03][14:20:25][ruff_workspace::resolver][DEBUG] Included path via `include`: "/scratch/vm/testDir/ruff_test/src/ruff_test/classA.py"
[2024-07-03][14:20:25][ruff::commands::check][DEBUG] Identified files to lint in: 5.58252ms
[2024-07-03][14:20:25][ruff::diagnostics][DEBUG] Checking: /scratch/vm/testDir/ruff_test/pyproject.toml
[2024-07-03][14:20:25][ruff::commands::check][DEBUG] Checked 4 files in: 165.711µs
format:1:1: E902 No such file or directory (os error 2)
```

So the `ignore` library is removing the folder `NewTests` at the start and the files within `NewTests` are not shown even though they have been added to the repo. 


---

_Comment by @MichaReiser on 2024-07-03 12:38_

Ruff doesn't check whether a file is added to your git repository. It only interprets your gitignore and determines whether a file should be excluded. 

I recommend you update your git ignore to only ignore files that are not part of your repository. This also makes it less likely that someone forgets to commit a python file that's later added to `NewTest`. 

---

_Comment by @vigneshmanick on 2024-07-03 12:40_

Yup, this is what i did now. Just posting it here so that it's documented on why this is happening.  Someone else might be wondering the same as me in future :)

---

_Closed by @vigneshmanick on 2024-07-03 12:40_

---

_Label `question` added by @dhruvmanila on 2024-07-04 05:27_

---
