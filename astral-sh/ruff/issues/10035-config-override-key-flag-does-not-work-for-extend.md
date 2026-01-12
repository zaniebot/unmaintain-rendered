```yaml
number: 10035
title: "Config override key flag does not work for `extend` option"
type: issue
state: open
author: ofek
labels:
  - configuration
  - cli
assignees: []
created_at: 2024-02-19T03:37:33Z
updated_at: 2024-03-18T09:14:18Z
url: https://github.com/astral-sh/ruff/issues/10035
synced_at: 2026-01-12T15:54:49Z
```

# Config override key flag does not work for `extend` option

---

_@ofek_

Example command:

```
ruff format --check --diff --config "extend='ruff_defaults.toml'" .
```

---

_Label `configuration` added by @MichaReiser on 2024-02-19 07:19_

---

_Label `cli` added by @MichaReiser on 2024-02-19 07:19_

---

_Comment by @MichaReiser on 2024-02-19 07:27_

This is an interesting usage of the new override system and should be supported. However, It may not have the desired result depending on your use case:

```
pyproject.toml

tests\
	tests.config.toml // extends pyproject.toml
```

Overriding the `extend` would override the `extend` value for all configurations, meaning that `tests.config.toml` would suddenly no longer extend the `pyproject.toml`. Now, that might be exactly what you want but may be unintentional if the intention was to make all configurations inherit from a base configuration.

CC: @AlexWaygood 

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-02-19 17:11_

---

_Comment by @AlexWaygood on 2024-02-19 17:18_

Ah I see -- so, I think this doesn't work because we currently apply the configuration overrides given in the `--config` flag _after_ any configuration options found in any `pyproject.toml`/`ruff.toml` files have been parsed and validated. But this particular override affects which config files ruff should be looking at in the first place, so we'd need to look for it at an earlier stage in the process (I think).

I'm unlikely to have time to look at this in any more depth this week, unfortunately, but I'll work on this as soon as I'm able. Thanks for the report @ofek!

---

_Comment by @ofek on 2024-02-19 17:24_

I'm okay with a fix or literally any other way to define an `extend` via command line! If it means a separate flag then that's cool.

---

_Comment by @AlexWaygood on 2024-02-19 17:28_

Yeah, I'll have a think...

If we decide that this kind of thing _should_ be done via a separate flag, then we should probably explicitly _reject_ trying to override `extend` via `--config`, on the grounds that it's probably not going to do what you expect, as @MichaReiser mentioned above. So whether we go with "fix behaviour of `--config` w.r.t. `extend`" or "introduce another flag", it's likely the behaviour of `--config` should be changed in some way here :)

---

_Comment by @QuentinSoubeyranAqemia on 2024-03-15 14:02_

+1 on this feature
This would be helpful to extend a base configuration with more specific overrides, when the path to the base config is not static and thus cannot be hard-coded into the more-specific configuration.

If that is any indication, when searching the doc I was also expecting a dedicated `--extend` flag, and tried out the `--config` override after failing to find one.
Given that `--config "KEY = VALUE"` has highest priority and thus needs to be applied last, it makes sense to me that `--config` cannot support `--extend` behavior as is currently the case.

I suppose a tricky part could be extend chains: with `extend` in the files, you can only have a linear chain of files extending one another, so the behavior is well-defined. No diamond inheritance.
With both the file passed to `--config`, and `--extend`, possibly extending another file, there can be multiple inheritance. This could reasonably result in an error unless a use-case pops up, though?

---

_Comment by @zanieb on 2024-03-15 17:42_

Just for some more context, can someone explain to me how `--config <path> --config <some-other-override>` differs from `--config extend=<path> --config <some-other-override>`?

---

_Comment by @ofek on 2024-03-15 18:34_

Are you implying that specifying multiple configuration files is implicitly an extend operation? If so, then I suppose there is no feature request here but rather a documentation update.

---

_Comment by @zanieb on 2024-03-15 19:33_

I'm not sure! The configuration hierarchy is confusing and inheritance via extend is even more complicated. I'm asking for someone to try it, I guess, so we can understand what we're missing.

---

_Comment by @ofek on 2024-03-16 21:16_

![image](https://github.com/astral-sh/ruff/assets/9677399/03d70b8b-09c9-4b75-93e6-b697a255ebb5)


---

_Comment by @zanieb on 2024-03-17 03:52_

Oh okay, I think we _can_ support multiple configuration files on the command line. I originally intended for it to be that way, but I think it might have been cut from scope on the initial implementation. I'd be curious to hear if it would address your use-case to support merging multiple `--config` file options. Also @AlexWaygood if you have any more context on why we dropped that from scope that'd be helpful, I can't remember if there were reasons it was infeasible.

---

_Comment by @QuentinSoubeyranAqemia on 2024-03-18 09:14_

If you can have more than one config file, what happens when the `extend`-chain is no longer linear?

```bash
ruff --config config1.toml --config config2.toml
```

```toml
# config1.toml
extend = "config1_base.toml"
```

```toml
# config2.toml
extend = "config2_base.toml"
```

I guess this needs to be properly documented.

---

Also, as a workaround for my own use-case, I template the ruff config and dynamically write a temporary file where I replace a placehold with the actual filepath to `extend`-from, since the CLI itself can't handle it. It's a bit ugly but it works.

---
