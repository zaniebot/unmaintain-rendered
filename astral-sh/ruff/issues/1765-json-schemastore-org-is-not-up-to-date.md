```yaml
number: 1765
title: json.schemastore.org is not up to date
type: issue
state: closed
author: JonathanPlasse
labels: []
assignees: []
created_at: 2023-01-10T09:10:37Z
updated_at: 2023-03-16T15:36:21Z
url: https://github.com/astral-sh/ruff/issues/1765
synced_at: 2026-01-10T11:09:43Z
```

# json.schemastore.org is not up to date

---

_Issue opened by @JonathanPlasse on 2023-01-10 09:10_

Hi,
There is `flake8-pytest-style` missing in the schema from [json.schemastore.org](https://json.schemastore.org/ruff.json).

---

_Comment by @JonathanPlasse on 2023-01-10 09:19_

In [schemastore/CONTRIBUTING.md](https://github.com/SchemaStore/schemastore/blob/master/CONTRIBUTING.md) it is said:
> Keep single source of truth. Do not copy an external schema here, but point the catalog to the external schema.

Maybe we should open a pull request to link to [ruff.schema.json](https://raw.githubusercontent.com/charliermarsh/ruff/main/ruff.schema.json) instead.

---

_Comment by @JonathanPlasse on 2023-01-10 11:20_

I went ahead and made a [pull request](https://github.com/SchemaStore/schemastore/pull/2724).

---

_Comment by @charliermarsh on 2023-01-10 12:20_

Unfortunately, the Schema Store doesn't support URLs for embedded schemas. So we can't point it to a URL if we want the schema to appear for `pyproject.toml` files. I initially implemented it as a URL (https://github.com/SchemaStore/schemastore/pull/2673), then intentionally changed it to bundle in the repo to support `pyproject.toml` files (https://github.com/SchemaStore/schemastore/pull/2683). It's an unfortunate limitation, but for now we have to update it periodically.


---

_Comment by @charliermarsh on 2023-01-10 22:09_

We should just be sure to update the schema in the store periodically. It looks like it was updated three days ago to include `flake8-pytest-style`: https://github.com/SchemaStore/schemastore/pull/2718/files.

---

_Closed by @charliermarsh on 2023-01-10 22:09_

---

_Comment by @layday on 2023-01-14 11:27_

The linked commit added the `flake8-pytest-style` codes but not the config section.  There's other codes and sections missing, e.g. `PIE`.  I wonder if updates could be automated somehow?

---

_Comment by @charliermarsh on 2023-01-14 12:10_

I’ll update it today. We could definitely automate it, but I’m worried about spamming the Schema Store repo. I’ll ask in the PR.

---

_Comment by @sasanjac on 2023-03-15 11:19_

> Unfortunately, the Schema Store doesn't support URLs for embedded schemas. So we can't point it to a URL if we want the schema to appear for `pyproject.toml` files

What exactly does not support embedded schemas? Can we not just add something like this to `catalog.json`:
```json
{
  "name": "Ruff",
  "description": "JSON schema for Ruff, a fast Python linter",
  "url": "https://github.com/charliermarsh/ruff/blob/v0.0.256/ruff.schema.json",
  "fileMatch": ["ruff.toml"],
  "versions": {
    "0.0.253": "https://github.com/charliermarsh/ruff/blob/v0.0.253/ruff.schema.json",
    "0.0.254": "https://github.com/charliermarsh/ruff/blob/v0.0.254/ruff.schema.json",
    "0.0.255": "https://github.com/charliermarsh/ruff/blob/v0.0.255/ruff.schema.json",
    "0.0.256": "https://github.com/charliermarsh/ruff/blob/v0.0.256/ruff.schema.json"
  }
}
```

And then update only new versions after their release? That would keep a single source of truth and reduce the amount of work necessary to update the schema as well as reduce the possibility of errors when copying manually.

This approach works in `VSCode`:
- `yaml`: `YAML` extension
- `toml`: `Even Better TOML` extension
- `json`: `JSON Schema Store Catalog` extension

---

_Comment by @charliermarsh on 2023-03-15 21:31_

The issue is that we want autocompletion to work in `pyproject.toml`, which requires that we reference the `ruff.toml` schema in the `pyproject.toml` schema, which in turn requires that both [schemas exist locally in the SchemaStore](https://github.com/SchemaStore/schemastore/blob/master/CONTRIBUTING.md#how-to-ref-from-schema_xjson-to-schema_yjson). We _used_ to link via a URL like that, but then autocompletion would only work in `ruff.toml` files IIRC. Perhaps I'm misunderstanding though.

---

_Comment by @sasanjac on 2023-03-16 09:10_

What tool provides the autocompletion? I have working autocompletion in `pyproject.toml` with `Even Better TOML`. You can test it for example with `starship.toml`. Its schema is also hosted externally, but the it is working fine with autocompletion.

---

_Comment by @charliermarsh on 2023-03-16 15:36_

There are two pieces to this: first, Ruff's own schema in [SchemaStore](https://github.com/SchemaStore/schemastore/blob/c90ae52f68c01de30025adddfe2d46e281a1096a/src/schemas/json/ruff.json#L2); second, the `pyproject.toml` schema in [SchemaStore](https://github.com/SchemaStore/schemastore/blob/c90ae52f68c01de30025adddfe2d46e281a1096a/src/schemas/json/pyproject.json#L503).

Ruff's own schema only applies directly to `ruff.toml` files. The `pyproject.toml` schema applies to `pyproject.toml` files. The `pyproject.toml` file "references" the `ruff.toml` schema via `"$ref": "https://json.schemastore.org/ruff.json"`.

The issue, as I understand it, is that if you want to do this kind of internal linking of schemas, _both_ schema files _have_ to be present in the repo, and _cannot_ be referenced via URL. This works today, because we copy over the schema file to SchemaStore manually. If we try to replace the version in SchemaStore with a URL, tests and checks will fail in SchemaStore, it won't be able to resolve `[tool.ruff]` within `pyproject.toml`.

I'd love to be proven wrong, this is based on my understanding and testing, but it was a while ago. I believe that if we try to replace the `catalog.json` as you've described, CI will fail on SchemaStore (and presumedly it won't work in practice), but I could be mistaken.


---
