```yaml
number: 20954
title: "Feat: Parse `[tool.uv.workspace].members`/`.exclude` for first-party code"
type: issue
state: closed
author: thejcannon
labels:
  - question
  - uv
assignees: []
created_at: 2025-10-18T11:47:03Z
updated_at: 2025-11-19T09:31:49Z
url: https://github.com/astral-sh/ruff/issues/20954
synced_at: 2026-01-12T15:54:57Z
```

# Feat: Parse `[tool.uv.workspace].members`/`.exclude` for first-party code

---

_@thejcannon_

### Question

This idea isn't new, I know I asked about it just about as soon as workspaces was announced, [this comment from Micha](https://github.com/astral-sh/ruff/issues/16712#issuecomment-2722345752) mentions thinking about monorepo support, and [this comment](https://github.com/astral-sh/ruff/issues/16712#issuecomment-3322734137) suggests specifically this.

# The problem

If you're a very wise individual who is going "all in" on the Astral ecosystem, you're likely using both `ruff` and `uv`. Let's say you're a monorepo-stan so [`uv`'s workspace](https://docs.astral.sh/uv/concepts/projects/workspaces/) call to you. You have your project set up like (somewhat arbitrarily complex for demonstration)

```toml
[tool.uv.workspace]
members = [
    # Most projects belong here
    "src/python/*",

    # Legacy/Infra projects
    "_infra/globbinator",
    "_infra/turbo-cat-wrangler",

    # Somehow Bob managed to put his project
    # in the repo root, and we're too scared to tell him to
    # move it...
    "bob"
]

# Not a "real" package, ignore it
exclude = ["src/python/sandbox"]
```

and you're using `ruff` to sort imports (wisdom indeed), so unfortunately for you the [default heuristic](https://docs.astral.sh/ruff/faq/#how-does-ruff-determine-which-of-my-imports-are-first-party-third-party-etc) doesn't apply here.

The current workarounds have two options:
1. mirror the workspace member globs in `[tool.ruff].src` (if they are compatible, globs can be tricky like that)
2. splat-o all over `[tool.ruff.lint.isort].known_first_party` (not great, since it is verbose, brittle, and it isn't clear it plays with the rest of `ruff` features)

# The solution

Have `ruff` seed `[tool.ruff].src` with `[tool.uv.workspace].members` (if it exists).

- `ruff` is already parsing `pyproject.toml`
- It's just some simple globs
- It would make me rejoice

Like all good software features, I'm sure there's some nasty complexities hanging out under the hood, but optimistically this _does_ seem kinda like an easy no-brainer 😄


### Version

_No response_

---

_Label `question` added by @thejcannon on 2025-10-18 11:47_

---

_Comment by @thejcannon on 2025-10-18 11:51_

(I should see if `ty` already supports `uv` workspaces, and if not I'll open an issue there, link it here, and replace this comment with the URL)

---

_Renamed from "Feat: Parse `[tool.uv.workspace].members` for first-party code" to "Feat: Parse `[tool.uv.workspace].members`/`.exclude` for first-party code" by @thejcannon on 2025-10-18 11:53_

---

_Comment by @MichaReiser on 2025-10-20 07:24_

A better integration story between our tools is very much on our minds and I outlined (internally) a few ideas on how it could be done. Unfortunately, I was kept busy with working on other parts of ty that prevented me to make much progress on it. I hope that I can come back to it soon, once we're further along with ty. 


In short: We probably don't want to parse any uv metadata from pyproject.tomls because any change to the schema (or how uv discovers projects) would then have to be coordinated across all our tools. Instead, a more likely outcome is to have a `uv metadata --version <X>` command that allows other tools (not just Astral tools but any python tool) to query metadata about a workspace and use that to enrich its configuration. 

The benefit of this is that uv can change its project discovery or settings, for as long as it doesn't break the output of the metadata command. The metadata command can also expose more information that would be hard for ruff to compute on its own, like:

* The python version 
* installed dependencies
* package layouts (and which package depend on each other)
* ...


> (I should see if ty already supports uv workspaces, and if not I'll open an issue there, link it here, and replace this comment with the URL)

ty doesn't support uv workspaces yet. But it's on our mind. 

---

_Label `uv` added by @MichaReiser on 2025-10-20 07:25_

---

_Comment by @thejcannon on 2025-10-20 11:02_

A better answer than I was hoping for! ♥️

---

_Closed by @MichaReiser on 2025-11-06 13:34_

---

_Comment by @RmStorm on 2025-11-19 09:21_

Hello, I also think this would be a great feature for uv/Ruff (and other tools I guess, though I'm all in on Astral hahaha).. Can this issue be kept open until the metadata feature is implemented in uv _and_ Ruff correctly consumes the metadata?

The issue is now closed as completed but none of this is currently implemented is it?

---

_Comment by @MichaReiser on 2025-11-19 09:31_

I created https://github.com/astral-sh/ruff/issues/21520

---
