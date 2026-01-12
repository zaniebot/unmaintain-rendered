```yaml
number: 8953
title: "Logging which files were changed when running `ruff format .` ?"
type: issue
state: open
author: thvasilo
labels:
  - cli
  - formatter
assignees: []
created_at: 2023-12-01T17:53:51Z
updated_at: 2025-10-07T19:06:45Z
url: https://github.com/astral-sh/ruff/issues/8953
synced_at: 2026-01-12T15:54:48Z
```

# Logging which files were changed when running `ruff format .` ?

---

_@thvasilo_

Hi team, thanks for the great tool.

One of the small features I appreciated with `black` is that it would list which files were changed when I ran the command, is it possible to have that with ruff as well?

---

_Renamed from "Logging which files where changed when running `ruff format .` ?" to "Logging which files were changed when running `ruff format .` ?" by @thvasilo on 2023-12-01 17:53_

---

_Comment by @T-256 on 2023-12-02 12:48_

There are already `--check` and `--diff` options for formatter:
```shell
> ruff format . --check
Would reformat: demo.py
Would reformat: wgpy.py
2 files would be reformatted
```

if you need unified command for both check and modify files in one command, then we could consider an option to do it:
```shell
> ruff format . --check --force-modify
Would reformat: demo.py
Would reformat: wgpy.py
2 files reformatted
```

---

_Label `cli` added by @charliermarsh on 2023-12-02 13:53_

---

_Comment by @MichaReiser on 2023-12-04 00:27_

> One of the small features I appreciated with black is that it would list which files where changed when I ran the command, is it possible to have that with ruff as well?

May I ask what you use the printed file names? Is it to better understand what the command changed? Would you expect `ruff check --fix` (linter) to show the names of fixed files too? 

The reasons that I'm aware why we aren't printing file names today are: a) printing many names to the console may require more time than ruff linting all your files and b) can be very verbose if many files were changed.

---

_Comment by @thvasilo on 2023-12-05 18:19_

> Is it to better understand what the command changed ?

Yes that would be it. At least for smaller projects usually there's only a few files changed for each commit, with the black pre-commit hook I could see which files were changed, so if there's an option to do it with ruff I'd use it.

> `ruff format . --check --force-modify`

Yup something like that would work.

---

_Comment by @MichaReiser on 2023-12-06 00:26_

I must admit I never used pre-commit myself so happy to learn with you. Can you use `ruff format` without the check inside of your pre-commit. The step should fail if there are any modified files according to @charliermarsh's [comment](https://github.com/astral-sh/ruff-pre-commit/issues/61#issuecomment-1838777145). Any chance that the pre-commit hook lists the changed files in the error message? 

Another option that is supported today is to use `--diff`, but it not only prints the file name but also the diff for each file. Not sure if that's too noisy. 

---

_Label `formatter` added by @AlexWaygood on 2024-04-17 19:20_

---

_Comment by @Dev-iL on 2024-05-02 12:45_

> May I ask what you use the printed file names?

I can share my own reasoning why I need to know which files were changed: when I have commits which modify multiple files and files have some staged as well as unstaged changes, I have no idea which parts of which files I need to stage to make ruff happy.

Therefore, if there was an "auto-stage fixes" option, I wouldn't need to know which files were changed.

---

_Comment by @markbaird on 2024-10-25 19:07_

As another user switching over from `black` I would like to echo the request to show the list of files that were modified. If I commit 10 files, and I all I get is this message from Ruff:
```
 - files were modified by this hook 
Found 1 error (1 fixed, 0 remaining).
```

It is a bit disconcerting not having any indication of which file was modified. 

> The reasons that I'm aware why we aren't printing file names today are: a) printing many names to the console may require more time than ruff linting all your files and b) can be very verbose if many files were changed.

@MichaReiser  I think it's reasonable that this should be a configuration option that is disabled by default, so people would have to opt-in to the extra output if they accept the performance hit and the extra noise.  I tried using `--verbose` but this is  **way** too noisy to be useful. I think something like a `--info` log level would be appropriate.

---

_Comment by @justinTM on 2025-02-28 01:14_

this would be necessary for undo, otherwise the user has to manually specify `--output-format concise`, parse out the files and lines for each file, and undo them somehow using git stashing or whatever. having this built in would be great. all it takes is one accidental repo-wide ruff run and you sped the next few minutes to an hour manually putting everything back the way it was

---

_Comment by @TParcollet on 2025-10-07 13:23_

Hello there, bumping the issue as this is a concern for us as well. We basically need a --check flag that would also apply the changes after showing the list of changed files! Thanks! 

---
