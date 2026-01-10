---
number: 15792
title: How to uninstall additional deps installed with a tool
type: issue
state: open
author: idnsunset
labels:
  - question
assignees: []
created_at: 2025-09-11T14:41:30Z
updated_at: 2025-09-12T15:57:43Z
url: https://github.com/astral-sh/uv/issues/15792
synced_at: 2026-01-10T01:25:59Z
---

# How to uninstall additional deps installed with a tool

---

_Issue opened by @idnsunset on 2025-09-11 14:41_

### Question

Is there any command and options than I can use to uninstall the additional dependencies installed with a tool via the `--with` option for the `uv tool` command?

For example, I installed mkdocs using `uv tool install --with=mkdocs-with-pdf mkdocs`, now I want to use mkdocs-exporter instead. I know I can run `uv tool install --with=mkdocs-exporter mkdocs` to install it, but I also want to uninstall mkdocs-with-pdf.

### Platform

Fedora 42 amd64

### Version

uv 0.8.17

---

_Label `question` added by @idnsunset on 2025-09-11 14:41_

---

_Comment by @zanieb on 2025-09-11 15:35_

We'll already remove the other dependencies 

```
❯ uv tool install --with=mkdocs-with-pdf mkdocs
Resolved 34 packages in 232ms
Prepared 22 packages in 2.01s
Installed 34 packages in 41ms
 + beautifulsoup4==4.13.5
 + brotli==1.1.0
 + cffi==2.0.0
 + click==8.2.1
 + cssselect2==0.8.0
 + fonttools==4.59.2
 + ghp-import==2.1.0
 + jinja2==3.1.6
 + libsass==0.23.0
 + markdown==3.9
 + markupsafe==3.0.2
 + mergedeep==1.3.4
 + mkdocs==1.6.1
 + mkdocs-get-deps==0.2.0
 + mkdocs-with-pdf==0.9.3
 + packaging==25.0
 + pathspec==0.12.1
 + pillow==11.3.0
 + platformdirs==4.4.0
 + pycparser==2.23
 + pydyf==0.11.0
 + pyphen==0.17.2
 + python-dateutil==2.9.0.post0
 + pyyaml==6.0.2
 + pyyaml-env-tag==1.1
 + six==1.17.0
 + soupsieve==2.8
 + tinycss2==1.4.0
 + tinyhtml5==2.0.0
 + typing-extensions==4.15.0
 + watchdog==6.0.0
 + weasyprint==66.0
 + webencodings==0.5.1
 + zopfli==0.2.3.post1
Installed 1 executable: mkdocs
❯ uv tool install --with=mkdocs-exporter mkdocs
Resolved 26 packages in 138ms
Prepared 5 packages in 6.72s
Uninstalled 14 packages in 59ms
Installed 6 packages in 12ms
 - brotli==1.1.0
 - cffi==2.0.0
 - cssselect2==0.8.0
 - fonttools==4.59.2
 + greenlet==3.2.4
 + lxml==6.0.1
 + mkdocs-exporter==6.2.0
 - mkdocs-with-pdf==0.9.3
 - pillow==11.3.0
 + playwright==1.55.0
 - pycparser==2.23
 - pydyf==0.11.0
 + pyee==13.0.0
 + pypdf==6.0.0
 - pyphen==0.17.2
 - tinycss2==1.4.0
 - tinyhtml5==2.0.0
 - weasyprint==66.0
 - webencodings==0.5.1
 - zopfli==0.2.3.post1
Installed 1 executable: mkdocs
```

---

_Comment by @idnsunset on 2025-09-12 15:57_

There seems a problem that I have to supply all the additional dependencies for just changing one dependency. For example, if I first run `uv tool install mkdcos --with=mkdocs-material,mkdocs-with-pdf`, then I run `vu tool install mkdocs --with=mkdocs-exporter`, it will removes `mkdocs-material` as well. You have to supply both `mkdocs-material` and `mkdocs-exporter` to the `--with` option.  I can give all the already installed dependencies again for sure, but it is a bit of cumbersome. 

---
