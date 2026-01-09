---
number: 6298
title: "Add a command to read and update (i.e., bump) the project version, e.g., `uv version`"
type: issue
state: closed
author: cauebs
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-08-21T04:56:20Z
updated_at: 2025-05-19T13:41:15Z
url: https://github.com/astral-sh/uv/issues/6298
synced_at: 2026-01-07T13:12:17-06:00
---

# Add a command to read and update (i.e., bump) the project version, e.g., `uv version`

---

_Issue opened by @cauebs on 2024-08-21 04:56_

:white_check_mark: Both Rye and Poetry have this, and it's quite useful both...
- to avoid manually incrementing the version number (and potentially making a mistake there with the digits),
- and to have this field on the `pyproject.toml` file serve as the single source of truth for a project's version, easily accessible (via `uv`) in CI workflows, etc.

:stop_sign: The latter is where my personal interests lie: I have a CI workflow that tags and deploys releases when the version is bumped, currently using Poetry for it. I don't really want to pull in an additional tool to parse the project specs, so it's currently keeping me from considering migrating to `uv`.

:scroll: My proposal is replacing the current functionality of the `version` subcommand with this, since `uv`'s version can already be accessed via the `-V | --version` option, and other project management currently exist as "root" subcommands.

:information_source: I'm willing to give this one a go myself if maintainers greenlight the proposal.

---

_Label `enhancement` added by @zanieb on 2024-08-21 12:36_

---

_Label `projects` added by @zanieb on 2024-08-21 12:36_

---

_Referenced in [astral-sh/uv#6311](../../astral-sh/uv/issues/6311.md) on 2024-08-21 12:39_

---

_Referenced in [astral-sh/uv#6430](../../astral-sh/uv/issues/6430.md) on 2024-08-22 13:33_

---

_Comment by @JonZeolla on 2024-08-22 14:19_

Would maintaining version identifiers outside of `pyproject.toml` be considered out of scope for `uv`? For instance, updating something like a `__version__` in a `src/package/__init__.py`?

Or, even better, support a dynamic version like hatch does with a

```toml
[project]
dynamic = ["version"]

[tool.hatch.version]
path = "src/package/__init__.py"
```

---

_Referenced in [astral-sh/uv#6440](../../astral-sh/uv/issues/6440.md) on 2024-08-22 14:35_

---

_Comment by @deadnews on 2024-08-22 15:08_

It's most convenient to me to have `git tag` based versioning:

https://github.com/mtkennerly/poetry-dynamic-versioning

I would love to have this functionality.


---

_Referenced in [astral-sh/uv#6636](../../astral-sh/uv/issues/6636.md) on 2024-08-26 17:35_

---

_Comment by @raayu83 on 2024-08-29 12:27_

Would also love to see this... I'm currently using poetry-version-plugin in versioned projects and want to switch them to uv 

---

_Referenced in [locustio/locust#2857](../../locustio/locust/issues/2857.md) on 2024-08-30 08:09_

---

_Referenced in [astral-sh/uv#6854](../../astral-sh/uv/issues/6854.md) on 2024-08-30 12:06_

---

_Comment by @menzenski on 2024-09-05 11:54_

Also very interested in this feature. This kind of read/update version command seems very common (not only Poetry but npm, etc) and it'd make migration to uv from other tools that much more straightforward.

> Would maintaining version identifiers outside of pyproject.toml be considered out of scope for uv? For instance, updating something like a __version__ in a src/package/__init__.py?

@JonZeolla  I've used PDM's dynamic versioning in the past (which is essentially the same as the Hatch example you provide) but have found it kind of annoying in practice. IMO it's preferable to have the version in pyproject.toml be the source of truth. That version can be exposed in the Python package using `importlib.metadata.version`:

```toml
[tool.poetry]
name = "my-package"
version = "1,2,3"
```

```python
# my_package/__init__.py

from importlib.metadata import version

__version__ = version("my_package")
```


---

_Comment by @raayu83 on 2024-09-05 13:33_

Personally I also like the idea of basing the version on the current git tag when using together with CI/CD.
In that case it would be great if there was a command to set the version in pyproject.toml to the value of the git tag.

---

_Comment by @HenriBlacksmith on 2024-09-06 20:54_

> Personally I also like the idea of basing the version on the current git tag when using together with CI/CD.
> 
> In that case it would be great if there was a command to set the version in pyproject.toml to the value of the git tag.

Covering what bump-my-version (and previously bumpversion) do would cover that (including customizing the git tag and commit message format)

An interesting addition would be to have a way to trigger a custom
script to bump the CHANGELOGs like a hook to a python script.

An even better but probably too opinionated option could be to add a CHANGELOG management system (like changie) but it might be out of scope of uv?

---

_Comment by @phi-friday on 2024-09-07 04:19_

For others who need this.
```bash
uvx --from=toml-cli toml set --toml-path=pyproject.toml project.version $VERSION
```

I'm using it like this in github action.
```bash
VERSION=$(uvx dunamai from any --no-metadata --style pep440)
uvx --from=toml-cli toml set --toml-path=pyproject.toml project.version $VERSION
uv build
```

---

_Referenced in [astral-sh/uv#7248](../../astral-sh/uv/pulls/7248.md) on 2024-09-10 09:17_

---

_Comment by @daviewales on 2024-09-11 00:20_

Consider how `poetry` does it: https://python-poetry.org/docs/cli#version

My workflow looks like this:

``` sh
# Show current version
poetry version

# Bump version
poetry version minor

# Add a git tag with a matching version to what's in pyproject.toml
git tag "v$(poetry version -s)"
```

Poetry tries to be clever to avoid needing an option to specify whether you are auto-bumping or manually setting a version:

> The new version should be a valid [PEP 440](https://peps.python.org/pep-0440/) string or a valid bump rule: `patch`, `minor`, `major`, `prepatch`, `preminor`, `premajor`, `prerelease`

This assumes that no-one will ever want to use one of those strings as a version, but that seems like a reasonably safe assumption.

---

_Comment by @pkucmus on 2024-09-12 19:44_

With hatch even when you have the `version` set as dynamic and red from an example `__about__.py` module and you bump `hatch version $(git describe --broken --tags --always --dirty)` (that what I do on build in CI/CD) it will actually change the python module. Which is kinda nice.

---

_Comment by @phi-friday on 2024-09-14 10:06_

> With hatch even when you have the `version` set as dynamic and red from an example `__about__.py` module and you bump `hatch version $(git describe --broken --tags --always --dirty)` (that what I do on build in CI/CD) it will actually change the python module. Which is kinda nice.

Using `uvx hatch version ...` is mostly fine, but I can't use it when it's a stub only package because there is no `*.py`.

---

_Referenced in [astral-sh/uv#7507](../../astral-sh/uv/issues/7507.md) on 2024-09-18 16:47_

---

_Referenced in [CSSUoB/TeX-Bot-Py-V2#329](../../CSSUoB/TeX-Bot-Py-V2/issues/329.md) on 2024-09-20 22:49_

---

_Referenced in [astral-sh/uv#7609](../../astral-sh/uv/issues/7609.md) on 2024-09-21 12:33_

---

_Referenced in [astral-sh/uv#7672](../../astral-sh/uv/issues/7672.md) on 2024-09-24 20:22_

---

_Referenced in [astral-sh/uv#7750](../../astral-sh/uv/issues/7750.md) on 2024-09-28 03:51_

---

_Referenced in [astral-sh/uv#7818](../../astral-sh/uv/issues/7818.md) on 2024-09-30 19:40_

---

_Referenced in [astral-sh/uv#7785](../../astral-sh/uv/issues/7785.md) on 2024-10-06 14:42_

---

_Comment by @david-waterworth on 2024-10-10 02:10_

I'd love to see something like this as well - I currently use `python-semantic-version` but that uses SemVer rather than PEP440 - which causes a few minor issues such as not being able to parse tags that are valid PEP440 but not SemVer git tags (i.e. tags we added before migrating to PSV). It also has a nice CHANGELOG generation module, and the ability to generate release or dev version based on branch name (main vs feature). Version bumping rules are based on commit message parsing (i.e. if message starts with `feat:` bump minor version, if `fix:` bump patch etc.). As part of the PSV build process, all files that are modified (i.e. pyproject.toml, __version__.py) are staged, committed and pushed. The commit is also tagged with the version. You can then filter by the commit message in your CI pipeline to prevent infinite recursion.

---

_Referenced in [astral-sh/uv#8162](../../astral-sh/uv/issues/8162.md) on 2024-10-14 16:30_

---

_Comment by @zanieb on 2024-10-14 16:31_

As a workaround, I use this in my project:

```
sed -i -e "s/0.0.0/${GITHUB_REF#refs/*/}/" pyproject.toml
```

https://github.com/astral-sh/packse/blob/70abfe8f64a9746452c02cb514942f879c7eaccc/.github/workflows/release.yaml#L34-L36

---

_Comment by @struckchure on 2024-10-14 16:33_

> As a workaround, I use this in my project:
> 
> ```
> sed -i -e "s/0.0.0/${GITHUB_REF#refs/*/}/" pyproject.toml
> ```
> 
> https://github.com/astral-sh/packse/blob/70abfe8f64a9746452c02cb514942f879c7eaccc/.github/workflows/release.yaml#L34-L36

Exactly what I did, I just wasn't comfortable w/ it

---

_Comment by @struckchure on 2024-10-14 16:33_

Is there a reason why this won't/can't be implemented?

---

_Comment by @zanieb on 2024-10-14 16:51_

The `uv version` command already exists, so I need to come up with a design to either phase that out in favor this functionality or choose a new command name.

Basically, that part isn't trivial and we're doing a lot of other things.

---

_Comment by @sam57719 on 2024-10-14 17:00_

@zanieb according to the [docs](https://docs.astral.sh/uv/getting-started/help/#viewing-the-version) there are already other ways to get the uv version. 

E.g.
```bash
uv --version      # Same output as `uv version`
uv -V             # Will not include the build commit and date
uv pip --version  # Can be used with a subcommand
```

---

_Comment by @zanieb on 2024-10-14 17:07_

@sam57719 that doesn't mean it's not a breaking change to switch the functionality. There is downstream code that _will_ break if we change it.

---

_Comment by @struckchure on 2024-10-14 17:13_

> The `uv version` command already exists, so I need to come up with a design to either phase that out in favor this functionality or choose a new command name.
> 
> Basically, that part isn't trivial and we're doing a lot of other things.

Maybe we could just have a way to bump by updating the .toml file?
something like ...

`uv build --override-config "project.version=x.x.x"`

Maybe someone already thought of that, I'm new to `uv` 🤷‍♂️ 

---

_Comment by @chrisrodrigue on 2024-10-14 17:52_

Related: https://github.com/astral-sh/uv/issues/6440

---

_Referenced in [astral-sh/uv#8294](../../astral-sh/uv/issues/8294.md) on 2024-10-17 16:38_

---

_Comment by @OverkillGuy on 2024-10-20 06:11_

Here's a workaround that may help wait for the feature: `uv tree --depth 0` shows the tree of version dependencies of ... just our package:

```shell
$ uv tree --depth 0
Resolved 66 packages in 1ms
mass-driver v0.18.0a1
```
Noting the "Resolved 66 packages in 1ms" is coming on stderr (debug message), so with a bit of fanciful shell:
```shell
$ uv tree --depth=0  2> /dev/null | awk '{print $2}'
v0.18.0a1
```

Not perfect, but it's a start!

PS: Still would love the feature, and personally believe the breakage/change of semantics of `uv version` is worth it (given `uv -V` and `uv self` both exist), but I understand this requires careful thought.

---

_Comment by @bluss on 2024-10-20 08:44_

That `uv tree -q` removes the actual tree output itself is probably a bug and now reported #8379

---

_Comment by @Satge96 on 2024-10-24 06:27_

Hi everyone, I'm new to this and have a question about the version bump functionality. Since the existing `uv version` command poses challenges for integrating version bumping, would it make sense to introduce a new subcommand like `uv project`? This subcommand could handle project-related tasks, including version management. For example:
`uv project --bump-patch`
`uv project --bump-minor`
`uv project --bump-major`
Would this approach help avoid conflicts with the existing `uv version` command and provide a dedicated space for expanding project management features in the future?


---

_Comment by @zanieb on 2024-10-24 12:28_

Thanks for engaging on the design!

I think that `uv project` might be weird since all the other "project" commands are top-level, e.g., `init`, `sync`, `lock` are all project operations.

---

_Comment by @acdha on 2024-10-24 12:39_

> Thanks for engaging on the design!
> 
> I think that `uv project` might be weird since all the other "project" commands are top-level, e.g., `init`, `sync`, `lock` are all project operations.

I was wondering whether it’d be an opportunity to reconsider that since right now there isn’t a consistent mental model for what things update pyproject.toml, the lock file, or the venv. A dedicated project command would allow setting other things like bumping the target Python version as well. 

---

_Comment by @CarrotManMatt on 2024-10-24 15:42_

Maybe the top level command could be something like:
- `uv bump python <version>` - increase the minimum required python version to the given version (if the given version is lower or equal to the current minimum an error is given), the allowed syntax for the given version should adhere to the [Python requests format](https://docs.astral.sh/uv/concepts/python-versions#requesting-a-version)
- `uv bump python --minor` - increase the minimum required python version to the next highest minor version
- `uv bump python --major` - increase the minimum required python version to the next highest major version
- `uv bump project <version>` - increase the project version to the given version (if the given version is lower or equal to the current version an error is given), the allowed syntax for the given version should follow semantic versioning with an optional `v` character at the start
- `uv bump project --patch` - increase the project version to the next highest patch version
- `uv bump project --minor` - increase the project version to the next highest minor version
- `uv bump project --major` - increase the project version to the next highest major version

---

_Referenced in [PyVRP/PyVRP#659](../../PyVRP/PyVRP/issues/659.md) on 2024-10-28 20:51_

---

_Referenced in [astral-sh/uv#8714](../../astral-sh/uv/issues/8714.md) on 2024-10-31 04:49_

---

_Referenced in [mbarkhau/bumpver#239](../../mbarkhau/bumpver/issues/239.md) on 2024-10-31 13:42_

---

_Comment by @epicserve on 2024-10-31 13:59_

[Bumpver](https://github.com/mbarkhau/bumpver) would be a good project for inspiration. I submitted https://github.com/mbarkhau/bumpver/issues/239, but it was closed as a duplicate of https://github.com/mbarkhau/bumpver/issues/193, which seems like it's a ways off.


---

_Comment by @HenriBlacksmith on 2024-10-31 17:39_

> [Bumpver](https://github.com/mbarkhau/bumpver) would be a good project for inspiration. I submitted https://github.com/mbarkhau/bumpver/issues/239, but it was closed as a duplicate of https://github.com/mbarkhau/bumpver/issues/193, which seems like it's a ways off.
> 
> 

Looks a bit older compared to: https://github.com/callowayproject/bump-my-version but I assume uv maintainers already have good references 😃 

---

_Referenced in [astral-sh/uv#8755](../../astral-sh/uv/issues/8755.md) on 2024-11-01 23:21_

---

_Renamed from "Add a subcommand for reading/bumping the version in pyproject.toml" to "Add a command for reading and modifying (i.e., bumping) the version in the `pyproject.toml` " by @zanieb on 2024-11-02 13:56_

---

_Comment by @zanieb on 2024-11-02 13:56_

Retitled to try to improve discoverability. We get a lot of duplicates of this one.

---

_Comment by @nc9 on 2024-11-08 08:40_

> The `uv version` command already exists, so I need to come up with a design to either phase that out in favor this functionality or choose a new command name.

fwiw - when I first used `uv` at the very early stages, I instinctively thought the `version` subcommand would replicate the `poetry version` et al. behaviour. It would probably make more sense to keep `uv` related commands under `uv self` (ie. `uv self version`) and keep the top-level subcommands as project / environment scoped commands. 

---

_Referenced in [astral-sh/uv#9126](../../astral-sh/uv/issues/9126.md) on 2024-11-14 18:25_

---

_Comment by @tastyminerals on 2024-11-26 13:27_

So, imagine we have dozens of `poetry` based projects that use Jenkinsfiles configured to use `poetry version` to build versioned prod docker images. And in order to get this version, we either go back to the good old setuptools days or use some `python -c 'import toml ...` hacks, which obviously nobody will do. This basically means `uv` cannot be used as a build tool for us 😿 which is a shame.
A workaround with `pyproject-info` could work, but if you have dependency groups in your `pyproject.toml`, the toml is no longer PEP 621 compliant, and it fails.
`uv` ability to resolve project versions is very important, in our case it is a blocker for its adoption, unfortunately.

---

_Comment by @M0NsTeRRR on 2024-11-26 13:35_

> So, imagine we have dozens of `poetry` based projects that use Jenkinsfiles configured to use `poetry version` to build versioned prod docker images. And in order to get this version, we either go back to the good old setuptools days or use some `python -c 'import toml ...` hacks, which obviously nobody will do. This basically means `uv` cannot be used as a build tool for us 😿 which is a shame. A workaround with `pyproject-info` could work, but if you have dependency groups in your `pyproject.toml`, the toml is no longer PEP 621 compliant, and it fails. `uv` ability to resolve project versions is very important, in our case it is a blocker for its adoption, unfortunately.

I resolved this issue with this temporary workaround:  
`RUN sed -i -e "s/0.0.0/${APP_VERSION}/" pyproject.toml`.  

This approach was inspired by the workaround shared https://github.com/astral-sh/uv/issues/6298#issuecomment-2411738585. The key is to pass the version as a build arg (e.g., from a Git tag) to your build tool, in my case, Kaniko.
Alternatively, if Git is installed in your image, you can use sed directly with the Git tag.



---

_Comment by @krishan-patel001 on 2024-11-26 13:48_

> So, imagine we have dozens of `poetry` based projects that use Jenkinsfiles configured to use `poetry version` to build versioned prod docker images. And in order to get this version, we either go back to the good old setuptools days or use some `python -c 'import toml ...` hacks, which obviously nobody will do. This basically means `uv` cannot be used as a build tool for us 😿 which is a shame. A workaround with `pyproject-info` could work, but if you have dependency groups in your `pyproject.toml`, the toml is no longer PEP 621 compliant, and it fails. `uv` ability to resolve project versions is very important, in our case it is a blocker for its adoption, unfortunately.

I use this as a workaround:
`version_number=$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.version)`

---

_Referenced in [zanieb/uv#6](../../zanieb/uv/issues/6.md) on 2024-11-26 17:33_

---

_Comment by @NathanVaughn on 2024-11-26 18:43_

Personally I do this with a version of Python that includes `tomllib`:

```bash
version=$(python -c 'import tomllib;fp=open("pyproject.toml","rb");print(tomllib.load(fp)["project"]["version"]);fp.close()')
```

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2024-11-26 21:15_

---

_Renamed from "Add a command for reading and modifying (i.e., bumping) the version in the `pyproject.toml` " to "Add a command to read and update (i.e., bump) the project version, e.g., `uv version`" by @zanieb on 2024-11-26 23:52_

---

_Comment by @jake-dunn on 2024-11-27 18:08_

> It's most convenient to me to have `git tag` based versioning:
> 
> https://github.com/mtkennerly/poetry-dynamic-versioning
> 
> I would love to have this functionality.

This is basically the only thing which blocks me porting all my projects over to UV right now.

---

_Comment by @Galtozzy on 2024-12-03 15:30_

> The `uv version` command already exists, so I need to come up with a design to either phase that out in favor this functionality or choose a new command name.
> 
> Basically, that part isn't trivial and we're doing a lot of other things.

My 2 cents on the issue:

I don't think there are many places where there is a dependency on a `uv version`, but even if there are and this will be something to phase out/deprecate I think there is a pretty straightforward way of doing it.

- The `-V`, `--version` args should always point to an actual `uv` version.
- Then at first, the project version management is under a certain prefix, e.g `uv version project <args>`
  - So that users can do `uv version project minor`, `uv version project major`, etc (the command design could be anything, I just assumed the most common would be the poetry one)
- Later on, after deprecation notices the project prefix could be dropped and the command will be just `uv version`
  - Ideally, everyone who used an old `uv version` to check the `uv` version would switch to `uv --version`.

It's just something I thought of and any part could be changed/tweaked to better fit the desired outcome. 
But anyway I think this is a major one and should be considered since it would make `uv` better and will allow for an easy drop-in replacement from any `poetry` based project.

---

_Referenced in [plugboard-dev/plugboard#58](../../plugboard-dev/plugboard/issues/58.md) on 2024-12-13 16:38_

---

_Referenced in [astral-sh/uv#9899](../../astral-sh/uv/issues/9899.md) on 2024-12-15 16:55_

---

_Referenced in [unitycatalog/unitycatalog#782](../../unitycatalog/unitycatalog/pulls/782.md) on 2024-12-17 20:33_

---

_Referenced in [astral-sh/uv#8352](../../astral-sh/uv/issues/8352.md) on 2024-12-18 10:33_

---

_Referenced in [Fraunhofer-AISEC/gallia#657](../../Fraunhofer-AISEC/gallia/pulls/657.md) on 2024-12-19 14:38_

---

_Referenced in [datnguye/dbterd#126](../../datnguye/dbterd/issues/126.md) on 2025-01-02 01:59_

---

_Referenced in [langroid/langroid#662](../../langroid/langroid/pulls/662.md) on 2025-01-05 22:18_

---

_Comment by @leandrodamascena on 2025-01-06 12:51_

Hello uv team!

The "uv version" feature for updating project versions is crucial for projects with frequent alpha/dev releases. Currently, Poetry's simple command `poetry version prerelease --short` makes this very easy. The lack of a similar feature in `uv` is preventing us from replacing `poetry` with `uv`, as managing versions via scripts is impractical for fast release cycles.

Is there a roadmap or timeline for implementing this functionality in uv?

Thanks

---

_Comment by @tipanverella on 2025-01-10 20:38_

The simplest solution I can envision is to run:

- `uvx poetry version`
- `uvx poetry version patch`
- `uvx poetry version minor`
- `uvx poetry version major`

---

_Comment by @mirkolenz on 2025-01-11 08:26_

To update the version, I use the following workaround:

`sed -i 's/^version = ".*"$/version = "NEW_VERSION"/' pyproject.toml && uv lock`


---

_Comment by @Kavan72 on 2025-01-15 06:32_

> The `uv version` command already exists, so I need to come up with a design to either phase that out in favor this functionality or choose a new command name.
> 
> Basically, that part isn't trivial and we're doing a lot of other things.

One option could be to change `uv version` to `uv self version`, as `uv version` currently displays the version of the installed uv on the system.  after 
```
uv version 
uv version patch
.....
```

There’s a rule to keep in mind: if you want to create something new, you may need to break something old.

---

_Comment by @mardiros on 2025-01-15 08:49_

@Kavan72 
> 
> One option could be to change `uv version` to `uv self version`, as `uv version` currently displays the version of the installed uv on the system. after
> 

I completely agree, note that:

```
𝝿 uv self --version
uv-self 0.5.18 (27d1bad55 2025-01-11)
```


---

_Comment by @bw-matthew on 2025-01-21 13:30_

You can read the pyproject file and print the version with the following:
```shell
uv run --python 3.12 -- \
    python -c '
import tomllib, pathlib
file = pathlib.Path("pyproject.toml")
metadata = tomllib.loads(file.read_text())
print(metadata["project"]["version"])
'
```

Need to use a python version that has tomllib (i.e. 3.11 or greater).

---

_Referenced in [TechnologyBrewery/pants-uv-lifecycle-plugin#11](../../TechnologyBrewery/pants-uv-lifecycle-plugin/issues/11.md) on 2025-01-21 16:09_

---

_Comment by @engeir on 2025-01-22 08:12_

Expanding on the answer in https://github.com/astral-sh/uv/issues/6298#issuecomment-2335034247, the following bash script can both print and increment versions:

```bash
#!/usr/bin/env bash

v="$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.version)"
if [ -z "$1" ]; then
  echo "$v"
  exit 0
fi
uvx --from bump2version bumpversion --allow-dirty --current-version "$v" "$1" pyproject.toml
```

Further, if you use [mise](https://mise.jdx.dev), you may get tab-completion with the following task (run as `mise run bump` or with mise run aliased to mr, `mr bump`):

```toml
# .mise.toml
[tasks.bump]
description = "Bump the python package version. Takes one argument: `major`, `minor` or `patch`."
quiet = true
usage = '''
arg "[semver]" help="The SemVer name that should be incremented." {
  choices "major" "minor" "patch"
}
'''
run = """
#!/usr/bin/env bash

v="$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.version)"
if [ -z {{arg(name="semver")}} ]; then
  echo "$v"
  exit 0
fi
uvx --from bump2version bumpversion --allow-dirty --current-version "$v" {{arg(name="semver")}} pyproject.toml
"""
```

---

_Comment by @thclark on 2025-01-26 09:52_

Inspired by one of the comments above, here's my GitHub action for getting version which is a workaround for at least one small chunk of the functionality being discussed here:

```yaml

      - name: Install uv
        uses: astral-sh/setup-uv@v5

      - name: Get package version
        id: version
        run: |
          VERSION=$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.version)
          echo "package_version=$VERSION" >> $GITHUB_OUTPUT
```

---

_Referenced in [spotify/luigi#3338](../../spotify/luigi/pulls/3338.md) on 2025-02-01 22:00_

---

_Comment by @Kavan72 on 2025-02-05 06:34_

@zanieb Any updates on this? Almost everyone who uses UV as a package manager needs this feature 😁.

---

_Comment by @jake-dunn on 2025-02-05 09:08_

We've managed to get (what I think everyone else means by) this behaviour using setuptools_scm with uv:

https://pypi.org/project/setuptools-scm/
Explicilty in the pyproject.toml:

[project]
dynamic = ["version"]

[tool.setuptools_scm]

[build-system]
requires = ["setuptools>=64", "setuptools_scm>=8"]
build-backend = "setuptools.build_meta"

Hopefully this is helpful. Python packaging and distribution seems to be much more complex than I hoped.

---

_Comment by @OverkillGuy on 2025-02-06 02:13_

Another alternative, for those who feel this is the last feature missing from uv to move over:
Accept that we don't care about current version as much as bumping, and <details>
<summary> replace the package bumping command with `bump-my-version`.</summary>

Docs: https://callowayproject.github.io/bump-my-version/

Configured for your particular repo via `pyproject.toml`.

In my case:
```toml
[tool.bumpversion]
allow_dirty = false
commit = true
message = "Bump to version v{new_version}"
tag = true
tag_message = "Release v{new_version}"
setup_hooks = ["make"]  # Run 'make' as validation before starting

[[tool.bumpversion.files]]
filename = "pyproject.toml"
search = 'version = "{current_version}"'
replace = 'version = "{new_version}"'

[[tool.bumpversion.files]]
filename = "CHANGELOG.md"
search = "## [Unreleased]"
replace = "## [Unreleased]\n\n## v{new_version} - {now:%Y-%m-%d}"
```
Running it via:
```
bump-my-version bump patch
``` 

This does:
- Update the actual version in `pyproject.toml` with semver meaning
- Inserts the latest version marker + date in changelog's unreleased section
- Commits the result with a specific message
- Git tags the new version with specific message
```diff
--- a/CHANGELOG.md
+++ b/CHANGELOG.md
@@ -5,6 +5,8 @@ The project uses semantic versioning (see [semver](https://semver.org)).

 ## [Unreleased]

+## v0.1.1 - 2025-02-06
+
 ### Fixed
 - No longer crashing on the bad thing

 ## v0.1.0 - 2025-01-20

 ### Added
diff --git a/pyproject.toml b/pyproject.toml
index eec4d73..d64caef 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -3,7 +3,7 @@ name = "redis_backend"
 description = "A cool project"
 authors = ["..."]
 readme = "README.md"
-version = "0.1.0"
+version = "0.1.1"
```


So I call 
```make release BUMP=minor```,
 which runs this magic in `Makefile`
```make
# Make a release commit + tag, creating Changelog entry
# Set BUMP variable to any of supported (major, minor, patch)
# See 'bump-my-version show-bump' for options
# Variable BUMP defaults to 'patch' (v1.2.3 -> v1.2.4)
.PHONY: release
BUMP=patch
release:
	uvx bump-my-version bump ${BUMP}
```

And to remind ourselves of the patterns, we can print the possible future versions:
```shell
$  uvx bump-my-version show-bump
0.1.1 ── bump ─┬─ major ─ 1.0.0
               ├─ minor ─ 0.2.0
               ╰─ patch ─ 0.1.2
```
</details>

I hope others like me will feel this is good enough to be unblocked, and move their life over to uv, and Be Happy.

... but I'd still love to get this natively in uv if we can ;)

---

_Referenced in [aws-powertools/powertools-lambda-python#5624](../../aws-powertools/powertools-lambda-python/issues/5624.md) on 2025-02-06 11:49_

---

_Referenced in [yamatt/homoglyphs#16](../../yamatt/homoglyphs/pulls/16.md) on 2025-02-06 19:37_

---

_Comment by @kwaegel on 2025-02-10 19:44_

There are a number of workarounds posted so far, but I'll add one more to the list.

Target workflow.
* Package version is automatically derived from the last Git tag.
* `make version` prints the current package version.
* `make next-version $PART` prints the next version, with the {major, minor, patch} sections incremented. This is used to create a new tag when CI merges an appropriately tagged PR.

Implementation:
* I used the [uv-dynamic-versioning](https://github.com/ninoseki/uv-dynamic-versioning/) plugin, which works with the hatchling build backend. Seems like it's intended to be a drop-in replacement for `poetry-dynamic-versioning`. This causes `uv build` to build package files with the correct version.
* To print the current version, I use the [dunamai](https://github.com/mtkennerly/dunamai) CLI directly:
```
.PHONY: version
version:
	@uv run dunamai from git --bump --style pep440
```
* To bump the version, using `bump-my-version` seemed like a moderately heavyweight dependency, so I ended up using the [dunamai](https://github.com/mtkennerly/dunamai) API directly from a tiny Python script called from the Makefile:
```
.PHONY: next-version
## Compute the next package version, using NEXT={major|minor|patch}
next-version:
	#@uv run bump-my-version show new_version --increment $(NEXT)  # Alternative
	@uv run python next_version.py $(NEXT)
```
`next_version.py`:
```
import sys
from dunamai import Version
bump_type = sys.argv[1]
index = {"major": 0, "minor": 1, "patch": 2}[bump_type]
version = Version.from_git(strict=True)
bumped_version = version.bump(index=index).serialize(format="{major}.{minor}.{patch}")
print(bumped_version)
```

Keeping my fingers crossed that this only needs to be a "temporary" workaround, but I'm not going to let it stop me from migrating some of our legacy build systems over to uv.

---

_Referenced in [astral-sh/uv#11475](../../astral-sh/uv/issues/11475.md) on 2025-02-13 14:23_

---

_Referenced in [TechnologyBrewery/habushu#224](../../TechnologyBrewery/habushu/pulls/224.md) on 2025-02-13 20:22_

---

_Comment by @Blindstars on 2025-02-15 20:16_

+1 - Would use this all day long in replacement of poetry version.

---

_Referenced in [astral-sh/uv#11679](../../astral-sh/uv/issues/11679.md) on 2025-02-21 18:07_

---

_Comment by @vishnu-itachi on 2025-03-04 19:10_

Any idea on when this will be merged?. This is the only thing keeping me from switching to uv in my company. Don't want to pull poetry as dependency or use a custom script.

---

_Comment by @zanieb on 2025-03-04 19:59_

As previously mentioned, please don't post +1 comments (👍 the issue instead) or ask for updates (we will share updates as they come). There are many subscribers to this issue, let's keep notifications relevant and high quality.

I'm actively working on the design for this and would appreciate more context on _when_ and _how_ this command is used. 

1. Do you just need this on release?
2. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.
3. Are there other use-cases for changing the version?
4. When do you need to inspect the version? What do you do with the retrieved version?
5. Are you currently using `uv version` to inspect uv's version?
6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?
7. How do you currently decide if a version should be patch, minor, or major?
8. Do you need to set a specific version number instead of bumping? Why?

---

_Comment by @badrobit on 2025-03-04 20:14_

1. Do you just need this on release?
   - We usually do the bump manually during our PR process.
 
2. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.
   - my personal preference would be for a new command such as `uv bump-version --major|minor|patch`

3. Are there other use-cases for changing the version?
   - Probably but I think those would be outside a `bump-version` command and could be handled manually at least to start 🙂 
 
4. When do you need to inspect the version? What do you do with the retrieved version?
   - We do config control and it is used to set the version of the python package? 

5. Are you currently using `uv version` to inspect uv's version?
   - Yup 
 
6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?
   - semver 

7. How do you currently decide if a version should be patch, minor, or major?> 
   - its mostly an internal decision not sure if it could be automatically detected, usually we follow semvers guidance
  
8. Do you need to set a specific version number instead of bumping? Why?   
   - sometimes, almost always because a mistake was made, in this case for us requiring a manual edit to the `pyproject.toml` would be expected.

9. Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.
   - We do as part of some of our "in-house" tooling but I don't think this is something I would rely on `uv` providing the functionality for. that said if `uv` were to provide a pythonic interface that would be awesome but most of the time we just read it in as TOML and grab what we are looking for.

Hope this helps!! (edit was for formatting) (added response to other question)

---

_Comment by @slhck on 2025-03-04 20:21_

> 1. Do you just need this on release?

No, we might also want to get the current version directly without resorting to TOML parsing or `__init__.py` file grepping etc.

> 2. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

This would be nice to have but there may be many cases where people still do a lot of manual things between bumping the version and actually making the release, e.g. bumping first, creating a commit, creating the changelog, then git-amending that changelog etc.

> 3. Are there other use-cases for changing the version?

Not that I can think of.

> 4. When do you need to inspect the version? What do you do with the retrieved version?

We sometimes use it for other, more complex build scripts, or within CI runs.

> 5. Are you currently using `uv version` to inspect uv's version?

No, and I would have expected it to be `uv --version` like most other CLI tools do.

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

Semver exclusively.

> 7. How do you currently decide if a version should be patch, minor, or major?

This is mostly a manual decision.

However there are projects using conventional commits where you could infer this automatically based on the previous commit types (feature/fix/…).

> 8. Do you need to set a specific version number instead of bumping? Why?

We have some use cases where we have one project A's version be dependent on another project B's version, so if B gets released with a new version, _sometimes_ A will also get a new version. So we might just skip some versions in between.

---

_Comment by @HenriBlacksmith on 2025-03-04 22:40_

> 1. Do you just need this on release?

Yes.

> 2. If so, would your need be served by (as an example) uv release which does a version bump, uv build, and uv publish? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., uv release --prepare --bump-version minor then uv release. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

We usually bump the version from a manual CI job (GitLab CI), this creates a tagged commit which triggers a tag pipeline which does the build and release. If it is a library, then it is published to something like Artifactory.

We usually also bump a few other files configured via templates in the `.bumpversion.toml` file (like a `Chart.yaml` for Helm) or even in a bash script which creates the release changelog from an unreleased changelog

> 3. Are there other use-cases for changing the version?

No

> 4. When do you need to inspect the version? What do you do with the retrieved version?

Version is not inspected from `pyproject.toml` otherwise.

At runtime or in tests we read a `VERSION` file.
This file is maintained by bump-my-version too (as other versionned files)

> 5. Are you currently using uv version to inspect uv's version?

No but looks like a useful and natural interface. (Like `go version` or `terraform version`)

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

We use semver (`major.minor.patch`).

> 7. How do you currently decide if a version should be patch, minor, or major?

It is a manual action, as bump-my-version provides a way to bump each version part.
We have three manual GitLab CI jobs from main branch, we manually choose the right job.

> 8. Do you need to set a specific version number instead of bumping? Why?

No

---

_Comment by @zanieb on 2025-03-04 22:45_

Thanks for these quick responses! I want to add another question

9. Do you need to programmatically read or update other `pyproject.toml` fields? Like `project.authors`, `project.requires-python`, `build-system`, `tool.uv.*`, etc.

---

_Comment by @kwaegel on 2025-03-04 23:06_


> 1. Do you just need this on release?

No, we use git-based dynamic versioning via a hatch plugin (`hatch-vcs` or `uv-dynamic-versioning`) and want a way to print the project version without resorting to external tools or git tag parsing that could get out of sync with the plugin.

> 2. If so, would your need be served by (as an example) uv release which does a version bump, uv build, and uv publish? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., uv release --prepare --bump-version minor then uv release. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

Split commands would work better. I want to either get the current version, or get a bumped version string to externally create a new git tag, which then propagates back to dynamic version generation.

Something like `uv version` and `uv version bump patch` would be sufficient for my use case, though the latter would probably need a `--dry-run` flag or similar if it normally modifies the project file.

> 3. Are there other use-cases for changing the version?

No.

> 4. When do you need to inspect the version? What do you do with the retrieved version?

During CI (when triggered via a comment command to create a release), we get the bumped version string, create a git tag, then run the build that dynamically uses the git-derived version. Since it's on a tag, it is now exactly the bumped version string. For non-commit releases, it just reads the current version and posts a comment saying what development version is available for download.

> 5. Are you currently using uv version to inspect uv's version?

No.

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

Semver, in the form `x.y.z.devN+hash`. Builds on a git tag use the tag version, and builds not on a tag append the `.devN+hash` postfix to denote a development release.

> 7. How do you currently decide if a version should be patch, minor, or major?

Manually set by a Git comment command (e.g. `!patch`), which triggers a CI build.

> 8. Do you need to set a specific version number instead of bumping? Why?

No.

> 9. Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.

No.


---

_Comment by @HenriBlacksmith on 2025-03-04 23:36_

> Thanks for these quick responses! I want to add another question
> 
> 9. Do you need to programmatically read or update other `pyproject.toml` fields? Like `project.authors`, `project.requires-python`, `build-system`, `tool.uv.*`, etc.

The only field I could think of could be the `project.requires-python` as we have an homemade internal bot (somewhat similar to Renovate or Dependabot) which bumps our CI and runtime Docker Python images.

uv is part of our worker image to build the `uv.lock` files when updating Python packages.

But `uv` is not strictly required for that (TOML parsing lib would be enough as this bot touches non-Python related files like `Dockerfile` and `.gilab-ci.yml`). 


---

_Comment by @sstoefe on 2025-03-05 07:54_

We currently use a monorepo structure with one common library and 5 different applications that are using the common library. When we change the version in the common library we also update the version in all applications that use that library. Currently, we are using a manual approach with `bumpver` but we are looking for an alternative to do it automatically via CI.

> 1. Do you just need this on release?

No, we also need the current version sometimes in our CI environment. Currently, we grep it from __init__.py files.

> 2. If so, would your need be served by (as an example) uv release which does a version bump, uv build, and uv publish? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., uv release --prepare --bump-version minor then uv release. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

For our workflow we would also need a way to bump the version to a dirty/dev release since in our pipelines we also run end2end tests before we merge a PR (currently every PR leads to a new version of a component). In the end2end tests we also install the applications from our internal pypi. But since the version is not yet released we create development drops for those tests. When the PR gets merged we then finally release the actual version.

> 3. Are there other use-cases for changing the version?

Yes, as described in 2. Before we actually release a new version we currently replace the version in pyproject.toml and __init__.py in our CI with a `-dev` version (currently follows <major>.<minor>.<patch>-dev+pr<pr-number> to have unique drops per PR). Would also be cool if that was supported.

> 4. When do you need to inspect the version? What do you do with the retrieved version?

See 3. For creating a development drop.

> 5. Are you currently using uv version to inspect uv's version?

No

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

Semver `<major>.<minor>.<patch>(-dev+pr<pr-number>`

> 7. How do you currently decide if a version should be patch, minor, or major?

Currently, it's a manual step. Developer creates a PR that already has the bumped version. But this leads to problems since when another PR got merged there are merge conflicts and the version needs to be bumped again. Currently, we use `bumpver` for that.

> 8. Do you need to set a specific version number instead of bumping? Why?

Maybe for the `-dev` use case. But in general no.

> 9. Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.

No, currently I don't see a use case in our workflow.

---

_Comment by @Galtozzy on 2025-03-05 09:21_

> 1. Do you just need this on release?

No, we also need to get the version dynamically in CI and other checks. (`poetry version -s` is used).

> 2. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

I don't think so, I think it is best when those concepts are separated and controlled differently.

> 3. Are there other use-cases for changing the version?

We use version bumps manually at different stages of development.

> 4. When do you need to inspect the version? What do you do with the retrieved version?
Mostly in CI/CD:
- To alert which version broke e2e tests.
- To bake it into the built container.
- When a release is created through CI, the version is read and a tag is created with it.

> 5. Are you currently using `uv version` to inspect uv's version?

No.

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

We use semver. I think the concepts are the same, though I think there will have to be a template of some sort to derive the calver version format.

> 7. How do you currently decide if a version should be patch, minor, or major?

It's a manual action, so it depends on the work that was done.

> 8. Do you need to set a specific version number instead of bumping? Why?

Very rarely. When this is the case it's almost always to align separate apps to have the same version on a major release.

> 9. Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.

Currently we don't, but I can see where reading `requires-python` could come in handy.

---

_Comment by @krishan-patel001 on 2025-03-05 09:41_

> 1. Do you just need this on release?

No we need this throughout different stages of development.

> 3. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

No, as we bump versions on applications that aren't packages and therefore don't need to be built or published. 

> 5. Are there other use-cases for changing the version?

Yes, for unique github releases and tracking what version of code broke anything.

> 7. When do you need to inspect the version? What do you do with the retrieved version?

We use the package version when creating releases. We don't use uv's version for anything.

> 9. Are you currently using `uv version` to inspect uv's version?
No

> 11. Are you using semver? Are you using calver? How would you expect this to work in a calver project?
We use semver

> 13. How do you currently decide if a version should be patch, minor, or major?

Dependent on if we are doing a breaking change or not.

> 15. Do you need to set a specific version number instead of bumping? Why?

To set unique dev versions per developer during development.

---

_Comment by @nim65s on 2025-03-05 10:19_

> 1. Do you just need this on release?

Yes

> 2. If so, would your need be served by (as an example) `uv release` which does a version bump, `uv build`, and `uv publish`? As a minor note, if we go this direction we'd definitely need a way to separate version bumps from build for things like committing that change separately, e.g., `uv release --prepare --bump-version minor` then `uv release`. The goal here isn't to design the final interface, but explore the concept, so take the examples with a grain of salt.

I would prefer a way to let me tag a release, and let that tag trigger a github action that would build (for multiple OS / arch / python) and publish (throug Trusted Publisher workflow)

> 3. Are there other use-cases for changing the version?

No

> 4. When do you need to inspect the version? What do you do with the retrieved version?

I need both the current and updated ones to generate a commit which update the version number, date, and diff link in the changelog. I would not have to bake a script for that if uv provided hooks like [cargo-release](https://github.com/crate-ci/cargo-release)

> 5. Are you currently using `uv version` to inspect uv's version?

No, `uv --version` seems more natural for that

> 6. Are you using semver? Are you using calver? How would you expect this to work in a calver project?

Yes, yes (different projects).  eg., given pyproject `tool.uv.version-scheme = "YYYY.MINOR"` (`"semver'` could be an alias for `"MAJOR.MINOR.PATCH"`)
```
uv version # show current
uv version {major,minor,patch,micro} # bump a version component
uv version bump # bump date based components
```

> 7. How do you currently decide if a version should be patch, minor, or major?

breaking changes / new features / any other reason

> 8. Do you need to set a specific version number instead of bumping? Why?

No

> Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.

No

---

_Comment by @ebits21 on 2025-03-06 01:57_

> Do you just need this on release?

No

> Are there other use-cases for changing the version?

No

> When do you need to inspect the version? What do you do with the retrieved version?

Just for my personal knowledge.

> Are you currently using uv version to inspect uv's version?

Rarely, as others have said `uv —version` seems more appropriate.

> Are you using semver? Are you using calver? How would you expect this to work in a calver project?

Both. Calver I personally would want it to bump to the current date as yyyy.mm.dd but I’m sure there’s many ways to do this.

> How do you currently decide if a version should be patch, minor, or major?

Major: breaking changes / minor: new features / patch: anything else.

> Do you need to set a specific version number instead of bumping? Why?

Sometimes but I would just set it manually

>Do you need to programmatically read or update other pyproject.toml fields? Like project.authors, project.requires-python, build-system, tool.uv.*, etc.

No

---

_Comment by @daviewales on 2025-03-07 00:47_

This is what my current process looks like with Poetry.
I'd be hoping to replicate something like this with UV.

``` sh
poetry version # show current version from pyproject.toml (includes project name)
poetry version minor # bump minor version in pyproject.toml
git tag "v$(poetry version -s)" # add git tag with current version from pyproject.toml (-s flag shows only the version number)
```

1. I don't just need this on release.
2. An all-in-one `uv release` command wouldn't suit my workflow. (For example, my PyPI deployments are automated with GitHub Actions which trigger when I push new version tags. So all I need is a way to bump the version and include it in a git tag.)
3. I'm not sure what other usecases would be for changing the version?
4. I sometimes want to interactively view the current version, without needing to inspect the `pyproject.toml` file. Other times, I want to fetch just the version number to include in a git tag.
5. I do not use `uv version`. `uv --version` and `uv -V` are sufficient.
6. I'm using semver. Note for calver that `poetry version` accepts _either_ a valid [PEP 440 version](https://peps.python.org/pep-0440/) or a [valid bump rule](https://python-poetry.org/docs/cli/#version). Because the bump rules are not valid PEP 440 versions, this overloading works fine. So, for calver, a `poetry` user could set the version to the current date with: `poetry version $(date +%Y.%m.%d)` (This would work for `rye version` too.)
7. Most of my projects are still in the `0.1.0` phase of semver, so breaking changes are `minor` and all other changes are `patch`. For projects which make it to `1.0.0`, then breaking changes would be `major`, big non-breaking changes (new features) would `minor`, and everything else would be `patch`.
8. I don't usually need to set a specific version number instead of bumping, but I can imagine that this would be a useful escape hatch.
9. I haven't needed to programmatically read or update other `pyproject.toml` fields.

I may be biased as an existing Poetry user, but I think they get the API pretty right for [poetry version](https://python-poetry.org/docs/cli/#version).

---

_Comment by @HenriBlacksmith on 2025-03-07 02:12_

A few suggestions/additions: 
- bump-my-version provides a useful and visual `show-bump` command which would be a nice addition to the interface: https://github.com/callowayproject/bump-my-version?tab=readme-ov-file#visualize-the-versioning-path
- As the potential need to read more fields from the `pyproject.toml` has been mentioned and the conflict with existing `uv version` is a concern, maybe using `uv project version` could be an option

This could give commands like:
- `uv project version bump minor`
- `uv project version show` or even simply `uv project version`
- `uv project version show-bump`
- `uv project add author` (why not?)

The commands are a bit longer though (but those can easily be aliased).

---

_Referenced in [astral-sh/uv#12055](../../astral-sh/uv/issues/12055.md) on 2025-03-07 18:56_

---

_Referenced in [astral-sh/uv#12349](../../astral-sh/uv/pulls/12349.md) on 2025-03-20 21:11_

---

_Comment by @Gankra on 2025-03-20 21:49_

We've got a PR up for an interface/implementation we're reasonably happy with: https://github.com/astral-sh/uv/pull/12349

We might still make `uv version` sugar for `uv metadata version` if people really want but for now it's cleanest to not change the semantics of an existing command, and we want this metadata namespace claimed.

---

_Comment by @ebits21 on 2025-03-21 19:53_

> We've got a PR up for an interface/implementation we're reasonably happy with: [#12349](https://github.com/astral-sh/uv/pull/12349)
> 
> We might still make `uv version` sugar for `uv metadata version` if people really want but for now it's cleanest to not change the semantics of an existing command, and we want this metadata namespace claimed.

Honest feedback: it’s pretty verbose command wise. I would love the uv version sugar as my 2 cents of feedback.

I personally like just uv bump as well.

Thanks for adding this! Very appreciative.

---

_Comment by @zanieb on 2025-03-21 19:55_

`uv bump` doesn't really make sense for the use-case of reading the version.

We're not particularly settled between `uv metadata version` and `uv version` though.

---

_Comment by @ebits21 on 2025-03-21 20:08_

> `uv bump` doesn't really make sense for the use-case of reading the version.
> 
> We're not particularly settled between `uv metadata version` and `uv version` though.

I get it.

I do think `uv self version` for uv’s version and `uv version —bump` is quite elegant.

---

_Comment by @daviewales on 2025-03-22 03:00_

> I do think uv self version for uv’s version and uv version —bump is quite elegant.

I feel that `uv --version` is better than `uv self version`.

`uv version --bump` is more explicit than `uv version [patch|minor|major|version_string]` (like Poetry), but I still think the latter is the most elegant, at the expense of disallowing versions called 'patch', 'minor' or 'major'.

---

_Comment by @zanieb on 2025-03-23 03:14_

`uv --version` isn't going anywhere — we just also have a whole `uv self` interface and that's where I'd expect a long-form command for uv to report its own version to be.

---

_Comment by @dusktreader on 2025-03-24 01:16_

I think that there would be unnecessary confusion by having a subcommand named `version` _and_ a root-command `--version` option. You're going to find yourself trying to help people understand the difference between `uv version` and `uv --version` _constantly_.

I think it makes a lot of sense to have a `uv metadata version` subcommand, but I have to say that typing out `metadata` every time is going to be irritating. What about shorting `metadata` to just `meta`. It still conveys the idea well but is much nicer to type.

I would be very excited about being able to type `uv meta version --bump=minor` in my projects soon.

---

_Comment by @slingshotsys on 2025-03-24 01:33_

One more vote for shorter being better. As the uv command I will probably use the most, I'd much rather type `uv version minor` than `uv metadata version --bump minor` every time.

`uv --version` or `uv -v/-V` or `uv self version` all seem very intuitive to me and `--version/-v/-V` in particular align with how `poetry`, `npm`, and `python` all work.

---

_Comment by @HenriBlacksmith on 2025-03-24 10:00_

> I think that there would be unnecessary confusion by having a subcommand named `version` _and_ a root-command `--version` option. You're going to find yourself trying to help people understand the difference between `uv version` and `uv --version` _constantly_.
> 
> I think it makes a lot of sense to have a `uv metadata version` subcommand, but I have to say that typing out `metadata` every time is going to be irritating. What about shorting `metadata` to just `meta`. It still conveys the idea well but is much nicer to type.
> 
> I would be very excited about being able to type `uv meta version --bump=minor` in my projects soon.

The command syntax is not as important as the feature, setting aliases is quite easy for those who need them.

And for my use-cases, it will be used in CI hence typing the command is not a question.

And the coherence with Cargo is interesting too: https://doc.rust-lang.org/cargo/commands/cargo-metadata.html

Having more automations around that command would be awesome (optional/configurable `git tag` and `commit` on bump), having a way to template other files, etc.

---

_Comment by @chrisrodrigue on 2025-03-24 10:07_

`--version`/`-V` is probably the most universal command line parameter there is (next to `--help`/`-h`). `uv self version` would be a welcome complement to the legacy `--version` parameter.

In case you’re unfamiliar, or if it’s been a while and you’ve forgotten, run this in your terminal:
```bash
python3 -c "import this"
```
I tend to concur with all of it, especially the last line. 

The `uv metadata` namespace is a step in the right direction, but "metadata" is ambiguous. Which metadata are we talking about, uv metadata or project metadata? If we are targeting a project, `uv project version` might better reflect the intent.

In the same manner as `uv tool`, the `uv project` namespace could be added to collect all the commands that target projects (`uv project add`, `uv project sync`, `uv project init`, etc.), but the top-level commands should still be available since they make  the common case fast.

In the case of `uv version`, I think that it could print the version of the project first, followed by the version of uv, along with some type of hint promoting the namespaced commands `uv self version` and `uv project version`.

The [Concepts overview](https://docs.astral.sh/uv/concepts/) page gives a good list of what might be desirable to capture as command namespaces:
- Projects (i.e. `uv project version` and `uv project sync`)
- Tools (i.e. `uv tool add`)
- Python (i.e. `uv python list`)



---

_Comment by @mkk5 on 2025-03-31 18:49_

I didn’t have experience with `poetry version`, so I might be missing something, but what is the advantage of 

> pyproject.toml file serving as the single source of truth for a project's version

 instead of using a git tag as the single version source, which, in my opinion, has certain advantages?

---

_Comment by @slhck on 2025-03-31 19:21_

I'd say: Because projects are not always shared as Git repositories (in particular the actual Python distributions), and not everyone might use Git. The pyproject.toml file should be a single source of truth for the project and its metadata.

---

_Comment by @kwaegel on 2025-04-01 01:15_

> The current implementation errors the command on all operations if the version field is not defined (which I believe would be the case if dynamic = ["version"]).

If I'm reading the current PR correctly, it looks like it won't be able to read the dynamic version generated by a plugin? That's a bit unfortunate, since I'm not entirely sure how to extract that bit of information without using external tools that could get out of sync with the project file (e.g. `uv run dunamai ...`).

If the `uv metadata version` command can't/won't handle dynamically generated version strings, is there a best practice on how to handle that?

---

_Comment by @codejunction on 2025-04-02 20:00_

Just a thought based on https://pypi.org/project/bumpver/
1. `uv project bump --major` - for major version udpate. Eg: 1.9.11 -> 2.0.0
2. `uv project bump --minor` - for minor version update. Eg: 1.8.11 -> 1.9.0
3. `uv project bump --micro/--patch` - for micro or patch update. Eg: 1.9.11 -> 1.9.12



---

_Referenced in [astral-sh/uv#12749](../../astral-sh/uv/issues/12749.md) on 2025-04-08 16:50_

---

_Closed by @zanieb on 2025-04-29 21:37_

---

_Referenced in [Homebrew/homebrew-core#221925](../../Homebrew/homebrew-core/pulls/221925.md) on 2025-04-29 23:35_

---

_Referenced in [wntrblm/nox#953](../../wntrblm/nox/issues/953.md) on 2025-04-30 02:18_

---

_Comment by @Francesco146 on 2025-04-30 19:36_

> For others who need this.
> 
> uvx --from=toml-cli toml set --toml-path=pyproject.toml project.version $VERSION
> 
> I'm using it like this in github action.
> 
> VERSION=$(uvx dunamai from any --no-metadata --style pep440)
> uvx --from=toml-cli toml set --toml-path=pyproject.toml project.version $VERSION
> uv build

since https://github.com/astral-sh/uv/pull/12349 got merged, what's the new standard way to handle version bumps in github action?

---

_Comment by @Gankra on 2025-04-30 19:46_

I believe what you posted is, as of 0.7.1, equivalent to `uv version $VERSION`

---

_Comment by @lurebat on 2025-05-18 21:53_

1. #7785 was closed as a duplicate of this - however, the current implementation doesn't let you access the version from your package's code. Should I open a new issue?
2. Unlike most commands, this command doesn't support workspaces. Again, is this a separate issue to open? 
Thanks

---

_Comment by @charliermarsh on 2025-05-18 21:54_

What does "access the version from your package's code" mean?

---

_Comment by @lurebat on 2025-05-18 22:00_

See for example - "dynamic project version" from [pdm](https://backend.pdm-project.org/metadata/#dynamic-project-version) 

Basically I want to be able to access the "version" value from the pytoml in my code, so I could, for example, log it.

---

_Comment by @slhck on 2025-05-19 06:47_

Couldn't you get that via [`importlib.metadata.version`](https://docs.python.org/3/library/importlib.metadata.html#importlib.metadata.version)? It takes the version value from the `pyproject.toml` package:

```python
version('my-package')
```

Then you would only have to specify it there.

---

_Comment by @jamesmyatt on 2025-05-19 11:44_

> Couldn't you get that via [`importlib.metadata.version`](https://docs.python.org/3/library/importlib.metadata.html#importlib.metadata.version)? It takes the version value from the `pyproject.toml` package:
> 
> version('my-package')
> 
> Then you would only have to specify it there.

This is the way. See https://packaging.python.org/en/latest/discussions/single-source-version/.

---

_Comment by @lurebat on 2025-05-19 12:15_

Thank you - very useful.

Now the question of running the command on specific packages or the entire workspace.

Or even having just a singular version for a workspace, that the child packages inherit

---

_Comment by @zanieb on 2025-05-19 13:41_

Please open new issues for both of these questions. You're pinging hundreds of people subscribed here.

---

_Referenced in [encord-team/encord-client-python#943](../../encord-team/encord-client-python/pulls/943.md) on 2025-06-27 12:26_

---

_Referenced in [jakubplichta/grafana-dashboard-builder#227](../../jakubplichta/grafana-dashboard-builder/issues/227.md) on 2025-10-12 21:28_

---

_Referenced in [aaronclong/xspfify#2](../../aaronclong/xspfify/pulls/2.md) on 2025-11-22 01:00_

---
