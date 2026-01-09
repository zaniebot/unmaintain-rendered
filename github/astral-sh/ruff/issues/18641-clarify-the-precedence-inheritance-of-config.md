---
number: 18641
title: Clarify the precedence/inheritance of config settings in documentation
type: issue
state: open
author: pakal
labels:
  - documentation
assignees: []
created_at: 2025-06-12T10:16:11Z
updated_at: 2025-07-18T13:03:38Z
url: https://github.com/astral-sh/ruff/issues/18641
synced_at: 2026-01-07T13:12:16-06:00
---

# Clarify the precedence/inheritance of config settings in documentation

---

_Issue opened by @pakal on 2025-06-12 10:16_

### Summary

There are unclear/misleading paragraphs in the page regarding ruff configurations :
https://docs.astral.sh/ruff/configuration/

_"If no config file is found in the filesystem hierarchy, Ruff will fall back to using a default configuration. If a user-specific configuration file exists at ${config_dir}/ruff/pyproject.toml, that file will be used instead of the default configuration"_

_"Unlike [ESLint](https://eslint.org/docs/latest/use/configure/configuration-files#cascading-configuration-objects), Ruff does not merge settings across configuration files; instead, the "closest" configuration file is used, and any parent configuration files are ignored. In lieu of this implicit cascade, Ruff supports an [extend](https://docs.astral.sh/ruff/settings/#extend) field, which allows you to inherit the settings from another config file, like so:..."_

These paragraphs assert that the default (implicit) settings are ditched as soon as a configuration file is found; however, ruff seems to, on the contrary, remember default settings (like excluding .venv/.git...), even if ruff settings are added to e.g. pyproject.toml.

And  settings like "extend-include" can be used without "extend = <config-file>", thus demonstrating that default settings are inherited, not ignored.

However, it seems true when an explicit settings file is found, other config files are ignored unless the "extend=..." setting is used to force their inheritance. 
But when "extend" is used, it should be probably be clarified what *"merging"* means, e.g. the fact that "exclude" directives override each other whereas "extend-exclude" values are concatenated.

### Version

0.11.13

---

_Label `documentation` added by @ntBre on 2025-06-12 14:43_

---

_Comment by @MichaReiser on 2025-06-19 14:41_

Thanks for raising this. I find the description fairly accurate. What we could add is a paragraph mentioning that settings left unspecified (in any configuration file) fall-back to their default. Although this is sort of the default of any tool. So I'm not sure if it's worth adding.

---

_Comment by @pakal on 2025-06-20 08:16_

Yes using "default values" is normal in about any tool, that's actually the overall phrasing of this doc page https://docs.astral.sh/ruff/configuration/ which made me believe that, for once, this wasn't the case  ^^

And I felt like it was hinted **thrice** :

"If left unspecified, Ruff's default configuration is equivalent to:" -> Made me believe that once some config is specify, this default configuration "file" disappears
"If a user-specific configuration file exists at ${config_dir}/ruff/pyproject.toml, that file will be used INSTEAD of the default configuration" -> No, it will just override SOME of the default value, the "default configuration" is STILL used.
"Ruff does not merge settings across configuration files" -> Yes it does merge, but only with the default configuration, not user-specified configuration files.

Would you want me to have an attempt at rephrasing the whole first paragraphs, so that it's clear that they just present DEFAULT VALUES of SETTINGS, and that these settings can be overridden setting-by-setting by custom configuration files, and that custom configuration files are not merged together unless explicitly ordered so ?

---

_Comment by @MichaReiser on 2025-06-21 17:13_

> Would you want me to have an attempt at rephrasing the whole first paragraphs, so that it's clear that they just present DEFAULT VALUES of SETTINGS, and that these settings can be overridden setting-by-setting by custom configuration files, and that custom configuration files are not merged together unless explicitly ordered so ?

That would be great. I can't guarantee that we'll merge it but it would at least help starting a discussion :)

---

_Comment by @pakal on 2025-06-29 14:35_

Oki right, I'll use a current project to be sure I understand all about the *actual* algorithm under Ruff settings precedence, and will have a look at a Docs PR in a few weeks

---

_Referenced in [astral-sh/ruff#19236](../../astral-sh/ruff/pulls/19236.md) on 2025-07-09 15:18_

---

_Comment by @pakal on 2025-07-18 13:00_

Oki I've tweaked several lines to include the "default settings as fallback" into the workflow description.

I also added a comment regarding select/extend-select inheritance, as per https://github.com/astral-sh/ruff/issues/18884#issuecomment-3056416228 , since it can be unexpected to newcomers.

( link to the pr - https://github.com/astral-sh/ruff/pull/19236 )

---
