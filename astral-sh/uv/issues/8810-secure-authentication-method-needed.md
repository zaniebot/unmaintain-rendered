---
number: 8810
title: Secure authentication method needed
type: issue
state: open
author: stoney95
labels:
  - question
assignees: []
created_at: 2024-11-04T15:31:58Z
updated_at: 2025-11-13T14:02:04Z
url: https://github.com/astral-sh/uv/issues/8810
synced_at: 2026-01-10T01:24:33Z
---

# Secure authentication method needed

---

_Issue opened by @stoney95 on 2024-11-04 15:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I would like to use `uv` to install dependencies from a private artifactory repository. Authentication for the private repository works with username and password. I would also like to store the index-url in the pyproject.toml. This simplifies the usage of uv for every team-member. The current minimal config looks like the following:

```toml
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[tool.uv]
index-url = "https://artifactory.company.com/pypi/simple"
native-tls = true
keyring-provider = "subprocess"
```

[The documentation for authentication](https://docs.astral.sh/uv/configuration/authentication/#http-authentication) lists these options:

- The URL, e.g., `https://<user>:<password>@<hostname>/...`
- A [.netrc](https://everything.curl.dev/usingcurl/netrc) configuration file
- A [keyring](https://github.com/jaraco/keyring) provider (requires opt-in)

The first option is not possible, as the credentials would be shared via git. Using `.netrc` is only partially possible as it poses a security risk by storing credentials in plain text. I tried this option nonetheless and it worked. But I would like to avoid it due to the plain-text password.
The thrid option does not work. I will describe in detail, what I tried and where it failed.

1. `pip install keyring` 
2. `keyring set artifactory.company.com my_username` and entering the credentials as prompted
3. running `uv add <any-package>` with the configuration from above failed with
```
hint: An index URL (https://artifactory.company.com/pypi/simple) could not be queried due to a lack
      of valid authentication credentials (401 Unauthorized).
```

I changed the configuration value for `index-url` to `https://my_username@artifactory.company.com/pypi/simple`. By this `uv` reads the credentials from `keyring`. As this stores the username in the pyproject.toml the approach does not work. Every team-member uses their own credentials to authenticate with the private artifactory. 

- uv version: `uv 0.4.25 (97eb6ab4a 2024-10-21)`
- uv platform: `Windows 11`

If `uv` should work with the plain index-url (no username) and keyring, I would like to report this as a bug. In case it doesn't, I would like to request a secure methode for storing credentials while preserving a transferable `uv` configuration. 


---

_Comment by @charliermarsh on 2024-11-04 15:40_

Keyring alway require a username. If you name your index, you can provide credentials for your index via environment variables and store them however you want:

```toml
[[tool.uv.index]]
name = "company"
url = "https://artifactory.company.com/pypi/simple"
default = true
```

Then you can specify a username with `UV_INDEX_COMPANY_USERNAME` and a password with `UV_INDEX_COMPANY_PASSWORD`. See: https://docs.astral.sh/uv/configuration/indexes/#providing-credentials.


---

_Label `question` added by @charliermarsh on 2024-11-04 15:40_

---

_Comment by @stoney95 on 2024-11-04 16:57_

@charliermarsh, thanks for pointing out the solution using environment variables. 

Do you currently plan to provide a solution similar to [`poetry config http-basic`](https://python-poetry.org/docs/repositories/#installing-from-private-package-sources)? This uses `keyring` under the hood, but stores which username to use for each index on the user-level. 

Using your example, I would propose the following workflow:

1. Define the index (making this possible via cli, e.g. `uv index add ...` would be nice to have)
```
[[tool.uv.index]]
name = "company"
url = "https://artifactory.company.com/pypi/simple"
default = true
```
2. Define credentials with `uv index config company "my_username"`. This will prompt for the password. The username is stored in `$XDG_CONFIG/uv/uv.toml`, e.g.
```toml
[index.company]
username = "my_username"
```
The password is stored with keyring. 

Setting this config up once, makes it possible to run `uv add <package>`. Which is then pointing to the right index and using credentials automatically

---

_Comment by @ljtruong on 2024-11-14 01:16_

Thought this might help. I've made a hacky way to work as a "config" to set the environment variables to authenticate for private pypi repo. My company uses aws so the tokens are short lived, so it's safe to do. 

For more info, you would replace `PASSWORD` with the CodeArtifact auth command to retrieve the password token automatically and set the new credentials when it expires. 

```
#set_credentials.sh
if [ ! -f ~/.uv ]; then
  touch ~/.uv
fi

update_credentials() {
    local PASSWORD="updated_password"
    # Set UV_INDEX_INTERNAL_COMPANY_PASSWORD in the ~/.uv file
    if grep -q "^export UV_INDEX_INTERNAL_COMPANY_PASSWORD=" ~/.uv; then
      # Use sed to replace the value with the local $PASSWORD variable
      sed -i '' "s/^export UV_INDEX_INTERNAL_COMPANY_PASSWORD=.*/export UV_INDEX_INTERNAL_COMPANY_PASSWORD=${PASSWORD}/" ~/.uv
    else
      # Append the line with $PASSWORD if it doesn't already exist
      echo "export UV_INDEX_INTERNAL_COMPANY_USERNAME=aws" >> ~/.uv
      echo "export UV_INDEX_INTERNAL_COMPANY_PASSWORD=${PASSWORD}" >> ~/.uv
    fi
}
update_credentials
source ~/.uv
```

Run this using
```
source set_credentials.sh
```

---

_Comment by @zanieb on 2024-11-14 15:13_

> Do you currently plan to provide a solution similar to [poetry config http-basic](https://python-poetry.org/docs/repositories/#installing-from-private-package-sources)? This uses keyring under the hood, but stores which username to use for each index on the user-level.

Yeah I'm interested in this, but we'll need to switch from the Python keyring library to something more robust and Rust native which will take time.

---

_Comment by @ThomasHezard on 2024-11-25 12:14_

Hello,

I run into the same kind of question: What is the best way of authenticate on private indexes with `uv`?   
If I understand well, the recommended solution today is to use the [`UV_INDEX_XXX` env variables](https://github.com/astral-sh/uv/issues/8810#issuecomment-2455047015).    
However, this is not a very practical solution as [`.env` file is not supported by `uv sync`](https://github.com/astral-sh/uv/issues/1384), which means we have to set numerous specific variables for `uv`.

I ended up using the same "hacky" (or at least non-standard) solution as described [here](https://github.com/astral-sh/uv/issues/8810#issuecomment-2475149308), with a `set_env.sh` script in my repo setting the `UV_INDEX_XXX` env variables to the appropriate values on both devs and CI machines.

In my case, I see two solutions which would be less "hacky" and much more practical:
- accepting `.env` file in the `uv sync` command,
- or interpreting env variables in pyproject.toml.

Do you have any of these two planned or do you have something else to recommend?

Thank you 🙏 

---

_Comment by @zanieb on 2024-11-25 23:13_

> accepting .env file in the uv sync command,
 
 Why not activate your `.env` file with another tool like `direnv`? Why is it uv's job to read configuration from that file?

>  interpreting env variables in pyproject.toml

Wouldn't you still need to set the variables? Isn't that part of the complaint?

I believe you can also configure your credentials in the `uv.toml`, if you prefer.

---

_Comment by @ThomasHezard on 2024-11-26 15:04_

> > interpreting env variables in pyproject.toml
> 
> Wouldn't you still need to set the variables? Isn't that part of the complaint?
> 
> I believe you can also configure your credentials in the `uv.toml`, if you prefer.

The thing that makes it a bit more complicated with `uv` for me (and my team) is that `uv` has a strict convention about the env variable names for authentification. With other tools allowing using env variables directly in config files, we can easily have a universal configuration across tools that work on both devs and CI machines with simple setup, which is quiet convenient.

> > accepting .env file in the uv sync command,
> 
> Why not activate your `.env` file with another tool like `direnv`? Why is it uv's job to read configuration from that file?

I must admit I am not familiar with `direnv` as I never needed it. It may be a very good solution indeed, I'll have a deeper look at it.

Thank you for the answer 🙏 


---

_Comment by @zanieb on 2024-11-26 15:33_

> With other tools allowing using env variables directly in config files, we can easily have a universal configuration across tools that work on both devs and CI machines with simple setup, which is quiet convenient.

Can you share some example tools? I'd like to look at their approaches.

---

_Comment by @ThomasHezard on 2024-11-28 22:14_

> Can you share some example tools? I'd like to look at their approaches.

First, I need to say that I don't come from the Python world, I only started using Python at production scale quite recently. My programming background is mostly C++ and iOS/Android development, some UNIX system administration, Docker and k8s deployment, and Matlab quite some time ago.

I used quite a lot of different tools for configuration, environment management, compilation, testing, packaging or deployment, and most of them support reading environment variables within their "configuration files" (or equivalent). 
A few examples:
- [`bundler`](https://bundler.io) in gemfiles,
- [`fastlane`](https://fastlane.tools) and other Ruby-based tools,
- [`cmake`](https://cmake.org) and [`gradle`](https://gradle.org),
- I'm pretty sure Conan and Bazel support env variables one way or another but I'm not very familiar with these,
- Docker in Dockerfiles and at runtime,
- make in makefiles,
- in Python, I have used `setuptools` with `setup.py`, which is native python giving you the ability to use `os.environ`,
- gitlab CI yaml
- [helm charts](https://helm.sh/) for k8s

Many of these tools support env variables the same way you use it in shell: `$MY_ENV_VAR`.
The `ENV['MY_ENV_VAR']` syntax is also present in several of these tools and I'm pretty sure I've seen it in other contexts. cmake has it a bit different with `$ENV{MY_ENV_VAR}`. 

Hope this helps :)

---

_Comment by @farndt on 2024-12-03 23:14_

> > Do you currently plan to provide a solution similar to [poetry config http-basic](https://python-poetry.org/docs/repositories/#installing-from-private-package-sources)? This uses keyring under the hood, but stores which username to use for each index on the user-level.
> 
> Yeah I'm interested in this, but we'll need to switch from the Python keyring library to something more robust and Rust native which will take time.

Would keyring-rs be an option: https://docs.rs/keyring/latest/keyring/ to speed up transition?

---

_Comment by @zanieb on 2024-12-04 00:14_

Yeah we'd probably use that, though I haven't audited it.

Thanks @ThomasHezard !

---

_Comment by @farndt on 2024-12-05 23:18_

I would suggest the following implementation for Keyring:

1. define index in pyproject.toml
```toml
[[tool.uv.index]]
name = “service-name”
url = “https://artifactory.company.com/*/simple”
```

2. keyring set artifactory.company.com my_username -> enter password

3. **uv** reads _username_ and _password_ using Keyring get_credentials _service-name_ 

This would allow teams to use keyring without making changes to pyproject.toml. Due to lack of encryption and simple logging it should be avoided to use environment variables as storage for credentials.

This approach could be quickly implemented temporarily with keyring as a Python package. When switching to Keyring-RS at a later date, the process would not need to change.

---

_Comment by @stoney95 on 2024-12-07 10:20_

@zanieb, what are your concerns about keyring-rs? I would be interested to contribute the keyring credentials feature and would like to understand the constraints 

@farndt, can you share some documentation about get_credentials without a username? afaik, you need a service and a username to reference keyring entries. 

---

_Comment by @zanieb on 2024-12-07 16:20_

No concerns; we just audit new dependencies for trustworthiness, correctness, etc.

Feel free to contribute support if you're interested. I think the first step is to have `--keyring-provider <something>` use the crate as a backend?

---

_Comment by @stoney95 on 2024-12-08 00:03_

I would suggest the following: 
- `uv add` [loads the credentials for all indexes](https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/project/add.rs#L286-L290) into the cache
- This uses `[Index.credentials](https://github.com/astral-sh/uv/blob/main/crates/uv-distribution-types/src/index.rs#L141-L152)`
  - This currently only supports env variables `UV_INDEX_XXX` and `[Credentials.from_url](https://github.com/astral-sh/uv/blob/main/crates/uv-auth/src/credentials.rs#L119-L122)`
  - I would suggest to add logic for loading username from `auth.toml` in uv cache dir and checking keyring in `Index.credentials`
- To configure the credentials I would add a new command `index`. 
  - This provides `uv index auth <index-name>`.
  - It uses the already existing `[keyring provider](https://github.com/astral-sh/uv/blob/main/crates/uv-auth/src/keyring.rs)`

As there is already an implementation for keyring within `uv`, I would rely on it for this topic. Using `keyring-rs` can then be dealt with independently, if desired. 

---

_Referenced in [astral-sh/uv#9165](../../astral-sh/uv/issues/9165.md) on 2024-12-08 01:38_

---

_Comment by @farndt on 2024-12-10 14:25_

> [@zanieb](https://github.com/zanieb), what are your concerns about keyring-rs? I would be interested to contribute the keyring credentials feature and would like to understand the constraints
> 
> [@farndt](https://github.com/farndt), can you share some documentation about get_credentials without a username? afaik, you need a service and a username to reference keyring entries.

@stoney95 Coming from python keyring: https://github.com/jaraco/keyring?tab=readme-ov-file#api it has `get_credential(service, username)` but username can be anything and you will recieve username related to service.

I am not sure but you could recieve `username` as attribute of the service. For the request may the username os variable can be used? https://github.com/hwchen/keyring-rs/blob/master/src/windows.rs comments at begin of file (line 24)

```
The [get_attributes](crate::Entry::get_attributes) call will return the values in the
`username`, `comment`, and `target_alias` fields (using those strings as the attribute names),```

---

_Referenced in [astral-sh/uv#9920](../../astral-sh/uv/pulls/9920.md) on 2024-12-15 20:10_

---

_Comment by @zyaoj on 2025-02-05 09:28_

Hello! I encountered a similar issue when trying to install from a wheel that is publicly available. I've tried `[[tool.uv.index]]` as well but had no luck. To reproduce:

```sh
uv pip install pytorch3d --extra-index-url https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py310_cu121_pyt241/pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of pytorch3d are available:
          pytorch3d==0.1.1
          pytorch3d==0.2.0
          pytorch3d==0.2.5
          pytorch3d==0.3.0
          pytorch3d==0.4.0
          pytorch3d==0.5.0
          pytorch3d==0.6.1
          pytorch3d==0.6.2
          pytorch3d==0.7.0
          pytorch3d==0.7.1
          pytorch3d==0.7.2
          pytorch3d==0.7.3
          pytorch3d==0.7.4
      and pytorch3d<=0.6.2 has no wheels with a matching Python ABI tag, we can conclude
      that pytorch3d<=0.6.2 cannot be used.
      And because pytorch3d==0.7.0 has no wheels with a matching platform tag and
      pytorch3d==0.7.1 has no wheels with a matching Python ABI tag, we can conclude that
      pytorch3d<0.7.2 cannot be used.
      And because pytorch3d>=0.7.2 has no wheels with a matching platform tag and you
      require pytorch3d, we can conclude that your requirements are unsatisfiable.

      hint: An index URL
      (https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py310_cu121_pyt241/pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl)
      could not be queried due to a lack of valid authentication credentials (403
      Forbidden).
```

However on the same machine, we can download the wheel with `wget`:

```sh
wget https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py310_cu121_pyt241/pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl
--2025-02-05 09:15:31--  https://dl.fbaipublicfiles.com/pytorch3d/packaging/wheels/py310_cu121_pyt241/pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl
Resolving dl.fbaipublicfiles.com (dl.fbaipublicfiles.com)... 108.156.184.100, 108.156.184.129, 108.156.184.78, ...
Connecting to dl.fbaipublicfiles.com (dl.fbaipublicfiles.com)|108.156.184.100|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 20521514 (20M) [binary/octet-stream]
Saving to: ‘pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl’

pytorch3d-0.7.8-cp310-c 100%[=============================>]  19.57M  16.9MB/s    in 1.2s

2025-02-05 09:15:33 (16.9 MB/s) - ‘pytorch3d-0.7.8-cp310-cp310-linux_x86_64.whl’ saved [20521514/20521514]
```

Could we get some help? Thanks in advance!

---

_Comment by @zanieb on 2025-02-05 15:55_

I can confirm that I can download the file. I can't really speculate as to why they'd throw a 403 there. This looks unrelated to the topic in this issue though, please see #9452 

---

_Referenced in [TechnologyBrewery/habushu#312](../../TechnologyBrewery/habushu/issues/312.md) on 2025-04-01 19:38_

---

_Comment by @lendle on 2025-06-27 15:58_

I'm facing a similar issue as @stoney95 where we're using artifactory with every team member & CI pipeline having unique username & password credentials. An additional wrinkle is that each team has a separate artifactory repo, so managing dependencies from different teams is already a bit of a pain (independent of uv). 

I'm trying to encourage folks to move to uv from poetry and this seems to be the first major hiccup I've run into where things are decidedly worse than poetry. At least without something like `config.http-basic`. All options I can think of require an additional step or tool on top of learning uv which is a pain.

Our options: 

* Use keychain
  * Requires specifying username in index url. Works okay for users who want to set up indexes in a global uv.toml config and use `uv pip` in place of `pip`, but is a non-starter for indexes in pyproject.toml
  * Our repos are on the same host (or VIP or whatever) & keychain auth is at the host level so multilple repos aren't too bad
  * requires keychain
* Use `UV_INDEX_{name}_USERNAME` and `_PASSWORD`
  * Works for projects, but needs additional config. We can do something like: 
    ```
    export UV_INDEX_A_USERNAME=${ARTIFACTORY_USER}
    export UV_INDEX_A_PASSWORD=${ARTIFACToRY_PASS}
    export UV_INDEX_B_USERNAME=${ARTIFACTORY_USER}
    export UV_INDEX_B_PASSWORD=${ARTIFACTORY_PASS}
    ```
    but we need to manually source that file each time we work with the project, or adopt a tool like `direnv` (which I'd never heard of before this thread
  * Requires making changes in two places whenever adding an index (`pyproject.toml` and wherever env vars are configured)

---

_Comment by @alanwilter on 2025-09-04 17:09_

I'm trying to use `keychain` on my Mac.

I did:

```sh
UV_PREVIEW_FEATURES=native-auth uv auth login https://www.reportlab.com/pypi/
```
entered login and password and I can see in keychain an entry:`uv:https://www.reportlab.com/pypi/`

Then I tried:
```sh
UV_PREVIEW_FEATURES=native-auth uv sync
```
but it failed with:

```
$ UV_PREVIEW_FEATURES=native-auth uv sync                                                                                                                        on git:main|✚1-1…9
Resolved 86 packages in 2ms
  × Failed to download `preppy==5.0.1`
  ├─▶ Failed to fetch: `https://www.reportlab.com/pypi/packages/preppy-5.0.1-py3-none-any.whl`
  ╰─▶ HTTP status client error (401 Unauthorized) for url (https://www.reportlab.com/pypi/packages/preppy-5.0.1-py3-none-any.whl/)
  help: `preppy` (v5.0.1) was included because `pdfgen-showdown` (v0.1.0) depends on `rlextra` (v4.4.3.1) which depends on `preppy`
```

If I register in plain (`~/.local/share/uv/credentials/credentials.toml`), it works.
If I use:
```
export UV_INDEX_REPORTLAB_USERNAME="..."
export UV_INDEX_REPORTLAB_PASSWORD="..."
```
it works as well.

I'm simply failing to get it to work with `UV_PREVIEW_FEATURES=native-auth`.


---

_Comment by @zanieb on 2025-09-04 17:12_

The native store doesn't support "prefix" matches yet. I think we need to fix that before you example will work. Can you login to `www.reportlab.com` instead? (We do support domain-level matches)

p.s. Thank you for trying the feature and taking the time to provide feedback!

---

_Comment by @alanwilter on 2025-09-05 07:28_

Yes, I can login to `www.reportlab.com` using the same credentials I use above.
I'm not sure if I understand:

> The native store doesn't support "prefix" matches yet
or
> We do support domain-level matches

But let me know when you guys think Mac `keychain` should be working.

---

_Comment by @RStreitfeld on 2025-09-12 16:30_

> The native store doesn't support "prefix" matches yet. I think we need to fix that before you example will work. Can you login to `www.reportlab.com` instead? (We do support domain-level matches)
> 
> p.s. Thank you for trying the feature and taking the time to provide feedback!

I can confirm the issue and tried the domain login you suggested, means using
`UV_PREVIEW_FEATURES=native-auth uv auth login www.my_company.com`

Then I try:
`UV_PREVIEW_FEATURES=native-auth uv -v sync`
and see in the debug output, that uv is looking for plain text credentials:
`DEBUG Loaded credential file /home/...`
...
`DEBUG Checking text store for credentials for https://www.my_company.com/...`
...
`  ╰─▶ Missing credentials for https://www.my_company.com/...`

If I login with plain text storage, `UV_PREVIEW_FEATURES=native-auth uv -v sync` also works.

---

_Comment by @zanieb on 2025-09-12 16:58_

Thanks @RStreitfeld I'll look into that!

---

_Referenced in [astral-sh/uv#15818](../../astral-sh/uv/issues/15818.md) on 2025-09-12 16:59_

---

_Comment by @threddast on 2025-11-13 14:02_

I am having a similar issue, `uv auth` can read/write from/to keyring, but `uv sync` fails to load the credentials.

UV version: tested both 0.8.23 and 0.9.7
Platform: NixOS

* Set `UV_PREVIEW_FEATURES` to `native-auth`, `UV_KEYRING_PROVIDER` is unset
* Run `uv auth login gitlab.mycompany.com -t glpat-xxxx`
* Check keyring, a new entry was created:
  * application: `uv`
  * service: `uv:gitlab.mycompany.com`
  * target: `default`
  * username: `__token__`
* `uv auth token gitlab.mycompany.com` prints the correct token
* go to a project, `uv sync -vv`

```
...
DEBUG No cache entry for: https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl
TRACE Sending fresh GET request for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl
TRACE Handling request for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl with authentication policy always
TRACE Request for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl
TRACE Checking for credentials for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl
TRACE No credentials in cache for URL https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/simple
TRACE No credentials in cache for realm https://gitlab.mycompany.com
DEBUG No netrc file found
TRACE Checking lock for `credentials store` at `/home/user/.local/share/uv/credentials/credentials.toml.lock`
DEBUG Acquired lock for `credentials store`
DEBUG Released lock at `/home/user/.local/share/uv/credentials/credentials.toml.lock`
DEBUG No credentials file found at /home/user/.local/share/uv/credentials/credentials.toml
DEBUG Checking native store for credentials for index URL https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi
TRACE Checking keyring for URL https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi
TRACE Checking keyring for host gitlab.mycompany.com
TRACE Considering retry of error: Middleware(Missing credentials for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl)
TRACE Cannot retry error: Neither an IO error nor a reqwest error
TRACE Considering retry of error: Error { kind: WrappedReqwestError(DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("gitlab.mycompany.com")), port: None, path: "/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl", query: None, fragment: None }, WrappedReqwestError { error: Middleware(Missing credentials for https://gitlab.mycompany.com/api/v4/projects/17/packages/pypi/files/c3e6649ac4fe7d379735914a307f61b084d140fb9639cde3e81602397fd461cc/mypackage-0.2.2-py3-none-any.whl), problem_details: None }), retries: 0 }
TRACE Cannot retry nested reqwest middleware error
```

* unset `UV_PREVIEW_FEATURES`
* `uv auth login gitlab.mycompany.com -t glpat-xxx` -> token now written in `credentials.toml`
* `uv sync` works (just to confirm that the glpat token is valid)

---
