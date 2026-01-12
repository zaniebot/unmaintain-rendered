```yaml
number: 12481
title: "`uv sync` remove custom installed packages"
type: issue
state: closed
author: neolee
labels:
  - question
assignees: []
created_at: 2025-03-26T10:26:16Z
updated_at: 2025-05-09T10:46:29Z
url: https://github.com/astral-sh/uv/issues/12481
synced_at: 2026-01-12T16:01:04Z
```

# `uv sync` remove custom installed packages

---

_@neolee_

### Summary

Sometime we will install custom packages (wheels) in our project. For example I'm trying [spacy](https://github.com/explosion/spaCy) for text tokenization and splitting. This `spacy` requires installing language models (wheels from their repo). Something like:

```shell
.venv ❯ uv run spacy download en_core_web_sm 
Collecting en-core-web-sm==3.8.0
  Downloading https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl (12.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.8/12.8 MB 15.0 MB/s eta 0:00:00
Installing collected packages: en-core-web-sm
Successfully installed en-core-web-sm-3.8.0
✔ Download and installation successful
You can now load the package via spacy.load('en_core_web_sm')

.venv ❯ uv run spacy download zh_core_web_sm 
Collecting zh-core-web-sm==3.8.0
  Downloading https://github.com/explosion/spacy-models/releases/download/zh_core_web_sm-3.8.0/zh_core_web_sm-3.8.0-py3-none-any.whl (48.5 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 48.5/48.5 MB 14.1 MB/s eta 0:00:00
Collecting spacy-pkuseg<2.0.0,>=1.0.0 (from zh-core-web-sm==3.8.0)
  Using cached spacy_pkuseg-1.0.0-cp311-cp311-macosx_15_0_arm64.whl
Requirement already satisfied: numpy<3.0.0,>=2.0.0 in ./.venv/lib/python3.11/site-packages (from spacy-pkuseg<2.0.0,>=1.0.0->zh-core-web-sm==3.8.0) (2.2.4)
Requirement already satisfied: srsly<3.0.0,>=2.3.0 in ./.venv/lib/python3.11/site-packages (from spacy-pkuseg<2.0.0,>=1.0.0->zh-core-web-sm==3.8.0) (2.5.1)
Requirement already satisfied: catalogue<2.1.0,>=2.0.3 in ./.venv/lib/python3.11/site-packages (from srsly<3.0.0,>=2.3.0->spacy-pkuseg<2.0.0,>=1.0.0->zh-core-web-sm==3.8.0) (2.0.10)
Installing collected packages: spacy-pkuseg, zh-core-web-sm
Successfully installed spacy-pkuseg-1.0.0 zh-core-web-sm-3.8.0
✔ Download and installation successful
You can now load the package via spacy.load('zh_core_web_sm')
```

As you can see in the above for English and Chinese language, `spacy` download three wheels and install them in my project virtual env.

Now the problem is, all these packages are not registered in the `pyproject.toml` file. Next time I run `uv sync` they will be removed automatically.

```shell
.venv ❯ uv sync
Resolved 164 packages in 0.35ms
Uninstalled 3 packages in 9ms
 - en-core-web-sm==3.8.0 (from https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl)
 - spacy-pkuseg==1.0.0
 - zh-core-web-sm==3.8.0 (from https://github.com/explosion/spacy-models/releases/download/zh_core_web_sm-3.8.0/zh_core_web_sm-3.8.0-py3-none-any.whl)
```

Any suggestion about this issue?

p.s. I know the `--inexact` opt can prevent `uv` from cleaning unknown packages but if I would like to choose specific extra packages to keep and still want the auto cleaning feature of 'uv', how can I do?

### Platform

macOS 15.3.2 arm64

### Version

uv 0.6.9 (Homebrew 2025-03-20)

### Python version

Python 3.11.11

---

_Label `bug` added by @neolee on 2025-03-26 10:26_

---

_Comment by @charliermarsh on 2025-03-26 12:33_

Unfortunately there’s no support for this beyond the inexact flag.

---

_Label `bug` removed by @charliermarsh on 2025-03-26 12:33_

---

_Label `question` added by @charliermarsh on 2025-03-26 12:33_

---

_Closed by @neolee on 2025-03-26 16:04_

---

_Comment by @ph34tur3 on 2025-05-09 10:46_

@charliermarsh : 
Would it be possible to derive a feature request from this question? 
I would really appreciate if uv could keep track of this or if it was possible to exclude specific packages from clean up while also still supporting automatic removal of unused dependencies. 


---
