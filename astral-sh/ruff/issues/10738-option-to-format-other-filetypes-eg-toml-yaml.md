```yaml
number: 10738
title: "option to format other filetypes (eg. `toml`, `yaml`, `json`, `md`, etc.)"
type: issue
state: open
author: DetachHead
labels:
  - formatter
  - wish
assignees: []
created_at: 2024-04-02T13:31:50Z
updated_at: 2025-10-21T09:24:09Z
url: https://github.com/astral-sh/ruff/issues/10738
synced_at: 2026-01-12T15:54:50Z
```

# option to format other filetypes (eg. `toml`, `yaml`, `json`, `md`, etc.)

---

_@DetachHead_

it would be nice if ruff could format `toml`, `yaml` and `md` files.

i get that this may be considered out of scope for ruff, however in the nodejs ecosystem, the prettier formatter works on these filetypes in addition to js & ts files, which is useful because most projects contain files written in those languages.

---

_Comment by @KotlinIsland on 2024-04-02 13:33_

you can use [dprint](https://dprint.dev/) with dprint-py: https://pypi.org/project/dprint-py/ 🚀🚀🚀

also, [prek 🚀](https://github.com/j178/prek) and [lefthook](https://github.com/evilmartians/lefthook) are really powerful next generation git-hook managers

---

_Comment by @MichaReiser on 2024-04-02 13:36_

This is related to https://github.com/astral-sh/ruff/issues/9244

Yeah, that's something I want to do eventually and we have the infrastructure for it (`ruff_formatter` is entirely language agnostic). But there are some things we have to figure out (what parsers to use or implement our own? How to integrate it into existing commands) and implementing additional language is a large commitment 

---

_Label `formatter` added by @MichaReiser on 2024-04-02 13:36_

---

_Label `wish` added by @MichaReiser on 2024-04-02 13:36_

---

_Comment by @KotlinIsland on 2024-04-02 22:49_

I'm serious about [dprint](https://dprint.dev/), I would imagine it would be a massive amount of work, when dprint is doing exactly what you are setting out to achieve.

---

_Comment by @MichaReiser on 2024-04-03 06:14_

Integrating dprint as an optional formatter (that we call into when the project has installed it) is an option. But I'm unsure if that's the experience users seek. We need to do some designing around this and decide if we want to build out support for new languages ourselves or delegate to other formatters (even Prettier)

---

_Comment by @KotlinIsland on 2024-05-29 00:26_

Ruffs formatter is already integrated into dprint https://github.com/dprint/dprint-plugin-ruff 🚀

---

_Comment by @DetachHead on 2024-05-29 01:03_

dprint also doesn't have a yaml formatter

---

_Comment by @Avasam on 2024-05-29 01:40_

> dprint also doesn't have a yaml formatter

I mean *technically* it has prettier as a process plugin. Which you can configure to run only on yaml.
As much as I dislike prettier for JS/TS, it does just fine for markup/static languages.

Edit: dprint now has a plugin that wraps Pretty YAML: https://dprint.dev/plugins/pretty_yaml/

There's also pre-commit's https://github.com/macisamuele/language-formatters-pre-commit-hooks which satisfies all my toml, yaml, ini, json (haven't tried the others) needs.

dprint also still doesn't work through pre-commit's free version if you have an online shared config.

Not saying this to discourage the OP's request. I too would like if Ruff handled common python config format (.ini for historical reasons for a bunch of tools, and toml). Just offering some options in the short term.

---

_Comment by @inoa-jboliveira on 2024-12-27 17:27_

Again I failed my github search and here I am with a duplicate. 

Addressing the post 1st comment, toml file for me is almost nobrainer. It is essential part of python ecosystem including heavy usage in uv and ruff as configuration. For the other you have many different tools in pre-commit, prettier and so on. Also JSON is such an universal format that you can build your formatter in a couple minutes or ask chatgpt for one.

I would pick a format for toml file that is really just a cleanup with not many opinions (remove blank lines, extra or missing spaces, lines too long). For example it should not try to convert an object into a block or vice versa. Nor it should try to reorganize blocks or sort arrays. 

If any of those features ever become needed they should be their own separated issue.


---

_Comment by @KotlinIsland on 2024-12-28 09:08_

> I mean _technically_ it has prettier as a process plugin

they have a yaml plugin now 🚀
https://dprint.dev/plugins/pretty_yaml/

---

_Comment by @Avasam on 2024-12-28 17:48_

> > I mean _technically_ it has prettier as a process plugin
> 
> they have a yaml plugin now 🚀 [dprint.dev/plugins/pretty_yaml](https://dprint.dev/plugins/pretty_yaml/)

You beat me to it updating my message 😄  (I've updated it anyway). We're using that plugin at work now.

---

_Comment by @ashrub-holvi on 2025-10-21 07:39_

Yep, looks like dprint is a nice tool, but all pre-commit options are not well supported, see https://github.com/dprint/dprint/issues/442, perhaps https://github.com/dprint/dprint/issues/442#issuecomment-3166402560 is supported

---

_Comment by @DetachHead on 2025-10-21 08:51_

imo this feature is no longer necessary now that [dprint can now be installed via pypi](https://pypi.org/project/dprint-py/), making it a viable solution for python projects.

---

_Comment by @MichaReiser on 2025-10-21 08:55_

[Prettier's yaml implementation](https://github.com/prettier/prettier/blob/main/src/language-yaml/index.js) doesn't look too complicated and should be relatively straightforward to port, given that we use a very similar formatter infrastructure. 

The main open questions are:

* What Yaml parser to use (and how much this impacts ruff's binary size)
* How to configure yaml support
* How to make the formatter pick up yaml files in addition to python files

---

_Comment by @ashrub-holvi on 2025-10-21 09:11_

For YAML author of pre-commit [saying](https://github.com/pre-commit/pre-commit-hooks/issues/640#issuecomment-897201069) it's not that actually easy to do it properly

---

_Comment by @KotlinIsland on 2025-10-21 09:20_

> For YAML author of pre-commit [saying](https://github.com/pre-commit/pre-commit-hooks/issues/640#issuecomment-897201069) it's not that actually easy to do it properly

dprint has yaml support https://dprint.dev/plugins/pretty_yaml/

---
