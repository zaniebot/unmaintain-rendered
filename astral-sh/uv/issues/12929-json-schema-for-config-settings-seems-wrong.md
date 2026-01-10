---
number: 12929
title: JSON schema for config-settings seems wrong
type: issue
state: closed
author: udifuchs
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-04-17T01:40:24Z
updated_at: 2025-04-17T16:57:54Z
url: https://github.com/astral-sh/uv/issues/12929
synced_at: 2026-01-10T01:25:27Z
---

# JSON schema for config-settings seems wrong

---

_Issue opened by @udifuchs on 2025-04-17 01:40_

There is a mismatch between the documentation for `config-settings` and the JSON schema.
The documentation has this example:
```
[tool.uv]
config-settings = { editable_mode = "compat" }
```
But it fails to validate with uv.schema.json.
On the other hand, the JSON schema does validate this file:
```
[tool.uv]
config-settings = { single_mode = { String = "compat" }, multi_modes = { List = [ "one", "two"] } }
```

---

_Comment by @charliermarsh on 2025-04-17 01:42_

Interesting, thanks -- does seem like a bug.

---

_Label `bug` added by @charliermarsh on 2025-04-17 01:42_

---

_Label `help wanted` added by @charliermarsh on 2025-04-17 01:42_

---

_Comment by @maxmynter on 2025-04-17 09:56_

I wanted to try on this one, but couldn't reproduce the issue on [041c7a5](https://github.com/astral-sh/uv/commit/041c7a5e63d5559ce5571a30e177c072b1b357b2).

Using [`yj`](https://github.com/sclevine/yj) and [`check-jsonschema`](https://github.com/python-jsonschema/check-jsonschema) i get:

```bash
echo '[tool.uv]
config-settings = { editable_mode = "compat" }' > documented-format.toml

yj -t < documented-format.toml > documented-format.json

check-jsonschema documented-format.json --schemafile ../uv.schema.json
# Result: ok -- validation done
```

```bash
echo '[tool.uv]
config-settings = { single_mode = { String = "compat" }, multi_modes = { List = [ "one", "two"] } }' > structured-format.toml

yj -t < structured-format.toml > structured-format.json

check-jsonschema structured-format.json --schemafile ../uv.schema.json
# Result: ok -- validation done
```

Or am i completely misunderstanding `uv`s lifecycle and where the `.toml` is validated against the schema?

---

_Comment by @charliermarsh on 2025-04-17 12:51_

Hmm, I'm not sure why you're seeing a difference in the CLI but I _can_ verify it with Even Better TOML:

![Image](https://github.com/user-attachments/assets/e67dd1da-2567-419f-ad22-c323756c2ab8)
![Image](https://github.com/user-attachments/assets/0ee6a71e-1579-4824-ac81-f70257ef6823)

I think we need to make `ConfigSettingValue` untagged.

---

_Referenced in [astral-sh/uv#12947](../../astral-sh/uv/pulls/12947.md) on 2025-04-17 15:13_

---

_Comment by @maxmynter on 2025-04-17 15:16_

@charliermarsh I could reproduce it with the Even Better Toml extension, too. Weirdly enough, not with the `taplo` CLI on which that extension is based. 

Anyhow, I've proposed a small fix in #12947 that untags the `ConfigSettingValue`. With it, validation in Even Better Toml passes.

---

_Closed by @charliermarsh on 2025-04-17 16:57_

---

_Closed by @charliermarsh on 2025-04-17 16:57_

---
