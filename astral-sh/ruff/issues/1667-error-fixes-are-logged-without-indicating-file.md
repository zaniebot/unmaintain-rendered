```yaml
number: 1667
title: Error fixes are logged without indicating file names
type: issue
state: closed
author: devend711
labels:
  - cli
assignees: []
created_at: 2023-01-05T18:25:43Z
updated_at: 2025-03-19T04:16:38Z
url: https://github.com/astral-sh/ruff/issues/1667
synced_at: 2026-01-12T15:54:41Z
```

# Error fixes are logged without indicating file names

---

_@devend711_

Example output from running `ruff` via a git pre-commit hook:

```
ruff.....................................................................Failed
- hook id: ruff
- files were modified by this hook

Found 1 error(s) (1 fixed, 0 remaining).
```

Locally, it's possible to do something like `git status` to see which files were changed as a result of the hook. But when these hooks are run in a CI run for example, seeing the file names in ruff's output would be useful

---

_Comment by @charliermarsh on 2023-01-05 18:48_

Is there any way to enable this in pre-commit?

---

_Comment by @charliermarsh on 2023-01-06 23:16_

Oh, I guess this has nothing to do with pre-commit, since we don't actually log the names of fixed files. I wonder if we should.

---

_Label `cli` added by @charliermarsh on 2023-01-06 23:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-06 23:52_

---

_Closed by @charliermarsh on 2023-01-07 00:51_

---

_Comment by @telamonian on 2024-08-29 23:39_

@charliermarsh I'm still having this same problem. When I run eg `git commit -a` I see:

```
ruff.....................................................................Failed
- hook id: ruff
- files were modified by this hook

Found 1 error (1 fixed, 0 remaining).

ruff-format..............................................................Failed
- hook id: ruff-format
- files were modified by this hook

1 file reformatted, 1 file left unchanged
```

Especially when you try and commit with `git commit -a`, there's currently no way to see what files changed or what the changes actually were. Am I missing something, like how to apply the existing fix you linked above?

---

_Comment by @dhruvmanila on 2024-08-30 08:17_

@telamonian Can you try using the `--show-fixes` flag with the pre-commit hook? It'll show you the files (and rules) that were fixed:
```
Fixed 4 errors:
- src/lsp.py:
    3 × F401 (unused-import)
    1 × COM812 (missing-trailing-comma)

Found 8 errors (4 fixed, 4 remaining).
```

So, I think the following should give you what you want:
```yaml
- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.6.3
  hooks:
    - id: ruff
      args: [ --show-fixes ]
```

---

_Comment by @telamonian on 2025-03-18 04:37_

@dhruvmanila Hey, sorry I never noticed that you actually responded to my original post. Your suggestion of using `--show-fixes` solves part of my problem, but not all of it. I do now see the fixes caused by the `ruff` hook, but then any fixes made by the `ruff-format` hook are still hidden:

```
$ git commit -a
ruff.....................................................................Passed
ruff-format..............................................................Failed
- hook id: ruff-format
- files were modified by this hook

2 files reformatted, 1 file left unchanged
```

Is there an equivalent of `show-fixes` for the `ruff format` command? I did already try just passing `show-fixes`, but it is not recognized. 

---

_Comment by @dhruvmanila on 2025-03-18 05:21_

> Is there an equivalent of `show-fixes` for the `ruff format` command? I did already try just passing `show-fixes`, but it is not recognized.

There's no equivalent to "format AND show diff" on the formatter side but you can do either of those i.e., use `--diff` to get the diff between original and formatter source but that wouldn't change the file content on disk. https://github.com/astral-sh/ruff/issues/8191 seems related.

---

_Comment by @telamonian on 2025-03-19 04:15_

@dhruvmanila Yeah, I already figured out that I can run the `ruff-format` hook twice, the first time with the `--diff` arg. That *sort* of gets me what I wanted. but it's less than ideal, and would certainly be confusing for anyone else working on a project.

I don't think #8191 quite covers it either. I guess I'd want something like:

```
ruff format --diff-and-format
```

that would both print the diff and then also format the relevant files, instead of just printing the diff and exiting the program without making changes like `--diff` does. At the very least the formatter should print the names of any files changes, not just `x files reformatted, y files left unchanged`.

Thanks for bring up #8191, btw. Getting to learn about https://github.com/evilmartians/lefthook made me a bit happy. I've been waiting for someone to come up with a pre-commit alternative for some time now

---
