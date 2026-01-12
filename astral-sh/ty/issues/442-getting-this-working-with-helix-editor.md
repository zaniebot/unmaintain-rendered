```yaml
number: 442
title: Getting this working with Helix editor?
type: issue
state: closed
author: IanQS
labels:
  - question
assignees: []
created_at: 2025-05-18T22:12:10Z
updated_at: 2025-06-02T13:41:46Z
url: https://github.com/astral-sh/ty/issues/442
synced_at: 2026-01-12T15:54:23Z
```

# Getting this working with Helix editor?

---

_@IanQS_

### Question

**preface**: I know this is still in alpha, and my existing typechecker is working just fine, but I wanted to give this a shot after attending PyCon 2025 :) This issue isn't urgent

---

I can't seem to get `ty` working with helix. I've managed to get `ruff` working, so I'm now sure what I'm doing wrong. In my usual config I have `ruff` uncommented, and it runs just fine. I've just included the config here for more clarity

```
[[language]]
name = "python"
language-id = "python"
auto-format = true
language-servers = ["ty"] #, "ruff", "scls"]
file-types = ["py", "ipynb"]
comment-token = "#"
shebangs = ["python"]
roots = ["pyproject.toml", "setup.py", "poetry.lock", ".git", ".jj", ".venv/"]

########################################
# ty
########################################
[language-server.ty]
command = "ty"
args = ["server"]

# ########################################
# # https://docs.astral.sh/ruff/settings
# ########################################

[language-server.ruff]
command = "ruff"
args = ["server", "--preview"]
preview = true
show-fixes = true
include = ["*.py"]
fix = true
```




### Version

ty 0.0.1-alpha.5 (3b726d87a 2025-05-17)

---

_Label `question` added by @IanQS on 2025-05-18 22:12_

---

_Comment by @MichaReiser on 2025-05-19 07:11_

Hi. 

I'm not familiar with Helix myself, but this is a heads-up that diagnostics aren't expected to work because [Helix only supports push diagnostics](https://github.com/helix-editor/helix/issues/7757), while ty currently [only supports pull diagnostics](https://github.com/astral-sh/ty/issues/79). 

You can still setup ty to get hover and go-to-type-definition but diagnostics will, unfortunately not work at the moment.

---

_Comment by @dhruvmanila on 2025-05-19 15:45_

As Micha mentioned, it should mainly be that the diagnostics are not showing up in the editor but other supported capabilities should work. I'll close this in favor of #79.

---

_Closed by @dhruvmanila on 2025-05-19 15:45_

---

_Comment by @mxmerz on 2025-06-02 13:41_

For anyone who wants to try this, ty's diagnostics now show up in helix.
This is my `languages.toml` for ruff and ty:

```toml
[language-server.astral-ty]
command = "ty"
args= ["server"]

[[language]]
name = "python"
scope = "source.python"
auto-format = true
language-servers = [
  "ruff",
  "astral-ty",
]
```

---
