```yaml
number: 269
title: "`pre-commit` hook for `ty`"
type: issue
state: open
author: janosh
labels:
  - needs-decision
assignees: []
created_at: 2025-05-08T12:10:17Z
updated_at: 2025-12-21T14:39:03Z
url: https://github.com/astral-sh/ty/issues/269
synced_at: 2026-01-12T15:54:22Z
```

# `pre-commit` hook for `ty`

---

_@janosh_

looking forward to the https://github.com/astral-sh/ruff-pre-commit equivalent for `ty`

i understand `ty` is pre-release but i think there would be value in offering `pre-commit` integration even before the beta

---

_Label `wish` added by @MichaReiser on 2025-05-08 12:15_

---

_Comment by @lemorage on 2025-05-08 14:39_

Yes, I hope that as well. I use mypy a lot for static type checking, but the only available pre-commit hook is [mirrors-mypy](https://github.com/pre-commit/mirrors-mypy), which is maintained by the pre-commit team, not mypy itself.

---

_Comment by @AlexWaygood on 2025-05-08 15:06_

We don't plan on adding this right now because ty is still at a very early, experimental stage and we don't want to give the impression that it's ready for serious production use yet. When we're closer to feature-complete, however, we'll absolutely be adding this.

---

_Comment by @janosh on 2025-05-08 15:14_

@AlexWaygood makes sense.

for others wanting to ride out the experimental phase (given it might be a better UX than `mypy` 😄), you can use a `local` hook:

```yml
# .pre-commit-config.yaml
ci:
  skip: [ty]

repos:
  - repo: local
    hooks:
      - id: ty
        name: ty check
        entry: ty check .
        language: python
```

this will not work (e.g. in CI) unless you run `curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/ty/releases/download/0.0.0-alpha.7/ty-installer.sh | sh` first

the only issue i've had so far is i had to add this to `pyproject.toml`:


```toml
[tool.ty.rules]
unresolved-import = "ignore"
```

---

_Comment by @alon710 on 2025-05-21 12:03_

> - repo: local
>     hooks:
>       - id: ty
>         name: ty check
>         entry: ty check .
>         language: python

You can also add the ignore directly to the pre commit config file

```yaml
repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.11.0
    hooks:
      - id: ruff
        args: [--fix]
        files: \.py$
        exclude: ^(tests).*$ 
      - id: ruff-format
        files: \.py$
  - repo: local
    hooks:
      - id: ty
        name: ty check
        entry: uvx ty check . --ignore unresolved-import
        language: python
```

---

_Comment by @pelson on 2025-06-04 05:56_

Just to point it out explicitly: This will be a killer feature, since `mypy` in pre-commit is severely compromised because it runs in an isolated environment (as per https://github.com/pre-commit/mirrors-mypy). It takes quite some work to setup a correctly functioning pre-commit hook with mypy, and this is an area where `ty` can really stand-out IMO.

---

_Comment by @batiste on 2025-06-04 09:59_

I know this is very beta, but I coudn't resist trying to run ty, and here is my config:

```
- repo: local
  hooks:
    - id: ty
      name: ty check
      entry: uvx ty check . --project backend --python backend/.venv
      language: system
      files: ^backend/
```

It runs fine locally, with `pre-commit run --all-files`, but once on the CI, I get all sorts of `error[unresolved-attribute]: Type Self has no attribute creation_date` on my Django models. I did install `django-stubs` but I think its not related as the errors are present even for explicit Django model attributes.

Is there any fundamental reason we should expect `ty` to run differently inside a CI environement?

---

_Comment by @carljm on 2025-06-04 15:00_

> Is there any fundamental reason we should expect `ty` to run differently inside a CI environement?

I guess there are lots of possible reasons, mostly related to the venv used, the Python version detected, and the third-party dependencies installed. Running ty with `-vv` might clarify any differences there? But I'm not sure just based on the reported errors what it might be.

---

_Comment by @knyghty on 2025-06-24 14:35_

One issue I ran into when trying this with a project that is gradually adopting types is that `tool.ty.src.exclude` doesn't seem to be respected when running `ty check` against specific files as is done when using pre-commit.

I can work around a bit:

```yaml
  - repo: local
    hooks:
      - id: ty
        name: ty check
        entry: ty check .
        language: system
        pass_filenames: false
        always_run: true
```

And it's fine since ty is so fast, but still not quite ideal.

---

_Comment by @MichaReiser on 2025-06-24 14:38_

> One issue I ran into when trying this with a project that is gradually adopting types is that tool.ty.src.exclude doesn't seem to be respected when running ty check against specific files as is done when using pre-commit.

Yes, paths provided on the CLI take precedence. We would need to add an option similar to Ruff's `--force-exclude` to force-exclude paths that were provided on the CLI

---

_Comment by @daltunay on 2025-07-04 16:36_

My config:

```yaml
  - repo: local
    hooks:
      - id: ty-check
        name: ty-check
        language: python
        entry: ty check
        pass_filenames: false
        args: [--python=.venv/]
        additional_dependencies: [ty]
```

works in CI, no need to install `ty`

---

_Comment by @KotlinIsland on 2025-09-10 06:03_

this is very off topic, but what is the point of pre-commit? the maintainer is extremely toxic, there are more effective task runners (pyprojectx/poe etc), you have to maintain and keep in sync the version of something in two places (`uv.lock` + `pre-commit-config.yml`)

the only use-case that i can understand is easier installation/configuration of git-hooks, which personally i haven't used in years, my editor does my formatting/linting etc for me

---

_Comment by @janosh on 2025-09-10 06:11_

> the only use-case that i can understand is easier installation/configuration of git-hooks

that's also my use case

> there are more effective task runners (pyprojectx/poe etc), 

i've recently started using https://github.com/j178/prek (a rust port of `pre-commit`) and so far it's been great! maybe something astral wants to endorse/look into

---

_Comment by @AlexWaygood on 2025-09-10 06:53_

@KotlinIsland you're obviously entitled to your opinions, but please don't attack other open-source maintainers (especially if it's off-topic) on Astral issues. This isn't an appropriate forum to discuss that kind of thing.

---

_Comment by @UnoYakshi on 2025-09-10 10:00_

Currently trying to setup `ty` in a full-stack project [inside of PyCharm]. This will be a WIP-comment, hence, please, don't mind frequent updates.


# Given

File structure:
```
./
├── backend/
│   ├── .venv/
│   ├── scripts/
│   ├── src/
│   │   ├── __init__.py
│   │   ├── config.py
│   │   └── main.py
│   ├── tests/
│   │   ├── __init__.py
│   │   └── test_ping.py
│   ├── .gitignore
│   ├── .pre-commit-config.yaml
│   ├── .python-version
│   ├── pyproject.toml
│   ├── pytest.ini
│   ├── ruff.toml
│   ├── ty.toml
│   └── uv.lock
├── frontend/
│   └── ...
├── .env
├── .gitignore
└── README.md
```

`<root>/backend/.pre-commit-config.yaml`:
```yaml
repos:
  - repo: local
    hooks:
      - id: ty-check
        name: ty-check
        language: system
        entry: ty check --config-file backend/ty.toml .
```

`<root>/backend/ty.toml`:
```toml
[environment]
# Tried different varitions ― didn't help...
#root = ["./src"]
#root = ["./backend/src"]
#python = "./.venv"

python-platform = "linux"
python-version = "3.13"

[rules]
index-out-of-bounds = "ignore"
```

# CLI = O.K.
If I run `uv run ty check` from CLI under `<root>/backend`, everything seems to work just fine.

```sh
# <root>/backend (branch:main)
uv run ty check

WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 5/5 files                                                                       All checks passed!
```

# PyCharm + `git` = error
But when I try to utilize PyCharm's git-related functionality  (with `pre-commit install`), `ty`'s pre-commit hook gives me `unresolved-imports`:
```
error[unresolved-import]: Cannot resolve imported module `src.config`
  --> backend/src/main.py:21:6
   |
19 | from fastapi.templating import Jinja2Templates
20 |
21 | from src.config import settings
   |      ^^^^^^^^^^
22 |
23 | SHOW_DOCS_ENVIRONMENT = ('dev', 'staging')
   |
info: Searched in the following paths during module resolution:
info:   1. <root> (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. <root>/backend/.venv/lib/python3.13/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `src.main`
 --> backend/tests/test_ping.py:5:6
  |
3 | from httpx._status_codes import code as status_code
4 |
5 | from src.main import app
  |      ^^^^^^^^
  |
info: Searched in the following paths during module resolution:
info:   1. <root> (first-party code)
info:   2. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   3. <root>/backend/.venv/lib/python3.13/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default
``` 

# Ideas
1. I don't think turning off `unresolved-import` is a good idea.
2. The relate to PyCharm's "Content Root" set to `<root>`, not `<root>/backend`. But if I set to `<root>/backend`, it'd be unbearable to work with other parts of the project (frontend, Docker, docs).

(I think I should remove the comment as it's not really related to `ty`.)

---

_Comment by @jorenham on 2025-09-10 15:24_

@KotlinIsland isn't the only one who has his frustrations with this maintainer. It's painfully easy to find examples of the described behavior by looking through the pre-commit issues.

> This isn't an appropriate forum to discuss that kind of thing.

If the maintainer of open-source software is problematic, then by promoting the use of that software, you're increasing the likelihood that your users are confronted with this behavior. And even if we're just talking about a single person here, interactions like these can damage the trust in open-source in general.

I realize that pre-commit is the go-to git-hook manager. And technically speaking, there's nothing wrong with it, and is certainly very handy. But even so, I personally do not want to use it, because I don't trust the person that wrote it.

But I don't feel like playing the victim role here, so I'll just leave it at that.

It's also worth noting that there's no code of conduct.

---

_Comment by @janosh on 2025-09-10 15:29_

fwiw, i don't think [my comment](https://github.com/astral-sh/ty/issues/269#issuecomment-3273451387) was correctly marked as off-topic given `prek` is drop-in replacement for `pre-commit`

---

_Locked by @astral-sh on 2025-09-10 15:53_

---

_Comment by @AlexWaygood on 2025-09-10 15:54_

We'll reopen this issue in a week's time. I'll reiterate that attacks on other open-source projects and maintainers won't be tolerated on this thread

---

_Unlocked by @astral-sh on 2025-09-17 08:22_

---

_Comment by @velikopter on 2025-11-11 22:24_

I've made one: https://foundry.fsky.io/vel/ty-pre-commit

Usage instructions are there

---

_Label `needs-decision` added by @MichaReiser on 2025-11-14 08:13_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:14_

---

_Label `wish` removed by @carljm on 2025-11-15 01:55_

---

_Comment by @berkersal on 2025-12-17 13:38_

Now that beta for `ty` is released and it is said it can be used in production environments (https://astral.sh/blog/ty)
> Today, we're announcing the Beta release of [ty](https://github.com/astral-sh/ty). We now use ty exclusively in our own projects and are ready to recommend it to motivated users for production use.

Will we have the `pre-commit` hook for `ty`?

---

_Comment by @timotk on 2025-12-18 09:33_

If you have a lot of unstaged files, the current pre-commit hook will still have `ty` check them.

You can solve this by setting `pass_filenames: true` and specifying `types: [python]` (so it only checks python files). I also use `uv run` so that I don't have to rely on the virtualenv being activated.

```yaml
- repo: local
  hooks:
    - id: ty
      name: ty-check
      entry: uv run ty check
      language: python
      types: [python]
      pass_filenames: true
```

---

_Comment by @MichaReiser on 2025-12-18 09:38_

> You can solve this by setting pass_filenames: true

Note that setting `pass_filenames: true` has the downside that the pre-commit hook passes if your changes created typing errors in otherwise unchanged files. E.g. you delete a function but forgot to update all uses. 

---

_Comment by @timotk on 2025-12-18 09:40_

That's a very good point. Is there a better way of ignoring unstaged files for a hook?

---

_Comment by @MichaReiser on 2025-12-18 09:44_

Not really, this is an inherent problem of multifile checking. Your staged file might contain typing errors due to changes in your unstaged files. They're really not that disconnected anymore

---

_Comment by @spaceone on 2025-12-18 11:16_

hm, if i use the local version the pyproject.toml ignore rules aren't evaluated.

---

_Comment by @injust on 2025-12-18 16:47_

> Not really, this is an inherent problem of multifile checking. Your staged file might contain typing errors due to changes in your unstaged files. They're really not that disconnected anymore

I'm still using prek with basedpyright instead of ty, but I thought pre-commit also auto-stashes unstaged changes? Since the pre-commit hook is supposed to check the state of what you're about to commit.

---

_Comment by @hauntsaninja on 2025-12-18 18:28_

Note that the following will also apply to ty: https://github.com/python/mypy/issues/13916

---

_Comment by @hmellor on 2025-12-18 19:55_

In vLLM we accept that the staged files that are type checked might introduce errors in unchanged files.

Then we catch this in CI with a different hook that:

- runs on all files (`pass_filenames: false`)
- runs only on hook stage manual (`stages: [manual]` in the pre-commit config, `--hook-stage manual` in the GitHub Actions workflow)

---

Unrelated tip, make sure that you require serial (`require_serial: true`) so that pre-commit doesn't try to be smart and run multiple `ty` processes, therefore isolating their imports from each other. 

---

_Comment by @LordFckHelmchen on 2025-12-19 08:54_

> Currently trying to setup `ty` in a full-stack project [inside of PyCharm]. This will be a WIP-comment, hence, please, don't mind frequent updates.
> 
> ...
> 
> # Ideas
> 1. I don't think turning off `unresolved-import` is a good idea.
> 2. The relate to PyCharm's "Content Root" set to `<root>`, not `<root>/backend`. But if I set to `<root>/backend`, it'd be unbearable to work with other parts of the project (frontend, Docker, docs).

@UnoYakshi I think you're issue (and that of some of the other posts) is that
- `uv run ty` creates a `.venv` with the project's dependencies as defined in your pyproject.toml
- `prek run ty-check` creates an isolated `.venv.` for ty with only its dependencies

This might work by chance, if your project's dependencies are included in `ty`'s dependencies - but will fail otherwise. I would guess that PyCharm does not use `uv` to run `ty` (and therefore does not use the `.venv` of your project).
An easy way to check these issues, is by deleting the local `.venv` and then just run `prek` (or `pre-commit` if you must) and see if it works. If you cannot get around these issues, a hack-around is to create the `.venv` before running the hooks in CI as well. 

---

_Comment by @Cjkjvfnby on 2025-12-21 12:08_

> We don't plan on adding this right now because ty is still at a very early, experimental stage and we don't want to give the impression that it's ready for serious production use yet. When we're closer to feature-complete, however, we'll absolutely be adding this.

@AlexWaygood, ty is in Beta now. Should this ticket be prioritized?

https://astral.sh/blog/ty
> Today, we're announcing the Beta release of [ty](https://github.com/astral-sh/ty). We now use ty exclusively in our own projects and are ready to recommend it to motivated users for production use.

For my personal projects, I use pre-commit for CI checks, and having an easy hook will simplify my life.



---

_Comment by @NSPC911 on 2025-12-21 14:37_

Hey guys, so I tried a little something, before realising that others also made the same thing, so for those interested, here are some unofficial versions of it.

- https://github.com/NSPC911/ty-pre-commit (my attempt)
- https://github.com/JacobCoffee/ty-pre-commit
- https://github.com/hoxbro/ty-pre-commit
- https://github.com/allganize/ty-pre-commit (im very concerned on the rev that it uses)
- https://foundry.fsky.io/vel/ty-pre-commit (missed out on this being mentioned above)

But I noticed that there is still the issue of `unresolved-import` and `possibly-missing-import` causing failures. Until Astral manages to fix those, I wouldn't recommend using these in production with `--ignore unresolved-import --ignore possibly-missing-import` (and also the fact that these are still unofficial and not at all endored by Astral and friends)

---
