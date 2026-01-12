```yaml
number: 4162
title: Auto-generate links of rule codes to the corresponding row of the rule table
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
assignees: []
created_at: 2023-04-30T16:49:14Z
updated_at: 2023-11-03T12:34:12Z
url: https://github.com/astral-sh/ruff/issues/4162
synced_at: 2026-01-12T15:54:44Z
```

# Auto-generate links of rule codes to the corresponding row of the rule table

---

_@JonathanPlasse_

> > You could also link the individual rules mentioned elsewhere to the links you created. In [`task-tags`](https://beta.ruff.rs/docs/settings/#task-tags) setting, `ERA` and `E501` are mentioned and could link back to the links you added in this PR. They all seem to follow the regex `` \(`[A-Z]{1,3}[0-9]{0,4}`\) ``.
> 
> Thanks for the suggestion @JonathanPlasse. I wonder if doing this automatically might lead to quite a few false positives and, therefore, if it might be better to do this manually in the specific documentation sections.
> 
> An example of a false positive could be if `V101` in the [external settings section](https://beta.ruff.rs/docs/settings/#external) was linked to `https://beta.ruff.rs/docs/rules/#V101`, which is currently not valid a valid rule section. Another example that matches your provided regex is "(`key`)" in the [invalid-envvar-value (PLE1507)](https://beta.ruff.rs/docs/rules/invalid-envvar-value/) rules; this would direct users to `https://beta.ruff.rs/docs/rules/#key`, which is also not currently a valid rule section.

To avoid false positives like `V101` we could check if the rule exists.
``(`key`)`` should not match the regex as it uses lower cases.

This is dependent on #4158 being merged.

_Originally posted by @JonathanPlasse in https://github.com/charliermarsh/ruff/issues/4158#issuecomment-1529063617_
            

---

_Label `documentation` added by @MichaReiser on 2023-05-01 10:10_

---

_Comment by @charliermarsh on 2023-11-03 03:09_

I believe these actually do exist? https://docs.astral.sh/ruff/rules/#F404

---

_Closed by @charliermarsh on 2023-11-03 03:09_

---

_Comment by @JonathanPlasse on 2023-11-03 12:34_

The idea was to automatically create links when rule codes are mentioned (e.g. `` `E501` `` in [settings.line-length](https://docs.astral.sh/ruff/settings/#line-length)).
This is similar to the links automatically created for mentioned settings.

---
