---
number: 12352
title: "Feature: Extend ruff configuration from external file"
type: issue
state: open
author: yeoffrey
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2024-07-16T21:02:58Z
updated_at: 2025-12-04T18:14:29Z
url: https://github.com/astral-sh/ruff/issues/12352
synced_at: 2026-01-10T01:22:52Z
---

# Feature: Extend ruff configuration from external file

---

_Issue opened by @yeoffrey on 2024-07-16 21:02_

# TD;LR
Enable the ability to import ruff.toml configurations from external sources such as URLs or git repositories.

✅ I am willing to contribute to make this feature.

# Use Case
In our own projects, we maintain multiple repositories. When we need to update our linting rules, we currently have to modify the ruff.toml file in each project individually. This process is time-consuming and prone to inconsistencies. It would be beneficial to centralize these configurations, allowing us to update rules in a single location and propagate them across all projects.

For our Javascript projects, we have a shared eslint configuration from a repository that is imported via a project dependency. When we make an update to the configuration, the dependency will update. It saves us a lot of time, and I think Ruff should have a similar solution available for this case.

# Proposed Solution
Introduce support for remote imports in ruff.toml. This feature would allow users to specify an external URL or git repository from which the ruff.toml configuration can be fetched and extended. This could be implemented either as a Python dependency or directly in the ruff.toml file.

Example Configuration

```toml
# ruff.toml
[import]
url = "https://example.com/path/to/ruff.toml"

# or for git repository
[import]
git = "https://github.com/organization/repo"
branch = "main"
path = "path/to/ruff.toml"

# Local overrides
[tool.ruff]
line-length = 88
```

# Conclusion
This feature would streamline the process of sharing and updating linter configurations across multiple projects, enhancing maintainability and consistency. Centralizing configuration management would save time and reduce errors. Thank you for your continued efforts and a brilliant tool 🚀 

---

_Comment by @Avasam on 2024-07-16 23:35_

https://github.com/astral-sh/ruff/discussions/3363#discussioncomment-7266932
> If/when this gets implemented in Ruff, I'd ask to consider a system that allows downloading the configs at install rather than at runtime to avoid issues with pre-commit.ci restrictions. (like dprint is having, see https://github.com/dprint/dprint/issues/442 )



---

_Comment by @MichaReiser on 2024-07-17 06:32_

Thanks @yeoffrey for opening this issue and the kind words. 

The first step for adding preset support is to create a design proposal and consensus behind it. 

* How does it interact with `extend` (or should it be `extend`)
* Can presets be mixed? 
* How does extending a preset work?
* How is the configuration cached
* What are the performance implications (having to issue HTTP downloads during file traversal is expensive)
* How do we ensure builds are reproducible (not given when pointing to an arbitrary URL or a git branch)
* See @Avasam comment


---

_Comment by @yeoffrey on 2024-07-18 13:56_

If the configuration needs to be available at install time and not fetching at runtime, then I think the best option would be to create a preset as a development dependency that is installed with all of the other python packages. Inside of the ruff configuration file, you could point to a dependency that exports a configuration. I see this being a way to solve the question of caching, avoiding HTTP downloads during file traversal, ensuring reproducible builds. It also just leverages python's existing package system, so it doesn't feel unintuitive to handle it this way.

```shell
pip install my-ruff-config
```

```toml
# ruff.toml
[preset]
pkg = "my-ruff-config"
```

The only issue I see though, is how to export a toml file out of a python package:
- Do we allow users to configure Ruff in a Python / Rust file?
- Is there a way to export a toml file from a python package that I'm not aware of?

> * How does it interact with `extend` (or should it be `extend`)
> * Can presets be mixed?
> * How does extending a preset work?

I think presets should be able to be extended, but based on the documentation I don't think `extend` will work for our needs since it only is extending other pyproject.toml files, and that still requires an answer as to whether than can be a dependency.

To make it work there should be an order of priority:
1) Normal ruff priorities as described [here](https://docs.astral.sh/ruff/configuration/#config-file-discovery).
2) If a dependency is marked in a config file, then it should be overridden by local file configurations, and if those are overridden by other files then it shouldn't behave any differently.

These are just my basic thoughts right now, I might have others later.



---

_Comment by @MichaReiser on 2024-07-18 14:10_

> I don't think extend will work for our needs since it only is extending other pyproject.toml files

I'm not sure that i understand what you mean by that. `extend` supports extending any `ruff.toml`, `.ruff.toml` or a `pyproject.toml`. 

I like using Python packages. The main question is how to find the Python package. More specifically. How do we know where the venv is (if any)

---

_Comment by @Avasam on 2024-07-18 17:06_

 > I like using Python packages. The main question is how to find the Python package. More specifically. How do we know where the venv is (if any)

For projects like eslint and typescript it's easy: whatever environment they're run from so they can just import.

pyright is a non-python tool that needs to be aware of installed python packages, maybe you could take some inspiration from how it's doing it? Eric T. is generally a good reference for such standards too.

I see what looks like static typing efforts in Ruff (that red-knot thing?). Surely logic to statically find installed packages can be shared.

---

_Referenced in [astral-sh/ruff#10622](../../astral-sh/ruff/issues/10622.md) on 2024-08-19 05:58_

---

_Label `configuration` added by @MichaReiser on 2024-09-04 15:11_

---

_Referenced in [astral-sh/ruff#13245](../../astral-sh/ruff/issues/13245.md) on 2024-09-04 15:11_

---

_Referenced in [astral-sh/ruff#13505](../../astral-sh/ruff/issues/13505.md) on 2024-09-24 15:30_

---

_Referenced in [astral-sh/ruff#11415](../../astral-sh/ruff/issues/11415.md) on 2024-10-11 01:42_

---

_Referenced in [astral-sh/ruff#13433](../../astral-sh/ruff/issues/13433.md) on 2024-10-11 01:49_

---

_Referenced in [astral-sh/ruff#14443](../../astral-sh/ruff/issues/14443.md) on 2024-11-18 20:10_

---

_Referenced in [astral-sh/ruff#8470](../../astral-sh/ruff/issues/8470.md) on 2024-12-27 01:35_

---

_Comment by @ruhrohraggy on 2025-01-17 19:26_

Hey everyone -- just curious on the status of this. We were just discussing this exact pattern and how we might achieve it so that we don't have to continue keeping up with differing configs in every `pyproject.toml`. We have some abstraction built out using `Make` but something native would be ideal where the config for ruff in `pyproject.toml` could simply point to a remote config of some sort.

A lot of our previous work is in Java with a pretty robust Gradle ecosystem with tons of shared build configurations and everything -- hoping to find some similar pathways in Poetry and the linters we're using

---

_Label `needs-design` added by @dhruvmanila on 2025-01-20 06:43_

---

_Comment by @orenm-gloat on 2025-01-22 09:36_

hi @dhruvmanila , I saw you added the `needs-design` label. We are in the process of adding ruff (and other astral tools, you rock, thanks!) and have same use cases as described here.
Is there a timeline? can we help somehow?

---

_Comment by @yeoffrey on 2025-01-22 13:29_

Unfortunately I haven't had enough time to dedicate to this feature, however over the last few months I've had some other thoughts that might prove useful here.

Ruff already has a way to locate the virtual environment (venv), which is located in `ruff/crates/ruff_python_resolver/src/search.rs` [link](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_resolver/src/search.rs). I think we can use that to fetch the location of the venv.

I think we need to slot that new configuration file into the priorities described here `ruff/crates/ruff/src/resolve.rs` [link](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/resolve.rs), which will allow us to correctly prioritize the configuration file over other configurations.

So if we allow the `extend` option to point to a package like `my-config-package` which has a `ruff.toml` file inside it, we look for it inside the venv, find it, and then prioritize it against the existing rules.

---

_Comment by @orenm-gloat on 2025-01-22 13:39_

thanks for the quick reply @yeoffrey ! I actually tried to go into the code today and found exactly these logics and had very similar thought.
To make the search even easier we can define a strict naming convention, much like eslint does with the prefix `eslint-config-` so it will make the search and also discoverability easier.
Sounds like this should be between second and third priorities or inside the third priority**, we still want to resolve from current working dir to the root in hierarchical manner(I assume the setting will be set in the root config most of the times, although this can be a point to discuss) and then load with priority the plugin config and merge back to the working dir config.

** maybe this is even not in the priorities but in deeper level in the [ruff_workspace resolver.rs](https://github.com/astral-sh/ruff/blob/main/crates/ruff_workspace/src/resolver.rs) file, when resolving a config, if it uses a preset so try to find the preset and merge from the venv

---

_Comment by @dhruvmanila on 2025-01-23 09:20_

> hi, I saw you added the `needs-design` label. We are in the process of adding ruff (and other astral tools, you rock, thanks!) and have same use cases as described here.
> Is there a timeline? can we help somehow?

Thank you for the kind words. No, we don't have any timeline regarding this feature as it's not being worked on currently. The reason I added "needs-design" is because this is a fairly large feature which will require some amount of discussion on a design proposal, see https://github.com/astral-sh/ruff/issues/12352#issuecomment-2232532879.

---

_Comment by @yeoffrey on 2025-01-23 15:59_

@orenm-gloat cc: @dhruvmanila 

So here is what I propose we do. Its similar to what I suggested at the beginning, but after looking at the code for a bit and drinking a few beers last night I think we can make a proposal that is more serious than my original.

# High-Level
We use the `extend` field to link to an external `ruff.toml` file in a dependency. Currently it takes a path, but we could instead do something like this to let Ruff know that we are referring to a package in the virtual environment.

```toml
[tool.ruff]
extend = "pkg:ruff-config/ruff.toml" # Denoted by `pkg:`
```

This design would be ideal because:
1) No breaking changes (in the spirit of Rust).
2) Preloads the configuration, not at runtime to avoid the issue that @Avasam brought up https://github.com/astral-sh/ruff/discussions/3363#discussioncomment-7266932.
3) In the future we can also add something like `preset:` to denote a ruff preset like mentioned in https://github.com/astral-sh/ruff/discussions/3363
4) We could also add something like `url:` to just fetch a `toml` file directly from somewhere on the internet, or even a Github gist which might be really nice for people just playing around with the linter.

For the rest of this I'm only referring to the `pkg:` feature though, so forget about 3 and 4 for now.

# Technical Implementation
## File Specific
- `ruff_workspace/src/configuration.rs` `extends` can still be a `PathBuf`, but we might need to add some handling before that to look into the virtual environment to properly build this path.
- `ruff_workspace/src/resolver.rs` `fn resolve_configuration()` needs to order the stack properly to make sure that the package takes first priority.
   - It seems that this is also used by the LSP (I could be wrong here) so if we correctly handle it here then I think the rest of the logic around config resolving should work. The rest of it seems to be just making sure that Ruff is using settings from the nearest config file relative to the file its linting.

Most of the errors around path resolving expect a `pyproject.toml` file, which won't be true anymore. We'll need to properly handle the error and give the user feedback that its searching for a file in a package that it can't find (for whatever reason).

## Security Implications
I don't think theres anything here that is "more risky" then the risk already taken by installing packages off the internet, but its worth thinking about whether we need to sanitize the input or something just in case.

---

_Comment by @MichaReiser on 2025-01-23 17:06_

I think the hard part is virtual environment discovery which neither Ruff nor Red Knot support today. We may need a setting and or some heuristic for finding it. 

The other question is if it only supports venv or packages installed anywhere (even in system python). This would require a fully-fledged mdoule resolver. We do have that in `red_knot_python_semantic` (see `resolve_module`) but it doesn't support any non `py` extensions.


> I don't think theres anything here that is "more risky" then the risk already taken by installing packages off the internet, but its worth thinking about whether we need to sanitize the input or something just in case.

Agree, reading a configuration seems pretty fine compared to a package running arbitrary code with `setuptools` during installation ;)

---

_Comment by @Avasam on 2025-01-23 17:20_

As a sidenote: My concerns for runtime fetching of extended configurations are still valid. But if we can at least get the url-based one first, that'd already work for all use cases where the CI isn't blocked behind a proxy (ie pre-commit.ci's free tier and some corporate setups). And I don't think those two features need to come together / block each other.

Something @yeoffrey didn't mention is that the config could be cached (and busted w/ `ruff clean`).
Also to "pin" a configuration URL, one can simply point to a specific commit, for example:
- Unpinned: `https://raw.githubusercontent.com/BesLogic/shared-configs/refs/heads/main/ruff.toml`
- Pinned: `https://raw.githubusercontent.com/BesLogic/shared-configs/1eae2e41c99aeaeb6ac21bc1109b3e305171cfb2/ruff.toml`

---

_Comment by @yeoffrey on 2025-01-23 17:33_

> I think the hard part is virtual environment discovery which neither Ruff nor Red Knot support today. We may need a setting and or some heuristic for finding it.
> 
> The other question is if it only supports venv or packages installed anywhere (even in system python). This would require a fully-fledged mdoule resolver. We do have that in `red_knot_python_semantic` (see `resolve_module`) but it doesn't support any non `py` extensions.

So I see that `ruff_python_resolver/src/search.rs` has code but it seems to be only for finding `site-packages`.

Eric Traut pointed me towards the way PyRight does it: [theres a script](https://github.com/microsoft/pyright/blob/fe0dc216f09dd61c41ec8ba3d1aa4e7995f357d4/packages/pyright-internal/src/common/fullAccessHost.ts#L27) that tries to get the contents of `sys.path`.

I looked at the `resolve_module` code there. It's possible that we could try to just get a path from it. Otherwise we might need to just focus on a `url` option for now. @Avasam does make a good point that a url would be a lot easier to do right now. 


---

_Comment by @MichaReiser on 2025-01-23 17:40_

The `ruff_python_resolver` is only a prototype. It is currently unused. I would rather not write new code depending on it.


Yeah, the downside of URLs is that they require including an entire HTTP request library. They tend to be large, and this introduces complexity around caching, etc. 

---

_Comment by @silverwind on 2025-06-04 06:58_

> We use the extend field to link to an external ruff.toml file in a dependency

The [`extend` docs](https://docs.astral.sh/ruff/settings/#extend) say that the option only accepts a `pyproject.toml` file, not a `ruff.toml` file. Would it be made to support both file formats? I guess this may not be straightforward and may require some heuristics to detect which format it is.

I think would make much more sense if `extend` in a `pyproject.toml` would support extending a `ruff.toml` file.

---

_Comment by @MichaReiser on 2025-06-04 07:37_

> > We use the extend field to link to an external ruff.toml file in a dependency
> 
> The [`extend` docs](https://docs.astral.sh/ruff/settings/#extend) say that the option only accepts a `pyproject.toml` file, not a `ruff.toml` file. Would it be made to support both file formats? I guess this may not be straightforward and may require some heuristics to detect which format it is.
> 
> I think would make much more sense if `extend` in a `pyproject.toml` would support extending a `ruff.toml` file.

Both should be supported. If not, please open a new issue as this is slightly different from what the issue author is asking for.

---

_Referenced in [astral-sh/ruff#18701](../../astral-sh/ruff/issues/18701.md) on 2025-06-16 07:19_

---

_Comment by @silverwind on 2025-06-27 12:26_

> Both should be supported. If not, please open a new issue as this is slightly different from what the issue author is asking for.

You are right, I tested `extend = "ruff1.toml"` with the file being in `ruff.toml` format and it worked.

So I guess [these docs](https://docs.astral.sh/ruff/settings/#extend) are incorrect. I made new issue https://github.com/astral-sh/ruff/issues/18983 about this.

---

_Comment by @swanysimon on 2025-08-10 19:21_

To clarify here (and happy to open another issue/question if that would be better): if I want to share the same base `ruff` configuration across all Python repos that I'm working on, I would need do something like the following:

1. Create a `my-ruff-config` project
2. Put my ruff configuration into a `ruff.toml` file
3. Figure out how to add the `ruff.toml` to the build/publish for that repo (I'll figure this out when I start digging, I have confidence)

Then in every repo:

1. Add a dev dependency on `my-ruff-config`
2. Add to my `pyproject.toml`
    ```toml
    [tool.ruff]
    extend = ".venv/bin/lib/python<version>/site-packages/my_ruff_config/ruff.toml"
    ```

Is that correct? I see in the docs that `User home directory and environment variables will be expanded.`, is there a way to use these for a less brittle path?

---

_Referenced in [astral-sh/ruff#18983](../../astral-sh/ruff/issues/18983.md) on 2025-08-10 19:27_

---

_Comment by @kkom on 2025-08-10 19:42_

@swanysimon -- to provide some validation, this is almost exactly what I have done in my projects.

Only that instead of referring to a file in the venv, I exposed a function which overwrites a specified path with the base config file (original is packaged with the lib).

This way the base config is more explicit in the project using it. You need CI checks if you want to ensure that the base file is updated on a lib version bump.

A more first-class way to do this would be very welcome!

---

_Comment by @swanysimon on 2025-08-10 20:00_

> Only that instead of referring to a file in the venv, I exposed a function which overwrites a specified path with the base config file (original is packaged with the lib).

@kkom Appreciate the validation. I was about to go this route before realizing I should check if this was supported out of the box. Already have other code generation things going on in my pre-commit config (which is much more easily shared), and I'm considering something similar for our `mypy` (maybe in the future `ty`?) configuration.

> A more first-class way to do this would be very welcome!

100%. I liked most of the ideas in this thread, they're all better than trying to wire this together myself. But worst case, I'll wire it together myself :D 

---

_Comment by @phillipuniverse on 2025-08-10 20:36_

> Only that instead of referring to a file in the venv, I exposed a function which overwrites a specified path with the base config file (original is packaged with the lib).

@kkom could you give a bit more information on what you mean by this? Like how did you expose and invoke the function exactly?

---

_Comment by @kkom on 2025-08-10 20:41_

This is the helper defined in my package:

```python
"""Utility function to sync configs defined in kstd."""

import argparse
from pathlib import Path
from shutil import copyfileobj
from typing import Literal, get_args

_CONFIGS_DIR = Path(__file__).parent / "configs"

ConfigType = Literal["pyright", "ruff"]


def sync_config(
    config_type: ConfigType,
    destination_file_path: Path,
) -> None:
    """
    Sync base configuration file for a specified tool. This will overwrite the destination.

    Args:
        config_type: Type of configuration file to sync.
        destination_file_path: Destination path where the config file will be copied to.
    """
    match config_type:
        case "pyright":
            config_file = _CONFIGS_DIR / "pyrightconfig.json"
        case "ruff":
            config_file = _CONFIGS_DIR / "ruff.toml"

    with config_file.open("r") as config_file:
        with destination_file_path.open("w") as destination_file:
            copyfileobj(config_file, destination_file)


def cli() -> None:
    """Command line interface for syncing the configuration files."""
    parser = argparse.ArgumentParser(description="Sync configuration files")

    _ = parser.add_argument(
        "config_type",
        choices=get_args(ConfigType),
        help="Type of configuration to sync",
    )

    _ = parser.add_argument(
        "destination_file_path",
        type=Path,
        help="Destination path for the config file",
    )

    args = parser.parse_args()

    sync_config(args.config_type, Path(args.destination_file_path))


if __name__ == "__main__":
    cli()
```

And here is how I use it with [Poe](https://poethepoet.natn.io/index.html):

```toml
[tool.poe.tasks]
sync_config_pyright = 'uv run python -c "from kstd.sync_config import cli; cli()" pyright ../../pyrightconfig.json'
sync_config_ruff = 'uv run python -c "from kstd.sync_config import cli; cli()" ruff ../common-files/ruff_base.toml'
```

I haven't implemented dry run / check mode at the moment.

PS: Looking at my code again, I think I could probably invoke `sync_config` directly? I don't remember if there was a reason to go through the CLI, but I think you could bypass that.

---

_Comment by @silverwind on 2025-09-11 10:24_

>     2. Add to my `pyproject.toml`
>        [tool.ruff]
>        extend = ".venv/bin/lib/python<version>/site-packages/my_ruff_config/ruff.toml"
> 
> 
> Is that correct? I see in the docs that `User home directory and environment variables will be expanded.`, is there a way to use these for a less brittle path?

Maybe a syntax like this could be supported to extend from a file inside a installed python package:

```toml
[tool.ruff]
extend = { package = "my-ruff-config", path = "ruff.toml" }
```

My current workaround is via an npm package (npm package paths are stable).

```toml
[tool.ruff]
extend = "node_modules/ruff-config-silverwind/ruff.toml"
```

---

_Referenced in [astral-sh/uv#15800](../../astral-sh/uv/issues/15800.md) on 2025-09-12 02:27_

---

_Comment by @Avasam on 2025-09-13 01:48_

> 2. Add to my `pyproject.toml`
>    [tool.ruff]
>    extend = ".venv/bin/lib/python<version>/site-packages/my_ruff_config/ruff.toml"

it should be `".venv/lib/python<version>/site-packages/my_ruff_config/ruff.toml"`

If you package your config as a Script, it'll go under `.venv/bin/my_ruff_config.toml`. You solve the Python version on Linux, but now it's not compatible with Windows (where the path would be `.venv/Scripts/my_ruff_config.toml`. Unless something like https://github.com/astral-sh/uv/issues/15800 is implemented first.

Is it possible to build a python package such that a file would end up in a consistent location across OSes ? This is really a non-issue with node >.<


---

_Comment by @SamuelT-Beslogic on 2025-09-17 20:07_

I figured it out ! Using a discouraged/deprecated setuptools feature ([data-files](https://setuptools.pypa.io/en/latest/references/keywords.html#keyword-data-files)) it's possible to install files to arbitrary location relative to the environment. This of course only works with sdists and for projects with a known virtual environment location (which is the case for uv and virtualenv out of the box)

Since you can't use wheels anyway and Astral may implement an official way of handling this later, I wouldn't recommend publishing and polluting PyPI, so I did it through GitHub. Here's my full solution: https://github.com/BesLogic/Beslogic-Ruff-Config

And it even works for my multi-venv setup (Windows + WSL) as long as my original venv has the configs !
<img width="152" height="386" alt="Image" src="https://github.com/user-attachments/assets/a83f8756-5386-45ca-9970-66dbadb77b6e" />

-- @Avasam (using my work account rn as I did this for work)

---

_Comment by @PhilippAIO on 2025-09-19 16:21_

> Maybe a syntax like this could be supported to extend from a file inside a installed python package:
> 
> [tool.ruff]
> extend = { package = "my-ruff-config", path = "ruff.toml" }
> 
I just want to mention, that the mentioned design suggestion of @silverwind looks great to me. The pure filepath-logic of extend has many downsides when you want to use a shared ruff configuration between multiple projects and multiple developers with diverging (venv) paths / systems.

---

_Referenced in [posit-dev/air#411](../../posit-dev/air/issues/411.md) on 2025-10-06 11:12_

---

_Comment by @Sillocan on 2025-12-04 18:14_

I agree, I really like @silverwind's suggestion especially for sharing configurations within enterprise organizations. This allows a self contained `uv run ruff` to work without requiring slightly painful concepts like submodules. This also solves the runtime/install issue that was mentioned at the start for dprint.

---
