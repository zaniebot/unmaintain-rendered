```yaml
number: 1384
title: Support .env-files
type: issue
state: closed
author: woutervh
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-02-15T23:29:01Z
updated_at: 2025-12-02T10:42:51Z
url: https://github.com/astral-sh/uv/issues/1384
synced_at: 2026-01-10T03:23:52Z
```

# Support .env-files

---

_Issue opened by @woutervh on 2024-02-15 23:29_

In my workflow I make heavy use of env-files.

several tools support .env out of the box:

- docker / docker-compose
- just  (https://just.systems/man/en/chapter_26.html?highlight=env#settings)
- easy to support in makefiles
- easy to support in python-apps

It would be nice if uv could support .env-files in the $CWD or parent-dirs.
see https://crates.io/crates/dotenv

---

_Comment by @zanieb on 2024-02-15 23:30_

Thanks for engaging with the project!

Specifically, you would like us to read `.env` files before every operation? Or something else?

---

_Label `enhancement` added by @zanieb on 2024-02-15 23:33_

---

_Comment by @woutervh on 2024-02-16 11:48_

> Specifically, you would like us to read .env files before every operation? 
yes.


pip supports a config-file pip.ini / pip.conf
all those values in that config-support can also be set as environment-variables

I have not yet seen a reference that `uv pip` supports such config-file or these envvars,
but many times I set such project-specific pip-envvars,  the same for twine.

some examples:
- PIP_EXTRA_INDEX_URL
- RUFF_CACHE_DIR
- TWINE_USERNAME
- TWINE_REPOSITORY_URL

```
<project-dir>
   .env
  .venv/
   docs/
   src/
   tests/
```

so my request is to support .env-files in $CWD, 
and optionally in parent-levels (e.g in just this is configurable)



---

_Comment by @woutervh on 2024-02-16 11:54_

I always try to setup projects in an isolated way:

```
<project-dir>
   .env
  .venv/
   docs/
   src/
   tests/
   var/cache
   var/cache/ipython
   var/cache/pytest
   var/cache/python
   var/cache/ruff
   var/cache/sphinx
   var/cache   
   var/log
   var/tmp
```

to make ruff use the my .env to read RUFF_CACHE_DIR,
I currently have to install ruff in the .venv and use python-entrypoint to execute ruff (slowing things down)

I use my package  https://github.com/libranet/autoread-dotenv  to read the .env  via sitecustomize-hooks,
so the env-vars become available for any python-generated entrypoint.





---

_Label `configuration` added by @zanieb on 2024-02-16 14:49_

---

_Comment by @zanieb on 2024-07-01 21:48_

I'm going to mark this as not-planned for now. We may revisit in the future. If people have additional use-cases or thoughts please feel free to share!

---

_Closed by @zanieb on 2024-07-01 21:48_

---

_Comment by @dustalov on 2024-07-24 23:01_

I have a similar use case.

I am launching machine learning demos for students, and I am currently using [Pipenv](https://pipenv.pypa.io/en/latest/) which loads environment variables from a file named `.env`. That file contains variables like `DO_NOT_TRACK=1` and `HF_HUB_DISABLE_TELEMETRY=1` which are applied for all the commands I run via `pipenv run`.

As a workaround, I can define them in my shell profile, but it would be great to have it natively supported by uv.

---

_Comment by @Count-Count on 2024-08-21 04:24_

I am also migrating from pipenv which uses `.env` out of the box. It would be great if `uv run` could support this.

---

_Comment by @zanieb on 2024-08-21 12:35_

Willing to consider this further, i.e., we'd read it in `uv run` only.

---

_Reopened by @zanieb on 2024-08-21 12:35_

---

_Comment by @patrick91 on 2024-08-21 12:41_

This might of inspiration, PDM supports something like this: https://pdm-project.org/en/latest/usage/scripts/#env_file

I've personally used this as a way to not use tools like dotenv, since I usually need them locally and not on production 😊 (plus is one less thing to worry about)

---

_Comment by @lcoleman-scorability on 2024-08-22 04:24_

We would really love teh ability to load from a .env file for the same reason as above - we use a lot of our applications configuration in .env files.

---

_Comment by @simonw on 2024-08-24 04:37_

Supporting this in `uv run` seems like it could be really useful to me. When I'm working with Django I often have it pick up `os.environ["..."]` for things like API keys needed by my application. `uv run ./manage.py runserver` to pick up those from a `.env` file could be a really nice pattern.

---

_Comment by @LucasRoesler on 2024-08-29 15:59_

I am also interested in the `uv run` support. I am coming from Poetry and currently use this plugin to achieve the same flow to pick up env variables from the `.env` at the project root. 

---

_Comment by @kamuridesu on 2024-08-29 23:20_

I think it would be useful to have a flag like `--env-file` or some section in the config file like PDM does.

But why limit it to `uv run` only? `uv` uses a set of environment variables in place of command line arguments, that way you can use different definitions for `uv` depending on your environment. I personally think it would be useful for situations like when you use a custom packages index for example.

---

_Comment by @Dufran on 2024-09-01 06:04_

> I am also interested in the `uv run` support. I am coming from Poetry and currently use this plugin to achieve the same flow to pick up env variables from the `.env` at the project root.

Hi, I was also migrating from poetry to uv, and currently use [taskfile](https://taskfile.dev) to handle .env variable passing to uv run

Example of the taskfile
```
version: "3"
dotenv: [".env"]
tasks:
  manage:
    cmds:
      - uv run ./backend/manage.py {{.CLI_ARGS}}
```

and then i can run stuff like 
`task manage -- makemigraitons`
`task manage -- shell_plus`  etc



---

_Comment by @sminnee on 2024-09-04 00:06_

Another user-case: I supply environment variables to my dev server with a .env file. I've been using `poetry run bin/serve-dev` to start things with a dotenv loading plugin for poetry.

`uv run bin/serve-dev` auto-loading my .env file would be nice, but to be honest `uv run --env-file .env bin/serve-dev` is fine too, if you want to limit the blast radius of this change.

---

_Comment by @TheRealBecks on 2024-09-09 08:01_

After the migration from `pipenv` to `uv` our local Ansible environments are now broken as `uv` does not read the `.env` file.

@zanieb @charliermarsh After reading this thread it looks like that missing feature is a real showstopper?

---

_Comment by @gusutabopb on 2024-09-11 03:54_

Here's a non-Python example: Node JS added native support to this feature in September 2023 ([changelog](https://github.com/nodejs/node/blob/main/doc/changelogs/CHANGELOG_V20.md#2023-09-04-version-2060-current-juanarbol-prepared-by-ulisesgascon)) and it has been supported by Bun/Deno for a while now.

---

_Comment by @petermbauer on 2024-09-18 06:56_

My use case is to use a separate project-specific Conan Local Cache when entering the respective venv. This simply means setting the `CONAN_USER_HOME` env var to the venv path with `source .venv/bin/activate`.
Currently, this is done by fumbling this into the generated activate/deactivate scripts. These scripts also vary between the Python versions so this also needs to be tested and updated for new Python versions :-(

---

_Comment by @cbrnr on 2024-09-18 15:22_

I'm not sure if my use case is related, but I really enjoy `pyenv` automatically activating a specific venv if it finds a `.python-version` file in the directory. I guess the same can be achieved with `.env`, but maybe there is already a way to use `uv` to auto-activate envs? Or is this not necessary anymore with `uv run` (sorry for my ignorance, I haven't really used `uv` much yet)?

---

_Comment by @zanieb on 2024-09-18 15:24_

>  Or is this not necessary anymore with uv run (sorry for my ignorance, I haven't really used uv much yet)?

It shouldn't be needed if you use `uv run`, though it's a bit more nuanced than that. Feel free to open a new issue if you run into problems there.

---

_Comment by @blakeNaccarato on 2024-09-20 02:57_

I like [Brett Cannon's](https://snarky.ca/use-toml-for-env-files/) and [Jeff Triplett's](https://micro.webology.dev/2024/03/13/on-environment-variables.html) takes on `.env` files, which identify the orthogonal purposes of using such files for configuration and for secrets. They also identify the issue with the lack of a standard of `.env` files, e.g. path separator variation and other nuances.

Brett posed the question, could we standardize on TOML for this? I think if `uv` picks this up, it could be done in stages, supporting `.env` today but with no promises on the unstandardized format, then eventually incorporating a more opinionated take inside `pyproject.toml` in a subtable of `tool.uv`.

Secrets management is it's own beast, of course.

The upside to `uv` tackling lockfiles is the standards discussion on that is already pretty mature, but there's no clear consensus on standardizing something for `.env`, so maybe that's a bridge too far.

---

_Comment by @woutervh on 2024-09-22 00:38_

@blakeNaccarato  IMHO it is widely understood that .env-files should have a bash-compatible syntax. Besides that, there should not be anything language or tool specific.

---

_Comment by @sminnee on 2024-09-23 02:14_

> Brett posed the question, could we standardize on TOML for this?

The great thing about ruff is that it's essentially a drop-in replacement for pylint and other tools you were probably using before.

Doing the same with .env files would be a DX win.

Championing an emergent new format would be an interesting idea, as a separate and second step, but until it becomes comparable to .env in terms of adoption, it seems like a DX loss to deprecate .env.

---

_Comment by @seapagan on 2024-09-23 09:09_

I use [direnv](https://direnv.net/) for this exact use-case. It is installed as a global binary on pretty much any OS, then activates any `.envrc` or optionally also `.env` when the folder is entered. This is a mature tool that does exactly this one job very well, with the bonus of not being restricted to only Python as it works on any folder, anywhere.

The big plus for me is that you need to **explicitly** enable the file for each folder (once off check) so it will not activate any random file you get in a cloned repo.

`uv` doesn't need to do absolutely everything :wink:

---

_Comment by @petermbauer on 2024-09-23 09:18_

Unfortunately i have to use Windows so direnv won't work for me.

---

_Comment by @seapagan on 2024-09-23 09:25_

> Unfortunately i have to use Windows so direnv won't work for me.

I haven't tried it myself, but there is a windows download using `winget` offered, even though it does say only '*nix' compatible on their github 🤷‍♂️. Ignore me if you've tried this and it didn't work :grin: 

EDIT: people have used it with varying degrees of success, this [gist](https://gist.github.com/rmtuckerphx/4ace28c1605300462340ffa7b7001c6d) has a walk-through

EDIT 2: I had a bit of time so I booted into windows, updated powershell as it doesn't work in the default version, traced and sorted the issues it has by default and jumped through hoops (needs 3 badly documented env var set). Then came to the conclusion it is totally borked under windows. Sorry, carry on :grin:

---

_Comment by @blakeNaccarato on 2024-09-25 19:07_

I wrote an [isolated script for syncing `.env` with a `tool.dev.env` table in `pyproject.toml`](https://gist.github.com/blakeNaccarato/9871a3a36a1d0156a739af1fbc9a937e) (by default, but it's configurable via CLI) that you can run in a shell profile or in your shell initialization flow. It's barebones and only supports environment variables hard-coded in your `pyproject.toml` (e.g. no secrets), but I'm sure someone could extend it into a full utility. See also [`dump-env`](https://github.com/wemake-services/dump-env) for a feature set supporting secrets and `.env` templating.

---

_Comment by @charliermarsh on 2024-09-28 22:30_

I’m supportive of adding this to uv run. It would also enforce that the point is not to configure uv but to configure the underlying commands.

(I think it’s not too difficult to add and there’s clearly a lot of demand.)

---

_Comment by @charliermarsh on 2024-09-29 17:23_

I think there are two options here:

1. Respect `.env` in `uv run` (but not elsewhere). So, `.env` would be for providing runtime environment variables, but _not_ for configuring uv itself.
2. Respect `.env` everywhere.

My inclination is to do the former (and I think we could get consensus on + ship that quickly). The downside is that you still need to provide `UV_`-prefixed environment variables (which could include credentials) through some other mechanism.

Maybe we start with (1) and we can always expand to (2) if it's well-motivated.

If we do (1), my preference would be:

- _Always_ read `.env`
- Allow users to pass `--env-file` (or `UV_ENV_FILE`) to override the file.
- Add `--no-env-file` and `UV_NO_ENV_FILE` to disable it.

(In other words, I think it should be opt-out, not opt-in.)


---

_Comment by @command-tab on 2024-09-29 18:31_

For my use cases, option 1 is sufficient. I only need `.env` files to be loaded at `uv run` time, and I would prefer `uv run` do so by default, which is equivalent to `pipenv`'s `pipenv run` behavior. Offering opt-out flags seems like a good move.

With regard to credentials, which is often my reason for using a `.env` file in development, could you explain more about this statement?

>My inclination is to do the former (and I think we could get consensus on + ship that quickly). The downside is that you still need to provide UV_-prefixed environment variables (which could include credentials) through some other mechanism.

In that scenario, if I had a development credential that was previously kept in an un-committed/.gitignored as, say, `GOOGLE_API_KEY=123456` would I need to prefix `GOOGLE_API_KEY` with `UV_` for uv to load it? Or are you talking only about env vars for configuring uv?

---

_Comment by @charliermarsh on 2024-09-29 18:34_

> Or are you talking only about env vars for configuring uv?

I'm only referring to these. In other words, we'd pass the environment variables onwards to whatever `uv run` runs, but we wouldn't use them _within uv itself_.

---

_Comment by @command-tab on 2024-09-29 18:40_

Ah, OK. That makes sense, and requiring `UV_` vars to be set elsewhere is OK for my needs. If I configure uv, I usually do so with env vars defined in my shell profile, not per-project in a `.env` file.

---

_Comment by @seapagan on 2024-09-29 19:53_

> (In other words, I think it should be opt-out, not opt-in.)

I'd prefer the opposite, though I'm probably in the minority. I tend to exclusively use virtualenvs (and with `direnv` I don't even have to activate them), and a fair percentage of my code tends to run as services or sockets which would stil need a `dotenv`-type solution so i rarely use `uv run` even in development. I think making a new feature optional at least at first (if it's configurable by the `uv.toml` and well documented this should be usable?). 
More importantly, `direnv` for example requires **explicit** permission (once) to load any `.env` file - i'd not like any repo i cloned to be able to set any ENV var it wants  - overwrite PYTHONHOME/PYTHONPATH or system stuff?

---

_Comment by @sminnee on 2024-09-29 20:30_

@charliermarsh's recommendation (start with # 1, and make it opt-out) makes a lot of sense to me.



---

_Comment by @TheRealBecks on 2024-09-30 07:04_

> I think there are two options here:
> 
> 1. Respect `.env` in `uv run` (but not elsewhere). So, `.env` would be for providing runtime environment variables, but _not_ for configuring uv itself.
> 2. Respect `.env` everywhere.
> 
> My inclination is to do the former (and I think we could get consensus on + ship that quickly). The downside is that you still need to provide `UV_`-prefixed environment variables (which could include credentials) through some other mechanism.
> 
> Maybe we start with (1) and we can always expand to (2) if it's well-motivated.
> 
> If we do (1), my preference would be:
> 
> * _Always_ read `.env`
> * Allow users to pass `--env-file` (or `UV_ENV_FILE`) to override the file.
> * Add `--no-env-file` and `UV_NO_ENV_FILE` to disable it.
> 
> (In other words, I think it should be opt-out, not opt-in.)

Option (2) is exactly what tools like Docker do, so a big 👍 here.

With option (1) we will already get most of the use cases for running local (development) `run` commands. But option (2) will be needed for e.g. `build` commands for smaller projects that don't use a CI/CD pipeline, yet. I think that option will also fulfill their needs and the `env` discussion can be closed as a happy end 😀

IMHO without option 2 that topic will pop up sooner or later again and the discussion begins from the start...

---

_Comment by @petermbauer on 2024-09-30 07:33_

> With option (1) we will already get most of the use cases for running local (development) `run` commands. But option (2) will be needed for e.g. `build` commands for smaller projects that don't use a CI/CD pipeline, yet. I think that option will also fulfill their needs and the `env` discussion can be closed as a happy end 😀
> 
> IMHO without option 2 that topic will pop up sooner or later again and the discussion begins from the start...

Fully agree. `uv run` should give the same results as executing the command after activating the venv first to not confuse people.

---

_Comment by @darthtrevino on 2024-10-01 17:36_

Just to add a thought here, it would be really handy to define a project .env file next to the lockfile that could set the `UV_EXTRA_INDEX_URL` and `UV_KEYRING_PROVIDER` variables. This way I could switch between open-source project and work projects without editing my shell environment

---

_Comment by @woutervh on 2024-10-02 23:22_

I'm always using .env to set project-specific variables for pip, twine,django,uvicorn ... in each project.

All my projects are isolated application directories (think virtualenv on steroids),
where we actively try hard to avoid on anything depending in a users $HOME-dir.  
(Avoiding: "it works on my machine" ... because of your .dotfiles)


My first use-cases would be to set the env-variables equivalents of PIP_* and "TWINE_" 
for "uv pip install" + "uv publish"  
mainly to set credentials that can not be used directly in a pyproject.toml. 

For example:

in .env
```
PERSONAL_ACCESS_TOKEN='<personal-access-token>'
TWINE_REPOSITORY_URL='<url>'
TWINE_PASSWORD='<password>'
TWINE_USERNAME='<username>'
```

then in pyproject.toml if https://github.com/astral-sh/uv/issues/5734 is implemented
```
[tool.uv]
index-url = "https://__token__:${PERSONAL_ACCESS_TOKEN}@gitlab.com/..."
```



> Respect .env everywhere.

I'm favor to always read the .env to also support the project-specific UV_*-variables to override the global default config.

> Allow users to pass --env-file (or UV_ENV_FILE) to override the file.

Yes. 

> Add --no-env-file and UV_NO_ENV_FILE to disable it.
> In other words, I think it should be opt-out, not opt-in.

Yes. opt-out via an externally managed variable makes more sense logically 
(because all other vars are already managed outside .env) 
than opt-in (use the .env) via a non-.dotenv managed variable.


---

_Comment by @charliermarsh on 2024-10-22 20:24_

In https://github.com/astral-sh/uv/pull/8263, some commenters are making the case that this should be opt-in rather than opt-out. I want to open up the conversation for more opinions here before this ships.


---

_Comment by @joshsleeper on 2024-10-22 21:01_

I personally feel like reading any `.env` or otherwise named env file by default, without any input or config telling `uv run` to do so, is undesirable.

I _also_ feel like having simple `pyproject.toml` config like `read_dotenv_files = true` (or equivalent CLI option) should opt-in to that behavior being automatic.

that allows projects (or users of projects) to trivially opt-in to dotenv style reading if it's desired/utilized and poses no risk for projects that don't desire to use it.

---

_Comment by @seapagan on 2024-10-22 21:54_

> I personally feel like reading any .env or otherwise named env file by default, without any input or config telling uv run to do so, is undesirable.
> 
> I also feel like having simple pyproject.toml config like read_dotenv_files = true (or equivalent CLI option) should opt-in to that behavior being automatic.

Yeah. I commented similar on the other thread and suggested just a global config option but this would be a good solution too with a cli option to set it (like we have for app or lib already). Either way, opt in rather than out. 

---

_Comment by @command-tab on 2024-10-22 22:57_

A couple of Python tools come to mind that _do_ load `.env` files by default. For example, `pipenv run ...` and `flask run` do this out of the box in opt-out mode. pipenv considers the `PIPENV_DONT_LOAD_ENV` env var, and Flask uses `FLASK_SKIP_DOTENV`. As someone who uses pipenv for day-to-day application development, I am excited for uv to gain auto-loading of `.env` files by default. Having to add a `pyproject.toml` entry like `read_dotenv_files = true` would be OK, though I'm sure I would forget to add it occasionally in new projects.

---

_Comment by @seapagan on 2024-10-23 07:44_

>  For example, pipenv run ... and flask run do this out of the box in opt-out mode. 

'Flask run' is however less of a tool in the 'uv' mold than very specific to Flask users, not going to be used by EVERY Python project and environment you write. It's more equivalent to 'django-admin'.

I'm not saying this is a bad thing, just  make it opt-in so there are no surprises. The large majority of users generally don't read release notes🤷‍♂️

---

_Comment by @bluss on 2024-10-23 07:52_

One facet of it is that `uv run` is very versatile, and not just used in uv projects, but could be used in any directory, and with/without existing project or virtual environment. And you can also use `uv run` to run things in a different directory like `uv run --python ./path/to/venv` or `uv run --project foo`  which also puts some distance between the `.env` file and the installed python software. Anchoring env file support to a configuration or a uv project would be a more consistent model, possibly(?)

---

_Comment by @TheRealBecks on 2024-10-23 08:29_

After the migration from `pipenv` to `uv` our local Ansible environments are broken as `uv` does not read the `.env` file by default.

Also Docker ~and Ansible~ read `.env` by default.
**Edit:** My bad, Ansible is **not** loading the environment via `.env`, that's exactly the use case why I need `uv` to load it for Ansible on my local dev environment! 😀

In my case (what our company does): The `.env` files are only needed for local development where users also run `uv run ...` **manually**. Opt-in would be no comfort from the users perspective especially when projects will be migrated from tools like `pipenv` to `uv`. Also production systems get different environment variables that will be set for the run and will not use any `.env` file and if I would like to change that behaviour I will use the `UV_xx` environment variables.

So, maybe this approach will be accepted:
1) Use opt-out by default, so `uv` reads the `.env` file by default
2) Add `UV_xx` environment variables for not reading and reading `.env` files: You already planned this feature
3) Add another option for the `pyproject.toml` file so reading an `.env` file for a project can be set to opt-in instead of the default opt-out

@seapagan @AndreuCodina Would option 3 something you would like to see in `uv`?

---

_Comment by @seapagan on 2024-10-23 08:46_

@TheRealBecks 

My preferred choice would still be opt-in by default but I can see that's unlikely to happen 😂

I've never really used 'pipenv' (went pip to poetry to uv) so the whole concept of auto-loading env files is strange to me. 

Yeah, option 3 would work but I'd really love a global setting in the configuration file too, so I can opt-out for every project and just forget about it. I've not sat down and gone through the PR in depth yet so apologies if this is already there.

---

_Comment by @jankatins on 2024-10-23 09:01_

I don't really care as long as there is an env var which can overwrite the behaviour. I let direnv load .envs and so would disable it for uv. 

We also have some projects where the .env is in a parent folder, not the repo folder. So I guess for these repos, it wouldn't work anyways.

---

_Comment by @joshsleeper on 2024-10-23 12:33_

> Opt-in would be no comfort from the users perspective especially when projects will be migrated from tools like `pipenv` to `uv`.

@TheRealBecks fwiw this doesn't track with me as an argument for opt-in by default.

If you're already having to do various other config changes and generate new lock files (you almost certainly are) and anything else in the migration process I struggle to see how marginally tweaking the new tool's config isn't entirely _expected_ anyway. Migrating between these tools isn't overly hard, but it's never as simple as literally just calling a different tool with 0 config changes either. 

---

_Comment by @gusutabopb on 2024-10-23 13:11_

Isn't placing a file in the directory you are running `uv` from named **exactly** `.env` a sign you're opting-in already? If you have a `.env`-formatted file that you don't want auto-loaded by `uv`, then just name it something else.

Asking `uv` to force users to pass an "opt-in" flag every time (or set it via `uv.toml`) to load `.env` files is like saying `git` should require that users pass a `--use-ignore-file` (or equivalent in `.git/config`) to use `.gitignore`, or asking `bash` to require users to pass some flag to read `.bashrc`, etc.

I understand there are cases where opting out or explicitly passing a specific file would be needed (e.g. multiple dotenv variants (`.env.stage`, `.env.dev`, etc), dotenv files in other directories, etc) but having `uv` auto-load a plain `.env` file in the cwd is an absolutely reasonable default IMHO.

---

_Comment by @AndreuCodina on 2024-10-23 13:42_

I can give a practical example:

`.env` and `.env.myenvironment` is the Python convention to store settings used by the code, and the code should decide if it gives priority to .env files or other sources. The settings must be managed by code, and the execution of the code the simplest and the same in all environments: `uv run`.

`myenvironment` is a value obtained with an environment variable, so:

```python
dotenv = DotEnvSettingsSource(
    settings_cls,
    env_file=[".env", f".env.{os.environ["PYTHON_ENVIRONMENT"]"],
    env_nested_delimiter="__",
    case_sensitive=True,
)
```

Then, even if you're using `pydantic-settings` or a runner tool (`uv`), you'll need an environment variable.

So, we do agree, we need to provide an environment variable, but the issue is how.

With code is simple, but verbose (because we need this file in every project):

```python
class ApplicationEnvironment(StrEnum):
    LOCAL = "Local"
    DEVELOPMENT = "Development"
    STAGING = "Staging"
    PRODUCTION = "Production"

    @staticmethod
    def get_current() -> str:
        return os.getenv("PYTHON_ENVIRONMENT", ApplicationEnvironment.LOCAL)
```

and it'll be used this way:

```python
dotenv = DotEnvSettingsSource(env_file=[".env", f".env.{ApplicationEnvironment.get_current()}"],
```

This environment variable will be used for configuring the code itself, e.g. not adding telemetry if you're in localhost, adding secrets from Azure Key Vault if you're not in localhost, not showing the Swagger documentation in production, etc.

```python
docs_url = (
    "/docs"
    if ApplicationEnvironment.get_current() != ApplicationEnvironment.PRODUCTION
    else None
)
app = FastAPI(
    docs_url=docs_url,
    redoc_url=None,
)
```

So, the thing is, when we deploy an application, we provide an environment variable on the cloud or with `docker run`, and the proper `.env.myenvironment` file is read.

But in localhost we can't do that, so where do we specify the environment variable?

**.env files are for code configuration, so use another thing**: pyproject.toml, mynewfile.json, etc.

In .NET we have a command for everything called `dotnet`, and it's the `uv` equivalent.

The launchSettings.json file is used in ASP.NET Core to configure how an application is launched during development, and you can specify environment variables and tool `dotnet` configurations.

**In summary,** `uv` or your framework should provide a (configurable) default environment variable to run the project, but not from the code settings. And this environment variable will be different in each environment, not relying on different `uv` parameters to read from here of there.

What environment variables do you need? TBH I don't think more than the PYTHON_ENVIRONMENT variable is necessary. On the cloud, with user-assigned managed identities, you add for example the `AZURE_CLIENT_ID` environment variable, but when you run the code in localhost, you don't set it by hand, you execute `az login`.


---

_Comment by @charliermarsh on 2024-10-23 13:48_

(Candidly, I don't find this framing very convincing because dotenv loading is something that other runners support and is much-used and appreciated by users of those tools.)

---

_Comment by @seapagan on 2024-10-23 16:31_

> Isn't placing a file in the directory you are running `uv` from named **exactly** `.env` a sign you're opting-in already? If you have a `.env`-formatted file that you don't want auto-loaded by `uv`, then just name it something else.

Nope, it's a sign we're following best practices, and have writtten our code to already take care of this file, on our own terms. We also did this before dotenv was a thing though it may have had a different name. I did this when using `pip` then `poetry` both of which ignored it.

> Asking uv to force users to pass an "opt-in" flag every time (or set it via uv.toml) to load .env files is like saying git should require that users pass a --use-ignore-file (or equivalent in .git/config) to use .gitignore, or asking bash to require users to pass some flag to read .bashrc, etc. 

Setting in `uv.toml` would be a once-off system-wide thing. 

Regarding the `pyproject.toml` - as already pointed out by others, there are always changes in the `pyproject.toml` needed coming from another tool to `uv` beore it works. Your argument also works the other way around - why should those who DONT want this functionality have to pass a flag or set an env each time?

You may never use this option in the settings or project toml but many people will, so why not have it implemented?

I understand the decision is already made and it's going to be opt-out regardless of this discussion, but please just at least give us a global option to set-and-forget to disable it 🤷‍♂️It's not something I'm EVER going to use as `uv` isn't used in a prod setting (for me) after the virtual env is setup. An optional setting in the project toml to disable would be icing on the cake.

For me, I DO read changelogs and release notes and will adapt regardless, not everyone does. 

---

_Comment by @theintz on 2024-10-24 14:42_

We're switching from pipenv to uv, so in order to maintain our current workflows, I'd like to add my vote to the "include .env files by default" option.

---

_Comment by @aidandj on 2024-10-24 15:47_

Another vote for load .env by default in build and run cases. A `.env` file is a sign a user is opting in, and being able to configure a build env with a .env file is incredibly useful for private repositories. 

Pipenv has a nice `loading .env file` output when it runs if you have a .env file, which makes it clear what is going on. Also, uv is probably where most people are headed after escaping pipenv, so there is some reason to maintain an existing behavior. Where new uv users can just use an opt-out flag to get the behavior they want.

---

_Comment by @jonaslb on 2024-10-27 19:43_

> In https://github.com/astral-sh/uv/pull/8263, some commenters are making the case that this should be opt-in rather than opt-out. I want to open up the conversation for more opinions here before this ships.

I don't know if it's too late now since that MR is merged. But, I'll try to outline a few arguments against making it a default:

1. Dotenv files are often used in situations where a "real" configuration file (toml, json, etc etc) with an app defined schema would be more appropriate (easier to both organise, read and validate).
2. Environment variables are often given preference over configuration files. Dotenv files are usually hidden (due to the leading dot) and it is counter intuitive that a hidden file should override a visible file.
3. What `uv` makes default, a lot of people will think is good practice. It follows that if `uv` enters the space of app configuration, it should give at least some thought to what is considered best practice for app configuration. In my opinion, it does *not* involve automatic loading of hidden files into the environment.
4. Runtime "environments" such as Kubernetes, Docker Swarm, Systemd services already offer ways of setting environment variables, and in my opinion their way should be preferred when running in those environments. They also all do it using non-hidden files (either explicitly configured environment files, or as part of their own service/resource configuration). In my opinion, it's fine for `uv` to similarly *allow* setup of environment variables, to enable locally running something that needs certain variables, but it should be explicit and opt-in, not opt-out.

---

_Comment by @charliermarsh on 2024-10-27 19:55_

(It's not too late, this was just merged into a feature branch for v0.5, but may still change between now and the actual v0.5 release.)

---

_Comment by @aidandj on 2024-10-27 20:14_

Is this issue also being used for tracking .env support in tools like `uv sync`, or is that already included in the merged PR.

---

_Comment by @jonaslb on 2024-10-27 23:20_

A few points raised in this thread that I want to respond to:

> variables like DO_NOT_TRACK=1 and HF_HUB_DISABLE_TELEMETRY=1 [...] As a workaround, I can define them in my shell profile

These "purely preferential" variables probably *do* belong in each developer's personal setup rather than having to be defined in every project he works on. So, I don't think this should be considered just a "workaround". If you are making an application where some variables must always be set, you should probably make it part of the code.

> Here's a non-Python example: Node JS added native support to this feature in September 2023 ([changelog](https://github.com/nodejs/node/blob/main/doc/changelogs/CHANGELOG_V20.md#2023-09-04-version-2060-current-juanarbol-prepared-by-ulisesgascon)) and it has been supported by Bun/Deno for a while now.

While it seems Bun does automatically load the `.env`-file, going by their docs both Node and Deno appear to need an explicit `--env-file=config.env` argument, which is what I think should be required here as well. Besides, I would point out that most tools in other languages don't support this at all. *When* there is a blessed config provider, it is usually not environment files (instead you see e.g. [`appsettings.json`](https://stackoverflow.com/q/74046813) (C#), `application.properties` (Java), ususally `something.toml` (Rust)). However, although I think it is misguided to use environment files, I can certainly accept that there is already some usage of them among Python users.

> I also feel like having simple pyproject.toml config like `read_dotenv_files = true` (or equivalent CLI option) should opt-in to that behavior being automatic.

This would be a good way to also satisfy those who want it "always on", but I think it should probably be a string option, i.e. `env_file = "dev.env"` (such that the option name matches the `--env-file` cli option).

---

_Comment by @majidaldo on 2024-10-29 16:26_

> Unfortunately i have to use Windows so direnv won't work for me.

you can [hack it](https://github.com/direnv/direnv/issues/796#issuecomment-1612192023). part of me says that this is best left to direnv. direnv should work on windows. this issue shouldn't have to do with a programming language. but it's in pixi's field.

---

_Comment by @charliermarsh on 2024-11-04 17:15_

I've decided to make this opt-in for now. I'm going to remove it from the v0.5 branch and PR it to main, since it's no longer a "breaking" change in any sense -- we can ship it in a patch release.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-04 17:15_

---

_Comment by @palewire on 2024-11-05 14:23_

> We're switching from pipenv to uv, so in order to maintain our current workflows, I'd like to add my vote to the "include .env files by default" option.

I'm in this camp. I just lost an hour of my time trying to sort of this discrepancy. I'd be much more happy switching to uv and recommending it to others if it "just worked" as a pipenv replacement.

---

_Comment by @weihenglim on 2024-11-06 02:18_

I understand the decision to make this an opt-in feature, but can we also make it configurable via `pyproject.toml`?

---

_Comment by @woutervh on 2024-11-06 08:43_

Ii would be nice to also enable the env-file via uv.toml.

---

_Comment by @ReinforcedKnowledge on 2024-11-17 03:38_

Hi!

I'd like to know what to know what are the actual best security practices when it comes to publishing to a private index. I know we can do it with one of the following ways:

- use the CLI like `uv publish --publish-url <url> --username <username> --password <password>` or `uv publish --publish-url <url> --token <token>`.
  - But it seems like redundant / hassle, and I don't think it's the most secure way of doing (shell history, process list etc.).

- use environment variables, `UV_PUBLISH_URL`, `UV_PUBLISH_USERNAME`, `UV_PUBLISH_PASSWORD`, `UV_PUBLISH_TOKEN`.
  - It's not the most secure way of doing things as well and the environment variables are available globally (in the limits of the shell session).

I recognize this might be delving into the extreme side of security concerns, I’m curious about what would be considered the best practice in this context.

Would implementing support for a `.env` file (I think not the most secure as well when we consider the security risk for the CLI, but at least it reduces the hassle and increases the flexibility I guess) or another secure configuration method be worth exploring?

I guess we can always come up with a way to set up the environment variables from an `.env` file and then remove them 🤔

You can consider my comment useless 😅

---

_Comment by @vemonet on 2024-12-10 11:29_

> I understand the decision to make this an opt-in feature, but can we also make it configurable via `pyproject.toml`?

Opt-in through CLI flags should be made illegal please. It is really bad UX design (imo at least, if there are argument for it and against config in config file I would love to hear them), configuration should be defined in the config file it seems quite logical and intuitive and reproducible.

Is it complex to add a new field to the `pyproject.toml`/`uv.toml`?

---

_Comment by @reuben on 2024-12-19 21:52_

We use several private package indexes and are currently migrating from pipenv to uv. With `pipenv`, one can setup an .env file once and copy it into projects. This file contains the equivalent of `UV_INDEX_BLAH_USERNAME` and `UV_INDEX_BLAH_PASSWORD` variables. So I would really like if `uv sync` and `uv lock` also read from .env by default, not just `uv run`.


---

_Comment by @jefvantongerloo on 2025-01-07 08:17_

Interested in .env support for exposing API keys for local testing.

---

_Comment by @reuben on 2025-01-07 08:58_

> We use several private package indexes and are currently migrating from pipenv to uv. With `pipenv`, one can setup an .env file once and copy it into projects. This file contains the equivalent of `UV_INDEX_BLAH_USERNAME` and `UV_INDEX_BLAH_PASSWORD` variables. So I would really like if `uv sync` and `uv lock` also read from .env by default, not just `uv run`.

Since writing this I've moved to `.netrc` for the credentials and to `direnv` to load `.env` files and it's working pretty well. `direnv` is more flexible than pipenv's .env file loading so we were able to put shared variables in the root of the project and only have project-specific overrides in the project folders, which we couldn't do before and required some duplication. So in the end I feel like being forced to think about a different solution landed us in a better setup.

---

_Comment by @woutervh on 2025-01-07 09:44_

> Since writing this I've moved to .netrc

In a virtualenv-based project I'd like to be independent from a user's $HOME-dir.
.netrc feels a very ancient antiquated solution.

---

_Comment by @reuben on 2025-01-07 09:52_

> > Since writing this I've moved to .netrc
> 
> In a virtualenv-based project I'd like to be independent from a user's $HOME-dir. .netrc feels a very ancient antiquated solution.

Yea, I put a `netrc.template` in the root of the project dir which users copy and paste into the `.netrc` file in the root of the project. Then, next to those files in the root there's an `.envrc` file which does:

```bash
REPO_ROOT=$(expand_path .)

export NETRC=$REPO_ROOT/.netrc
dotenv $REPO_ROOT/.env
```

Inside each project folder (in our case `projects/foo`), I have the following `.envrc` file:

```bash
# Load root .envrc first
source_env ../../.envrc

# Load .env file
dotenv_if_exists .env
```

Which loads the root `.envrc` file and optionally the project overrides in the adjacent `.env` file.

Alternatively you could also just put the UV_INDEX_BLAH env vars in the root `.envrc` and not use netrc at all.

---

_Comment by @gryznar on 2025-03-11 14:26_

For me the whole point of `.env` file is to define project specific variables. I am using it in running pytest tests directly via `pytest` rather than `uv run pytest`. I do not want to have these variables available globally, as every project has different values. Specifying path to `.env` file via another environment variable won't work in this case. Currently I am using `python-dotenv` to achieve that, but having a possibility to set this via setting in `pyproject.toml` would be very neat :)

---

_Comment by @command-tab on 2025-03-11 17:02_

I set `UV_ENV_FILE=.env` in my shell to cause uv to load .env files at runtime everywhere they are present, and this works great most of the time, but there are a few projects I work on that do not have a .env file. In those scenarios, having the `UV_ENV_FILE` environment variable set causes uv to error with `error: No environment file found at: '.env'`.

It would be great if setting `UV_ENV_FILE` caused uv to load the specified .env file if it's present but, critically, _not error_ when it's absent. This would mimic the behavior of pipenv, for example, which loads .env files automatically but happily marches onward if one is not present.

---

_Comment by @zanieb on 2025-03-11 17:19_

It seems fine to ignore missing environment files, though it's a bit of a pain to distinguish between a CLI or environment variable source.

---

_Comment by @Avasam on 2025-03-11 19:09_

> I am also migrating from pipenv which uses `.env` out of the box. It would be great if `uv run` could support this.

Funny thing is, pipenv (and not being configurable on the project level) has doing this has been *causing* us issues at work (older project we haven't had the time to switch to uv 😉 ). Since our programs cannot load `.env`, `.env.local` etc. in layers as pipenv reads the `.env` and puts those as system environment variables (with a system like Pydantic's configs, that can read the layered .env configurations, but won't override system environment). I've had to manually load our `.env.local` before initializing the config system that's supposed to automate that.

Not against this feature, just make sure it doesn't break existing workflows, or there's an opt-out that doesn't require adding a flag/venv to every `uv run` command!

---

_Comment by @sanmai-NL on 2025-03-11 21:16_

> I set `UV_ENV_FILE=.env` in my shell to cause uv to load .env files at runtime everywhere they are present, and this works great most of the time, but there are a few projects I work on that do not have a .env file. In those scenarios, having the `UV_ENV_FILE` environment variable set causes uv to error with `error: No environment file found at: '.env'`.
> 
> It would be great if setting `UV_ENV_FILE` caused uv to load the specified .env file if it's present but, critically, _not error_ when it's absent. This would mimic the behavior of pipenv, for example, which loads .env files automatically but happily marches onward if one is not present.

@command-tab Better idea, run `env -u UV_ENV_FILE uv ...` instead.

---

_Comment by @yaniv-aknin on 2025-03-22 11:55_

`uv pip` support for `.env` would be useful for me.

Suppose I have this line in my `requirements.txt`:
`git clone https://${ACCESS_TOKEN}@host/repo.git/`

In my CI environment, the key is present as a secret. In local dev, it's in a `.env` file. It'd be nice for something like `uv --env=.env pip install -r requirements.txt` to work.

---

_Comment by @0xAmaan on 2025-03-30 01:26_

> I understand the decision to make this an opt-in feature, but can we also make it configurable via `pyproject.toml`?

Would love for this to be implemented 🙏  — seems like the best solution to satisfy those who want loading .env as opt-in but reduce the UX issues of needing to load the file before executing `uv run` commands.

How difficult would this be to implement?

---

_Comment by @LeonarddeR on 2025-04-17 20:42_

+1 for making this configurable in pyproject.toml!

---

_Comment by @MaleicAcid on 2025-05-01 07:46_

+1 really need this

---

_Comment by @eelkevdbos on 2025-05-03 04:09_

Personally, I use an alias to quickly swap between behaviours:

```bash
alias uvenv='UV_ENV_FILE=.env uv'
```

Maybe this is useful for cases like described by @command-tab, where setting the environment var during shell init would otherwise error in directories not containing an `.env` file.

---

_Comment by @sofianhw on 2025-05-09 03:40_

Or someone can create plugin like poetry
https://pypi.org/project/poetry-dotenv-plugin/

after we install plugin the .env automatically loaded.

---

_Comment by @woutervh on 2025-05-09 13:00_

@sofianhw 

I developed this python package
https://github.com/libranet/autoread-dotenv/

to source the .env-file for any python-process started from the venv.

But uv is not a python-based executable installed in the venv, 
hence this request.



---

_Comment by @PushUpek on 2025-06-23 17:38_

So basically `uv pip` command is not a replacement for `uv run pip`. No support for pip.conf, no option to load .env file (using an environment variable to load a file with environment variables is absurd). In my opinion the command name should be changed because it misleads people.

---

_Comment by @charliermarsh on 2025-06-23 17:45_

`uv pip` and `uv run pip` are very different conceptually, and I don't think it would make sense to rename either of them.

`uv pip` is a set of native subcommands that mimic the `pip` CLI.

`uv run [command]` runs `[command]` in a project's virtual environment, after installing any necessary dependencies. `[command]` could be anything at all. In the case of `uv run pip`, that means "run the `pip` executable that's installed within my project's virtual environment".  (It's generally a strange to command to run at all, because if you're using `uv run`, you probably want to use uv for dependency management, not pip.)

---

_Added to milestone `v0.8.0` by @charliermarsh on 2025-06-23 17:45_

---

_Comment by @notatallshaw on 2025-06-23 17:54_

> No support for pip.conf

That's a seperate issue: https://github.com/astral-sh/uv/issues/1404, and pros and cons should be discussed there.

> no option to load .env 

Pip doesn't support ".env" files, so you're using `uv run` to get env vars from a `.env`, equally you could use `uv run uv pip ...`.

---

_Comment by @anentropic on 2025-07-07 21:17_

I would like to be able to specify in pyproject.toml that an env-file is loaded for [`project.scripts`](https://docs.astral.sh/uv/concepts/projects/config/#command-line-interfaces) commands

like pdm offers https://pdm-project.org/en/latest/usage/scripts/#env_file

often I have projects with non-version-controlled credentials in `.env` file for local development, and then various utility actions as project scripts

---

_Removed from milestone `v0.8.0` by @zanieb on 2025-07-24 21:01_

---

_Added to milestone `No target release` by @zanieb on 2025-07-24 21:01_

---

_Removed from milestone `No target release` by @zanieb on 2025-07-24 21:02_

---

_Comment by @mrexodia on 2025-08-07 13:55_

~~Just wanted to chime in that having this feature (with an opt-in in `pyproject.toml`) would be helpful for vibe-coding. Currently I use the `python-dotenv` package like this in all my projects:~~

```py
# Automatically load .env file
from dotenv import load_dotenv
load_dotenv()
```

~~Agents are often forget to use `uv run --env-file .env main.py`, so I have a template with `load_dotenv` in it.~~

You can do `uv add pyauto-dotenv`, which handles this automatically. Additionally there is `auto-truststore`, which handles a similar issue with self-signed HTTPS certificates.

---

_Comment by @Danipulok on 2025-10-14 11:14_

@zanieb , hi, sorry for pinging!
Are there any plans for this now?

Is has almost 90 likes and a lot of discussion and seems really needed, because people use workarounds to simulate this feature. So are there any plans for this? 

Just to support `UV_ENV_FILE` but declared in `pyproject.toml`

---

_Comment by @woutervh on 2025-10-16 12:26_

> because people use workarounds to simulate this feature.

FYI I just witnessed a templated pyproject.toml in a startup. "It works".

---

_Comment by @mrexodia on 2025-10-16 13:10_


> FYI I just witnessed a templated pyproject.toml in a startup. "It works".

Could you elaborate what this looks like concretely?


---

_Comment by @Danipulok on 2025-10-16 13:16_

@zanieb, hey, sorry for pining again!
Could you please update us on this topic, any plans to support it in the nearest future?
My question from 2 days ago got some activity, showing people really want it:
https://github.com/astral-sh/uv/issues/1384#issuecomment-3401292406

---

_Comment by @brendan-morin on 2025-11-02 18:13_

@Danipulok looks like this feature exists here: https://docs.astral.sh/uv/concepts/configuration-files/#env

> To load a .env file from a dedicated location, set the UV_ENV_FILE environment variable, or pass the --env-file flag to uv run.

Does that meet your intent? I tried this morning on 0.8.22 and it worked as expected for me. I didn't dive through release notes, but looks maybe to have been added in 0.8.0 from the tag above.

---

_Comment by @aidandj on 2025-11-02 18:19_

Having to set an environment variable to load, it is a non-starter for me. If it can't be configured in the pyproject.toml it doesn't exist to me. Additional steps that need to be run every time I want to use the project, or system-wide configurations aren't great.



---

_Comment by @LucasRoesler on 2025-11-02 18:38_

Adding to what @aidandj mentioned, the command isn't graceful to missing dotenv files, so it might be fine if you work on a single project or in an environment in which you can always ensure the current folder has a dotenv file, but this simply isn't true for me. I simply had to unset the env variable because it caused more issues than it resolved!

---

_Comment by @Danipulok on 2025-11-03 21:40_

Hey @zanieb, @charliermarsh, @konstin, sorry for pinging!
Is there any way you could please give some insight on your plans for this feature?
It seems to be really wanted and has a lot of likes and activity

Even my [comment](https://github.com/astral-sh/uv/issues/1384#issuecomment-3401292406) from 3 weeks ago got some likes, so people still want this feature. It would be really nice to be able to set `UV_ENV_FILE` but declared in `pyproject.toml`, not as environment variable

---

_Comment by @konstin on 2025-11-04 15:58_

Please don't repeat-ping people asking for updates, it won't make things go faster. This is not an easy feature to get correct.

---

_Comment by @jack-mcivor on 2025-11-24 17:27_

> It would be great if setting UV_ENV_FILE caused uv to load the specified .env file if it's present but, critically, not error when it's absent. 

> It seems fine to ignore missing environment files, though it's a bit of a pain to distinguish between a CLI or environment variable source.

Ah right - so `uv run --env-file .idontexist python` not erroring would be confusing to users? I guess erroring there helps highlight typos in the filename.

uv does support filenames or directory names in other configuration options, so I wonder what happens in those cases. I think:
* `--config-file`/`UV_CONFIG_FILE` - **errors** if file doesn't exist
* `--constraints`/`UV_CONSTRAINT` - **errors** if file doesn't exist
* `--excludes`/`UV_EXCLUDE` - **errors** if file doesn't exist
* `--overrides`/`UV_OVERRIDE` - **errors** if file doesn't exist
* `--cache-dir`/`UV_CACHE_DIR` - **creates** the directory if doesn't exist
* There are a few other directory options I haven't checked. AFAICS these don't have CLI versions - eg. `UV_CREDENTIALS_DIR`, `UV_INSTALL_DIR`, `UV_PYTHON_BIN_DIR` ...
* Externally defined variables also include a large number of filenames/directory names I haven't checked

I don't really have a conclusion here, was just interested in API consistency

---

_Comment by @kompre on 2025-11-25 12:09_

the ability to pick up automatically an .env file would likely solve #1495 as well.

I really want to have my `.venv` outside the project folder, and setting `UV_PROJECT_ENVIRONMENT` to some path would easily fix that. Having any env variable in `.env` file would standardise the process, but passing the flag  `--env-file .env` each time a call to `uv` is made can become tedious and easy to forget. In case of `UV_PROJECT_ENVIRONMENT` it means that if I forget, uv will create a `.venv` folder that I need to delete.

It would be nice also that `.env` file to be loaded for any uv related command. I just tested and discovered that `--env-file` does not work with `uv sync`, but it will create the `.venv` in the location specified by `UV_PROJECT_ENVIRONMENT` if I use with `uv run --env-file`, so no real reason to not allow with other commands.

---

_Comment by @Carbaz on 2025-11-25 23:08_

> **tl;dr**  
> `.env` files should not be treated as config, but as variable definitions loaded explicitly.  
> Auto‑loading only makes sense in `uv run`; outside of it, it risks mixing system and project environments and leads to misuse.  
> What is really missing in `uv` is support for centralized virtual environment folders, as pipenv, poetry, or venv already provide.  

I don't really understand this. Doesn't `uv` already allow loading a `.env` file using the `--env-file` parameter? ❓

Personally, I see *".env auto-loading"* as a potential misuse of `.env` files, since they are meant to be variable definitions that may or may not be loaded into an environment.  
*(That is why it only makes sense in `uv run` and done explicitly with `--env-file`, because it creates the environment to run within)*

I regularly use `.env` files (and variations) in projects, but they do not always match the actual environment variables in use.

I even disable *pipenv* auto-loading (via a system-wide env var), and similar tools, so that I can remain in full control of what gets loaded into the project's environment.

> I think it would be useful to have a flag like `--env-file`, or a config section similar to what PDM provides.  
> But why restrict it to `uv run` only? `uv` already uses environment variables in place of command-line arguments, so you can define different setups depending on your environment. This is particularly useful when, for example, you rely on a custom package index.

This is actually an example of the common confusion around `.env` usage: 
- `uv command` runs on your system using your system environment.  
- `uv run command` runs inside your project's environment.  

Also, `.env` files are not environment variables themselves. They are files containing variable declarations that may or may not be loaded.  
Making them auto-loaded by default could interfere or collide with the intended environment setup.

For example: consider running a Docker container with variables already defined, while a `.env` file is present.  
Which should take precedence? It is better to let the user decide, since this is purely environment-specific.  
Load your variables first, then run. (That is what `uv run --env-file env_file command` does inside the virtual environment, as far as I know.)

So, why limit the flag to `uv run`? Because system-wide environment variables should not be mixed with project-specific ones.

**In the end, the only use of `uv command --env-file xxx` would be treating the `.env` file as a config file, which is another common misuse.**

If I want to load a specific `.env` file into my environment, system or virtual, I prefer to do it explicitly. It is straightforward and ensures that the code runs in the environment as intended, regardless of whether such files are present. This is the behavior you would expect in Kubernetes, Docker Swarm, Docker Compose, etc. In those cases the environment is already set up, and your code simply reads from it.

"""Explicit is better than implicit."""

> the ability to pick up automatically an .env file would likely solve [#1495](https://github.com/astral-sh/uv/issues/1495) as well.  
> I really want to have my `.venv` outside the project folder, and setting `UV_PROJECT_ENVIRONMENT` to some path would easily fix that.

I do not think so. That issue is about configuring UV system-wide with a central folder where all projects' `.venv` directories are created, not about using each project's internal `.env` file to inject system env vars. Doing so could affect two environments at once and even create conflicts.

The challenge with `UV_PROJECT_ENVIRONMENT` is that it points to the specific folder for a single environment, not a folder containing multiple venvs. If you set it globally, you risk messing them all together. This reflects a design limitation in `uv`, since it uses an env var as a specific parameter instead of treating it as proper configuration.

i.e. what is actually needed is a `UV_PROJECTS_ENVIRONMENTS` (plural), where `uv` places all environments created, each in its own subfolder, similar to what pipenv and poetry already provide, and as requested by the OP there.

From my perspective, the fact that it is not currently easy to have centralized virtual environment folders, instead of a `.venv` in each project, makes it harder to move from tools like pipenv or poetry to uv.

Related to this issue, `.env` files are not the right place to configure `uv`. They affect how things run **inside** the project's environment, not the system's one.

The broader point is that `uv` is not a virtual environment manager like pipenv, poetry, or even venv itself. It creates the venv for you and can autoload it on `uv run`, yes, but that is the extent of it.

`uv` is an excellent dependency manager, but its environment management remains a partially implemented convenience feature. That is why, to activate the virtual environment, you still rely on `venv` directly.


---

_Comment by @mrexodia on 2025-11-25 23:20_

> For example: consider running a Docker container with variables already defined, while a .env file is present.
> Which should take precedence? It is better to let the user decide, since this is purely environment-specific.

It seems pretty clear that the user has decided to put a `.env` file in the folder with variables to set. This means they should overwrite any system variables set in the Docker environment, otherwise the user wouldn't put them in the `.env` file to begin with?

I think the feature should be opt-in in the `pyproject.toml` as various people mentioned before though, to avoid any surprises...

---

_Comment by @Carbaz on 2025-11-26 00:04_

> > For example: consider running a Docker container with variables already defined, while a .env file is present.
> > Which should take precedence? It is better to let the user decide, since this is purely environment-specific.
> 
> It seems pretty clear that the user has decided to put a `.env` file in the folder with variables to set. This means they should overwrite any system variables set in the Docker environment, otherwise the user wouldn't put them in the `.env` file to begin with?
> 
> I think the feature should be opt-in in the `pyproject.toml` as various people mentioned before though, to avoid any surprises...

Maybe is the system administrator, the operations manager, who set the environment as is and those are the ones the code should honored and the `.env` file just slipped in during deployment (Seen that happen).

Don't think in terms of "users", someones that runs his code personally and locally for himself, think on a project where you have developers, deployers, operations mngrs, etc. they may or not be the same ones (different hats), ...

Using `.env` files as config files creates this uncertainties, easy way is just not do it, you can always load a `.env` on environment if you really wants to  but "automagic default features" are always problematic.

`pyproject.toml` is the perfect place to set up how is `uv` expected to behave **inside the project as a dependency manager**, nevertheless it's the "project's file", but not the place to set how it should work as an environment manager, because your rig to develop may not be the same as mine nor the one to production deploy, environments management is not related to the project (code, app, you call it) it's related to the system hosting the virtual environment.

The right place to set up how 'uv' should behave could be a system's config file, system's env vars, system's something, so your `uv` will create envs where and how you like and mine will do as I like. 

This is, for example, how git works with it's .gitconfig on your user's folder, as many other tools.
(Always local override of those files is possible ofc, but not taking advantage of other files, other files have their own purpose.)

* You like project's folder `.venv` and autoload `.env` file on you local venv? rigth, set up as that.
* I do like centralized `.venv` folders and no implicit `.env` loading, I'll set as that.

App's code should ignore `.env` files and rely on env vars, that way *"operations"* can decide how to set up those vars too on prod: using `.env` files, using kubernetes, docker-compose, dockerfile (i do not recoment harcode env vars on images but...) using whatever they do consider the best.

I think you get my point, is the same as stated on the ***12 factor app***, the key point here is `.env` files are not config files nor environment variables, but something in between, if you think of them as "presets for environment variables" everything fits together.

* then you'll may have `.env.test`, `.env.production`, `.env.local_docker`, you name it.


P.d: Just to illustrate the first point.

I do have `.env` file on my projects locally (.env should always be .gitignored) but those are only intended values when I run the code directly on the machine within a virtual env (which is just a session with tricked paths)

But when I run it inside a container I do not want that `.env` file to load, basically it does not provide right values for a container which may even be running a different OS, so I set the containers environment on the containers manifest.

But to have auto-reload features on code changes I mount the actual local code folder as a volume so the `.env` file is still present. Is just no one loads or reads it by default. It works as a charm for local, for remote and for deploy.

---

_Comment by @mrexodia on 2025-11-26 01:50_

Personally I think if `uv run` detects a `.env` file, it should load it by default, that's the obvious user intent. Those who disagree can opt out globally. But even if that's too aggressive (which seems to be the general sentiment of this issue thread), a `pyproject.toml` option would be a huge improvement over the current situation already.

> Maybe is the system administrator, the operations manager, who set the environment as is and those are the ones the code should honored and the .env file just slipped in during deployment (Seen that happen).

I think there are separate points here. There should be no 'slipping through' during deployment, we have `.dockerignore` for a reason and this should be flagged during review. Personally I also do not think you should design a convenience feature that users _clearly_ need around the _possibility_ of messing up deployment.

You said it so yourself pretty well:

> uv is an excellent dependency manager, but its environment management remains a partially implemented convenience feature. That is why, to activate the virtual environment, you still rely on venv directly.

The person responsible for the deployment could:
1) If they call `uv run`, always specify an explicit `--env-file=.env.empty` and/or set `UV_ENV_FILE=.env.empty`
2) Not use `uv run` at all (which seems to be what you do)

Currently I have to add `pyauto-dotenv` to all my projects or always remember to pass `--env-file=.env`, it would be nice as a user if I can do the following workflow:

1) Clone a project
2) Copy `.env.example` -> `.env` and adjust the variables
3) `uv run xxx.py` and it just works™

---

_Comment by @kompre on 2025-11-26 08:59_

>> the ability to pick up automatically an .env file would likely solve https://github.com/astral-sh/uv/issues/1495 as well.
>> I really want to have my .venv outside the project folder, and setting UV_PROJECT_ENVIRONMENT to some path would easily fix that.
> 
> I do not think so. That issue is about configuring UV system-wide with a central folder where all projects' .venv directories are created, not about using each project's internal .env file to inject system env vars. Doing so could affect two environments at once and even create conflicts.
> 
> The challenge with UV_PROJECT_ENVIRONMENT is that it points to the specific folder for a single environment, not a folder containing multiple venvs. If you set it globally, you risk messing them all together. This reflects a design limitation in uv, since it uses an env var as a specific parameter instead of treating it as proper configuration.
> 
> i.e. what is actually needed is a UV_PROJECTS_ENVIRONMENTS (plural), where uv places all environments created, each in its own subfolder, similar to what pipenv and poetry already provide, and as requested by the OP there.

The main issue there, or at least the branch I subscribe too, is that people don't want their `.venv` in the same folder as the project, so the project folder could be easily sync on a samba share, dropbox, etc; the fact that a `system-wide` config location has been proposed I guess is an artifact derived by how other tools works (e.g. poetry).ù

The main issue with `uv` and a `.venv` saved outside the project folder is that `uv` needs to know about it for any subsequent call, hence setting the `UV_PROJECT_ENVIRONMENT = "~/<path to cache>/<project name>/.venv"` take care of that. The problem is that this `UV_PROJECT_ENVIRONMENT` needs to be set up for each terminal session. My workaround for that is to set the env variable in the .code-workspace so that each new terminal spawned by vscode will inherit it, but this is fragile since a random terminal session will ignore .code-workspace and also AI agent will don't know anything about. In short it require more manual setup and busywork.

What I envision is to have `UV_PROJECT_ENVIRONMENT` set in .env file stored in the project folder, so that it will affect **only** uv command run in that project. I would even prefer if I could set up `uv` behaviour to pick up .env in the pyproject.toml, or in uv.toml (always local to the project folder).

> Related to this issue, .env files are not the right place to configure uv. They affect how things run inside the project's environment, not the system's one.

I agree with you, .env file should only affect how `uv` run inside the project folder, not system-wide.



---

_Comment by @Carbaz on 2025-11-28 09:18_


> The main issue with `uv` and a `.venv` saved outside the project folder is that `uv` needs to know about it for any subsequent call, hence setting the `UV_PROJECT_ENVIRONMENT = "~/<path to cache>/<project name>/.venv"` take care of that. The problem is that this `UV_PROJECT_ENVIRONMENT` needs to be set up for each terminal session. My workaround for that is to set the env variable in the .code-workspace so that each new terminal spawned by vscode will inherit it, but this is fragile since a random terminal session will ignore .code-workspace and also AI agent will don't know anything about. In short it require more manual setup and busywork.
> 
> What I envision is to have `UV_PROJECT_ENVIRONMENT` set in .env file stored in the project folder, so that it will affect **only** uv command run in that project. I would even prefer if I could set up `uv` behaviour to pick up .env in the pyproject.toml, or in uv.toml (always local to the project folder).
> 
> > Related to this issue, .env files are not the right place to configure uv. They affect how things run inside the project's environment, not the system's one.
> 
> I agree with you, .env file should only affect how `uv` run inside the project folder, not system-wide.

That's why I propose having the UV_PROJECTS_ENVIRONMENTS in plural, that's not were an specific project's `.venv` is created, that's where all project's `.venv` folders are created, then for project XYZ you'll always find its `.venv` folder at `UV_PROJECTS_ENVIRONMENTS/XYZ-Some-hash` avoiding the need to set it up on each session (and being aware that each session can only work on a project-service, with the obvious problem when dealing with multiservice-multirepo projects), we both agree on the core problem 😃 

The problem of having the `UV_PROJECT_ENVIRONMENT` set in `.env` file is that the `.env` file is supposed to be the values of the *"project's running environment"*, not the *"hosting machine environment"* where `uv` runs, if `uv` looks inside the `.env` file it will technically using some others environment values as it's own. (And you'll be storing foreign values on the project's environment)

An `uv.toml` or `pyproject.toml` file have the problem that they are shared files, they are in repository, and they should not be changing on each one of collaborators machines depending on their own preferences or host environments.

* Where `uv` stores the `.venv` folder is irrelevant for the project itself, the project will run anyway as `uv run python app` for example, so where to store the `.venv` folder is more a personal preference on how to arrange and organize your own system.

That's why usually tools like `uv` takes their own configuration for some file on the system, because they work at host system level (at least the environment management part, for the dependencies part an in project file is the way to go obviously)

---

> Personally I think if `uv run` detects a `.env` file, it should load it by default, that's the obvious user intent. Those who disagree can opt out globally. But even if that's too aggressive (which seems to be the general sentiment of this issue thread), a `pyproject.toml` option would be a huge improvement over the current situation already.

That's quite a hard asumption, I disagree that should be the defult behaviour, "explicit always bet's implicit" but I agree that's for example the `pipenv` way and that's why I always create an alias `alias pipshell='PIPENV_DONT_LOAD_ENV=1 pipenv shell'
` I live perfect with alias, Opt in / opt out features and behaviors on this kind of highly opinionated things rules.

> 
> > Maybe is the system administrator, the operations manager, who set the environment as is and those are the ones the code should honored and the .env file just slipped in during deployment (Seen that happen).
> 
> I think there are separate points here. There should be no 'slipping through' during deployment, we have `.dockerignore` for a reason and this should be flagged during review. Personally I also do not think you should design a convenience feature that users _clearly_ need around the _possibility_ of messing up deployment.

I gave later on the `P.d` and example of a real `slip in` case on a `local deployment` , I know I can make the dockerfile and manifests to filter out the `.env` specifically, and that's probably the best way to go, but I still believe the app shouldn't asume the `.env` file is there to be loaded, if I think an app needs to get conf from a file i'll make an specific conf file rather that take advantage of another purpose file that may be there, that way you avoid ambiguities and confusions, something like `app.conf` or `config.json` is clearer, more versatile and less problematic. Slipping in a `.env` file should not happen, but being a much more common and present file that an specific one ... Still I'm in general against pre-loaded config files to deploy, many things can go wrong in many points of control for that file. 

> You said it so yourself pretty well:
> 
> > uv is an excellent dependency manager, but its environment management remains a partially implemented convenience feature. That is why, to activate the virtual environment, you still rely on venv directly.
> 
> The person responsible for the deployment could:
> 
> 1. If they call `uv run`, always specify an explicit `--env-file=.env.empty` and/or set `UV_ENV_FILE=.env.empty`
> 2. Not use `uv run` at all (which seems to be what you do)


No, I actually use things like `uv run python app` or `pipenv run python app` as entry points for the images, that helps a lot running without messing with base image environment, never the less the venvs are just a trick on paths not actual virtualization so the performance impact is nothing, ...

Ant that is why I see dangerous asume some file that may be there for other purpose or unexpected cause should be taken into account and I prefer to rely on pre-set environments (manifests or whatever ops sees fits better) or, in extreme case, pre-loaded specific config files.


> Currently I have to add `pyauto-dotenv` to all my projects or always remember to pass `--env-file=.env`, it would be nice as a user if I can do the following workflow:
> 
> 1. Clone a project
> 2. Copy `.env.example` -> `.env` and adjust the variables
> 3. `uv run xxx.py` and it just works™

That's exactly how I work, but the point is I prefer no load by default and use `uv run --env-file=.env.desired_env command` when I want to load.

I usually deliver several manifest `docker-compose-usage.yaml` or similar so I can there control what's passed in the environment so 

But if I finally have to make an `alias uvrun = uv run --env-file=/dev/null` or like I'll do, of course.

I always think of my code aimed to be used as is directly from the clone by develop collaborators, testers or some kind of already aware and trained ones, not a final user, so the need to make it a "single short command magic" is not present.

It seems this is moving to an "implicit by default Vs explicit by default" disquisition, while I prefer explicit, meanwhile there are an env var or parameter I can set on an alias/entrypoint/manifest to make it behave my way I'll be in.

The real pain points I was trying to signal were:

* Using "internal environment configs" (`.env`) to alter "external environment behavior": `uv` behavior. `uv` belongs to the host machine not to the project, despite the project takes advantage of it you install it on the host, don't you?

* Using "team shared configuration files" to store personal machine/preferences configurations that may lead to constant conflicts or "un-pushable modifications". (Same reason why usually you don't store on the repo your `.my-ide` config files)

Those two can really add more problems than the value of the convenience short command they may provide.

---

_Comment by @zanieb on 2025-12-02 10:33_

This issue is getting a bit off topic and we've supported reading `.env` files for a while now, so I'm going to close it.

See

- https://github.com/astral-sh/uv/issues/9994
- https://github.com/astral-sh/uv/issues/16672
- https://github.com/astral-sh/uv/issues/16926

---

_Closed by @zanieb on 2025-12-02 10:33_

---
