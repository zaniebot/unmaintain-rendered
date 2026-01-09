---
number: 1404
title: "Use settings from the pip configuration file (`pip.conf`)"
type: issue
state: open
author: msharp9
labels:
  - compatibility
  - configuration
assignees: []
created_at: 2024-02-16T00:54:00Z
updated_at: 2025-12-20T03:50:58Z
url: https://github.com/astral-sh/uv/issues/1404
synced_at: 2026-01-07T13:12:16-06:00
---

# Use settings from the pip configuration file (`pip.conf`)

---

_Issue opened by @msharp9 on 2024-02-16 00:54_

Pip allows a user to set configuration settings inside a `pip.conf`, usually stored at `~/.config/pip/pip.conf`.  For example, setting a default index-url. uv seems to ignore these settings completely.  

This is a problem since enterprise companies typically have their own PyPI images and don't want users to use the public PyPI for security reasons. For example, users who use AWS codeartifact have this config file updated automatically when they run aws login. https://docs.aws.amazon.com/codeartifact/latest/ug/python-configure-pip.html

---

_Label `enhancement` added by @zanieb on 2024-02-16 00:55_

---

_Label `compatibility` added by @zanieb on 2024-02-16 00:55_

---

_Comment by @zanieb on 2024-02-16 00:55_

Thanks for the clear issue!

---

_Label `configuration` added by @zanieb on 2024-02-16 01:03_

---

_Label `enhancement` removed by @zanieb on 2024-02-16 01:04_

---

_Referenced in [astral-sh/uv#1393](../../astral-sh/uv/issues/1393.md) on 2024-02-16 01:59_

---

_Comment by @owenlamont on 2024-02-16 07:02_

I was just about to raise an issue for this exact use case. We also use codeartifact for a few in house packages but mostly pull public PyPi packages via AWS codeartifact.

Anyway +1 to this. Would love to use uv if this can be supported.

---

_Referenced in [astral-sh/uv#1503](../../astral-sh/uv/issues/1503.md) on 2024-02-16 15:52_

---

_Comment by @kkpattern on 2024-02-20 10:45_

We also set `index-url` in our venv by `pip config --site set install.index-url "http://path/to/internal/pypi/server"`

---

_Referenced in [astral-sh/uv#1859](../../astral-sh/uv/issues/1859.md) on 2024-02-22 09:01_

---

_Comment by @schlamar on 2024-02-22 09:22_

BTW, for Windows this is `%APPDATA%\pip\pip.ini`, see https://pip.pypa.io/en/stable/topics/configuration/#location




---

_Referenced in [astral-sh/uv#1907](../../astral-sh/uv/issues/1907.md) on 2024-02-23 16:12_

---

_Comment by @mcrumiller on 2024-02-25 15:02_

Can you perhaps add to the README instructions on how to set the index-url? My company uses a proxy repo and it's not obvoius how to read from it. Do we have to supply `--index-url` every time we call `python -m pip uv`? That seems a bit cumbersome.

---

_Comment by @zanieb on 2024-02-25 15:53_

Hi @mcrumiller you can use `UV_INDEX_URL` instead. We may want to add all of our environment variables to the README, but they are currently listed in the `--help` output e.g.

```
  -i, --index-url <INDEX_URL>
          The URL of the Python package index (by default: https://pypi.org/simple)
          
          [env: UV_INDEX_URL=]
```

---

_Referenced in [astral-sh/uv#2043](../../astral-sh/uv/issues/2043.md) on 2024-02-29 02:37_

---

_Comment by @PetterS on 2024-03-01 12:21_

The environment variables are nice, but reading the pip config file would be very good for enterprises. It would allow a seamless transition to uv without installing additional configuration on the machines.

---

_Referenced in [astral-sh/uv#2148](../../astral-sh/uv/issues/2148.md) on 2024-03-04 14:23_

---

_Comment by @edwardpeek-crown-public on 2024-03-04 21:30_

We'd like the system config to be considered as well (`/etc/xdg/pip/pip.conf` on Ubuntu). Our organisation configure a local mirror for all dev machines & users this way.

---

_Comment by @RichardDally on 2024-03-05 09:31_

> The environment variables are nice, but reading the pip config file would be very good for enterprises. It would allow a seamless transition to uv without installing additional configuration on the machines.

Exactly, this is what I was expecting from uv, a drop-in replacement with a major performance boost.

---

_Comment by @zanieb on 2024-03-05 15:10_

This is still under consideration, but the comment at https://github.com/astral-sh/uv/issues/1789#issuecomment-1978810917 explains our current stance well.

---

_Comment by @RichardDally on 2024-03-13 14:24_

> Hi @mcrumiller you can use `UV_INDEX_URL` instead. We may want to add all of our environment variables to the README, but they are currently listed in the `--help` output e.g.
> 
> 
> 
> ```
> 
>   -i, --index-url <INDEX_URL>
> 
>           The URL of the Python package index (by default: https://pypi.org/simple)
> 
>           
> 
>           [env: UV_INDEX_URL=]
> 
> ```

I've been trying environment variables but it raises an error related to Unexpected fragment on URL: contact.
I've seen issues that have been closed for this, I'm a little bit puzzled here.
Using version 0.1.12 on Windows on private Artifactory repository.

---

_Comment by @zanieb on 2024-03-22 19:27_

@RichardDally could you please create a new issue if this is not related to `pip.conf` support?

---

_Comment by @zanieb on 2024-03-22 19:32_

Okay, we are officially considering support for this feature. Here is my proposal.

### Require explicit selection of configuration files

`pip` supports discovery of configuration files at various locations

- Do not support automated discovery of `pip.conf`
- Do not support global / user / site
- Use of `pip.conf` requires opt-in e.g. `--pip-config <path>` or `UV_PIP_CONFIG=<path>`

### Only support index configuration

`pip` supports all of the command-line options via the configuration file

- Do not support flags like `no-compile`, etc.
    - Unknown flags are ignored without warning
    - Flags we support in the CLI but not in the config file result in a warning in verbose mode
- Support for:
    - `find-links`, `index-url`, `extra-index-url`
- Possible support for `native-tls` toggle

### Do not support configuration per command

`pip` supports separate configuration for each sub-command like `install` 

- All configuration must be either in a `[global]` declaration or in the top-level
- Declarations in the top-level will override those in a `[global]` section
    - We could alternatively error or avoid checking `[global]` at all if a top-level option exists
- Configuration per command will be silently ignored
- If configuration file is provided without `[global]` or top-level options we will warn it is empty

---

_Comment by @kkpattern on 2024-03-23 08:37_

> ### Require explicit selection of configuration files
> `pip` supports discovery of configuration files at various locations
> 
> * Do not support automated discovery of `pip.conf`
> * Do not support global / user / site
> * Use of `pip.conf` requires opt-in e.g. `--pip-config <path>` or `UV_PIP_CONFIG=<path>`

Maybe UV can auto discovery `pip.conf` in a virtual environment?

`UV_INDEX_URL` is actually enough for most use cases. I think many want uv to support `pip.conf` because they want uv as a drop-in replacement for pip. Without auto-discovery `pip.conf` from a virtual env, this cannot be achieved.

Also, in our project's CI job, there're multiple stage install different dependencies into the same `.venv`. For example, the bootstrap script may setup the `.venv` and install all the essential dependencies. Then in a later stage, the test script will install all the test related dependencies into the `.venv`.

By auto-picking up the `pip.conf` in the virtual environment, all the jobs in the following stages don't need to worry about which index it should use, which is very convenient for us. 

---

_Referenced in [astral-sh/uv#2640](../../astral-sh/uv/issues/2640.md) on 2024-03-24 20:36_

---

_Comment by @zanieb on 2024-03-29 16:50_

> Maybe UV can auto discovery pip.conf in a virtual environment?

What exactly do this mean? There's a `pip.conf` file _in_ the `.venv` folder?

---

_Comment by @kkpattern on 2024-03-29 17:14_

> What exactly do this mean? There's a `pip.conf` file _in_ the `.venv` folder?

Yes. If you activate a `.venv` then run `pip config --site set install.index-url "http://pypi.internal.com"`, there will be a `pip.conf` in the `.venv` containing following content:

```
[install]
index-url = http://pypi.internal.com
``` 

Later `pip` commands will automatically pick up this config file.

---

_Comment by @zanieb on 2024-03-29 17:16_

Why not just export an environment variable selecting the configuration file? e.g. `UV_PIP_CONFIG=./.venv/pip.conf`?

---

_Comment by @kkpattern on 2024-03-29 17:28_

It's convenient when we want to install some libs into a .venv across multiple sessions.

For example, a developer may set up a local `.venv` for development and use it for a relatively long time. Then, one day, they may need to update or install a new lib from an internal Pypi index. Since pip can automatically pick up the `pip.conf` file in the `.venv `folder, they don't need to remember what the index is or where the `pip.conf` is located.

This is not a deal breaker. We already use `uv` in our project and are very happy with it. But I think it's a nice-to-have feature.

---

_Comment by @zanieb on 2024-03-29 17:44_

Thanks for the additional details! I don't think we'd launch this feature with support for that, but I'm not fully opposed. I worry about _sometimes_ supporting inference, but having it work in the most-scoped context makes some sense.

---

_Comment by @notatallshaw on 2024-03-29 19:34_

Just as another viewpoint for this conversation, as someone who has helped set up a lot of different users Python environments a lot of users get confused by all the different places pip configuration can go, e.g. environmental variables, multiple user directories, multiple system directories, venv directories etc.

I've encountered many time someone not being able to find where a setting was set, and have a broken environment until someone assisted them.

---

_Comment by @ikamensh on 2024-04-03 14:58_

> Do not support automated discovery of pip.conf
Do not support global / user / site

Maybe one default location can be supported without causing any confusion? I would pick user config as the one config if I had to choose. I see how not picking any discovery is a simple solution, but perhaps there is a benefit if this choice is done well.

With environment variables the issue is e.g. I use fish shell, but sometimes things are run through bash. Then I have to maintain two configs and export variables there. Even for one shell its an extra step that sometimes could be avoided.

---

_Comment by @zanieb on 2024-04-03 16:44_

The intent is to introduce our own configuration in the future and we'll support automatic discovery of that. We're just not particularly comfortable reading other tool's configuration files without opt-in.

---

_Comment by @msharp9 on 2024-04-03 20:58_

Since I first created the issue there's been a lot of conversation, this is what I would suggest as I've thought about everyone's different viewpoints.

UV should create it's own configuration file and provide automatic discovery for it.
There should then be another tool that converts pip configs to uv configs, e.g pip.conf -> uv.conf.
This second tool could provide a simple extensible configuration to add default locations. Collect all pip.conf, and create one combined uv.conf to the auto discovery location.

If UV then added support for plugins, then all a user would have to do is pip install both, say, `pip install uv uv-pip-conf`. This would make the user experience feel much more like UV is actually a drop in replacement to pip and pip-tools since no flags or environment variables would have to be set.

This would be similar to how flake8 plugins work, simply pip install flake8 and flake8-import-order as an example, and now flake8 automatically checks your codes imports along with other syntax.

---

_Comment by @d-miketa on 2024-04-03 21:37_

@zanieb Slightly off topic, but would you be willing to share an expected timeline for the config file feature? It’s the one thing preventing me from immediately swapping all of my projects to uv. 😅 Many thanks, love the project.

---

_Comment by @zanieb on 2024-04-03 23:59_

Hey @d-miketa — we don't have a timeline yet. I think it will be relatively straight-forward to implement but I'm giving time for feedback on my proposal which only has four upvotes compared to the ~50 on the original post.

---

_Referenced in [astral-sh/uv#2929](../../astral-sh/uv/issues/2929.md) on 2024-04-09 10:54_

---

_Comment by @gaby on 2024-04-14 23:21_

As stated by @PetterS not having built-in support for `/etc/pip.conf` will be detrimental for Enterprise adoption.

In our case we drop a configuration in `/etc/pip.conf` using Ansible, we then symlink this file to `/etc/xdg/pip/pip.conf` for Ubuntu based systems.

After that any Python package installed using ansible.pip module automatically uses the configuration from `/etc/pip.conf`, we don't have to expose ENV's or write extra config files.

---

_Comment by @charliermarsh on 2024-04-14 23:37_

That requirement sounds more like you need a persistent configuration file rather than a pip.conf specifically - or am I misunderstanding?

---

_Comment by @gaby on 2024-04-14 23:43_

@charliermarsh In enterprise environments `/etc/pip.conf` is used to managed several things:
- Index-url
- Search-index
- HTTP/HTTPS proxies
- Custom CA's

User are just able to do "pip install" without having to configure anything since it's managed by the platform via `/etc/pip.conf`.

---

_Comment by @astronautas on 2024-04-18 11:30_

Any progress on this? It's critical for enterprise environments.

---

_Comment by @CostcoFanboy on 2024-04-20 19:01_

> The intent is to introduce our own configuration in the future and we'll support automatic discovery of that. We're just not particularly comfortable reading other tool's configuration files without opt-in

Then uv will stay indefinitely a non-viable product for enterprise solutions. 

`/etc/pip.conf` or even sometimes `pip.conf` files getting cp'd inside of the venv folder during the dependency installation phase is an industry-wide practice/trend, for better or for worse.

Regardless, huge fan of the product and thank you immensely for your time and hard work. 
Just sad to see this will only stay on personal projects.

---

_Comment by @charliermarsh on 2024-04-20 19:09_

I think what I don't understand yet is: if you're going through the effort to migrate from pip to uv, why is it any harder to ask that you use (for sake of argument) `uv.conf` rather than `pip.conf`? Why is that not possible? Like, if I told you that it was the exact same file, but it had to be named `uv.conf`, why would _that_ be a problem?

We may be convinced to change the behavior here (I'm genuinely looking for answers to that question), but FWIW I can personally name a bunch of enterprises that are already using uv, so I'm not worried about it only being viable for personal projects :)


---

_Comment by @gaby on 2024-04-20 19:11_

@charliermarsh If `uv.conf` becomes a thing, and location is predefined, ex `/etc/uv.conf`. It should work enterprise environments. 

---

_Comment by @charliermarsh on 2024-04-20 19:15_

That’s the thing I’m mostly trying to understand. Is it important the file is literally named pip.conf, that it follows the exact same schema, that it’s auto-discovered in the same way? If so, why? (I’m genuinely open to being convinced, it just adds work and a maintenance burden for us so I want to make sure we have the right motivation.)

Or is it that users just need a way to specify persistent configuration? What if we (eg) supported reading pip.conf but you had to set a UV environment variable for it?

---

_Comment by @CostcoFanboy on 2024-04-20 19:54_

> I think what I don't understand yet is: if you're going through the effort to migrate from pip to uv, why is it any harder to ask that you use (for sake of argument) uv.conf rather than pip.conf? Why is that not possible? Like, if I told you that it was the exact same file, but it had to be named uv.conf, why would that be a problem?

Apologies I don't think I expressed myself clearly. 

While the actual name of the file doesn't necessarily need to be the same, the feature set need to be mirrored and it would be highly desirable for the discovery mechanism to be mimicked. Mostly having a global predefined location (e.g. /etc/pip.conf) and a project-specific/localized one (e.g. inside of venv in its root) is very important.. 

From the previous posts it didn't feel like there was functionality-mirroring in mind, but from your clarification it does feel like I'm wrong.

> FWIW I can personally name a bunch of enterprises that are already using uv, so I'm not worried about it only being viable for personal projects :)

Would this mean there is another way that UV supports trusted-host, index-url AND extra-index-url (fallback)?

---

_Comment by @hutch3232 on 2024-04-21 15:03_

In my enterprise environment, sys admins configure `/etc/pip.conf` to work with our PyPI mirror. A sizeable portion of our users probably do not really even understand the mirror or bother with `pip.conf` since it "just works". Thinking short-term, it would be nice if `uv` picked up `pip.conf` so that it was a drop-in replacement for `pip` in enterprise environments. It wouldn't require convincing the sys admins to create/maintain an additional config file or to teach users how to do it themselves. Longer-term though, as `uv` adoption increases, it probably wouldn't be much of an ask for sys admins to create the file. I think supporting `pip.conf` would primarily help less comfortable users get up and running with `uv`. More advanced users would have little issue figuring it out, using a hypothetical `uv.conf` or environmental variables. With that in mind, less comfortable users probably are sticking with `pip` in the short-term anyway. More advanced users are probably the people very excited about `uv` and jumping into it.

---

_Comment by @astronautas on 2024-04-21 20:00_

@charliermarsh In our case (a small data point, but still) we don't care that much how the file is named, as long as uv can resolve deps from non pypi sources and it's possible to define those sources within *.conf.

---

_Comment by @PetterS on 2024-04-21 20:22_

There can be (and have been where I've worked) automated setup scripts that make sure there is a `pip.conf` with appropriate settings. If `uv` just works with that file the gradual adoption within the enterprise could be quicker.

It's not a dealbreaker of course since we are talking about developers here.

---

_Referenced in [astral-sh/uv#3321](../../astral-sh/uv/issues/3321.md) on 2024-05-02 15:30_

---

_Comment by @eswolinsky3241 on 2024-05-25 12:27_

> I think what I don't understand yet is: if you're going through the effort to migrate from pip to uv, why is it any harder to ask that you use (for sake of argument) `uv.conf` rather than `pip.conf`? Why is that not possible? Like, if I told you that it was the exact same file, but it had to be named `uv.conf`, why would _that_ be a problem?
> 
> We may be convinced to change the behavior here (I'm genuinely looking for answers to that question), but FWIW I can personally name a bunch of enterprises that are already using uv, so I'm not worried about it only being viable for personal projects :)

@charliermarsh Just to comment since I came across this post while playing with uv: We also use AWS Codeartifact, which uses short-lived credentials that need to be refreshed every twelve hours. The AWS CLI offers a single [helper command](https://docs.aws.amazon.com/cli/latest/reference/codeartifact/login.html) that can obtain the credentials and rewrite the index url in `pip.conf` (or the equivalent config file for npm, maven, etc.). Obviously AWS currently offers no such support for uv, so it's on the user to update `uv.conf` every twelve hours. Not saying this is a huge deal, just something that could be tedious enough to convince an enterprise user to stick with pip. Loving uv so far though!

---

_Comment by @ilCatania on 2024-05-28 22:32_

I also work in a corporate environment where someone else manages `pip.conf` on my behalf and I do not have full control over my users' installations.

My short term plan to be able to replace `pip` with `uv pip` has been a handmade utility that reads from `pip.conf` and sets the corresponding `uv` environment variables.

I would be very happy, once `uv` supports its own configuration file, if there was a command to create / update said configuration file with values from `pip.conf`. Something like
```shell
uv import-pip-conf --overwrite --ignore-unsupported --pip-conf-file=~/.pip.conf
````
(I am making it up but you get the idea hopefully)

---

_Referenced in [pypa/pip#12827](../../pypa/pip/issues/12827.md) on 2024-07-05 18:58_

---

_Referenced in [astral-sh/uv#6537](../../astral-sh/uv/issues/6537.md) on 2024-08-23 18:41_

---

_Referenced in [astral-sh/uv#7676](../../astral-sh/uv/issues/7676.md) on 2024-09-24 22:03_

---

_Comment by @quartox on 2024-10-18 17:06_

I also have the short-lived credentials problem in an corporate environment. My `pip.conf` file is updated every 8 hours with new credentials. The tools to update these credentials are owned by other teams at my company. It would be more challenging for me to convince them to adopt `uv` before I am able to test it out and demonstrate some improvements with existing workflows.

I was wondering if it would it be possible for `uv.conf` to reference specific configurations in `pip.conf`? For example what if the `uv.conf` file contains the line

`index-url = ${/path/to/pip.conf:index-url}`

Don't care much about specific syntax (which I took from [here](https://docs.python.org/3/library/configparser.html#configparser.ExtendedInterpolation)) but the intent is to reference values in the `pip.conf` file explicitly. This way we could keep updating `pip.conf` every 8 hours until I can get broad consensus to move to `uv.conf` workflows.

---

_Comment by @T-256 on 2024-10-22 21:31_

FYI, https://github.com/astral-sh/uv/pull/7851 landed, which now allows you to set global configuration for `uv.toml`.

---

_Referenced in [astral-sh/uv#8486](../../astral-sh/uv/issues/8486.md) on 2024-10-23 10:50_

---

_Comment by @darksim33 on 2024-10-24 16:55_

> FYI, #7851 landed, which now allows you to set global configuration for `uv.toml`.

Just to make this clear since I'm new to the tool. Is it possible to link the pip.conf using the uv.toml? 
I tried a few things but they don't seem to work.

---

_Comment by @gaby on 2024-10-25 13:20_

> @charliermarsh In enterprise environments `/etc/pip.conf` is used to managed several things:
> 
> * Index-url
> * Search-index
> * HTTP/HTTPS proxies
> * Custom CA's
> 
> User are just able to do "pip install" without having to configure anything since it's managed by the platform via `/etc/pip.conf`.

Quoting myself since I just tried using `/etc/uv/uv.toml` and some of these options are not even supported.

`uv` keeps pushing some of these configs to ENV variables. These are not practical for enteprise environments.

Currently there's no way to set a `cert` file for a proxy/index, and there's no way to set `http/https` proxies via `pyproject.toml` / `uv.toml`

Enteprise environments typically set `SquidProxy` or `Pypi Mirror`. These services are exposed to users with internal TLS certs signed by the Enterprise CA.

tldr: If the ENV's listed here https://docs.astral.sh/uv/configuration/environment/ were supported via `uv.toml` and `pyproject.toml` it would solve a lot of the use-cases listed on this thread.

---

_Comment by @schlamar on 2024-10-25 17:11_

@gaby for this use case you can use native-tls setting https://docs.astral.sh/uv/reference/settings/#native-tls

In this case SSL works automatically if your internal cert is in the system's certificate store (which is usually the case in enterprise setups).

---

_Comment by @gaby on 2024-10-25 22:46_

@schlamar Yes, that works. But sometimes the Proxy CA is not installed in the system.

Regardless, you can't set http/https proxy via `toml` only via ENV.

---

_Referenced in [zanieb/uv#6](../../zanieb/uv/issues/6.md) on 2024-11-26 17:33_

---

_Referenced in [astral-sh/uv#9452](../../astral-sh/uv/issues/9452.md) on 2024-11-26 21:15_

---

_Renamed from "Use users pip.conf" to "Use settings from the pip configuration file (`pip.conf`)" by @zanieb on 2024-11-26 23:59_

---

_Comment by @brant-qai on 2025-03-28 08:51_

There are a number of important issues that arise by *not* honoring ``pip.conf``.

## Unexpected
The ``uv pip``command is understood to only support a subset of pip's options, but it still "feels" like pip. Right up until you realize it isn't honoring your pip configurations. This feels very counterintuitive. Of course RTFM, but it's easy to not read the whole thing; it's huge. The behavior is "surprising".

## Custom Tools/History
There exist a plethora of tools out there that write custom ``pip.conf`` files for you. AWS Code Artifact is one but there are others, including lots of custom internal tooling written by various companies (The last two companies I worked for had their own). Since pip has been around for a VERY long time, there is a lot of tooling written around it. The ``pip.conf`` file may be managed by a sysadmin (I've worked places where it was a symlink to a copy on the network drive where it was updated every hour OR occasionally at random for other reasons). 

One could argue that everyone should just rewrite their tools, but that is often a hard sell.

The years of history behind the ``pip.conf`` file make it a very stable standard. It is also a very simple standard. It has very few options and is easy to parse. The maintenance burden on the ``uv`` project will be vanishingly minimal; the up-front development cost would be the primary cost, and the way it functions would map very well onto the existing pip shim (it's an ini file and the global section maps directly to command line arguments). 

## Change/Risk Adverse Teams

The reality is that change is hard, and it's *definitely* hard to do change "all at once". Even in a shop committed to switching entirely to ``uv``, that is going to be a thing that happens gradually, one repo at a time or one project/package at a time. The easier that transition is, the easier it is for someone to advocate for the adoption of ``uv``.

It makes a simple proof-of-concept difficult because you can't just convert one project to ``uv`` and have it work without extra steps.  *waves hand; ignore this weird copy/paste export of environment variables...*

## Interstitial State & Maintenance Burden

Many companies aren't monoliths. They may have an overarching sysadmin team but multiple departments or other organizational units. There will be many places with hybrid environments - that is, one department may switch to ``uv`` while other change/risk-adverse departments may not, or may delay in doing so. By forcing those who manage those config files to manage another separate one you place a maintenance burden that was not asked for and shouldn't be necessary.

It would also be unsurprising for some companies to always be in mixed state with some teams never adopting newer (better imo) tools for the life of the company.

## Environment Variable Limitations

Environment variables are a bad solution. What should be simple/streamlined becomes challenging. Take this example script setup (using Code Artifact again as an example) as a 'happy path':

```bash
#!/bin/bash

aws codeartifact login --tool pip --repository python-packages --domain <DOMAIN> --domain-owner <ACCOUNT_ID> --region <REGION>
```

Which makes the process painless. It updates my ``pip.conf`` file for me. I set it up once and voila, I don't have to think about it anymore; I'm off to the races. Can I do something like this easily for ``uv`` ? No.

The documentation (to again use Code Artifact as an example) suggests this:

```bash
export AWS_CODEARTIFACT_TOKEN="$(
    aws codeartifact get-authorization-token \
    --domain <DOMAIN> \
    --domain-owner <ACCOUNT_ID> \
    --query authorizationToken \
    --output text
)"
export UV_INDEX_PRIVATE_REGISTRY_USERNAME=aws
export UV_INDEX_PRIVATE_REGISTRY_PASSWORD="$AWS_CODEARTIFACT_TOKEN"
```

I *can't* make that into a shell script because scripts run in a different execution context than the parent shell. The child shell that executes the script can't set environment variables for the parent shell. 



---

_Comment by @juhaszp95 on 2025-04-25 12:26_

I'm trying to migrate from `pip` to `uv`. I have a `pip.conf` file with a `[global] index-url` and `extra-index-url` specified. These are persistent configurations so that `pip` always installs from these indexes.

I'd like to achieve the same behaviour in `uv`, so that I can use these indexes automatically when I call `uv add` in any project. I've tried to add them to a user-level `uv.toml` file, but without success - I'm probably using the wrong format, but I couldn't figure out what the right format is (I couldn't find in the docs).

Any help would be appreciated...

---

_Comment by @charliermarsh on 2025-04-25 12:30_

You should be able to put them in the top-level of your user- or system-level `uv.toml`, like:

```toml
index-url = "..."
extra-index-url = ["..."]
```

---

_Comment by @juhaszp95 on 2025-04-25 12:38_

Ah perfect, thanks!

---

_Referenced in [astral-sh/uv#13106](../../astral-sh/uv/issues/13106.md) on 2025-04-25 12:38_

---

_Comment by @gioxc88 on 2025-04-29 14:39_

> You should be able to put them in the top-level of your user- or system-level `uv.toml`, like:
> 
> ```toml
> index-url = "..."
> extra-index-url = ["..."]
> ```

Can you please give an example?

If I have a list do I separate entries inside square brackets with commas ?

Do I need to use double quotes for each entry ?

Would this work for trusted-hosts as well?

Thanks

---

_Comment by @juhaszp95 on 2025-04-29 14:57_

Exactly! You can put muliptle entries as a list of strings, i.e. enclosed in quotation marks, spearated by commas, all within a pair of square brackets.

I have only tried `index-url` and `extra-index-url`. 

---

_Comment by @gioxc88 on 2025-04-29 15:17_

> Exactly! You can put muliptle entries as a list of strings, i.e. enclosed in quotation marks, spearated by commas, all within a pair of square brackets.
> 
> I have only tried `index-url` and `extra-index-url`. 

I am trying, but that doesn't work .

I have a uv.toml file placed in C:\Users\myuser\.config\uv

And I am doing uv add mypackage but it can't find anything 

I wonder if this what I am doing wrong  

---

_Comment by @alexjolig on 2025-05-02 11:39_

> > Exactly! You can put muliptle entries as a list of strings, i.e. enclosed in quotation marks, spearated by commas, all within a pair of square brackets.
> > I have only tried `index-url` and `extra-index-url`.
> 
> I am trying, but that doesn't work .
> 
> I have a uv.toml file placed in C:\Users\myuser.config\uv
> 
> And I am doing uv add mypackage but it can't find anything
> 
> I wonder if this what I am doing wrong

You can use `[[tool.uv.index]]` and add as many urls as you need

```
[[tool.uv.index]]
url = "FIRST-URL-MARKED AS DEFAULT"
default = true

[[tool.uv.index]]
url = "SECOND-URL"
```

---

_Referenced in [astral-sh/uv#1384](../../astral-sh/uv/issues/1384.md) on 2025-06-23 17:54_

---

_Referenced in [apache/airflow#52287](../../apache/airflow/pulls/52287.md) on 2025-06-26 12:18_

---

_Referenced in [astral-sh/uv#15942](../../astral-sh/uv/issues/15942.md) on 2025-09-19 07:02_

---

_Referenced in [astral-sh/uv#16060](../../astral-sh/uv/issues/16060.md) on 2025-09-29 10:39_

---

_Comment by @lemming622 on 2025-10-22 16:08_

> You should be able to put them in the top-level of your user- or system-level `uv.toml`, like:
> 
> index-url = "..."
> extra-index-url = ["..."]

This looks great.  How can this functionality be added to a project toml so that it's done automatically?

---

_Referenced in [piwheels/piwheels#221](../../piwheels/piwheels/issues/221.md) on 2025-11-19 19:28_

---

_Comment by @suewonjp on 2025-12-20 03:50_

Most enterprises use proxy repositories like Artifactory, etc. and they configure custom URLs in pip.conf or pip.ini (on Windows) and currently, they will hesitate to use uv since they can't seamlessly migrate from pip unless you support pip.conf/pip.ini

I'm not sure how you prioritize this issue, but clearly, this looks like a big blocker against expanding uv community more and more, IMO.

---
