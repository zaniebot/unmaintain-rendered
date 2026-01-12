```yaml
number: 2261
title: Feedback from a flake8 user
type: issue
state: closed
author: alexprengere
labels: []
assignees: []
created_at: 2023-01-27T15:13:45Z
updated_at: 2023-02-03T13:34:46Z
url: https://github.com/astral-sh/ruff/issues/2261
synced_at: 2026-01-12T15:54:42Z
```

# Feedback from a flake8 user

---

_@alexprengere_

I just spent a couple of hours testing ruff (0.0.236), replacing my flake8 config.
It was a very pleasant experience! _flake8-to-ruff_ was great and the transition went smoothly.
Here are a few differences that could make it even better IMO:

* flake8 has a default config file that can be read from `~/.config/flake8`, I think it would be great if ruff could default to looking for `~/.config/ruff.toml` when *no config file is found*. I tend to use a global unique config for all my projects (+IDE config), and this would save the extra `--config ~/.config/ruff.toml` flag to pass when calling ruff
* flake8 can read its config from `tox.ini`, and I think it would benefit ruff as well. pip still has different behaviour based on the presence of `pyproject.toml`, so for some projects introducing this file can be problematic. Adding a `ruff.toml` can be a downside when there is already a `tox.ini` that centralizes tooling config and execution
* finally, here is the list of plugins I frequently use but could not find in the docs, but frankly I would not bother too much, as the current state of rules is truly impressive
   * [flake8-async](https://pypi.org/project/flake8-async/)
   * [flake8-warnings](https://pypi.org/project/flake8-warnings/)
   * [flake8-broken-line](https://pypi.org/project/flake8-broken-line/)
   * [flake8-printf-formatting](https://pypi.org/project/flake8-printf-formatting/)
   * [flake8-markdown](https://pypi.org/project/flake8-markdown/)
   * [flake8-rst](https://pypi.org/project/flake8-rst/)

If you feel my first two points could be interesting I can file separate issues.

---

_Comment by @charliermarsh on 2023-01-27 15:38_

Thank you for this!

> flake8 has a default config file that can be read from ~/.config/flake8, I think it would be great if ruff could default to looking for ~/.config/ruff.toml when no config file is found. I tend to use a global unique config for all my projects (+IDE config), and this would save the extra --config ~/.config/ruff.toml flag to pass when calling ruff

We do support this, I think, but it's not well-documented -- in fact, it may not be documented at all! If we can't find a config, we'll respect a `ruff.toml` at `${sys::config_dir()}/ruff/ruff.toml`. E.g., if you're on Linux, it's `/home/alice/.config/ruff/ruff.toml`:

```rust
/// Returns the path to the user's config directory.
///
/// The returned value depends on the operating system and is either a `Some`, containing a value from the following table, or a `None`.
///
/// |Platform | Value                                 | Example                                  |
/// | ------- | ------------------------------------- | ---------------------------------------- |
/// | Linux   | `$XDG_CONFIG_HOME` or `$HOME`/.config | /home/alice/.config                      |
/// | macOS   | `$HOME`/Library/Application Support   | /Users/Alice/Library/Application Support |
/// | Windows | `{FOLDERID_RoamingAppData}`           | C:\Users\Alice\AppData\Roaming           |
pub fn config_dir() -> Option<PathBuf> {
    sys::config_dir()
}
```


---

_Comment by @charliermarsh on 2023-01-27 17:21_

(I'll add this to the docs later today.)

---

_Comment by @alexprengere on 2023-01-27 17:31_

I tested this and it works perfectly, thanks. Indeed a couple of lines in the docs should be enough 😉 
What are your thoughts on `tox.ini` based config?

---

_Comment by @Jackenmen on 2023-01-27 20:38_

I think that having a centralized pyproject.toml *and* ruff.toml is already enough and tox.ini seems like a rather bad place for putting non-tox config even if some tools decided to put their config there (same as with some tools that decided to put their config in setup.cfg). Additionally, it would require supporting another *file format* since it's not a TOML file.

---

_Comment by @charliermarsh on 2023-01-27 22:59_

Yeah unfortunately we're unlikely to support `tox.ini`. I totally understand why it would be convenient, but I really want to avoid supporting another file format for now.

---

_Closed by @charliermarsh on 2023-01-28 00:25_

---

_Comment by @alexprengere on 2023-02-03 13:34_

After thinking a bit more about this, I believe not supporting `tox.ini` is indeed the right choice.
Out of all the tools I use, `flake8` was the only one that did not have support for `pyproject.toml` config.
So in the end, I could [migrate](https://github.com/alexprengere/currencyconverter/commit/a61c90d25ad3ace9a4613326bf215a941997b752) all the config to `pyproject.toml` (Black, pytest, coverage, ... and now ruff), in most of my projects.
This makes even more sense now that [PEP 621](https://peps.python.org/pep-0621/) was implemented and landed in setuptools 61+, as we will be able to get rid of `setup.cfg` and `setup.py` as well.

---
