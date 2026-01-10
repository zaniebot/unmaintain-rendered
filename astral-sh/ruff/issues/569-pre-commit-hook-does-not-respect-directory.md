```yaml
number: 569
title: pre-commit hook does not respect directory
type: issue
state: closed
author: brizzbuzz
labels:
  - question
assignees: []
created_at: 2022-11-03T15:26:45Z
updated_at: 2022-11-03T16:26:10Z
url: https://github.com/astral-sh/ruff/issues/569
synced_at: 2026-01-10T15:56:05Z
```

# pre-commit hook does not respect directory

---

_Issue opened by @brizzbuzz on 2022-11-03 15:26_

Very cool tool 🔥 Ran it on a gigantic repo and it crunched through everything in 100 seconds, where as Flake8 would just puke if we tried to lint everything in 1 shot.

Only problem, I'm getting an issue trying to configure it to run as a pre-commit hook :( 

I have a bad feeling that I'm just doing something dumb here, but for the life of me I cannot figure it out, so gonna file it as a bug just in case.  

I have a repo where all my python files are inside a `src` directory in my repo root.  

Running `ruff src` from the root dir works as expected, no errors.  However, with the following pre-commit hook 

```
fail_fast: false
repos:
  - repo: local
    hooks:
      - id: ruff
        name: ruff
        entry: ruff src
        language: system
```

Ruff seems to completely ignore the directory, and all of my files from root downwards get linted. 

```
pre-commit run
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

Found 469 error(s).
.gitignore:1:3: E999 SyntaxError: invalid syntax. Got unexpected token '.'
.pre-commit-config.yaml:2:8: E999 SyntaxError: invalid syntax. Got unexpected token Newline
docker-compose.yml:2:11: E999 SyntaxError: invalid syntax. Got unexpected token Newline
poetry.lock:4:1: E501 Line too long (99 > 88 characters)
poetry.lock:12:31: E999 SyntaxError: invalid syntax. Got unexpected token '='
poetry.lock:16:1: E501 Line too long (175 > 88 characters)
...
```

---

_Comment by @charliermarsh on 2022-11-03 15:28_

Thank you! Taking a look...

---

_Comment by @charliermarsh on 2022-11-03 15:43_

I think the issue here is that ruff is trying to run against _all staged files_. (The way `pre-commit` is designed is that it expects to pass the list of changed files to the underlying tool, rather than always running against a pre-defined directory like `src`.)

That being said, if you change `.pre-commit-config.yaml` to this, it should then only check staged _Python_ files:

```yaml
repos:
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: v0.0.95
    hooks:
      - id: ruff
```

Alternatively, if you want to keep using `repo: local`, you could do this:

```yaml
repos:
  - repo: local
    hooks:
      - id: ruff
        name: ruff
        entry: ruff
        language: python
        "types": [python]
```

There might be a way to _always_ run against all files in `src` (and no other Python files), I'll play around with it.


---

_Comment by @charliermarsh on 2022-11-03 15:43_

Separately: 100 seconds is long! Is this a public repo??


---

_Comment by @charliermarsh on 2022-11-03 15:46_

Oh, I think you could do this to limit to `src`:

```yaml
repos:
  - repo: local
    hooks:
      - id: ruff
        name: ruff
        entry: ruff
        # Limit to files in `src`
        files: src
        language: python
        "types": [python]
```

---

_Label `question` added by @charliermarsh on 2022-11-03 15:59_

---

_Comment by @brizzbuzz on 2022-11-03 16:11_

ah yep looks like that did the trick.  Thanks so much! Will close this out 

> Separately: 100 seconds is long! Is this a public repo??

Nope, private company monolith. ~19000 python files, some of them terrifyingly large.  This was also totally cold, I'm sure that it takes a fraction of that with the cache in place :) 

---

_Closed by @brizzbuzz on 2022-11-03 16:11_

---

_Comment by @charliermarsh on 2022-11-03 16:26_

If you run into other issues, don't hesitate to reach out!

---
