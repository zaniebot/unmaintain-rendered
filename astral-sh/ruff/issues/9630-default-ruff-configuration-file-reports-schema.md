```yaml
number: 9630
title: " Default Ruff configuration file reports schema error"
type: issue
state: closed
author: ssteinerx
labels:
  - bug
assignees: []
created_at: 2024-01-23T19:35:43Z
updated_at: 2024-01-24T04:26:04Z
url: https://github.com/astral-sh/ruff/issues/9630
synced_at: 2026-01-10T11:09:51Z
```

#  Default Ruff configuration file reports schema error

---

_Issue opened by @ssteinerx on 2024-01-23 19:35_

[Ruff config](https://docs.astral.sh/ruff/configuration/) gives a .toml file that is equivalent to the default configuration.

[Even Better TOML](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml) reports an error in that .toml file:

{"quote-style":"double","indent-style":"space","skip-magic-trailing-comma":false,"line-ending":"auto","docstring-code-format":false,"docstring-code-line-length":"dynamic"} is not valid under any of the given schemas

I reported this in the .toml validator used by Even Better TOML: [Taplo issue tracker #536](https://github.com/tamasfe/taplo/issues/536) and they tell me the .toml file does not match the schema and that:

>>  this is not a problem in Taplo but in the [Ruff schema](https://github.com/SchemaStore/schemastore/blob/master/src/schemas/json/ruff.json).

and that there's an [issue for this in SchemaStore](https://github.com/SchemaStore/schemastore/issues/2731).

I couldn't find another issue here about it, so here one is.   

---

_Label `bug` added by @charliermarsh on 2024-01-24 02:58_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-24 02:58_

---

_Comment by @charliermarsh on 2024-01-24 02:59_

Thanks -- let me check if the schema is out-of-date. We have to update it manually and it sometimes slips through the cracks during releases.

---

_Comment by @charliermarsh on 2024-01-24 03:04_

The PR is here: https://github.com/SchemaStore/schemastore/pull/3549. But I don't expect any of those to fix the specific error you're reporting, those settings were added long ago. Let me try in VS Code.

---

_Comment by @charliermarsh on 2024-01-24 03:06_

It seems like the `format` section isn't being picked up by the extension. But the schema file _looks_ right to me on our side. Hmm...

---

_Comment by @charliermarsh on 2024-01-24 03:15_

Ah, I see, it's specifically this line:
```toml
# Set the line length limit used when formatting code snippets in
# docstrings.
#
# This only has an effect when the `docstring-code-format` setting is
# enabled.
docstring-code-line-length = "dynamic"
```

---

_Comment by @charliermarsh on 2024-01-24 03:34_

Thanks for reporting this, should be fixed by https://github.com/astral-sh/ruff/pull/9632.

---

_Closed by @charliermarsh on 2024-01-24 04:26_

---
