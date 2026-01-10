```yaml
number: 5605
title: Documentation feedback
type: issue
state: open
author: zanieb
labels:
  - documentation
  - tracking
assignees: []
created_at: 2024-07-30T16:35:02Z
updated_at: 2025-11-20T14:54:55Z
url: https://github.com/astral-sh/uv/issues/5605
synced_at: 2026-01-10T03:23:52Z
```

# Documentation feedback

---

_Issue opened by @zanieb on 2024-07-30 16:35_

This is a tracking issue for feedback on the new documentation at https://docs.astral.sh/uv/

---

_Label `tracking` added by @zanieb on 2024-07-30 16:35_

---

_Assigned to @zanieb by @zanieb on 2024-07-30 16:35_

---

_Label `documentation` added by @zanieb on 2024-07-30 17:00_

---

_Comment by @dmfigol on 2024-08-16 22:47_

https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences
it is not clear with what subcommand i can use `--python-preference only-managed` to set it permanently.

---

_Comment by @zanieb on 2024-08-16 22:50_

We don't have a commands to modify a persistent configuration file — you can put it in a `uv.toml` or `pyproject.toml` per

- https://docs.astral.sh/uv/configuration/files/
- https://docs.astral.sh/uv/reference/settings/#python-preference

Thanks for the feedback though! Sounds like we should link to the persistent configuration documentation here.

---

_Comment by @shoucandanghehe on 2024-08-21 00:55_

A little suggestion: add the use of `generate-shell-completion` to `First steps with uv`

I habitually use --help to see what commands I can use.
Then I realized that there was no `completion` command, and didn't search for it until I want to open an issue!😝

PS: I don't understand why it should be hidden in --help. Except for this command, the outputs of `help` and `--help` are almost identical!

---

_Comment by @zanieb on 2024-08-21 03:06_

I'm not sure why it's hidden — I copied this from Ruff. I think it might be because it shifts the indent for the rest of the commands _way_ to the right and dramatically reduces the space we have for concise documentation.

Thanks for the feedback! Tracked in https://github.com/astral-sh/uv/issues/6153

---

_Comment by @zanieb on 2024-08-21 17:09_

Maybe we can improve the `uvx` to `uv tool` transition in the guide https://github.com/astral-sh/uv/issues/6334#issuecomment-2302439899

---

_Comment by @gusutabopb on 2024-08-22 08:24_

On [`getting-started/installation/#standalone-installer`](https://docs.astral.sh/uv/getting-started/installation/#standalone-installer) it says 

  > When uv is installed via the standalone installer, self-updates are enabled
   
  This sounded to me as if `uv` would automatically update itself (like many GUI apps do), but it seems this is not the case. I guess it just means `uv self update` is not available at all unless the standalone installer was used. I think wording here could be improved to clarify that.
   
  By the way, I added this to my `crontab` to get auto-updates: 
  ```cron
  00 00 * * * uv self update
  ```

---

_Comment by @minusf on 2024-08-22 11:22_

Minor confusion for me in https://docs.astral.sh/uv/concepts/tools/, emphasis mine:

> Tools can also be installed with `uv tool install`, in which case their executables are [available on the PATH](https://docs.astral.sh/uv/concepts/tools/#the-path) — an isolated virtual environment is still used but it is **not treated as disposable**.

...

> When running a tool with `uvx` or `uv tool run`, a virtual environment is stored in the uv cache directory and is **treated as disposable**.

For `uv tool`, the venv is, or is not treated as disposable?

Furthermore, what does it mean "disposable" in this context?

In my first reading I understood `uvx` only as a convenience alias to `uv tool run`. However they seem to create different types of venvs in different locations in different ways and I am not able to understand the difference after reading this concept page...

If I do `uvx posting` and `uv tool install posting && uv tool run posting`, what is the conceptual difference and which one should i use?

---

_Comment by @prrao87 on 2024-08-22 12:56_

> Maybe we can improve the uvx to uv tool transition in the guide https://github.com/astral-sh/uv/issues/6334#issuecomment-2302439899

One suggestion would be to reiterate in the [CLI reference docs](https://docs.astral.sh/uv/reference/cli/#uv-tool-run) for `uv tool run` that it's also available as `uvx`. Users might end up there via google searches and not be aware that there's an alias.

Another thing is that in the [tool concepts](https://docs.astral.sh/uv/concepts/tools/) page, it directly references `uvx` in these ways:
> Tools can be invoked without installation using uvx ...
> When running a tool with uvx or uv tool run, a virtual environment is stored...

In both these cases, the implicit assumption is that the user knows that `uvx` is an alias for only `uv tool run` and that it invokes the tool instead of installing it (hence the `x`, rather than `r`, which is what one would intuitively think would be the alias). But the only way to know these distinctions is to have carefully read the [tools guide](https://docs.astral.sh/uv/guides/tools/) section of the docs first, and not all users may end up on the guide page first and read things in the exact order they're shown in the sidebar 😅.


---

_Comment by @zanieb on 2024-08-22 15:49_

(Thanks for the feedback everyone, I'll attempt to address all that)

We should talk about defining constraints in the `pyproject.toml` in the project concept per https://github.com/astral-sh/uv/issues/6425

---

_Comment by @zanieb on 2024-08-22 23:52_

@gusutabopb let me know if #6468 is sufficient! 

@minusf and @prrao87 I've attempted to address those in:

- https://github.com/astral-sh/uv/pull/6474 and
- https://github.com/astral-sh/uv/pull/6455


---

_Comment by @alandelevieTR on 2024-08-28 13:50_

Hi! I hope this is the correct place, but I would request examples for:

```
Other indexes[#](https://docs.astral.sh/uv/#other-indexes)

uv is also known to work with JFrog's Artifactory, the Google Cloud Artifact Registry, and AWS Code Artifact.
```

I ask because this does not work from my existing pyproject.toml:

```
my-custom-package = {version = "0.0.5", source = "my-pip-repo-happens-to-be-gcp-artifact-registry"}
# ...

[[tool.poetry.source]]
name = "my-pip-repo-happens-to-be-gcp-artifact-registry"
url = "https://us-west2-python.pkg.dev/my-gcp-org/hello/simple/"
priority = "explicit"
```

I add auth to the config by running:

```
$ pip install keyring
$ pip install keyrings.google-artifactregistry-auth
$ gcloud auth application-default login
$ poetry config http-basic.my-pip-repo-happens-to-be-gcp-artifact-registry oauth2accesstoken "$(gcloud auth print-access-token)"
```

Finding the equivalent of that last line for `uv` is what has me stumped.


I'm similarly interested in jfrog examples, but this is the one I can provide the most specific details on.

---

_Comment by @gusutabopb on 2024-08-30 00:21_

The FastAPI guide needs to be updated to reflect the changes introduced to the default behavior of `uv init` in version 0.4.0. The current docs still say a `src` -based layout would be created: https://docs.astral.sh/uv/guides/integration/fastapi/#initializing-a-fastapi-project

---

_Comment by @devmcp on 2024-09-05 09:07_

`uv` can be installed on Windows using `winget`, but this isn't mentioned in the docs. Could/should it be added as an alternative installation method along with homebrew etc?

---

_Comment by @alper on 2024-09-10 08:57_

I'm not sure why there's a [python-version](https://docs.astral.sh/uv/guides/projects/#python-version) file if this information is also in the `pyproject.toml`.

---

_Comment by @shunichironomura on 2024-09-19 00:25_

The documentation about running scripts using the inline metadata ([link](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies)) doesn't mention support for specifying the dependency sources via the `tool.uv.sources` section, but it does seem to support it when I run, for example:

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [
#     "requests",
# ]
# [tool.uv.sources]
# requests = { git = "https://github.com/psf/requests.git", tag = "v2.32.2" }
# ///

import requests

print(requests.__version__) # 2.32.2 (2.32.3 is the latest)
```

I think the documentation can be explicit about officially supporting (or not supporting) it.

---

_Comment by @zanieb on 2024-09-19 00:26_

@shunichironomura thanks! I think we need to create a separate "Scripts" concept page because that's way too advanced for the "guide" documentation.

---

_Comment by @scimas on 2024-09-23 18:02_

Would be nice to have information on whether virtual workspaces (no `project` and `build-system` sections in `pyproject.toml`?) like Cargo are supported.

---

_Comment by @charliermarsh on 2024-09-23 18:03_

You might be looking for this: https://docs.astral.sh/uv/concepts/projects/#applications

---

_Comment by @scimas on 2024-09-23 18:11_

That is still an application that has its own python code from what I can tell. The [Cargo virtual workspaces](https://doc.rust-lang.org/cargo/reference/workspaces.html#virtual-workspace) just combine related packages together, where there isn't necessarily one "main" binary.

Taking from the workspace example in uv docs,
```
albatross
├── packages
│   ├── bird-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── bird_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   ├──squirrel-feeder
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── squirrel_feeder
│   │           ├── __init__.py
│   │           └── foo.py
│   └── seeds
│       ├── pyproject.toml
│       └── src
│           └── seeds
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
└── uv.lock
```
imagine `bird-feeder` and `squirrel-feeder` were equally important packages with the common `seeds` dependency.

And just to be clear, I'm not asking this feature to be implemented, only clarification on whether or not it is supported because the documentation page explicitly refers to Cargo.

---

_Comment by @willmurphyscode on 2024-09-24 00:02_

I have a couple questions about the lockfile after reading the docs on it.

Is the `uv.lock` file specified anywhere? I looked at https://docs.astral.sh/uv/concepts/projects/#project-lockfile and expected to find a schema or a link to a schema or specification for what can go in `uv.lock`.

The reason I'm looking for a schema / specification is that I would like to be able to parse `uv.lock` files so that I can add a `uv.lock` parser [Syft](https://github.com/anchore/syft). 

My other question is whether you expect the `uv.lock` file to remain stable, or whether there is planned work to change or extend its schema.

---

_Comment by @astrojuanlu on 2024-09-24 07:56_

> The [Cargo virtual workspaces](https://doc.rust-lang.org/cargo/reference/workspaces.html#virtual-workspace) just combine related packages together, where there isn't necessarily one "main" binary.

@scimas Virtual projects were removed in #6720 (from the docs at least - cannot quickly find other pointers)

---

_Comment by @charliermarsh on 2024-09-24 12:06_

@scimas -- Yeah that layout is fully supported. You can create a `pyproject.toml` at the root that is not itself linked to any Python code, and just lists workspace members and dependencies, i.e., a virtual workspace root as in Cargo.

@astrojuanlu -- We removed most mentions of "virtual" since it wasn't a familiar concept, but the idea of a project that just lists members and dependencies is still supported.

---

_Comment by @charliermarsh on 2024-09-24 12:07_

@willmurphyscode -- It's not documented anywhere, only implicitly in code. We don't have any planned changes but I'm sure it will change over time (it's also versioned). Separately, if [PEP 751](https://peps.python.org/pep-0751/) is approved (which I've been very involved in), we'll likely migrate to whatever the approved format is, if it works for uv.

---

_Comment by @jonasdeyson on 2024-09-25 20:10_

The section [Non-editable installs](https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs) led me to a mistake.

It mentions the following:

> By default, uv installs projects and workspace members in editable mode...

And then:

> In the context of a multi-stage Docker image, --no-editable can be used to include the project in the synced virtual environment from one stage, then copy the virtual environment alone (and not the source code) into the final image.

The problem is that, if I understand correctly, this is only true if the project was created as a library (`uv init --lib`).
An app project doesn't seem to be installed as editable by default in the venv (contrary to what the previous quote says).
Since the example Dockerfile shown there is clearly for an application, it got me confused for a while.

I suggest to make it more clear that only library projects are installed in the environment, regardless of the flag `--no-editable`.

---

_Comment by @debnath-d on 2024-10-05 21:04_

https://github.com/astral-sh/uv/issues/5605#issuecomment-2340069888 :
> I'm not sure why there's a [python-version](https://docs.astral.sh/uv/guides/projects/#python-version) file if this information is also in the `pyproject.toml`.

@zanieb I would also like to know this.

---

_Comment by @zanieb on 2024-10-06 14:40_

@debnath-d The `.python-version` file can be read by other tools like GitHub Actions to determine which version to install. The version file pins to a single version whereas the `pyproject.toml` field is intended to define a _range_ of versions supported by your application or library (per the Python standards).

---

_Comment by @AndreuCodina on 2024-10-06 15:36_

> @debnath-d The `.python-version` file can be read by other tools like GitHub Actions to determine which version to install. The version file pins to a single version whereas the `pyproject.toml` field is intended to define a _range_ of versions supported by your application or library (per the Python standards).

For me, if you use `uv init --lib`, it makes sense to have `requires-python` and `.python-version`. But if you use `uv init --app`, I think it isn't useful (`requires-python` is not required and you already have `.python-version`).

It's the same feeling I have when I use `uv init --app` and the `description` field is added to my application with a default value I'll never use.

If `requires-python` could be used by child packages it'd be okay, but when you run `uv init --lib` inside a project, another `pyproject.toml` with a `requires-python` is created. For example, in my .NET repositories, I define the .NET version of all packages in a root file (similar to `pyproject.toml`).

BTW, `pyproject.toml` can also be used with GitHub Actions.

---

_Comment by @bongomachine on 2024-10-08 08:34_

It would be great if you:
1. Add some example(s) of a larger and more complex project/workspace.
2. If you add an example of how to use Hatchling build hooks.

---

_Comment by @mdegis on 2024-10-11 09:41_

I think it would be great if there a page about how to migrate from poetry.

---

_Comment by @Rogdham on 2024-10-13 17:14_

In the [documentation about configuration](https://docs.astral.sh/uv/reference/settings/#configuration), I find it confusing that many settings can be set in both `tool.uv` and `tool.uv.pip` (and they seem to have the same description).

What is the difference? Is there inheritance? Should I set them in both places?

<details><summary>List of the 23 impacted settings at the time of writing</summary>
<p>

- allow-insecure-host
- compile-bytecode
- config-settings
- dependency-metadata
- exclude-newer
- extra-index-url
- find-links
- index-strategy
- index-url
- keyring-provider
- link-mode
- no-binary
- no-build
- no-build-isolation
- no-build-isolation-package
- no-index
- no-sources
- prerelease
- reinstall
- reinstall-package
- resolution
- upgrade
- upgrade-package

</p>
</details> 

---

_Comment by @zanieb on 2024-10-13 19:27_

@mdegis you can track that at #5200 

@Rogdham please see https://docs.astral.sh/uv/configuration/files/#configuring-the-pip-interface

---

_Comment by @yaleman on 2024-10-15 00:55_

It'd be good to mention in the [working on projects](https://docs.astral.sh/uv/guides/projects/) section of the guide, perhaps under the "running commands" part, that you need to include a build system or the library won't be available. This is a pretty confusing footgun that lost me most of a day while trying to work out why `pytest` wouldn't find my library. 

The only mention of this I can find is an easy-to-misread sentence under the [build systems](https://docs.astral.sh/uv/concepts/projects/#build-systems) part of `Concepts -> Projects`.

---

_Comment by @zanieb on 2024-10-15 00:57_

👍 sorry to hear that @yaleman — that change was made after we wrote most of the documentation and I've been meaning to rewrite the project documentation to account for that.

---

_Comment by @Rogdham on 2024-10-15 10:13_

> @Rogdham please see https://docs.astral.sh/uv/configuration/files/#configuring-the-pip-interface

Thank you for this, much helpful! Maybe just add a link to it in https://docs.astral.sh/uv/configuration/files/#configuring-the-pip-interface?

A small note in each field of pip config saying that it inherits would be ideal, but I understand that this is quite some work for little benefit.

---

_Comment by @powercoconola on 2024-10-16 23:09_

Thanks for your work on the project.
It would greatly help to write a way to change these environment variables. I am a bit lost how to change `UV_TOOL_DIR` to something different. In other words, change the result of `uv tool dir`
https://docs.astral.sh/uv/configuration/environment/

Here in the documentation it says to change the environment variable but nowhere on how to do that
https://docs.astral.sh/uv/concepts/tools/#tools-directory

---

_Comment by @powercoconola on 2024-10-24 00:03_

https://docs.astral.sh/uv/guides/tools/
In this page, the documentation reads
"The uvx command invokes a tool **without installing** it."
Then a few lines down it says:
"When uvx ruff is invoked, uv **installs** the ruff package which provides the ruff command."
So does uvx install the package or not? Thank you for your work on this project :)

---

_Comment by @merlinz01 on 2024-10-24 02:58_

https://docs.astral.sh/uv/guides/integration/github/

Do I have to specify `enable-cache` in a separate caching workflow step, or do I simply add it to the original `setup-uv` step? If it is a separate step, where does it go in relation to the other steps?

```yaml
- name: Enable caching
  uses: astral-sh/setup-uv@v3
  with:
    enable-cache: true
```

Will this cache both Python and dependencies?

---

_Comment by @bjornasm on 2024-10-24 09:14_

How do I actually work with the uv.lock file?

I have pushed it to the repo, and are now pulling it on another computer. If I do uv.sync then the uv.lock file is emptied to reflect (the empty) pyproj.toml. But I want the opposite, that all the packages in uv.lock are installed to pyproj.toml?

---

_Comment by @merlinz01 on 2024-10-24 12:11_

The dependencies are primarily specified in pyproject.toml, then `uv sync` updates uv.lock with the exact versions of what it installed. The uv.lock file is not meant to be manually edited.

---

_Comment by @nolanshah on 2024-10-27 19:35_

In addition to the poetry migration guide requested above, migration guides for pipenv, requirements.txt, and even direct from the environment would be helpful. It took too long to find that the `-r requirements.txt` semantics from `pip install` works with `uv add`.

Really a nit -- The concept pages are very in the weeds with details about the CLI. Being more opinionated on the patterns/anti-patterns of python project/dep/version/tool management with uv and focusing on concepts that influence user decisions (e.g. do I use `ux tool` or install a dev dependency) in the concepts guides would be helpful. Like for [tools](https://docs.astral.sh/uv/concepts/tools/), the "Execution vs installation" section is the really critical "concept" there. 

---

_Comment by @astrojuanlu on 2024-10-30 13:08_

The ["when (not) to use workspaces"](https://docs.astral.sh/uv/concepts/workspaces/#when-not-to-use-workspaces) section conflates

- Actual use cases for workspaces:

https://github.com/astral-sh/uv/blob/94fc35edd91bf9773470db08a82925c066693f6d/docs/concepts/workspaces.md?plain=1#L152-L165

- Use cases _not_ well suited for workspaces

https://github.com/astral-sh/uv/blob/94fc35edd91bf9773470db08a82925c066693f6d/docs/concepts/workspaces.md?plain=1#L167-L168

It would be nice if the latter were more clearly explained, specifically the "independent/loosely related packages with possibly conflicting dependencies". The docs give an example of a "path dependency", but that seems to be targeted to a root member depending on a subpackage, which maybe isn't even needed. And the potential overlap with Dependency Groups is unclear.

Maybe a dedicated page (and, dare I say, another name?)

---

_Comment by @bjornasm on 2024-10-30 15:28_

> The dependencies are primarily specified in pyproject.toml, then `uv sync` updates uv.lock with the exact versions of what it installed. The uv.lock file is not meant to be manually edited.

My question was not how to edit the lock file, but how to let it dictate the dependencies of a project.

---

_Comment by @charliermarsh on 2024-10-30 15:29_

I don't quite understand. The lockfile is created from the `pyproject.toml`. The `pyproject.toml` dictates the dependencies of the project; the lockfile is downstream of that.

---

_Comment by @bjornasm on 2024-10-30 22:25_

@charliermarsh Thank you for taking the time, and sorry for creating confusion. 

From the documentation:

> Unlike the pyproject.toml which is used to specify the broad requirements of your project, the lockfile contains the exact resolved versions that are installed in the project environment. This file should be checked into version control, allowing for consistent and reproducible installations across machines.

I understand how this important for two reason, knowing the exact packages and version used for every commit in the project, and if anyone want to fork or clone a project and replicate the environment.

So say that I fork SomeRepo, that contains a uv.lock, to a new machine and want to have the exact resolved versions that was installed in the project environment by the creator of SomeRepo. How do I interact with uv.lock to replicate the project environment? 

Reading the documentation it is described how uv.lock is generated (or how we avoid it to be changed), but it doesn't detail how uv.lock is used by uv. (Is there for instance a `uv add uv.lock` or `uv init uv.lock`)



---

_Comment by @dylwil3 on 2024-11-06 17:57_

> So say that I fork SomeRepo, that contains a uv.lock, to a new machine and want to have the exact resolved versions that was installed in the project environment by the creator of SomeRepo. How do I interact with uv.lock to replicate the project environment?

Maybe [`uv sync --frozen`](https://docs.astral.sh/uv/reference/cli/#uv-sync) is what you're looking for @bjornasm? 

---

_Comment by @TonyYanOnFire on 2024-11-07 07:30_

I think it would be better to have a chapter to introduce how to integrate uv into existing projects, or to improve the user experience of using uv to replace the tools it claims to replace.

Taking me as an example, I use pip/requirements.txt to maintain the dependencies of existing projects, use pyenv to manage multiple versions of Python interpreters, and use virtualenvwrapper to manage virtual environments. When I heard about the all-in-one tool uv, I was excited, but the next second I was frustrated: I had to figure out the details of UV to gradually replace these tools

Take [this issue](https://github.com/astral-sh/uv/issues/9007) as an example  to illustrate the challenges that new users of uv might encounter.

---

_Comment by @Mapiarz on 2024-11-11 14:34_

If anybody is looking to get started with migrating from poetry to uv, I documented the approach I took here: https://github.com/astral-sh/uv/issues/5200#issuecomment-2468319391.

---

_Comment by @bjornasm on 2024-11-12 10:51_

> > So say that I fork SomeRepo, that contains a uv.lock, to a new machine and want to have the exact resolved versions that was installed in the project environment by the creator of SomeRepo. How do I interact with uv.lock to replicate the project environment?
> 
> Maybe [`uv sync --frozen`](https://docs.astral.sh/uv/reference/cli/#uv-sync) is what you're looking for @bjornasm?

Thank you, that seems what I am after! 

---

_Comment by @sh-shahrokhi on 2024-11-12 18:30_

Could you please provide information on the best way to install PyTorch with GPU support in a project that is portable across different systems and can be easily upgraded?

---

_Comment by @GiovanniGiacometti on 2024-11-13 09:04_

Hi guys, I just wanted to report that the Discord invitation link provided [here](https://docs.astral.sh/uv/getting-started/help/#chat-on-discord) in the documentation is expired.

---

_Comment by @Muzych on 2024-11-24 04:06_

As a Chinese language user, I would like to inquire whether the UV community's documentation requires internationalization translation, and I am willing to contribute a Pull Request for the Chinese translation of the UV documentation. 

---

_Comment by @zanieb on 2024-11-26 00:42_

Hey @GiovanniGiacometti — that link works for me still, are you sure?

@Muzych I appreciate the offer! I'm not quite sure how we'd maintain that — we make frequent changes to the documentation. What do you think the long-term plan for that would be?

---

_Comment by @Muzych on 2024-11-26 05:53_

> Hey @GiovanniGiacometti — that link works for me still, are you sure?嘿——那个链接对我来说还能用，你确定吗？
> 
> @Muzych I appreciate the offer! I'm not quite sure how we'd maintain that — we make frequent changes to the documentation. What do you think the long-term plan for that would be?我感谢您的提议！我不太确定我们该如何维护——我们的文档经常变动。您认为这方面的长期计划会是什么样的呢？

Hello, my idea is to first complete the internationalization translation based on the current version, and then make minor adjustments to the details once UV's performance is stable. What do you think about this approach?

---

_Comment by @tekumara on 2024-12-03 11:09_

A surprising behaviour I've noticed, that I couldn't see documented and possibly good to capture somewhere:

`uv sync` creates and uses `.venv` in the parent directory, when the parent directory contains `pyproject.toml`, and even if the current directory also has a `pyproject.toml`.


---

_Comment by @zanieb on 2024-12-03 14:23_

> uv sync creates and uses .venv in the parent directory, when the parent directory contains pyproject.toml, and even if the current directory also has a pyproject.toml.

I think this should only be the case if the current directory is a workspace member of the parent directory.

---

_Comment by @zanieb on 2024-12-03 14:26_

> Hello, my idea is to first complete the internationalization translation based on the current version, and then make minor adjustments to the details once UV's performance is stable. What do you think about this approach?

We'll be making large, frequent adjustments to the documentation. I'm concerned it will go out of date. Let's discuss this over in a dedicated issue https://github.com/astral-sh/uv/issues/9606

---

_Comment by @tekumara on 2024-12-04 08:14_

> I think this should only be the case if the current directory is a workspace member of the parent directory.

In my case I hadn’t set up a workspace.

---

_Comment by @zanieb on 2024-12-04 17:05_

@tekumara that sounds like a bug, could you open an issue with a reproduction?

---

_Comment by @abitrolly on 2024-12-08 09:15_

https://docs.astral.sh/uv/guides/scripts/ doesn't explain where `uv` creates environments and how to clean up them after.

---

_Comment by @eliasdabbas on 2024-12-09 13:11_

Hello, 

I came across a broken link while reading the documentation, so I thought I'd crawl the whole site to check.

The following are 404 pages:

- url: Where the broken link is located
- link: The URL of the broken link
- text: The anchor text, just to make it easier to find the link on the page


|    | url                                                                               | link                                                                                              | text                                       |
|---:|:----------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------|:-------------------------------------------|
|  0 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://docs.astral.sh/ruff/rules/open-sleep-or-subprocess-in-async-function/                     | open-sleep-or-subprocess-in-async-function |
|  1 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://astral.sh/blog/%60https:/docs.astral.sh/ruff/rules/blocking-os-call-in-async-function/%60 | blocking-os-call-in-async-function         |
|  2 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://docs.astral.sh/ruff/rules/trio-async-function-with-timeout/                               | trio-async-function-with-timeout           |
|  3 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://docs.astral.sh/ruff/rules/trio-unneeded-sleep/                                            | trio-unneeded-sleep                        |
|  4 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://docs.astral.sh/ruff/rules/trio-zero-sleep-call/                                           | trio-zero-sleep-call                       |
|  5 | https://astral.sh/blog/ruff-v0.5.0#changes-to-e999-and-reporting-of-syntax-errors | https://docs.astral.sh/ruff/rules/misplaced-bare-raise%60/                                        | misplaced-bare-raise                       |
|  6 | https://astral.sh/blog/ruff-v0.0.276                                              | https://astral.sh/blog/settings.md#include                                                        | include                                    |
|  7 | https://docs.astral.sh/uv/reference/build_failures/                               | https://docs.astral.sh/uv/pip/compatibility.md#pep-517-build-isolation                            | build isolation by default                 |
|  8 | https://docs.astral.sh/uv/reference/build_failures/                               | https://docs.astral.sh/uv/reference/settings.md#dependency-metadata                               | provide dependency metadata manually       |
|  9 | https://astral.sh/blog/ruff-v0.2.0                                                | https://docs.astral.sh/ruff/rules/trio-timeout-without-await                                      | trio-timeout-without-await                 |
| 10 | https://astral.sh/blog/ruff-v0.2.0                                                | https://docs.astral.sh/ruff/rules/trio-async-function-with-timeout                                | trio-async-function-with-timeout           |
| 11 | https://astral.sh/blog/ruff-v0.2.0                                                | https://docs.astral.sh/ruff/rules/trio-unneeded-sleep                                             | trio-unneeded-sleep                        |
| 12 | https://astral.sh/blog/ruff-v0.2.0                                                | https://docs.astral.sh/ruff/rules/trio-zero-sleep-call                                            | trio-zero-sleep-call                       |

Code to reproduce (or if you want to run this periodically): 


```python

import advertools as adv
import pandas as pd


adv.crawl(
    url_list="https://astral.sh/",
    output_file="astral.jsonl",
    follow_links=True)


crawldf = pd.read_json("astral.jsonl", lines=True)

error_urls = crawldf[crawldf['status'].ne(200)]['url']
linksdf = adv.crawlytics.links(df)

linksdf[linksdf['link'].isin(error_urls)]
```

Thanks!

---

_Comment by @zanieb on 2024-12-09 14:53_

@abitrolly that's beyond the scope of the a "guide", when we introduce a "concept" document for scripts we can cover that — but they're just in the uv cache.

---

_Comment by @zanieb on 2024-12-09 14:54_

@eliasdabbas cool thank you! cc @dhruvmanila for Ruff's documentation.

---

_Comment by @abitrolly on 2024-12-09 17:04_

@zanieb it still may worth to mention it somewhere in the middle, or link to relevant cleanup docs. I assume `uv` users are advanced enough to care.

---

_Comment by @depau on 2024-12-18 09:01_

Hi! I noticed the documentation about configuring alternative indexes seems to be outdated and incomplete: https://docs.astral.sh/uv/guides/integration/alternative-indexes/

- It doesn't mention anything about `[[tool.uv.index]]` settings, which are useful
- In [Environment variables](https://docs.astral.sh/uv/configuration/environment/#uv_extra_index_url), `UV_EXTRA_INDEX_URL` is said to be deprecated, but in the alternative indexes page it is used.

Also the Environment variables page doesn't explain how to properly authenticate to alternative indexes other than embedding the credentials in the URL

Have a good day!

---

_Comment by @hrmnjt on 2025-01-02 12:53_

First, thanks for `uv` and ever growing documentation. 

I was reading Versioning Policy (https://docs.astral.sh/uv/reference/policies/versioning/) document and was unable to visualize what would be needed for 1.0 stable APIs. Is it possible to write (10000 ft. level) bullet points which explain the path to 1.0 for `uv`?

---

_Comment by @eliasdabbas on 2025-01-10 15:22_

I found a broken link here:

|    | url                                | link                                                    | text                  | nofollow   |
|---:|:-----------------------------------|:--------------------------------------------------------|:----------------------|:-----------|
|  0 | https://astral.sh/blog/ruff-v0.2.0 | https://docs.astral.sh/ruff/rules/non-pep604-annotation | non-pep604-annotation | False      |

Crawling the whole site doesn't take long (~40 seconds). 

In case you want to run this periodically: 

```
uv run https://gist.githubusercontent.com/eliasdabbas/681c9c7a3d1c00512c665bd4415ac52b/raw/79403412daceb86c2a9839855bccdad67e86277c/uv_docs_broken_links.py
```

Gist: https://gist.github.com/eliasdabbas/681c9c7a3d1c00512c665bd4415ac52b

Thanks again for all the great work.

---

_Comment by @zanieb on 2025-01-10 15:28_

@depau thanks!

> It doesn't mention anything about [[tool.uv.index]] settings, which are useful

That's on the todo-list, e.g., #9867.

> Also the Environment variables page doesn't explain how to properly authenticate to alternative indexes other than embedding the credentials in the URL

Authenticating with credentials outside the URL is at https://docs.astral.sh/uv/configuration/environment/#uv_index_name_password — but this is just a reference document it isn't likely to have further exposition.

@hrmnjt 

> I was reading Versioning Policy (https://docs.astral.sh/uv/reference/policies/versioning/) document and was unable to visualize what would be needed for 1.0 stable APIs. Is it possible to write (10000 ft. level) bullet points which explain the path to 1.0 for uv?

We don't have a plan for this yet. We're not really in a rush to reach 1.0. I think there's not really a list of things we need to do before we switch to 1.0, it's more like we want the tool to be in the world for a while so we can fix rough edges and iterate quickly.

@eliasdabbas Thank you!

---

_Comment by @abitrolly on 2025-01-10 18:32_

@eliasdabbas I like the Python version of the link checker. Definitely useful. Would be interested to see how it compares to Rust based https://lychee.cli.rs/

---

_Comment by @eliasdabbas on 2025-01-10 22:33_

@abitrolly I'm not familiar with Lychee, but I just took a look. Based on what I saw so far:

**Pros**: It can check links in a tree locally, supports .md .rst and HTML. This makes it great for pre-commit checks/hooks.

**Cons**: It doesn't support recursive crawling of websites, which includes discovering links, following them, redirects, etc. 

**Python pros**: Supports recursive crawling with customizable rules, and can be as fast as you want it to be.

**Python cons**: Does not support .md or .rst.

So if you want the actual live website the advertools/Python solution seems better, if you want to check .md or .rst files, it seems Lychee is the good choice. Again I'm not that familiar with it, so take what I said with a grain of salt.

---

_Comment by @carrollpaul on 2025-01-14 22:23_

When I build and run the container from the [Docker example](https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile) as-is, I get this error:

```
Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create fa
iled: runc create failed: unable to start container process: exec: "fastapi": executable file not found in $PATH: 
unknown
```

---

_Comment by @zanieb on 2025-01-14 22:28_

@carrollpaul I tested that just now and cannot reproduce. Please open an issue with a reproduction.

---

_Comment by @clbarnes on 2025-01-17 11:05_

The docs for deploying a zip archive for AWS lambda ( https://docs.astral.sh/uv/guides/integration/aws-lambda/#deploying-a-zip-archive ) includes the usage of `uv pip install`'s `--no-installer-metadata` argument; that doesn't seem to exist https://docs.astral.sh/uv/reference/cli/#uv-pip-install 

---

_Comment by @charliermarsh on 2025-01-17 16:21_

It exists, it's just not visible on the CLI. It's analogous to: https://docs.astral.sh/uv/configuration/environment/#uv_no_installer_metadata.


---

_Comment by @judgewooden on 2025-01-22 10:59_

I have switched to uv for most of my projects and am wondering if there is an easy way to create a snapshot of the environment. I need to experiment with upgrades and dependencies, and if things go wrong, revert to the original snapshot.

In the past, I used `pip freeze > requirements-pre-upgrade.txt` before making changes. I know I can do the same with `uv pip freeze`, but I was curious if there is a better way to handle this using uv tools.

---

_Comment by @debnath-d on 2025-01-22 20:11_

> I need to experiment with upgrades and dependencies, and if things go wrong, revert to the original snapshot.

> In the past, I used `pip freeze > requirements-pre-upgrade.txt` before making changes. I know I can do the same with `uv pip freeze`, but I was curious if there is a better way to handle this using uv tools.

@judgewooden 

All information is contained in `pyproject.toml` and `uv.lock`. Just backup these two files.

If things go wrong, replace these files with the backed up files and run `uv sync`.


---

_Comment by @edmorley on 2025-01-26 18:10_

Some misc docs feedback before I forget...

## Locking and syncing concepts page

If I search the docs for "sync", the top result is the "Locking and Syncing" concepts page:
https://docs.astral.sh/uv/concepts/projects/sync/

The page has "syncing" in the title and "sync" in the URL, but the page content itself really only seems to be about locking rather than syncing, which was surprising - since I was hoping to find more about `uv sync`?

## `uv sync` CLI reference docs hidden in search results

After not finding what I was after on the "Locking and Syncing" concepts page, I tried searching for "sync" again, hoping to find the `uv sync` CLI reference docs (ie: https://docs.astral.sh/uv/reference/cli/#uv-sync). 

However, instead the search results preview highlights some of the `uv pip` usage docs instead (with the `uv sync` content being hidden under the "9 more on this page" collapsed section).

eg:

<img width="811" alt="Image" src="https://github.com/user-attachments/assets/8c0a763a-b92a-4a48-bf5b-fd4fd6197abf" />

I'm presuming this is mostly due to the way mkdocs/its search plugin is designed, so I don't know if there is much that can be done (other than eg splitting CLI command pages up into smaller chunks?), and now I know to check the collapsed sections I can work around it - but I imagine this may hinder docs discovery for others too?

## Clarifying the difference between `python-downloads` and `python-preference`

As part of deciding which `uv sync` arguments I should use in our build scripts, I read through the CLI command reference docs here:
https://docs.astral.sh/uv/reference/cli/#uv-sync

I knew I wanted to ensure uv always used our own Python distribution, rather than downloading a uv-managed installation - however, at first glance it wasn't immediately apparent whether I should be using `python-downloads`, `python-preference`, or both. (ie: Part of that uncertainty was wondering whether one option was a superset of the other, or if they were orthogonal.)

I was able to work it out after using doc search to find other pages like these:
https://docs.astral.sh/uv/concepts/python-versions/#disabling-automatic-python-downloads
https://docs.astral.sh/uv/concepts/python-versions/#adjusting-python-version-preferences

...but I wonder if the following might help:
(a) some backlinks from the reference pages back to the concepts sections (both the CLI reference usage docs, and the settings reference docs)
(b) A half-sentence or so added to both sections on the concept page explaining how they interact

## Understanding what `uv sync` installs by default

When reading https://docs.astral.sh/uv/reference/cli/#uv-sync it wasn't immediately clear to me what dependency groups if any would be installed by default. 

For example, the intro for the `uv sync` command doesn't mention what it installs, and none of the CLI args like `--all-groups` and related hint at it. From seeing the `--no-dev` arg I was able to infer that dev dependencies might be installed by default, but I had to search the docs manually to find:
https://docs.astral.sh/uv/concepts/projects/dependencies/#development-dependencies
https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups

I think some backlinks from the CLI args to the concepts docs would similarly help in this case.

## General docs discoverability/navigation/ordering

I think several of the issues above only occurred because:
1. I had skipped over some of the concept docs and used the CLI command reference docs as my starting point.
2. The CLI reference docs don't tend to backlink to the other parts of the docs.

I'm normally someone that fully reads docs from the start/intro - I think part of the reason I skipped ahead was due to the volume of the docs (eg number of sections listed in the sidebar), and some of the earlier docs sections being less relevant to me (eg running scripts, manually managing Python installations, installing/running tools). 

I don't think the volume of docs can really be avoided (uv does a lot, and comprehensive docs are needed/worthwhile), but I wonder if some re-ordering or adjusting of the initial intro/sections might help? For example, to me `uv sync` is uv's bread and butter use-case, but yet it's mentioned quite late in the intro (I think this might have been part of the person's issue in https://github.com/astral-sh/uv/issues/10813 too?). 

I also think the size of the docs means that backlinks from one-section to another (particularly CLI reference -> concepts) are particularly important for ensuring people end up in the right place if their initial entry-point to the docs wasn't ideal (eg an experienced user who jumps ahead to the CLI reference section, or if a search engine deep links to a reference page etc).

Anyway, thank you for having put so much time into docs already - hope the above was helpful (I know it's hard to get fresh eyes back on content with which you're already familiar).

---

_Comment by @debnath-d on 2025-02-01 14:24_

Perhaps it would be a good idea to point this out: https://docs.astral.sh/uv/reference/settings/#find-links on this page: https://docs.astral.sh/uv/configuration/indexes ?

@zanieb

---

_Comment by @charliermarsh on 2025-02-01 16:19_

That’s awesome feedback, thanks @edmorley.

---

_Comment by @cmsirbu on 2025-02-07 11:26_

I'm having some difficulty with the [Build Systems](https://docs.astral.sh/uv/concepts/projects/config/#build-systems) as a new `uv` user converting a project from `poetry`. So Poetry has its own build backend, which means I've never had to choose and this is where I think the docs could be more helpful.

Right now, the docs are explaining what a build system does for you and mentions that you can choose one with a parameter to `uv init`, but otherwise sends you to undefined external docs for your chosen build system (works only if you know what to choose in the first place). Some questions arise here:

- What should I choose? If there's an opinionated default, docs should guide me towards setting it up correctly.
- Do I need to install it alongside `uv` or does it manage that for me?
- What happens if I don't define it or have an incompatible build-backend defined (e.g. migrating from Poetry).

And because I'm still in poetry/uv limbo, I have this still defined, but `uv build` seems to work fine despite not explicitly supporting Poetry as a backend. Not trying to turn this into a migration guide, but just to reinforce the questions above.

```
[build-system]
requires = ["poetry-core>=2.0.0,<3.0.0"]
build-backend = "poetry.core.masonry.api"
```

```
> uv build
Building source distribution...
Building wheel from source distribution...
Successfully built dist/mkdocs_ansible_collection-0.2.1.tar.gz
Successfully built dist/mkdocs_ansible_collection-0.2.1-py3-none-any.whl
```

---

_Comment by @zanieb on 2025-02-07 17:50_

uv supports all build backends because we are a standards-complaint build frontend. The `uv init` command only supports a subset of build backends, because those are the ones that have received a pull request adding the necessary template.

We don't have an opinionated default. We use hatchling right now, because we think it's a reasonable option. We're working on our own build backend, which will be used by default eventually. `uv init --package` or `uv init --lib` will set up the "default" build backend for you.

If you don't define a build backend, we won't install your project itself into the environment (but will still install its dependencies).

I believe these things are outlined in https://docs.astral.sh/uv/concepts/projects/init/#creating-projects (in addition to the build system document you referenced).

We could clarify the role of build frontends in the document you linked though.

---

_Comment by @rsyring on 2025-02-21 07:39_

> I'm normally someone that fully reads docs from the start/intro - I think part of the reason I skipped ahead was due to the volume of the docs (eg number of sections listed in the sidebar),

I think digestibility of the sidebar could be made better with some styling changes.  Currently, there isn't enough contrast between headings in the nav and the nav items.  They are almost the same size, use the same font, have the same spacing, same color, etc.  To me at least, that makes them run together and feels a bit overwhelming.  Makes it harder to visually scan for what I'm looking for.  

Consider [mise's docs](https://mise.jdx.dev/dev-tools/) which have a similar volume of content in the sidebar nav but, at least to me, don't feel nearly as "crowded" or overwhelming:

![Image](https://github.com/user-attachments/assets/bba62e3c-28ce-4cd2-ad18-edc2685bde48)

---

_Comment by @sanmai-NL on 2025-02-24 11:10_

The section [Resolution - Required environments](https://docs.astral.sh/uv/concepts/resolution/#required-environments) doesn't explain the syntax and certainly not the semantics of this environment marker language. From uv's docs on functionally similar features, I found a reference to [PEP 508](https://peps.python.org/pep-0508/#environment-markers). That, however, is marked as deprecated when visited, and superseded by [Dependency specifiers](https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers).

I propose that uv's documents this language in a single place, that references Dependency specifiers and just that.

---

_Comment by @zanieb on 2025-02-24 17:37_

> I think digestibility of the sidebar could be made better with some styling changes. Currently, there isn't enough contrast between headings in the nav and the nav items

Yeah I agree. This has been hard to get right and I've spent way too many hours futzing with it.

Unfortunately just copying over the colors from mise isn't great, imo

<img width="1251" alt="Image" src="https://github.com/user-attachments/assets/fa3018c9-d16b-41a8-b10e-8c4cf4339128" />

<img width="323" alt="Image" src="https://github.com/user-attachments/assets/681ed5c5-df99-4fba-8c87-151661a4cea3" />

<img width="323" alt="Image" src="https://github.com/user-attachments/assets/32b3ca14-bdb9-4160-98a9-89fa8fc3ee3c" />

---

_Comment by @zanieb on 2025-02-24 17:38_

Similarly, I think the bold looks bad 

<img width="1251" alt="Image" src="https://github.com/user-attachments/assets/5f3a63fd-6422-4595-bce3-9f3b2e2fbe52" />


---

_Comment by @cmsirbu on 2025-02-24 18:01_

How about this (fully realizing it steals the active link colour).

![Image](https://github.com/user-attachments/assets/fccb8ea7-5c34-41d0-85ee-7587b78cf56a)

![Image](https://github.com/user-attachments/assets/ee9623b7-a891-468c-ad24-f94d56c5bdd7)

- Remove the padding override for inner menu items (leave it at the theme default?)
- Make categories more emphasized using `md-nav__link--active`

---

_Comment by @rsyring on 2025-02-24 18:06_

@cmsirbu - I'm terrible at design.  I usually try to find a template that has this done "right" by someone who knows aesthetics better than me.  :)  But, here are some thoughts:

- I think the latest attempt is an improvement worth publishing.
- What's the third level nav look like.  I.e. when you are in "Integrations >" or "Projects >"?
- Try: making the 2nd level nav one size smaller font?
- I'm not a fan of the "purple" color in the light background version.  There isn't enough contrast between that and white IMO.  But that might be a different issue for a different time.  

HTH.  :)

---

_Comment by @zanieb on 2025-02-24 18:24_

I guess I don't really see that as a clear step forward, especially since it clobbers the active link styling.

---

_Comment by @zanieb on 2025-02-24 18:28_

It's also worth noting that ~half of the Mise nav is below the page fold. It's an explicit goal to minimize that here.

I think the proper fix for this is probably to add horizontal sections, which I've explored but will requires some serious reorganization and isn't a priority at the moment.

---

_Comment by @cmsirbu on 2025-02-24 18:55_

@rsyring There's a limit to what one can do in a browser "inspect" window with average css skills, I know when to stop 🙂 

> I guess I don't really see that as a clear step forward, especially since it clobbers the active link styling.

It's more of a take from it what might be useful than "do this".

More to the point, I initially only wanted to post the padding option, which might be a good solution for adding visual separation if you don't want to increase the vertical spacing too much (to keep as much as possible on the screen).

Is it too much indentation? 🤷🏻 It's python we're talking about... 

![Image](https://github.com/user-attachments/assets/a6408a80-aa1a-4d3c-8ee3-504ce193931e)


---

_Comment by @rsyring on 2025-02-24 19:39_

> It's also worth noting that ~half of the Mise nav is below the page fold. It's an explicit goal to minimize that here.

I'm not sure concern about what's above the fold is that helpful in technical documentation, especially navigation when it's intuitive, especially for technical users, to want to scan through something to easily get the "lay of the land."  That's usually reserved for a different type of content and user interaction.

The audience who reads these docs is technical and giving them the ability to easily scan what the docs offer is a more important concern.  I've never felt any hesitation when reviewing mise's docs b/c there is a lot of navigation.  On the contrary, I've always found it helpful b/c it makes me aware of what's the there but easily allows me to move past it when I'm focused on a specific task.

To move this out of the realm of opinion, this might be a helpful reference: https://www.nngroup.com/articles/eyetracking-tasks-efficient-scanning/#toc-design-for-efficient-scanning-7

> I think the proper fix for this is probably to add horizontal sections, which I've explored but will requires some serious reorganization and isn't a priority at the moment.

I'd encourage you to avoid horizontal navigation b/c it takes the scanning from a single dimension (vertical) to two dimensions (vertical + horizontal) and perhaps three dimensions if you have to click into a page in a section before you see it's horizontal navigation change.  Makes getting the lay of the land much more difficult.

I think your current nav, two layers on left plus page TOC on the right once you are on a page is one of the best docs layout there is.  Just need to visually de-clutter the left nav.

> I guess I don't really see that as a clear step forward, especially since it clobbers the active link styling.

IMO, it is a clear step forward.  Maybe not the last step, but the change in indentation and padding is a significant improvement.

~I guess I'd have to see the full results but I didn't see anything that required clobbering the active link styling in the screenshots @cmsirbu provided.~ (missed a comment regarding)

> Is it too much indentation? 🤷🏻 It's python we're talking about...

Nope.  Just right IMO.  

Don't forgot to preview the mobile/responsive versions.

Are all the font sizes the same?  I think a slightly smaller font at each level will enhance visual distinction.

---

_Comment by @rsyring on 2025-02-24 19:43_

Should we move the specifics of the nav changes to a different issue to avoid overwhelming this one? 

---

_Comment by @zanieb on 2025-02-24 20:10_

Let's chat in https://github.com/astral-sh/uv/issues/11757

---

_Comment by @jromal on 2025-03-01 07:42_

`uv` documentation uses "Material for mkdocs". It is a small team (until recently one single person). 

They are busy revamping it for speed and better search. But they have always the time to answer any question related to style or changes. 

---

_Comment by @Nugine on 2025-03-10 07:55_

(uv 0.6.5)
`uv pip install` does not update `pyproject.toml` or `uv.lock`. It seems surprising for new users.


---

_Comment by @zanieb on 2025-03-10 17:16_

> (uv 0.6.5) `uv pip install` does not update `pyproject.toml` or `uv.lock`. It seems surprising for new users.

`pip` does not do this, nor will `uv pip`. I think the solution here is #5200 

---

_Comment by @vitrinite on 2025-04-03 13:00_

I am not sure, if here is the right place, but It seems to miss in the documentation the recommended way of installation uv system wide, for an admin managing a multi-user linux it could be helpful.

---

_Comment by @zanieb on 2025-04-03 13:17_

@vitrinite yeah the multi-user setup is kind of complicated and depends a lot on the use-case so we haven't captured it in the documentation. There are various issues in the tracker where people are talking about their setups though.

---

_Comment by @harrymander on 2025-04-07 04:44_

In the [docs for the official GitHub action](https://docs.astral.sh/uv/guides/integration/github/#caching), the advice for manually managing the uv cache is to set `UV_CACHE_DIR` in the global environment for the job. However, the `setup-uv` action overwrites the value of `UV_CACHE_DIR`; this behaviour is documented in the action's [README](https://github.com/astral-sh/setup-uv?tab=readme-ov-file#local-cache-path). It seems the proper thing to do is to use the value of `UV_CACHE_DIR` set by the action:

```yaml
jobs:
  install_job:
    steps:
      # This action sets the UV_CACHE_DIR env var, even if it was already set
      - name: Setup uv
        uses: astral-sh/setup-uv@v5
       
      - name: Restore uv cache
        uses: actions/cache@v4
        with:
          path: ${{ env.UV_CACHE_DIR }} # changed
          key: uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
          restore-keys: |
            uv-${{ runner.os }}-${{ hashFiles('uv.lock') }}
            uv-${{ runner.os }}

      # ... install packages, run tests, etc ...

      - name: Minimize uv cache
        run: uv cache prune --ci
```

Alternatively, `--cache-dir $UV_CACHE_DIR`  could be explicitly passed to all `uv` invocations in the workflow, or possibly `cache-local-path` could be set in the options for `setup-uv`.

---

_Comment by @dasAtRagedy on 2025-04-13 10:48_

How do you guys deal with minor errors in documentation (or code), such as typos, in such a massive repository? 'a' instead of 'an' is such an insignificant issue, should it be somewhere reported, proposed inprovement with a PR, or ignored completely?

---

_Comment by @zanieb on 2025-04-15 20:22_

@dasAtRagedy feel free to just post a pull request! We're always happy to merge small fixes.

---

_Comment by @tabedzki on 2025-05-01 02:26_

Looking at the documentation for `uv export`, it appears that the [CLI page](https://docs.astral.sh/uv/reference/cli/#uv-export) as well as the [lockfile page](https://docs.astral.sh/uv/concepts/projects/layout/#the-lockfile) were covered in PRs (#12955, and  #12732, respectively), but the page titled ["Exporting the Lockfile"](https://docs.astral.sh/uv/concepts/projects/sync/#exporting-the-lockfile) did not receive an update to mention the new pep 571 lockfile functionality. 

Happy to open a dedicated issue. I would've opened a PR but I didn't know what language y'all would want to address it. 



---

_Comment by @krisbrud on 2025-05-02 13:27_

In the [documentation for using `uv` in Docker](https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment), there is a recommendation to add `RUN uv run some_script.py`

I tried this, and it ended up installing dev dependencies in the project in the container during runtime. I'm not sure how the docs should be changed, but it should at least inform that it will install dev dependencies IMO.


---

_Comment by @zanieb on 2025-05-02 13:31_

@krisbrud you'll want to use `uv run --no-sync`. We can make that clearer. (or, set `UV_NO_SYNC`)

---

_Comment by @injust on 2025-07-13 08:19_

I noticed that https://docs.astral.sh/uv/concepts/tools/#including-additional-dependencies documents passing `--with <extra-package>` multiple times to `uv tool install`, but a comma-separated `--with` argument also works.

If this is intended and supported, then I think it should be added to the docs.

---

_Comment by @beetleb on 2025-08-11 02:54_

A number of us cannot read too much on a screen, and prefer printing out docs and reading on paper.

Is there a way to get all the uv docs either as a long HTML file, or as a PDF?

See, for example, how jj did it for their project: https://github.com/jj-vcs/jj/issues/5465

I'm not familiar with mkdocs, so I couldn't reproduce this with uv. But I assume it is fairly straightforward, and would benefit many!

Thanks.

---

_Comment by @satetheus on 2025-08-11 17:55_

## Issue
When reading through the [Non-editable installs section of the docker integration guide](https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs), I noticed the example returns a few different errors.
## To Reproduce
1. Run `uv init hello`
2. `cd` into the hello directory
3. Run `uv lock` (this deals with an error about the uv.lock file not existing)
4. Copy the example Dockerfile from the [docs](https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs) into a Dockerfile in the hello directory
5. Add lines to add user "app" & switch to it above the line where the .venv directory is copied: `RUN useradd app` && `USER app` (this deals with an error about unknown user)
6. Run `docker build . -t hello` or `podman build . -t hello`
7. Run `docker run hello` or `podman run hello`

This should return an error about `/app/.venv/bin/hello not in $PATH`
## Desired Actions
1. Biggest thing for me was figuring out how to be able to run this as intended. I managed to do it by running `uv init hello --build-backend uv` for step 1 above, but I'm not even 100% certain that's the best way to do this. If it is, [build systems](https://docs.astral.sh/uv/concepts/projects/config/#build-systems) should at the very least be mentioned in the non-editable installs & probably linked as well.
2. The Dockerfile either needs the 2 lines I mentioned above to add & switch to the "app" user, or the `--chown=app:app` should be removed from the line to copy .venv from the builder stage.

---

_Comment by @limjh16 on 2025-09-24 21:36_

https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion

Though the function to generate the fish completion code is already fast, we can use the [`__fish_cache_sourced_completions` function](https://github.com/fish-shell/fish-shell/pull/10223) to cache this function

```sh
echo -e '__fish_cache_sourced_completions uv generate-shell-completion fish 2>/dev/null\nor uv generate-shell-completion fish 2>/dev/null | source' > ~/.config/fish/completions/uv.fish
```

---

_Comment by @halms on 2025-09-29 15:39_

The [build backend namespace package docs](https://docs.astral.sh/uv/concepts/build-backend/#namespace-packages) say that for this kind of project structure:
```
pyproject.toml
src
└── foo
    ├── bar
    │   └── __init__.py
    └── baz
        └── __init__.py
```
the recommended configuration for the build backend is:
```toml
[tool.uv.build-backend]
module-name = "foo"
namespace = true
```
I'm just asking myself why it's not this that's recommended:
```toml
[tool.uv.build-backend]
module-name = ["foo.bar", "foo.baz"]
```
Looking [at the source](https://github.com/astral-sh/uv/blob/ddb826cfaa3ec1981d8ca3a00cac260cce0baaec/crates/uv-build-backend/src/lib.rs#L227) and trying it out, this appears to work fine.

The order in the docs is kinda like:
You can do `module-name = "foo.bar"`, or you can do `module-name = ["foo", "bar"]`, or you can use `namespace=true` for complex cases.
Would it make sense to split the case: multiple-namespace package with a common root out and say explicitly that one can do `module-name = ["foo.bar", "foo.baz"]` and just if it is complex/a large number of roots use `namespace=true`?
Especially since there is a warning attached to `namespace=true`...

Or is this too niche of a use case?

(Yes I know, workspaces are the preferred solution, but ... legacy)

---

_Comment by @konstin on 2025-09-29 16:07_

> Or is this too niche of a use case?

Pretty much, the recommended structure for modern projects is having a single `foo/__init__.py`, or when using a namespace (instead of entrypoints) using a single `foo/bar/__init__.py`. When reviewing existing namespace packages pattern, we found that they were often large enough that `namespace = true` makes sense, that's why it's documented here.

---

_Comment by @shumpohl on 2025-10-17 10:39_

I think the definition of modules and packages in the [build backend](https://docs.astral.sh/uv/concepts/build-backend/#modules) documentation is a bit confusing. 

> Python packages are expected to contain one or more Python modules, which are directories containing an __init__.py.

For this sentence to make sense the relative clause must refer to "Python packages" which for me as non-native isnt obvious. I would simplify this to

> Regular python packages are directories containing an __init__.py. They are expected to contain one or more python modules.

Maybe, it makes sense to link to/quote the official glossary entries for [module](https://docs.python.org/3/glossary.html#term-module), [package](https://docs.python.org/3/glossary.html#term-package), [regular package](https://docs.python.org/3/glossary.html#term-regular-package) and [namespace package](https://docs.python.org/3/glossary.html#term-namespace-package).

Furthermore, the module section did not explain how to package single file modules, i.e. what setuptools' `py-modules` does. I propose renaming the "Modules" section to "Regular packages" and creating a new section "Single file modules"(bikeshed) that explains that this is not possible/planned or how to do that.

---

_Comment by @konstin on 2025-10-17 15:32_

We're not following python.org's definition of "package", instead we're using "package" to refer to something you can install, and use "module" for something that you can import. python.org does not have any concept of package-you-install, if we were following that definition we'd be talking about "package" for two very different things, so we decided to use the terminology that's used in most other ecosystems instead. 

---

_Comment by @shumpohl on 2025-10-20 11:06_

This is a reasonable choice. However, it makes the relative clause 

> which are directories containing an `__init__.py`

even more puzzling to me.

---

_Comment by @konstin on 2025-10-20 12:07_

Is it because the `__init__.py` refers to modules, not packages?

---

_Comment by @shumpohl on 2025-10-20 13:00_

Ah, for uv build a "Python module" is **always** a directory containing an `__init__.py`. Then the documentation is correctly worded. I re-read it and it makes sense to me now.

My confusions stems from the fact the terminology differs from the standard, setuptools and the «"module" for something that you can import» guideline because it ignores files completly. But I understand the reasoning behind making that choice.

Nevertheless, I suspect some other people will stumble over this. I suggest adding a glossary. My personal choice would be to use the term "directory module" to avoid clashing with established terminology but I guess that it is more cumbersome and might bring other problems.

Thank you for your time.

---

_Comment by @AlexanderPershin on 2025-11-18 13:52_

i guess you already know this, but website is down

---

_Comment by @edmorley on 2025-11-18 14:03_

Looks like it's due to this Cloudflare incident:
https://www.cloudflarestatus.com/incidents/8gmgl950y3h7

---

_Comment by @matthewpleskow on 2025-11-19 00:03_

Doesn't look like anyone's mentioned this here: https://stackoverflow.com/questions/79783307/uv-is-building-projects-without-a-build-system-definition-in-the-pyproject-tom

The documentation [here](https://docs.astral.sh/uv/guides/package/#preparing-your-project-for-packaging) and [here](https://docs.astral.sh/uv/concepts/projects/config/#build-systems) state that if the `build-system` table doesn't exist in a project's `pyproject.toml` "uv will not build it". It appears as if this is not true. A debug build prints "missing field \`build-system\`", but then continues on with using `setuptools.build_meta:__legacy__` to create the artifacts (as mentioned in the Note in the docs). A simple reproducible example can be found in the link above.

---

_Comment by @zanieb on 2025-11-20 14:45_

That's in `uv build` which differs from `uv sync` or `uv run` in that you've explicitly requested that we build distributable artifacts so we follow the legacy behavior defined for setuptools.

It's problematic that we say we won't build it in that first link though. We should clarify that. https://github.com/astral-sh/uv/pull/16788

---
