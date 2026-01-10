```yaml
number: 4289
title: "Implement `--extend-fixable` and `--extend-unfixable` variants in CLI"
type: issue
state: closed
author: ljnsn
labels:
  - configuration
assignees: []
created_at: 2023-05-08T19:08:10Z
updated_at: 2023-05-19T02:20:20Z
url: https://github.com/astral-sh/ruff/issues/4289
synced_at: 2026-01-10T11:09:47Z
```

# Implement `--extend-fixable` and `--extend-unfixable` variants in CLI

---

_Issue opened by @ljnsn on 2023-05-08 19:08_

Consider the following ruff config:

```toml
[tool.ruff]
fix = true
unfixable = ["F401", "F841"]
```

F401 and F841 are added as unfixable so that ruff doesn't automatically remove violating lines on file save (it's set up in the editor for autoformat). I do want to enable them in my pre-commit config though. So what I have done is configure the ruff hook like so:

```yaml
      - id: ruff
        name: ruff
        entry: ruff
        args: ["--fixable=F401,F841"]
        require_serial: true
        language: system
        types: [python]
```

However, this makes ruff consider **only** those two codes as fixable. So I thought I could fix it by passing --unfixable=E501, for example, assuming that would override the value set in the ruff config. The doesn't seem to be the case though, ruff still does not fix the errors.

This does not depend on pre-commit btw, the same can be reproduced by just running ruff from CLI. Essentially, I'm looking for a way to overwrite unfixable, without using --isolated, because there is more config than the minimal example above.

Is there a generally recommended way to do this?


---

_Comment by @charliermarsh on 2023-05-08 19:12_

I would've expected `--unfixable=E501` to work. As a hacky workaround, could you try `--fixable=ALL,F401,F841`? If we add `--extend-fixable`, as requested today (hah) in https://github.com/charliermarsh/ruff/discussions/4272, then `--extend-fixable=F401,F841` would also work.


---

_Label `configuration` added by @charliermarsh on 2023-05-08 19:13_

---

_Comment by @ljnsn on 2023-05-08 19:30_

Thanks for the quick answer. It's not a coincidence, #4272 was opened by a colleague of mine, but I didn't know about it until now :smile: 

Your suggested workaround works, it's even sufficient to pass `--fixable=ALL`. An `extend-fixable` would be great though.

---

_Comment by @charliermarsh on 2023-05-08 19:36_

Oh, hah, that's funny :)

I'll keep this open to track `--extend-fixable`, it really should exist to mirror the `--extend-select` variants.


---

_Renamed from "How to overwrite unfixable set in config file?" to "Implement `--extend-fixable` and `--extend-unfixable` variants in CLI" by @charliermarsh on 2023-05-08 19:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-09 01:10_

---

_Closed by @charliermarsh on 2023-05-19 02:20_

---
