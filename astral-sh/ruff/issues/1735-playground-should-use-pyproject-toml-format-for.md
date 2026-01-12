```yaml
number: 1735
title: "Playground should use `pyproject.toml` format for the settings"
type: issue
state: closed
author: not-my-profile
labels:
  - playground
assignees: []
created_at: 2023-01-08T04:40:22Z
updated_at: 2024-10-13T14:17:05Z
url: https://github.com/astral-sh/ruff/issues/1735
synced_at: 2026-01-12T15:54:41Z
```

# Playground should use `pyproject.toml` format for the settings

---

_@not-my-profile_

The [playground](https://ruff.pages.dev/) currently uses JSON for the Settings ... which I don't think makes much sense given that ruff normally is configured via `pyproject.toml`.

So I think it would be better if the Settings tab of the demo was actually named `pyproject.toml` and would contain the contents of a valid `pyproject.toml` file. This way users could simply copy'n'paste between their local config file and the online playground.

---

_Label `playground` added by @charliermarsh on 2023-01-08 20:59_

---

_Comment by @charliermarsh on 2023-01-08 20:59_

Would be nice! Although I'm guessing we'd lose JSON Schema support (since VS Code doesn't support JSON Schema for TOML without an extension), which would be a bummer.

Maybe there's a middle ground, like allowing users to paste in TOML and auto-converting to JSON.

---

_Comment by @not-my-profile on 2023-01-09 03:09_

Ah, I didn't think about JSON schema support. Slightly off-topic: Does the Ruff VS code extension integrate with any other extension that provides JSON schema support for TOML? I just tried [Even Better TOML](https://open-vsx.org/extension/tamasfe/even-better-toml) but it does not recognize `tool.ruff`.

Back to the topic at hand: Yeah I think it's quite unfortunate that Monaco doesn't support extensions (https://github.com/microsoft/monaco-editor/issues/343). Auto-converting doesn't let the user get TOML out of the playground ... we could provide a toggle button to switch between TOML and JSON, but this obviously still isn't ideal. The ideal solution would be to get JSON schema support in Monaco but that's probably not possible until Monaco supports extensions.

---

_Comment by @charliermarsh on 2023-01-09 03:29_

Even Better TOML does work for me:

![Screen Shot 2023-01-08 at 10 28 32 PM](https://user-images.githubusercontent.com/1309177/211236697-2a96f989-8987-4221-b296-e3be8265d856.png)


---

_Comment by @not-my-profile on 2023-01-09 04:11_

Ah my bad, it does work! I was just getting confused by some errors it highlighted but these are just because my `pyproject.toml` file uses recently introduced settings that the VS code extension, that hasn't been updated since, doesn't yet know about. I am using the `ruff.path` setting to use the latest version of ruff with the extension ... would it be possible for the extension to obtain the latest JSON schema from the `ruff` binary?

---

_Comment by @charliermarsh on 2023-01-09 04:22_

I don't _think_ so. I haven't looked into it deeply. Unfortunately we have to update the schema in the [SchemaStore repository](https://github.com/SchemaStore/schemastore) in order to ship updates, which I do from time to time. (You _can_ point SchemaStore to a URL, rather than including the schema in the repo directly, but then we wouldn't be able to autocomplete in `pyproject.toml` files -- anything that's included in _another_ schema has to be bundled directly.)

---

_Comment by @MichaReiser on 2024-10-13 14:17_

You can now paste a TOML configuration into the playground using CTRL-V and it gets converted to JSON. The playground also supports copying the JSON configuration as TOML by right click -> Copy pyproject.toml. 

I looked into using TOML as the configuration format but, unfortunately, Monaco doesn't support TOML auto completion and I couldn't find any plugin. 

---

_Closed by @MichaReiser on 2024-10-13 14:17_

---
