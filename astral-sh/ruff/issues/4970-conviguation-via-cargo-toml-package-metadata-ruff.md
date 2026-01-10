```yaml
number: 4970
title: "Conviguation via Cargo.toml (`[package.metadata.ruff]`)"
type: issue
state: closed
author: tgross35
labels:
  - configuration
assignees: []
created_at: 2023-06-08T18:12:33Z
updated_at: 2023-06-12T15:18:05Z
url: https://github.com/astral-sh/ruff/issues/4970
synced_at: 2026-01-10T11:09:47Z
```

# Conviguation via Cargo.toml (`[package.metadata.ruff]`)

---

_Issue opened by @tgross35 on 2023-06-08 18:12_

Just a simple request - would it be possible to check `Cargo.toml` for rust configuration? This would be really cool for Rust projects that have a couple python scripts.

The [metadata table](https://doc.rust-lang.org/cargo/reference/manifest.html#the-metadata-table) can hold anything, so I'm just imagining something like this:

```toml
[package.metadata.ruff]
select = ["E", "F"]
ignore = []
# ... standard ruff config
```

---

_Renamed from "Conviguation via Cargo.toml" to "Conviguation via Cargo.toml (`[package.metadata.ruff]`)" by @tgross35 on 2023-06-08 18:13_

---

_Comment by @tgross35 on 2023-06-08 19:10_

I can probably do this one but am wondering about precedence - how does merging configs work, or config precedence?

---

_Comment by @konstin on 2023-06-09 08:05_

Would [ruff.toml](https://beta.ruff.rs/docs/configuration/#using-rufftoml) also work for you? I see the appeal of reusing an existing toml file for configuration, but adding more possible files for configuration unfortunately makes understanding how ruff gets its configuration harder.

---

_Comment by @tgross35 on 2023-06-09 08:21_

`ruff.toml` is our current solution. It works of course, it would just be nice to be able to avoid the extra file when there are only a few python files and a few lines of config.

Understood about the configuration options proliferation, that's just the opposite side of this problem :)

---

_Comment by @charliermarsh on 2023-06-12 15:18_

I do see the benefit here... but we've been trying hard to avoid adding more configuration sources and file formats, so I'm gonna vote to pass on this for now. We may revisit in the future, it just doesn't seem like a great complexity-to-usefulness tradeoff at the moment.

---

_Closed by @charliermarsh on 2023-06-12 15:18_

---

_Label `configuration` added by @charliermarsh on 2023-06-12 15:18_

---
