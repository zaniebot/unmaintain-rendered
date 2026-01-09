---
number: 12986
title: "CLI docs papercut for `--verbose`"
type: issue
state: open
author: edmorley
labels:
  - documentation
assignees: []
created_at: 2025-04-20T11:42:04Z
updated_at: 2025-04-20T19:00:10Z
url: https://github.com/astral-sh/uv/issues/12986
synced_at: 2026-01-07T13:12:18-06:00
---

# CLI docs papercut for `--verbose`

---

_Issue opened by @edmorley on 2025-04-20 11:42_

Many of the uv CLI commands support a `--verbose` arg. 

The docs for those currently say:

> [--verbose](https://docs.astral.sh/uv/reference/cli/#uv-sync--verbose), -v
>
>    Use verbose output.
>
>    You can configure fine-grained logging using the RUST_LOG environment variable. (<https://docs.rs/tracing-subscriber/latest/tracing_subscriber/filter/struct.EnvFilter.html#directives>)

eg:
https://docs.astral.sh/uv/reference/cli/#uv-sync--verbose

However:
1. The URL is being displayed as raw markdown rather than a clickable link
2. It seems like it might be better to link to uv's own `RUST_LOG` env var docs [here](https://docs.astral.sh/uv/configuration/environment/#rust_log) rather than an external Rust crate's docs?

(Sorry I would open a PR for this but my backlog is overflowing at the moment 😅)

---

_Comment by @edmorley on 2025-04-20 11:53_

(Btw I filed this as a blank GitHub issue rather than one of the templates, since it's not really a bug or feature request and there wasn't a "docs feedback" type template category? Though perhaps that's intentional since lower volume and would also overlap with the "Documentation" external URL in the template list?)

---

_Label `documentation` added by @zanieb on 2025-04-20 18:59_

---

_Assigned to @Gankra by @zanieb on 2025-04-20 19:00_

---
