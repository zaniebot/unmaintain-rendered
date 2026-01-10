```yaml
number: 8263
title: "Add support for `.env` and custom env files in `uv run`"
type: pull_request
state: merged
author: edugzlez
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: tracking/050
head: feat/uv-run-env-file
created_at: 2024-10-16T16:51:21Z
updated_at: 2024-10-27T20:17:38Z
url: https://github.com/astral-sh/uv/pull/8263
synced_at: 2026-01-10T12:54:06Z
```

# Add support for `.env` and custom env files in `uv run`

---

_Pull request opened by @edugzlez on 2024-10-16 16:51_

## Summary

I have been reading discussion #1384 about .env and how to include it in the `uv run` command.

I have always wanted to include this possibility in `uv`, so I was interested in the latest changes. Following @charliermarsh's [advice ](https://github.com/astral-sh/uv/issues/1384#issuecomment-2381434225) I have tried to respect the philosophy that `uv run` uses the default `.env` and this can be discarded or changed via terminal or environment variables.

The behaviour is as follows:
- `uv run file.py` executes file.py using the `.env` (if it exists).
- `uv run --env-file .env.development file.py` uses the `.env.development` file to load the variables and then runs file.py. In this case the program fails if the file does not exist.
- `uv run --no-env-file file.py` skips reading the `.env` file.

Equivalently, I have included the `UV_ENV_FILE` and `UV_NO_ENV_FILE` environment variables.

## Test Plan

I haven't got into including automated tests, I would need help with this.

I have tried the above commands, with a python script that prints environment variables.


---

_Review requested from @charliermarsh by @zanieb on 2024-10-16 16:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-16 17:23_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-16 18:36_

---

_Comment by @charliermarsh on 2024-10-16 18:39_

This generally looks good, but I think we should bundle it in v0.5.0, since it is a change in behavior.

---

_Comment by @charliermarsh on 2024-10-16 18:39_

Do you mind adding a test to `run.rs` too, to validate that the files are being loaded and passed to the runtime?

---

_@charliermarsh reviewed on 2024-10-16 18:39_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:111 on 2024-10-16 18:39_

You can use `unwrap_or_else` to avoid allocating (`.to_path_buf()`) in the event that `env_file` is provided.

---

_Review comment by @edugzlez on `crates/uv/src/commands/project/run.rs`:111 on 2024-10-16 19:58_

Good point! Thanks!

---

_@edugzlez reviewed on 2024-10-16 19:58_

---

_Comment by @edugzlez on 2024-10-16 22:45_

Thank you very much for your advice and input.

The coding experience when writing tests is fantastic, I didn't expect it to be so good.

There seems to be [a problem](https://github.com/astral-sh/uv/actions/runs/11374733190/job/31644056552) running the tests, but it escapes my understanding. I've seen that other PRs are having the same error.

---

_Review comment by @j178 on `crates/uv/src/commands/project/run.rs`:113 on 2024-10-17 01:33_

I think this clone is not necessary?

---

_@j178 reviewed on 2024-10-17 01:33_

---

_@edugzlez reviewed on 2024-10-17 21:35_

---

_Review comment by @edugzlez on `crates/uv/src/commands/project/run.rs`:113 on 2024-10-17 21:35_

This is necessary because you then need to use `env_file` to check whether it is passed as an argument or the default argument is taken.

```rust
if env_file.is_some() {
```

Maybe I could change it to this:

```rust
let is_env_file_present = env_file.is_some();
let env_file_path = env_file.unwrap_or_else(|| Path::new(".env").to_path_buf());

let loaded = dotenvy::from_path_override(&env_file_path);

match loaded {
    Err(dotenvy::Error::Io(err)) => {
        if is_env_file_present {
            bail!(
                "Failed to read environment file `{}`: {}",
                env_file_path.display(),
                err
            );
        }
    }
```
to avoid the clone (but at the cost of creating a variable first).

I'm not sure.

---

_Review requested from @charliermarsh by @edugzlez on 2024-10-18 12:24_

---

_Comment by @charliermarsh on 2024-10-18 18:13_

We're planning to do v0.5 next week (hopefully) which would include this.

---

_Comment by @charliermarsh on 2024-10-22 03:07_

Will get this merged into the v0.5 branch tomorrow.

---

_@charliermarsh approved on 2024-10-22 13:36_

---

_@charliermarsh reviewed on 2024-10-22 13:36_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:113 on 2024-10-22 13:36_

I was able to remove the clone, if you're interested in seeing how.

---

_Merged by @charliermarsh on 2024-10-22 14:18_

---

_Closed by @charliermarsh on 2024-10-22 14:18_

---

_Comment by @AndreuCodina on 2024-10-22 14:46_

@charliermarsh 

Sorry, but I have to comment before this is released.

Backends in serious projects have several sources: you use the `pydantic-settings` package or the equivalent in another language, and there you define the sources and their priority.

What do I mean by "sources"?
For example, when the backend starts, the backend itself configures the sources. In the code below, the Pydantic model must be filled with values of several sources. First, it reads from Azure Key Vault if you aren't in localhost, then it reads .env.MyEnvironment files (overriding the value if the same variable is found), and then it reads from environment variables (overriding the value if the same variable is found). **You can add more sources, change the order and change their priority** (https://docs.pydantic.dev/latest/concepts/pydantic_settings/#other-settings-source).

```python
from azure.identity import DefaultAzureCredential
from pydantic import Field
from pydantic_settings import (
    AzureKeyVaultSettingsSource,
    BaseSettings,
    DotEnvSettingsSource,
    PydanticBaseSettingsSource,
    SettingsConfigDict,
)

from common.application_environment import ApplicationEnvironment


class ApplicationSettings(BaseSettings):
    webhook_url: str
    cosmosdb_connection_string: str

    model_config = SettingsConfigDict(extra="ignore")

    @classmethod
    def settings_customise_sources(
        cls,
        settings_cls: type[BaseSettings],
        init_settings: PydanticBaseSettingsSource,
        env_settings: PydanticBaseSettingsSource,
        dotenv_settings: PydanticBaseSettingsSource,
        file_secret_settings: PydanticBaseSettingsSource,
    ) -> tuple[PydanticBaseSettingsSource, ...]:
        env = EnvSettingsSource(
            settings_cls, env_nested_delimiter="__", case_sensitive=True
        )
        dotenv = DotEnvSettingsSource(
            settings_cls,
            env_file=[".env", f".env.{ApplicationEnvironment.get_current()}"],
            env_nested_delimiter="__",
            case_sensitive=True,
        )
        azure_key_vault = AzureKeyVaultSettingsSource(
            settings_cls,
            dotenv()["AzureKeyVaultUrl"],
            DefaultAzureCredential(),
        )
        settings = (env, dotenv)

        if ApplicationEnvironment.get_current() != ApplicationEnvironment.LOCAL:
            settings += (azure_key_vault,)

        return settings

```

Furthermore, `.env.Production` is read from the production environment, but also `.env` because the variables in `.env` and shared through all `.env.*` files.

Plus, I don't want to run `uv run --env-file .env.Development`, just `uv run`.

I'm not a big fan of how `pydantic-settings` is done: it should provide good defaults and an interface with less boilerplate, but it must be done with code.

The code should declare the sources, not a runner tool such as `uv`, coupling to a single source (`uv` only reads .env files, and specific .env files) and affecting the code. This PR shouldn't be released.


---

_Comment by @charliermarsh on 2024-10-22 14:51_

It's really common for runners to support `.env` files -- Node does it, Bun does it, Deno does it. If you don't want the feature to be enabled, you can just opt out of it!

---

_Comment by @AndreuCodina on 2024-10-22 15:15_

I'm not a NodeJS developer, but this PR is breaking conventions and good practices.

Developers always need to load the `.env.Local` values when they execute the code in localhost. Why do they have to use `uv run --env-file .env.Local` instead of just `uv run`? It doesn't make sense to me, even if it's optional.

But also, the code loses the control to manage the sources, their priority and their order because the runner tool decided to load values from a configuration file by default.

---

_Comment by @charliermarsh on 2024-10-22 15:24_

> Developers always need to load the .env.Local values when they execute the code in localhost. Why do they have to use uv run --env-file .env.Local instead of just uv run? It doesn't make any sense for me.

I don't understand this critique. Are you asking for `uv run` to automatically load anything that's prefixed with `.env`? Or are you asking that `uv run` never detect these?

> But also, the code loses the control to manage the sources, their priority and their order because the runner tool decided to load values from a configuration file by default.

Again: I'm sorry you feel that way but you can just disable this feature if you don't want it. A more reasonable critique would be to say that this should be opt-in rather than opt-out.

---

_Comment by @AndreuCodina on 2024-10-22 16:07_

> I don't understand this critique. Are you asking for `uv run` to automatically load anything that's prefixed with `.env`? Or are you asking that `uv run` never detect these?

Never. Imagine this case:

You have three environments (.env.Development, .env.Staging, .env.Production) and optionally localhost (.env.Local). `pydantic-settings` reads the .env file to get shared settings, and then read an environment variable (to detect the environment name) to load the proper .env file (.env.Local for example).

Another example: in .NET, by default, the environment variables have priority over configuration files (https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/?view=aspnetcore-8.0#default-application-configuration-sources). At this point, this PR is mixing 2 sources into one, and they are different.

When you develop a backend, you must read from several sources, and therefore you need `pydantic-settings` or another library. You have to load secrets from Azure Key Vault (they can't be in configuration files), auto-reloadable configurations from Azure App Configuration, configuration files, environment variables, etc.

So, you have to load from several sources.

What's an example of a shared configuration? The number of messages to read from a queue, the server to log in against, the timeout of a webhook... And sometimes you need to override it, for example, modifying the App Service environment variable, Dockerfile, App Configuration...

This PR adds the capability of, by default, reading from a source (a configuration file) and loading its values into another source (the environment variables).


> Again: I'm sorry you feel that way but you can just disable this feature if you don't want it. A more reasonable critique would be to say that this should be opt-in rather than opt-out.

I'm against adding this because instead of advantages or options, it adds a feature that I'm pretty sure how it'll be used: instead of adding a library such as `pydantic-settings`, developers will add parameters to `uv` to read from configuration files, and they'll load the secrets modifying the cloud, adding environment variables by hand.

But yes, my issue is `uv` is reading from the `.env` file by default when I think it shouldn't.

---

_Comment by @pantheraleo-7 on 2024-10-22 17:48_

> A more reasonable critique would be to say that this should be opt-in rather than opt-out.

This (reading `.env` file) is a great feature, but not a sensible default. It should be opt-in.

If the environment is not loaded programmatically, `uv run` doing it (by default, without any flags) may cause different behaviour when running it _without_ `uv` and also it will be too much "magic BTS".

> Explicit is better than implicit.
~ Zen of Python, PEP20

---

_Comment by @seapagan on 2024-10-22 20:35_

> > A more reasonable critique would be to say that this should be opt-in rather than opt-out.
> 
> 
> 
> This (reading `.env` file) is a great feature, but not a sensible default. It should be opt-in.
> 
> 
> 
> If the environment is not loaded programmatically, `uv run` doing it (by default, without any flags) may cause different behaviour when running it _without_ `uv` and also it will be too much "magic BTS".
> 
> 
> 
> > Explicit is better than implicit.
> 
> ~ Zen of Python, PEP20

Opt-in as it's technically not backwards compatible and may be unexpected - and I'd argue the majority of users won't use this. In production my apps may need to read .env vars (though should be done by actual vars at that stage) but they are never RUN through 'uv'. So they need  to include dotenv or whatever anyway. 

My big issue is transparency- some env vars can change the actual paths and options of that process - I don't want to run 'uv' in a repo I've just cloned and have it QUIETLY set env vars I don't know about. 

Opt in please. Make it a global setting that is set and forget, but please don't do it behind our backs.

---

_Comment by @charliermarsh on 2024-10-22 20:36_

(Not disagreeing with your broader point, but I'll just note that this is already marked as a breaking change that would be included in v0.5 per our versioning policy. It's ok for it _not_ to be backwards compatible.)

---

_Label `breaking` added by @charliermarsh on 2024-10-22 20:39_

---

_Label `enhancement` added by @charliermarsh on 2024-10-22 20:39_

---

_Comment by @seapagan on 2024-10-22 20:41_

Fair point 😃

My points still stand though. There is a reason that 'direnv' REQUIRES you to physically allow auto-reading an env file the first time - security.

Maybe do this for each new project the first time? At least give the user the choice. Otherwise a global option, but please, Opt in.

---

_Comment by @zanieb on 2024-10-22 20:46_

> My big issue is transparency- some env vars can change the actual paths and options of that process - I don't want to run 'uv' in a repo I've just cloned and have it QUIETLY set env vars I don't know about.

I don't know if this is a fair concern. If you clone an arbitrary repository and execute `uv run` in it, you're already trusting the repository. They can execute arbitrary code at that point. `direnv` is different because it hooks into your shell and affects _any_ command you run in the directory (i.e. the repository can be untrusted and you can still operate on it).

---

_Comment by @seapagan on 2024-10-22 21:00_

> I don't know if this is a fair concern. If you clone an arbitrary repository and execute uv run in it, you're already trusting the repository. They can execute arbitrary code at that point. direnv is different because it hooks into your shell and affects any command you run in the directory (i.e. the repository can be untrusted and you can still operate on it).

This is very true yes. Maybe it's just my prejudice showing, and I really have no use-case for it. In many cases my code is running uvicorn-> guniciorn -> nginx so 'uv' is not in the pipe and any .env would need to be loaded manually. If I'm using an env file, I'd make sure my code explicitly reads it, and that is really more Zen.

I truly believe you may end up with new issues related to unexpected env vars being set - how many people really read the release notes or just go 'yeah yeah, upgrade'😜.

I'll stand by my 'opt-in' recommendation but obv not going to stop using uv if you choose otherwise. 


---

_Comment by @seapagan on 2024-10-22 21:01_

One more thing - anyone who wants and uses this feature should have no problem opting in?🤷‍♂️

---

_Comment by @TheRealBecks on 2024-10-23 08:28_

After the migration from `pipenv` to `uv` our local Ansible environments are broken as `uv` does not read the `.env` file by default.

Also Docker and Ansible read `.env` by default.

In my case (what our company does): The `.env` files are only needed for local development where users also run `uv run ...` **manually**. Opt-in would be no comfort from the users perspective especially when projects will be migrated from tools like `pipenv` to `uv`. Also production systems get different environment variables that will be set for the run and will not use any `.env` file and if I would like to change that behaviour I will use the `UV_xx` environment variables.

So, maybe this approach will be accepted:
1) Use opt-out by default, so `uv` reads the `.env` file by default
2) Add `UV_xx` environment variables for not reading and reading `.env` files: You already planned this feature
3) Add another option for the `pyproject.toml` file so reading an `.env` file for a project can be set to opt-in instead of the default opt-out

@seapagan @AndreuCodina Would option 3 something you would like to see in `uv`?

---

_Comment by @seapagan on 2024-10-25 10:26_

I'd still ask kindly if you can add a setting to the uv.toml so we can disable this globally please :pray: 

---

_Comment by @aidandj on 2024-10-27 20:17_

Suggestion here to make the `Read .env file` debug output info or something else visible more easily. It will help with the transition, and prevent confusion of people not realizing the silent behavior.

---
