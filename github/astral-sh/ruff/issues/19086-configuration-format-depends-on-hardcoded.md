---
number: 19086
title: "Configuration format depends on hardcoded `pyproject.toml` filename"
type: issue
state: open
author: sin-ack
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2025-07-02T11:49:54Z
updated_at: 2025-07-07T12:33:54Z
url: https://github.com/astral-sh/ruff/issues/19086
synced_at: 2026-01-07T13:12:16-06:00
---

# Configuration format depends on hardcoded `pyproject.toml` filename

---

_Issue opened by @sin-ack on 2025-07-02 11:49_

I am currently trying to write a Nix flake check to lint our Python project. In Nix, the actual mapped name of a file from the source directory can be different from the original file name. In our case, we have to validate a workspace with multiple Python packages so the source directory and the `pyproject.toml` files are separate. I tried to get only `pyproject.toml` as a single file, which turns its filename into `/nix/store/zgx...i2-pyproject.toml`. So the command to run becomes `.../bin/ruff format --config /nix/store/zgx...i2-pyproject.toml --check --diff /nix/store/abc...def-source/`.

However, the hardcoded check below makes Ruff fail, because it requires the filename to be exactly `pyproject.toml` (`Path::ends_with` compares against the whole path component):

https://github.com/astral-sh/ruff/blob/f7fc8fb084e30c6846709cc4aa8cbd18fcc86a8b/crates/ruff_workspace/src/pyproject.rs#L161

Could you please either:
- Check whether the filename _ends_ with `pyproject.toml` (not the whole path component, but as a string match)
- Provide a command-line switch to explicitly declare which format is being used (`pyproject` or `ruff` format)

Thank you!

---

_Comment by @ntBre on 2025-07-02 12:53_

Thanks for the report! I'd like to get @MichaReiser's input on this when he's back from PTO next week, but either of your suggested fixes make sense to me. The first would be quick and easy, but the second could be cool to allow arbitrary file names.

---

_Label `configuration` added by @ntBre on 2025-07-02 12:53_

---

_Label `needs-design` added by @ntBre on 2025-07-02 12:53_

---

_Comment by @MichaReiser on 2025-07-07 07:48_

Hmm, I'm not a huge fan of either approach. Do you configure other tools in your `pyproject.toml` that also support multiple configuration formats? How do they handle this case. 

The reason I'm not excited about checking if the file ends with `pyproject.toml` is because the specification (as far as I understand) is fairly clear that the file should be named [`pyproject.toml`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/). But I'm not totally oposed to it. 

What's your reason for not using a `ruff.toml` file?



---

_Comment by @sin-ack on 2025-07-07 11:29_

> Hmm, I'm not a huge fan of either approach. Do you configure other tools in your `pyproject.toml` that also support multiple configuration formats? How do they handle this case.

I looked at `isort` (which supports both `pyproject.toml` and `.isort.cfg` files) and `black` (which only seems to support `pyproject.toml`). Both of them allow specifying a custom file (isort through `--settings-file`, black through `--config`). isort seems to handle this case better by [assuming that the file can contain other things by default](https://pycqa.github.io/isort/docs/configuration/config_files#custom-config-files:~:text=Custom%20config%20files%20should%20place%20their%20configuration%20options%20inside):

> Custom config files should place their configuration options inside an `[isort]` section and never a generic `[settings]` section. This is because isort can't know for sure how other tools are utilizing the config file.

Black will only interpret files in `pyproject.toml` format. 

> The reason I'm not excited about checking if the file ends with `pyproject.toml` is because the specification (as far as I understand) is fairly clear that the file should be named [`pyproject.toml`](https://packaging.python.org/en/latest/guides/writing-pyproject-toml/). But I'm not totally oposed to it.

That's fair. We can hack around it right now by creating a derivation that outputs a folder with a single `pyproject.toml` file in it. However I like `isort`'s approach here better personally, and it would solve this particular issue (it would be breaking if implemented directly though, hence the flag).

> What's your reason for not using a `ruff.toml` file?

I think it's good to have a single file that can configure the whole project. Without this, we would have to separate out every tool's config. It also forces all configuration to be written in TOML (i.e. consistent).

---

_Comment by @MichaReiser on 2025-07-07 12:33_

Thanks for the research!

I think a reasonable option here would be:

* If the file is named `ruff.toml` or `pyproject.toml`, keep the existing behavior. 
* For any other file: Assume it's a `pyproject.toml` if it has a `tool` section. I'm not sure if this can be done "cheaply" without parsing the file (could we look for a line starting with `[tool.` ?

---
