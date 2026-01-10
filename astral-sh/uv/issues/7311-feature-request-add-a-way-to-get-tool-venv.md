---
number: 7311
title: "Feature Request: Add a way to get tool venv specific information when using `uv tool`"
type: issue
state: open
author: taranlu-houzz
labels:
  - enhancement
  - cli
  - uv tool
assignees: []
created_at: 2024-09-11T21:09:38Z
updated_at: 2024-11-28T05:23:04Z
url: https://github.com/astral-sh/uv/issues/7311
synced_at: 2026-01-10T01:24:13Z
---

# Feature Request: Add a way to get tool venv specific information when using `uv tool`

---

_Issue opened by @taranlu-houzz on 2024-09-11 21:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

It would be very helpful to have the following additional functionality as flags for `uv tool list`:
- `--show-full-dep-tree`: show all of the deps installed into the tool venvs.
- `--show-tool-install-command`: show the original command used to install the tool (sometimes the package name does not match what is on pypi, e.g. `p4python` installs the `P4` package).

To help differentiate tools installed with a more complicated setup, it is really helpful to have an additional suffix that gets appended to all installed script entry points (`pipx` does this via the `--suffix` flag).

Use case:
- I install `coverage` with additional `pytest` deps like this: `uv tool install coverage --with pytest --with pytest-spec --with pytest-sugar`.
- This lets me use `uvx coverage run -m pytest` to run tests with my desired plugins on any project.
- Depending on how long it has been since I used a tool, it may be hard to recall exactly what is installed in the tool environment (or how it was initially installed).


---

_Comment by @zanieb on 2024-09-11 21:17_

I think this may be a `uv tool show` command instead?

We're adding suffix support (https://github.com/astral-sh/uv/pull/7095)

---

_Comment by @zanieb on 2024-09-11 21:17_

Thanks for sharing your use-cases!

---

_Label `enhancement` added by @zanieb on 2024-09-11 21:17_

---

_Label `cli` added by @zanieb on 2024-09-11 21:17_

---

_Label `uv tool` added by @zanieb on 2024-09-11 21:17_

---

_Comment by @taranlu-houzz on 2024-09-11 21:22_

@zanieb Good to hear! Is `uv tool show` a new development? I am using `uv 0.4.9 (77d278f68 2024-09-10)` and that does not appear to be a subcommand yet?

---

_Comment by @zanieb on 2024-09-11 21:25_

Oh sorry no it doesn't exist but I think it should — I think it'd be a better experience for your use-cases than adding these flags to `tool list`.

---

_Comment by @KyleKing on 2024-10-10 11:55_

I would also like to see something like "show install command". `mdformat` and `poetry` have similar plugin behavior to `pytest`. Comparing pipx and uv:

```
> pipx install mdformat
> pipx inject mdformat 'mdformat-obsidian[recommended]'
> pipx install poetry
> pipx inject poetry poetry-plugin-sort poetry-relax
> pipx list --include-injected
venvs are in ~/.local/pipx/venvs
apps are exposed on your $PATH at ~/.local/bin
manual pages are exposed at ~/.local/share/man
   package mdformat 0.7.17, installed using Python 3.12.7
    - mdformat
    Injected Packages:
      - mdformat-obsidian 0.1.0
   package poetry 1.8.3, installed using Python 3.12.5
    - poetry
    Injected Packages:
      - poetry-plugin-sort 0.2.1
      - poetry-relax 1.1.0

> uv tool install --with 'mdformat-obsidian[recommended]' mdformat
> uv tool install poetry --with=poetry-plugin-sort --with=poetry-relax
> uv tool list
mdformat v0.7.17
- mdformat
poetry v1.8.3
- poetry
```

Being able to see this information across all installed tools at once would be really helpful. As a third option to the ones proposed in the original post, maybe there could be a `--show-with` flag to `uv tool list`?

```
> uv tool list --show-version-specifiers --show-with
mdformat v0.7.17 [required: ]
- mdformat
  With:
  - mdformat-obsidian 0.1.0
```

Or a fourth option of `--show-install-options`

```
> uv tool list --show-version-specifiers --show-install-options
mdformat v0.7.17 [required: ]
- mdformat
  Install options:
  --with 'mdformat-obsidian[recommended]'
  <additional `--with`, `--editable`, etc.>
```

---

_Comment by @KyleKing on 2024-10-10 11:58_

~~One hackish workaround for `uv` might be to use:~~ (**correction**: this only worked because I had separately experimented with `uv tool run`. Tested with `uv tool install --with pytest-sugar pytest` where where `which pytest` outputs `~/.local/bin/pytest`)

```
> which mdformat
uv tool run --with 'mdformat-obsidian[recommended]' mdformat
```

---

Alternatively, `mdformat` supports logging the found plugins and respetive versions. However, for `pytest`, I'm not sure if the plugins can be printed without running `pytest`.

```
> mdformat --version
mdformat 0.7.17 (mdformat_frontmatter: 2.0.8, mdformat_simple_breaks: 0.0.1, mdformat_tables: 1.0.0, mdformat_gfm: 0.3.6, mdformat_obsidian: 0.1.0, mdformat_wikilink: 0.2.0, mdformat_web: 0.1.0,
mdformat_beautysh: 0.1.1, mdformat_ruff: 0.1.3, mdformat_config: 0.1.3)
```

---

Related to #8028 where uv may not currently remember how a tool was installed

---

_Comment by @r-richmond on 2024-11-28 05:23_

I'm also replacing my pipx usage with UV and I was explicitly looking for UV's equivalent of `pipx list --include-injected`

FWIW I think @KyleKing 's [suggestion](https://github.com/astral-sh/uv/issues/7311#issuecomment-2404889024) of `uv tool list --show-with` would be perfect but I also see the value in the more complete/verbose `--show-install-options`.

@zanieb 's since just the `--show-with` request/suggestion has diverged a little from the OP should a separate issue be made for it or would like to keep it centralized here?


---
