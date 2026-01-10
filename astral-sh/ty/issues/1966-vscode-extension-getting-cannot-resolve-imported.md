```yaml
number: 1966
title: "vscode extension -- Getting \"Cannot resolve imported module `myproject.my_module.my_file`\" with a `backend` folder at the root"
type: issue
state: closed
author: ColemanDunn
labels:
  - question
assignees: []
created_at: 2025-12-16T23:48:22Z
updated_at: 2025-12-17T19:55:58Z
url: https://github.com/astral-sh/ty/issues/1966
synced_at: 2026-01-10T01:53:59Z
```

# vscode extension -- Getting "Cannot resolve imported module `myproject.my_module.my_file`" with a `backend` folder at the root

---

_Issue opened by @ColemanDunn on 2025-12-16 23:48_

### Summary

This is less so a bug than it is a feature request, but in my project, I have a `backend` and `frontend` folder. `ty` is unable to resolve the imports likely due to trying to resolve them from the root, but `backend` is the "root" of my python project (that holds my `pyproject.toml` file).

To get import to resolve with the normal python language server in vscode, I had to add the `"python.analysis.extraPaths": ["./backend"]` setting. So I would assume `ty` needs a setting like this to get projects like this to work.

Thank you 

### Version

0.0.2

---

_Renamed from "vscode extension -- Getting `Cannot resolve imported module `myproject.my_module.my_file`` with a `backend` folder at the root" to "vscode extension -- Getting "Cannot resolve imported module `myproject.my_module.my_file`" with a `backend` folder at the root" by @Gankra on 2025-12-16 23:54_

---

_Comment by @Gankra on 2025-12-16 23:56_

I believe you want the settings described in https://docs.astral.sh/ty/modules/

---

_Label `question` added by @MichaReiser on 2025-12-17 08:56_

---

_Comment by @ColemanDunn on 2025-12-17 19:48_

> I believe you want the settings described in https://docs.astral.sh/ty/modules/

Thanks! Would that work for my structure? To clarify it looks like this for example:
```
my_repo
├── backend
│   ├── pyproject.toml
│   ├── uv.lock
│   └── app
├── frontend
└── README.md
```

I tried 
```toml
[tool.ty.environment]
root = ["../backend"]
```

but that didn't work. But I also suspect `ty` is not finding my `pyproject.toml`, and I don't see a setting to specify the path to my `pyproject.toml`, so maybe one needs to be added (at least most extentions/plugins of this nature allow you to set a path to some config file)

Also to clarify I am able to get proper module resolution with the normal python/pyright/pylance stuff in vscode as well as pycharm works just fine. Just can't get `ty` to work in my case

---

_Comment by @ColemanDunn on 2025-12-17 19:55_

Turns out updating fixed it! Related comment: https://github.com/astral-sh/ty/issues/1965#issuecomment-3666911684

Closing and thank you for the help @Gankra!

---

_Closed by @ColemanDunn on 2025-12-17 19:55_

---
