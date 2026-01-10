---
number: 2077
title: "Support `--user` flag from pip"
type: issue
state: closed
author: potiuk
labels:
  - compatibility
  - great writeup
assignees: []
created_at: 2024-02-29T09:14:28Z
updated_at: 2025-02-18T23:48:38Z
url: https://github.com/astral-sh/uv/issues/2077
synced_at: 2026-01-10T01:23:11Z
---

# Support `--user` flag from pip

---

_Issue opened by @potiuk on 2024-02-29 09:14_

Currently (even in latest uv 0.1.12 that supports --python flag) it's not possible to use `uv` in case `--user` flag of `pip` (or PIP_USER="true" env variable is set). 

With `--user` flag, packages are installed to a ~/.local folder, which means that the user might not have access to the `python` system installation as a whole, but can locally install and uninstall packages, without having venv.

This is extremely useful for cases like `CI` and building container images, where having a venv is an extra overhead and it is unnecessary burden.

There is an interesting (and used in Apache Airflow) case for the `--user` flag (this currently prevent us from using `uv` by us and our users  also in PROD images in addition to our CI image). That is currently a blocker for https://github.com/apache/airflow/issues/37785

Why `--user` flag is useful and why we use it in Airflow? 

Using `--user` flag is pretty useful when you want to have an optimized image - because such `.local` folders can be copied between different stage of the image (based on the same base image and prod libraries installed) and you can copy the whole `.local` folder between the stages after packages are installed - which means that the final stage of the image does not have to have `build-essentials`/ compilers installed. 

You could do the same with `venv` if you keep it in the same folder, but that looses an essential capability of creating venv dynamically containing all the packages you have in your `.local` folder.  When you have your `--user` installed packages, and you create a new venv  with `--system-site-packages`,  the packages installed in `.local` folder are also installed in the new venv. This does not work when you create a new `venv` from another `venv` because `--system-site-packages` are only the ones installed in the system.

While I can think of some creative ways (maybe I will find some) - having an equivalent of `--user` installation by `uv pip install` would be a great simplification for our case to support the users who want to use `uv` (and seems that there is already a need for that looking at the https://github.com/apache/airflow/issues/37785





---

_Referenced in [apache/airflow#37785](../../apache/airflow/issues/37785.md) on 2024-02-29 09:15_

---

_Referenced in [apache/airflow#37796](../../apache/airflow/pulls/37796.md) on 2024-02-29 14:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-29 14:53_

---

_Label `compatibility` added by @charliermarsh on 2024-02-29 14:53_

---

_Label `great writeup` added by @charliermarsh on 2024-02-29 14:54_

---

_Comment by @charliermarsh on 2024-02-29 14:54_

Thanks! I really appreciate the clear write-up and motivation here. (I'm hoping to get to this today, it's been requested a few times.)

---

_Comment by @potiuk on 2024-02-29 17:04_

Just to take off the pressure a bit. I figured out how to get rid of the `--user` flag (still have a problem to fix) https://github.com/apache/airflow/pull/37796 - I attempted it quite some time ago and failed but this time I got a brilliant idea - I simply deleted the `~/.local` folder and created a new `~/.local` venv,  added `VIRTUAL_ENV=~/.local` env var and added ~/.local/bin to PATH and  .... voila ... It works 100% compatible with `--user` flag it seems - all the cases I had (including `pip -m venv --system-site-packages`) work like a charm.

So ... It seems I do not need it that much any more .... I will make Airflow PROD image also `uv` friendly now :) (128s instead of 280s is the gain I have :). Not bad - another 55% improvement.

There is a small difference though with venv when using `python -m venv` and `uv venv`. With `uv` you do not have `--system-site-packages` option yet. That might be the more useful of the two.

After that experience I have a new thought. The `--user` flag has a few bad side effects (that's why I am glad to finally get rid of it). One of the problems is that you cannot run `--user` and `--editable` any more with pip. So maybe - just maybe (?), rather than implementing `--user` flag - documenting how to get an equivalent by creating `~/.local` venv is a better option ?

---

_Comment by @charliermarsh on 2024-02-29 21:56_

@potiuk - At the very least I can add `--system-site-packages`, that's really easy. I'm somewhat undecided on `--user`... Gonna sleep on it.

---

_Comment by @potiuk on 2024-02-29 22:08_

> @potiuk - At the very least I can add `--system-site-packages`, that's really easy. I'm somewhat undecided on `--user`... Gonna sleep on it.

Yep. That would be a good start :). I **think** `pip` maintainers would gladly drop that one (`--user`) . So my proposal for the `uv` team is think very deeply on whatever you add. You are now way faster on adding things and responding to new requests - comparing to `pip` -  but mainly because you do not have the whole baggage that `pip` accumulated over the years and where adding every single small change will make some loud part of your users unhappy. I think it should be a very conscious decision to add somethig that you will have to maintain in the future.


---

_Comment by @charliermarsh on 2024-02-29 22:14_

Yeah I strongly agree with this. Very good callout. It's nice to add things that unlock user workflows, but there are some areas where we actively want to change user behavior, and we need to hold firm on some of those lines.

---

_Comment by @charliermarsh on 2024-02-29 22:14_

Can I ask why you use `--system-site-packages` here?

---

_Comment by @potiuk on 2024-02-29 22:34_

Yes. In Airflow we have something called `PythonVirtualenvOperator`. Airflow has operators (4000+ of them 😱 ) that can do tasks related to some "stuff" to do - some of them are specific ("GoogleCloudJobOperator`, `ApacheSparkJobOperator`, `CreateEKSClusterOperator`) but we have a number of a  few generic ones `BashOperator`, `PythonOperator`, `ExternalPythonOperator` - runs python code on prepared virtualenv. All of those are unaffected, but PythonVirtualenvOperator is special - because what it does is:

It creates a new virtualenv based on requirements, python version. One of the options (only valid if your python version matches the `system` version) is `system-site-packages` - when True, the new venv will have `airflow` and all airflow packages pre-installed. 

This is super important, because we serialize arguments that we pass to the operator, and de-serialze return value from it, as interface between airflow and the "new venv" method executed. And in order to serialize some objects, the target venv should have the "airflow + often other packages" installed. Moreover the python code to be executed (basically a methpd) can do some local imports - expecting airflow or other packages to be installed. 

The `--system-site-packages` in this case is best, because you do not have to explicitly specify which airflow version or which other packages you have to have installed. You can specify `extra` dependencies to add - but having the `base same as the Airflow execution environment` is often necessary.
 
Here howto: https://airflow.apache.org/docs/apache-airflow/stable/howto/operator/python.html#pythonvirtualenvoperator
Here Python API: https://airflow.apache.org/docs/apache-airflow/stable/_api/airflow/operators/python/index.html#airflow.operators.python.PythonVirtualenvOperator



---

_Comment by @potiuk on 2024-02-29 22:36_

Under the hood, the operator does: 

```
def _generate_virtualenv_cmd(tmp_dir: str, python_bin: str, system_site_packages: bool) -> list[str]:
    cmd = [sys.executable, "-m", "virtualenv", tmp_dir]
    if system_site_packages:
        cmd.append("--system-site-packages")
    if python_bin is not None:
        cmd.append(f"--python={python_bin}")
    return cmd
```

---

_Comment by @potiuk on 2024-02-29 22:48_

The way how it works now (just built and tested a new image):

With system-site packages:

```bash
airflow@a249d829a411:/opt/airflow$ python -m virtualenv --system-site-packages ~/.aaaa
created virtual environment CPython3.10.13.final.0-64 in 422ms
  creator CPython3Posix(dest=/home/airflow/.aaaa, clear=False, no_vcs_ignore=False, global=True)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/airflow/.local/share/virtualenv)
    added seed packages: pip==24.0, setuptools==69.1.0, wheel==0.42.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
airflow@a249d829a411:/opt/airflow$ ~/.aaaa/bin/python -m pip freeze
adal==1.2.7
adlfs==2024.2.0
aiobotocore==2.12.0
aiofiles==23.2.1
aiohttp==3.9.3
aioitertools==0.11.0
aiosignal==1.3.1
alembic==1.13.1
amqp==5.2.0
anyio==4.3.0
apache-airflow @ file:///docker-context-files/apache_airflow-2.9.0.dev0-py3-none-any.whl
apache-airflow-providers-amazon @ file:///docker-context-files/apache_airflow_providers_amazon-8.18.0.dev0-py3-none-any.whl
apache-airflow-providers-celery @ file:///docker-context-files/apache_airflow_providers_celery-3.6.0.dev0-py3-none-any.whl
apache-airflow-providers-cncf-kubernetes @ file:///docker-context-files/apache_airflow_providers_cncf_kubernetes-8.0.0.dev0-py3-none-any.whl
apache-airflow-providers-common-io @ file:///docker-context-files/apache_airflow_providers_common_io-1.3.0.dev0-py3-none-any.whl
.....  AND 300+ other packages.
```

Without:

```bash
airflow@a249d829a411:/opt/airflow$ python -m virtualenv  ~/.bbbb
created virtual environment CPython3.10.13.final.0-64 in 113ms
  creator CPython3Posix(dest=/home/airflow/.bbbb, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/airflow/.local/share/virtualenv)
    added seed packages: pip==24.0, setuptools==69.1.0, wheel==0.42.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
airflow@a249d829a411:/opt/airflow$ ~/.bbbb/bin/python -m pip freeze
airflow@a249d829a411:/opt/airflow$.
```




---

_Referenced in [astral-sh/uv#2352](../../astral-sh/uv/pulls/2352.md) on 2024-03-11 08:06_

---

_Comment by @charliermarsh on 2024-03-12 16:13_

> The `--user` flag has a few bad side effects (that's why I am glad to finally get rid of it). One of the problems is that you cannot run `--user` and `--editable` any more with pip. So maybe - just maybe (?), rather than implementing `--user` flag - documenting how to get an equivalent by creating `~/.local` venv is a better option ?

@potiuk -- I'm trying to make a decision on whether to support `--user`. Do you have any references you could share for these side effects in `pip`? 🙏 

---

_Comment by @potiuk on 2024-03-12 20:14_

> @potiuk -- I'm trying to make a decision on whether to support --user. Do you have any references you could share for these side effects in pip? 🙏 

The starting point I have is this:

![image](https://github.com/astral-sh/uv/assets/595491/d78f233f-e4db-44cd-8cd9-a25eecc1a8a7)

And I think there were lots of related discussions: 

Here is one: https://github.com/pypa/pip/issues/6375
And here is the PR that added the message above: https://github.com/pypa/pip/pull/6370

Looong discussions. But basically the gist of it is that `--editable` and `--user` do not play well together in the light of what has been agreed for PEP 517.

Personally I consider `--user` feature of `pip` as **should be depreacated** and besides the historical logic and the behaviour of making the `.local` venv automatically available for any venv, it has very little use, if you consider that you could create `.local` as a venv. I think it was introduced in https://peps.python.org/pep-0370/  and there were some discussions on deprecating it, 

But I am not fully aware about some history behind `.local` and even less about future of it to make some "good" advice on it - treat it with a pinch of salt.

We luckily got rid the `--user` flag (and PIP_USER variable) even if `pip` version of our container images and Airflow 2.9.0 one (in a month or so) will not have it any more.






---

_Comment by @imfing on 2024-03-12 22:18_

@potiuk Thank you for this issue, and thanks for the inputs you provided

Some of my thoughts regarding the comment above:

> The starting point I have is this:

using the `--system-site-packages` while creating the virtual environment should make this error message gone. 
If we take a step back, `--user` wouldn't be quite useful in this case since there's already an virtualenv.

> And I think there were lots of related discussions. Here is one: https://github.com/pypa/pip/issues/6375

I skimmed through it, and it looks like the issue was mainly about `--editable` install breaking when there's `pyproject.toml` file. 
It doesn't seem like `--user` flag had anything to do with it, since, according to the original issue: `both pip install --user -e . and pip install -e . fail`. 

From what I understand, PEP 517 itself discusses allowing projects to specify different build systems through `pyproject.toml`. The build process should be independent from where the packages are installed. Not sure how that would impact user site installation.

> Personally I consider --user feature of pip as should be depreacated ... there were some discussions on deprecating it,

`--user` was introduced over a decade ago in PEP 370. 
I also saw [this thread about deprecating it](https://groups.google.com/g/dev-python/c/jeb-tiiiRcE/m/JTe6EHGHDQAJ). some replies were not in favor of deprecation, for example:

```
Given that we're working towards making user site-packages the default
install location in pip, removing that feature at the interpreter
level would be rather counterproductive :)

Virtual environments are a useful tool if you're a professional
developer, but for a lot of folks just doing ad hoc personal
scripting, they're more complexity than is needed, and the simple "my
packages" vs "the system's package" split is a better option. (It's
also rather useful for bootstrapping tools like "pipsi" - "pip install
--user pipsi", then "pipsi install" the other commands you want access
to).

Cheers,
Nick.
```

If we do a quick search on GitHub for the `--user` flag: https://github.com/search?q=%22pip+install+--user%22&type=code&p=1 we'll see it's still widely used

Though I haven't used `--user` much lately, thanks to `venv` and Docker containers, it's still a popular choice for many Python users for installing packages without needing admin privileges in many environments other than CI

---

Putting `--user` flag in `uv` would bring some of the complexity accumulated in `pip` over the years.

PR #2352 was my preliminary attempt to fit it into the `uv pip` implementation without introducing too much complexity as the [script](https://github.com/astral-sh/uv/blob/main/crates/uv-interpreter/src/get_interpreter_info.py) does the heavy lifting here.

I haven't looked into every combination of how `--user` flag would play with other `pip` flags, but it would take some effort to make it compatible with the behavior of `pip` for sure.

Ultimately, it's up to the uv team to decide whether to support it. 

---

_Comment by @danielhollas on 2024-03-13 03:13_

> Virtual environments are a useful tool if you're a professional
developer, but for a lot of folks just doing ad hoc personal
scripting, they're more complexity than is needed, and the simple "my
packages" vs "the system's package" split is a better option.

This is an excellent quote, thanks @imfing for noting this. As somebody coming from the scientific Python world, I was meaning to write something along these lines in this thread as well.

Another useful framing of this is the distinction between a python project versus a single-file script. For the latter, having a separate venv for each individual script would not only be annoying but also error prone, as you'd need to be constantly activating / deactivating environments, when often all you need is stdlib and numpy/scipy. 

We also have a rather odball use case for this -- we have a very unholy setup where we provide a system python installation in a container image with a lot of pre-installed packages, but we allow users to install additional packages to `~/.local`, which is backed by a docker volume so that it is persisted when the container exits. We can't have separate venvs since we have a lot of packages installed in the system environment (e.g. the whole jupyter stack) and they can't be duplicated.

Small side note: If this feature is accepted, I think providing `--user` ahould also automatically enable `--strict`. That's what pip does as well, makes a lot of sense in terms of making sure the user knows if they broke themselves.

(all that said, it's brilliant that uv defaults to requiring a venv, that's already a huge change with respect to pip, and forced me personally to start using them, where previously I've used conda + pip, not a great combo. I never quite got myself to understand the various subtleties - venv x virtualenv etc.)

---

_Comment by @zanieb on 2024-03-13 03:25_

> the simple "my packages" vs "the system's package" split is a better option

Note this perspective is something that we could solve with new workflows rather than matching the existing `--user` interface.

It's worth keeping in mind throughout this discussion that we _will_ be building new workflows on top of the fundamental tooling we've built for pip-compatibility. If we can avoid implementing and maintaining something with complex compatibility concerns we can focus on innovation and comprehensive solutions.


---

_Comment by @potiuk on 2024-03-13 10:07_

> It's worth keeping in mind throughout this discussion that we _will_ be building new workflows on top of the fundamental tooling we've built for pip-compatibility. If we can avoid implementing and maintaining something with complex compatibility concerns we can focus on innovation and comprehensive solutions.

This is also my concern - I think (and Airflow use-case proves that) implementing something like that and having users rely on it should be conscious decision, because it will stay with `uv` once the versioning and compatibility rules are set (which is inevitable) and the more compatibility concerns and use cases you have the more it slows you down the moment you hit 1.0.0 version (assuming 1.0.0 and some kind of semver-ish approach will be adopted). 

And `--user` is a bit a can of worms when you open it, to be honest - especially that uv also allows to create venvs and have some default reliance on `venv` being effectively required. Which is eventually a good thing even if few years ago I went into a long (and in hindsight unnecessary and far too heated) debate with `pip` maintainers over their push to `venv` being primary and the only `valid` way of installing python packages (especially in the context of docker containers). It even resulted with this medium article: https://potiuk.com/to-virtualenv-or-not-to-virtualenv-for-docker-this-is-the-question-6f980d753b46

I re-read that article again (it's really funny to read such an article few years later and confront your current views with the views of few-years-younger yourself).  Suprisingly (or maybe now), I tend to agree with that `old-myself` in  many of the things I wrote there, but with `uv` opening not only new chapter, but a new not-yet-written book, maybe there is a good solution that I can propose here?

I think PEP-370 proposal was good, and the reason described above (some people do not care about venv and they want to do things quickly) is very valid (recognising that non-power users want to just get-on with their install is also an argument in my article actually). 

But the discovery I made in https://github.com/apache/airflow/issues/37785 that I can simply create a venv in `~/.local` made me think that maybe we can connect `venv` creation and `non-power users` approach and both eat cake and have it?

What happens currently when you start `uv pip install` without a venv ? You get an error telling you tha tno `virtualenv` is detected and possibly that you can use `--python` to point to a python installation.

Is it good for non power users ? Not at all. It's confusing for first time users who have no knowledge about `venv` and they have no idea where their python is. Do we want to teach them that? I am not sure. We do not want to teach people all the details about venv etc. if all they want is to instal some package and use it, and even less if they are running `uv pip install` in their Dockerfile. What we want instead - we want them to USE venv. Even if they have no idea they are doing it. 

But wait - we already have a way to create venvs (fast) in `uv` that we fully control here.... Why don't we ..... create venv for such user? Why don't we ..... create it in `~/.local` (or equivalent place on other system)  automatically if it is not there (and we can even check if `~/.local/bin` is in the path and suggest the user to add it to make use of installed entrypoints.  That's also nicely compatible with PEP-370 (which is more about using the `~/.local` installation place rather thatn creating it.

That would be my proposal in short:
 
* forget the `--user` flag
* fallback to creating / using `~/.local` venv when no virtualenv and no `--python` flag is used




---

_Comment by @matthew-brett on 2024-03-14 23:27_

Just to say - I too would be very interested in a mechanism to support beginners who do not yet know about virtualenvs.   I have taught a lot of students to use Python for data analysis, including data science.  In practice these beginners (and there are lot of them) will not need to (consciously) make virtualenvs for a while - because the standard Python data stack is pretty stable, and doesn't generate many dependency conflicts.   I'd estimate these beginners can make six months or more of progress before they need to start thinking about making and switching virtualenvs.   I think it will be a significant barrier if they have to learn virtualenvs and their structure before they install their first Python package.   So I would love a solution like @potiuk's - where the user can ask for, or even just be given, a default virtualenv, into which they install their packages.   Of course the `--user` install provides something like this, but I agree with @potiuk - a default virtualenv is preferable - and in fact I already found myself making that suggestion over at [this homebrew discussion](https://github.com/orgs/Homebrew/discussions/3404#discussioncomment-8728485).  I really care about this, so I am very happy to help with anything I can to test or even build (when term finishes in a couple of weeks).

---

_Comment by @matthew-brett on 2024-03-16 11:42_

In case it's helpful - the use-case I have in mind is - I believe - very common - and that is the beginner starting a class or an online tutorial.  For example, consider this tutorial: 

https://realpython.com/pygame-a-primer/

It has the instructions, right at the top, of:

```
pip install pygame
```

Or - for my students, I suggest:

```
pip install jupyterlab numpy scipy matplotlib
```

In both cases, this gets the beginner going with early Python work.    But - with various installation methods - including `uv pip` as stands, outside Conda, this gives e.g.:

```
$ uv pip install pygame
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

Of course this isn't going to worry an experienced user, but it will be confusing to a beginner, who does not know what a virtualenv is - and - for a beginner, who doesn't understand Python installation structure, the virtualenv is a relatively advanced subject.

So - it would be very good to have some default set up, such that the beginner would not face this learning curve immediately, but face it later, when their needs and experience have expanded.

I'm posting here because I have, up until now - suggested my students do e.g. `pip install --user pygame` as a workaround, but I can see the arguments against doing that.

---

_Comment by @stefanv on 2024-03-25 11:04_

Maybe the topic, of whether or not to add a `--user` flag, obfuscates the main concern here:

New users, as @matthew-brett mentions, are often given "pip install x" instructions. This should ideally just work, and definitely not raise obscure errors that confuse new users who know nothing about virtual envs.

Ideally, I'd imagine something like: if in venv, install there, otherwise install into a local default venv. The local default venv has access to system packages, but shadows them, so that the user can upgrade packages.

This would make for a simple first user experience, compatible with all tutorials out there, without impacting sophisticated users who can set up their own venvs. 

---

_Comment by @potiuk on 2024-03-25 23:41_

> This should ideally just work,

Yep. My proposal is that it will just install things in `.local` (or equivalent on other systems) venv created and used automatically by `uv` in this case. Bonus point to print warning if `.local/bin` is not on PATH

---

_Referenced in [astral-sh/uv#2665](../../astral-sh/uv/pulls/2665.md) on 2024-03-28 09:39_

---

_Referenced in [astral-sh/uv#3076](../../astral-sh/uv/issues/3076.md) on 2024-04-16 22:35_

---

_Referenced in [astral-sh/uv#3417](../../astral-sh/uv/issues/3417.md) on 2024-05-07 14:52_

---

_Comment by @Malix-Labs on 2024-05-07 15:00_

I would definitely love that

When you're already working in a containerized development environment (nix shell / devcontainer …), it makes perfect sense (and even should be the default).
Also, `--system` doesn't work in immutable OSes (and important tools such as Codespaces)

---

_Comment by @matthew-brett on 2024-05-07 15:04_

> > 
> 
> Yep. My proposal is that it will just install things in `.local` (or equivalent on other systems) venv created and used automatically by `uv` in this case. Bonus point to print warning if `.local/bin` is not on PATH

I wonder though, whether anyone would worry about previous `--user` installs either being overwritten, or appearing unexpectedly in the `uv` environment?    And where would the packages go?   In `.local/site-packages` or somewhere else?

---

_Referenced in [astral-sh/uv#1584](../../astral-sh/uv/issues/1584.md) on 2024-05-17 13:44_

---

_Referenced in [astral-sh/uv#3635](../../astral-sh/uv/issues/3635.md) on 2024-05-17 13:45_

---

_Referenced in [astral-sh/uv#2333](../../astral-sh/uv/issues/2333.md) on 2024-06-05 12:33_

---

_Comment by @Malix-Labs on 2024-06-05 13:24_

As of today, it's still not possible to use `uv` in immutable (where `--system` couldn't work) containers (such as Codespaces) without an useless (in that case) additional `venv`

This issue would be very appreciated for Codespaces users

---

_Comment by @imfing on 2024-06-05 13:48_

> As of today, it's still not possible to use `uv` in immutable (where `--system` couldn't work) containers (such as Codespaces) without an useless `venv`

@Malix-off You could use [`--target`](https://github.com/astral-sh/uv/pull/3257) as a workaround, and point to `/home/codespace/.local/lib/python3.10/site-packages/`. For example, in a blank workspace with Python 3.10:

```shell
/workspaces/codespaces-blank $ uv pip install --target /home/codespace/.local/lib/python3.10/site-packages/ -p /home/codespace/.python/current/bin/python3.10 flask
Resolved 7 packages in 55ms
Downloaded 5 packages in 38ms
Installed 5 packages in 8ms
 + blinker==1.8.2
 + click==8.1.7
 + flask==3.0.3
 + itsdangerous==2.2.0
 + werkzeug==3.0.3

/workspaces/codespaces-blank $ python3.10 -c "import flask; print(flask.__version__)"
3.0.3
```


---

_Comment by @zanieb on 2024-06-05 13:55_

Could you share a bit more about why the "useless" virtual environment matters? It seems like the overhead of creating it should be negligible. 

---

_Comment by @eswolinsky3241 on 2024-06-05 14:03_

@zanieb For us, we use containerized cloud development environments because our users aren't technically advanced enough to understand virtual environments. So we use development containers to offer the same project isolation. This works perfectly with pip, where we can just have them clone the repo, run `pip install -r requirements.txt` and have the packages installed to the `$HOME/.local` directory. Having them run commands they don't understand to create a venv and activate it every time they log in (or maintaining scripts to automate this) is something we'd rather not have to deal with to get the performance improvement of `uv`. 

---

_Comment by @jbcpollak on 2024-06-05 14:08_

As a work around, currently for our devcontainer we have (effectively):

`	"postCreateCommand": "uv venv && uv pip install -r ${PROJDIR}/requirements.txt",`

and in our production Dockerfile:

```
ENV VIRTUAL_ENV=/app/.venv

# Copy files and other setup here

RUN uv venv && \
    uv pip install -r ./requirements.txt && \
    uv pip install -e .
```

I would prefer if inside the containers we didn't need to have a virtual environment at all though.

---

_Comment by @imfing on 2024-06-05 14:42_

@jbcpollak @eswolinsky3241 to avoid creating the virtual environment, this may be a workaround:

```dockerfile
FROM python:3.10-slim
RUN pip install --no-cache-dir uv

RUN uv pip install \
    --python $(which python) \
    --target $(python -m site --user-site) \
    -r requirements.txt
```

it should behave the same as using the `--user` flag by [installing the packages into user site](https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site) (my `requirements.txt` contains only `requests`):

```shell
docker run --rm -it --entrypoint python 217c1ad27560 -m pip list
Package            Version
------------------ --------
certifi            2024.6.2
charset-normalizer 3.3.2
idna               3.7
pip                23.0.1
requests           2.32.3
setuptools         65.5.1
urllib3            2.2.1
uv                 0.2.6
wheel              0.41.2
```

---

_Comment by @blueraft on 2024-06-05 14:53_

Another [workaround](https://www.dataduel.co/uv-pip-install-system/) to using uv in codespaces.

---

_Comment by @matthew-brett on 2024-06-05 15:34_

Just a meta-point here - as I said above - I teach a lot of beginners - and to beginners, using a virtualenv is extremely confusing - because you really have to understand the details of a Python installation - which is far too much for a beginner coder to take on.   I mean the type of coder who is still learning for-loops.  And of course, we're very often teaching Python to beginner coders.    Unfortunately - but inevitably - installation tools tend to be written by very experienced developers, to whom virtualenvs are no barrier.    But I can assure you from long long experience that virtualenvs will be a barrier to beginners.   Just for example, my third year students, who already know Python and Git and Python data science libraries - got pretty edgy when faced with `pyenv` in a class - despite some explanation - and even for them, it would have been a few hours of explanation and practice to get them into a state where they were ready to experiment with virtualenvs.

On the other hand - it seems to me this is very easily soluble by having a default virtualenv - where the user would need to understand virtualenvs only at the point that they really need them - when they get version conflicts or start serious development.

---

_Comment by @Malix-Labs on 2024-06-05 15:40_

Nice workarounds,

Do you know if there is a current workaround (`--user` unavailable yet)
- Without `"remoteUser": "root"`
- Working on immutable (/ rootless) systems (such as codespaces…) AND on mutable systems (such as regular Docker devcontainers)
- No hardcoded target (i.e. hardcoding the "`/home/codespace/.local/lib/python3.10/site-packages/`" string)

?

---

_Comment by @imfing on 2024-06-05 15:50_

@Malix-off check out my other comment for Dockerfile: https://github.com/astral-sh/uv/issues/2077#issuecomment-2150232739 `root` is not needed, and usually not recommended.

The hardcoded target user site-packages directory can be easily retrieved via `python -m site --user-site`:

```bash
uv pip install \
    --python $(which python) \
    --target $(python -m site --user-site) \
    -r requirements.txt
```

It works for both codespaces as I commented in https://github.com/astral-sh/uv/issues/2077#issuecomment-2150008781 and docker containers https://github.com/astral-sh/uv/issues/2077#issuecomment-2150232739

---

_Comment by @zanieb on 2024-06-05 16:44_

@matthew-brett fwiw we're trying to address those problems holistically in new CLI interfaces instead of just tacking on a workaround to the `uv pip` interface.

---

_Comment by @matthew-brett on 2024-06-05 16:49_

> @matthew-brett fwiw we're trying to address those problems holistically in new CLI interfaces instead of just tacking on a workaround to the `uv pip` interface.

Nice - thanks - is there a good place to look for that?

---

_Comment by @zanieb on 2024-06-05 16:52_

All of the new interface work is [labeled as preview](https://github.com/astral-sh/uv/issues?q=label%3Apreview+). We'll be publicizing more user-facing content in the next couple months.

---

_Comment by @Malix-Labs on 2024-06-05 16:53_

Indeed @imfing, it seems that [your workaround](https://github.com/astral-sh/uv/issues/2077#issuecomment-2150406001) is the best until this issue is solved !

---

_Comment by @Malix-Labs on 2024-06-05 19:48_

I am testing @imfing's workaround:

I get different results between `uv pip list` and `pip list` after running
```sh
uv pip install \
	--python $(which python) \
	--target $(python -m site --user-site) \
	fastapi
```

![image](https://github.com/astral-sh/uv/assets/76160668/5f02fc97-227f-46e1-b698-2a49e6ddf74f)

Do you know if that's normal?
How should `uv pip compile`, `uv pip sync`, and `uv pip freeze` be used?

---

_Comment by @imfing on 2024-06-05 20:19_

@Malix-off this is expected as `uv pip list` doesn't look for packages other than virtual environment or system site-packages (if `--system` is specified). However, both `python` and `pip` would take user site packages directory into account ([PEP 370](https://peps.python.org/pep-0370/))

`uv pip compile` and `uv pip sync` support `--target` flag, but `uv pip freeze` doesn't. 
It is also worth mentioning that the default Python system packages could be ignored if `--target` is provided since it makes `uv pip` to only search for packages under the `target` directory.

---

_Comment by @Malix-Labs on 2024-06-05 22:06_

Okay, thanks for that clarification!

We will still be forced to use `venv` for now, until `--user` is available

---

_Comment by @charliermarsh on 2024-06-05 23:31_

To clarify, there's a good chance that we'll never support `--user`. The user install scheme predates virtual environments. If virtual environments existed today, but the user install scheme didn't, I'd be very surprised if the user install scheme would be added to the packaging ecosystem.


---

_Comment by @matthew-brett on 2024-06-06 09:31_

> To clarify, there's a good chance that we'll never support `--user`. The user install scheme predates virtual environments. If virtual environments existed today, but the user install scheme didn't, I'd be very surprised if the user install scheme would be added to the packaging ecosystem.

That's quite possible - requiring virtualenvs was clearly attractive to the `pip` maintainers - as seen here : https://peps.python.org/pep-0704 .    I suspect the reason that `--user` installs have persisted is the one I and several others have given above - virtualenvs are hard to explain to beginners, and a frequent source of confusion - but that is much less true for adding `--user` to `pip install`.

---

_Comment by @Malix-Labs on 2024-06-06 09:33_

Requiring venv by default but allowing user install with an explicit `--user` flag seems to be the best of both worlds

Again, venv is bloat in devcontainers

---

_Comment by @matthew-brett on 2024-06-06 09:42_

> Requiring venv by default but allowing user install with an explicit `--user` flag seems to be the best of both worlds

Requiring venvs by default means that the users who are most likely to be confused will get the default behavior, and have to work out how to avoid it.   `--user` is better than nothing for those users, but having a default - invisible to user - virtualenv is even better, in my opinion.

---

_Comment by @Malix-Labs on 2024-06-06 09:50_

> Requiring venvs by default means that the users who are most likely to be confused will get the default behavior, and have to work out how to avoid it.

> having a default - invisible to user - virtualenv is even better, in my opinion.

So you want

- `uv pip install` to execute `uv venv && source .venv/bin/activate` if no venv is already present
- `uv pip install --user` to allow install in user's python

?

---

_Comment by @matthew-brett on 2024-06-06 10:14_

> So you want
> 
>     * `uv pip install` to execute `uv venv && source .venv/bin/activate` if no venv is already present
> 
>     * `uv pip install --user` to allow install in user's python

No - not `uv venv && source .venv/bin/activate` - because part of the problem with virtualenvs is explaining to the user why they have to do `.venv/bin/activate` to get their installed packages.   With that result, when the beginner closes their terminal and start it again, perhaps with another working directory, they'll be in some trouble.

What I would like is for `uv pip install pygame` to *just work*(TM) for a beginner user, with no further setup, such that, when they close their terminal and start it again later, they can do e.g. `import pygame`.

One way that might work, is to follow the `conda` pattern - there is a default environment, that the user is installing into, often without them realizing it.   When they get more advanced they can make new environments and switch to them.

For `pip`, I am wondering if this can be achieved with a default virtualenv, say in `~/.venvs/default`.   When the beginner does `uv pip install pygame`, instead of seeing `error: No Python interpreters found in virtual environments`, `uv` creates `~/.venvs/default` if necessary, installs into that virtualenv, and activates it (as you've suggested, except with the standard location).     There is some default or simply-enabled setup that activates this virtualenv by default on starting a terminal, if no other is activated - so the user will by default be able to restart the terminal and do `python -c "import pygame"`. 

With that working, I think `--user` becomes an unnecessary confusion, and I'd be happy to see it go.

---

_Comment by @potiuk on 2024-06-07 04:24_

Actually as an author of the issue I stopped needing it. And @charliermarsh @zanieb - feel free to close that one if you feel that you do not want to implement `--user` flag.

I was in the same camp "venv in container images is a bloat" - and this was for me the main reason why I opened the issue, but since then I found out that when I create venv in `~./.local` - and simply set `VIRTUAL_ENV` vatiable to point to it and having `~/.local/bin` on the PATH  as first entry does precisely what everyone needs here:

* no need to explain user to do `activate`
* `activate` merely sets the `VIRTUAL_ENV` variable and inserts PATH entry - and this is as specified in venv defintion, so you can do precisely the same in your container image or environment definition
* it also nicely retains packages installed in `.local` when you create a new virtual environment with `--system` flag  (which was the reason why I opened the issue) - which means that you can nicely create and activate new venvs from such `.local` venv
* this all adds virtually no overhead comparing to `--user` flag

I believe now that there is no particular reason to have `--user` flag, really. People who say that they do not want to maintain extra scripts are really talking about running two commands. People mentioned in the discussion thread here, that  they do not want to teach their users about venv, and the approach above 100% solves that. You don't have to. You just create the venv and set the right environments for them and they can continue using `uv pip install` or `pip install`  and it will **just work**. Mission accomplished.


---

_Comment by @potiuk on 2024-06-07 04:30_

BTW. Kind request - If someone would like to discuss whether to use some .venv by default, this is NOT the right place to discuss it. This is a discussion about the `--user` flag.  Whether to use a venv by default is very tangential to the `--user` flag and I think if someone would like to discuss it, they should open their own issue/discussion, rather than hijack this issue -  which was initially on ly for `--user` support.

---

_Comment by @Malix-Labs on 2024-06-15 11:49_

[The associated pull request was just closed minutes ago](https://github.com/astral-sh/uv/pull/2352#event-13168683504)

Does that mean that devcontainers and codespaces will never be able to use uv without the venv bloat?

---

_Comment by @zanieb on 2024-06-15 16:27_

We didn't close the pull request, the author did because they have a clear work around.

I think the trade-offs discussed here are not suggesting that we _should_ add this flag. I don't think that means that we'll definitely never add the flag, just that the current evidence is not in favor of it.

I don't really see "virtual environment bloat" as a real problem, they're very lightweight and if you want to bypass it there are a couple workarounds. If this is a big deal to you, I think you'll need to provide more information on _why_.

> What I would like is for uv pip install pygame to just work(TM) for a beginner user, with no further setup, such that, when they close their terminal and start it again later, they can do e.g. import pygame

This is the most compelling argument for user-level installations, but mutable global environments have been repeatedly shown to be the source of a great deal of confusion and problems. We'd like to solve this problem in other ways.

---

_Comment by @potiuk on 2024-06-15 16:30_

> Does that mean that devcontainers and codespaces will never be able to use uv without the venv bloat?

My experience is tha venv is about the same bloat as what gets created with `--user` flag. But this has been discussed here and elsewhere -  i.e. approach where venv will be created automatically for you if not existing. That seems much more consistent approach. 

Though I think it's the question or where and how it should be created. I think it's quite appealting and constructive discussing that possibility is probably best we all can do. 

As the author of that issue, I am also OK with closing this one, because I also agree with @zanieb that just "straightforward" --user flag rip-off from `pip` is not the best appraoch.

Ideally, maybe we should discuss and agree a new PEP how venvs should be automatically created  for the out-of-the-box cases? 

---

_Comment by @matthew-brett on 2024-06-15 17:52_

Just to answer the previous post - why mix the discussion of `--user` and default virtualenvs? - because, if there are no immediate plans for a "just-works" solution for the beginner, such as default virtualenv, then I would be interested in having the `--user` option as a not-quite-as-good solution to the same problem - even if it was only temporary.

---

_Comment by @potiuk on 2024-06-15 19:03_

> Just to answer the previous post - why mix the discussion of `--user` and default virtualenvs? - because, if there are no immediate plans for a "just-works" solution for the beginner, such as default virtualenv, then I would be interested in having the `--user` option as a not-quite-as-good solution to the same problem - even if it was only temporary.

When you build product (like the `uv` team does) - temporary solutions usually turn into long-term maintenance headache, because those who start using it expect it to continue working. So while you might have a wish to solve your problem, you have to understand that maintainers will have to live with those decisions for a long time. 

---

_Comment by @matthew-brett on 2024-06-15 19:12_

Ah yes I understand - and I do build product.  But I do want to push back on "your problem" - it's really not my problem - but a problem that will be common for beginners.

---

_Comment by @potiuk on 2024-06-15 19:17_

> Ah yes I understand - and I do build product. But I do want to push back on "your problem" - it's really not my problem - but a problem that will be common for beginners.

So let maintainers solve it in their due time. Without pushing them, as they have likely many other things to work on wich are likely higher priority.

---

_Comment by @zanieb on 2024-06-15 19:48_

> because, if there are no immediate plans for a "just-works" solution for the beginner

These plans are immediate; we're building out next-generation workflows on top of the foundation we built for `uv pip` right now. These workflows are intended to abstract virtual environment management. We'll have more to announce on this front soon.

---

_Comment by @Malix-Labs on 2024-06-20 23:03_

Currently, UV makes it **impossible** to use python without a `.venv`, because it **purposely** disables the `--user` flag, **even if it would not the default, and no recommendation would be made, or even by putting additional warnings for too curious noobs**

Why not allowing `--user`, granted it's not the default, not recommended unless in a container, and need to be done with an explicit flag?

For me, a non-noob user but new to the mess of python ecosystem, and also using standard tools such as containers instead of virtual environment, `.venv` is another layer, and a different workflow which is also more manual and prone to error (`source .venv/bin/activate` each time), which has shot me in the feet enough to say I would definitely prefer container + `--user`.

I was hoping Astral would unify things
Yet, it doesn't for me

---

_Comment by @zanieb on 2024-06-20 23:21_

Frankly, we don't make it impossible to use Python without a virtual environment. We allow [installing into non-virtual Python environments](https://github.com/astral-sh/uv?tab=readme-ov-file#installing-into-arbitrary-python-environments) in a myriad of ways, e.g., with `--python <path>` or `--system`. There are workarounds for user-level installs, e.g., using [`--target`](https://github.com/astral-sh/uv/pull/2352#issuecomment-2150061839). There are ways to [avoid manual activation of the environment](https://github.com/astral-sh/uv/issues/2077#issuecomment-2153906603). Additionally, we are _actively_ building workflows for uv that do not require you to ever manually manage a virtual environment.

I understand you don't have write permissions to the system site packages in your devcontainer context and that means you need to understand a bit more to set up your environment. However, we must take into account the trade-offs for the situations of _many_ users.

edit: I see your post was edited a bunch times while I was writing this, sorry if I've responded to something out of date.

---

_Comment by @Malix-Labs on 2024-06-20 23:35_

- Are `--python` and `--target` available on every `uv pip` commands (or `--python` available for some commands and `--target` for the other, or some commands don't support either) ?

- Which flag + argument would be a 1:1 behaviour replication to `--user` for all `uv pip` use-cases ?

- > you need to understand a bit more to set up your environment

  For my use-case, understanding `.venv` would still be inferior to using raw `--user` (or another 1:1 flag + command), even if I was willing to specifically understand a bit more python's `.venv`

- > However, we must take into account the trade-offs for the situations of many users.

  I don't understand how allowing `--user`, while still continuing to not recommend it anywhere to make it foolproof, would be an inferior trade-off compared to not making it possible for those who want to use `--user` to use it (and would know they really want to use it, at that point)

- > I see your post was edited a bunch times while I was writing this, sorry if I've responded to something out of date

  Yes, no problem it's still up-to-date 🙂

---

_Comment by @zanieb on 2024-06-21 02:04_

> Are --python and --target available on every uv pip commands 

`--python` is available on all of the `uv pip` commands. `--target` is available on mutating commands like `install`, `uninstall`, and `sync`.

> Which flag + argument would be a 1:1 behaviour replication to --user for all uv pip use-cases ?

Generally the invocation looks like this:

```
❯ uv pip install --target "$(python3 -m site --user-site)" fastapi
...
❯ python3 -c "import fastapi; print(fastapi.__version__)"
0.111.0
```

Note this was also discussed [further up in this thread](https://github.com/astral-sh/uv/issues/2077#issuecomment-2150008781).

I think we'd need to add `--target` to the other commands if you want to use it for inspection — which I don't see an immediate problem with, nobody has asked for it yet.

> or my use-case, understanding .venv would still be inferior...

Perhaps you would find this draft documentation helpful https://github.com/astral-sh/uv/pull/4433

> I don't understand how allowing `--user`...

I think you've been quite clear that you want the `--user` flag — we appreciate the feedback but we'd rather focus on new ways to address this use-case. While it may not seem like much effort to add, we are ultimately responsible for maintaining this tool for years to come.  Let's try not to rehash the discussions we've already had here and focus on new perspectives and use-cases. 


---

_Comment by @jbcpollak on 2024-06-21 19:35_

I've settled on "faking" a venv in my environments so I'm fine without --user but I wanted to point out this:

> Generally the invocation looks like this:
>
>❯ uv pip install --target "$(python3 -m site --user-site)" fastapi
...
> ❯ python3 -c "import fastapi; print(fastapi.__version__)"
0.111.0

Does not seem to work, at least on Windows. Some of my team are still using "bare" Python installs on windows and global installations of dependencies, but when I try the above command (adapted the path resolution to Windows), I get the error about no venv found. Is that supposed to be bypassed when using --target?

---

_Comment by @imfing on 2024-06-21 19:41_

@jbcpollak I remember both `--target` and `--python` are needed 

See my previous comment:
https://github.com/astral-sh/uv/issues/2077#issuecomment-2150406001

```bash
uv pip install \
    --python $(which python) \
    --target $(python -m site --user-site) \
    -r requirements.txt
```


---

_Referenced in [astral-sh/uv#4624](../../astral-sh/uv/issues/4624.md) on 2024-06-28 15:49_

---

_Referenced in [astral-sh/uv#2059](../../astral-sh/uv/issues/2059.md) on 2024-07-14 16:16_

---

_Comment by @sqrl on 2024-08-06 15:26_

This workaround doesn't seem to create a `/bin` sub-directory the way `--user` does?
Testing on `uv` version 0.2.33 I believe.

---

_Comment by @charliermarsh on 2024-08-06 16:39_

Which workaround are you referring to?

---

_Comment by @sqrl on 2024-08-06 19:42_

> Which workaround are you referring to?

The one mentioned in this comment:
https://github.com/astral-sh/uv/issues/2077#issuecomment-2150406001

---

_Comment by @imfing on 2024-08-09 23:22_

> > Which workaround are you referring to?
> 
> The one mentioned in this comment: [#2077 (comment)](https://github.com/astral-sh/uv/issues/2077#issuecomment-2150406001)

@sqrl use `--prefix` instead of `--target` should work better in this case:

```bash
uv pip install \
    --python $(which python) \
    --prefix $(python -m site --user-base) \
    -r requirements.txt
```

See https://github.com/astral-sh/uv/issues/2059#issuecomment-2227402333 and https://github.com/astral-sh/uv/pull/4085

---

_Comment by @matthew-brett on 2024-08-26 14:33_

Hi - sorry - I scanned the preview labels - but couldn't see the proposed solution for having a default virtualenv - did I miss it?  Or is it still in progress?

---

_Comment by @Rotonen on 2024-08-26 15:06_

As this seems to lead into a lot of confusion and keeps resurrecting, here's a recipe.

Amend your own base container (concatenated as a multistage image for brevity / being a self contained demo):

```
FROM python:3.12-slim-bookworm as setup-user
# Always run your end software as non-root!
RUN useradd -ms /bin/bash python
USER python
WORKDIR /home/python

FROM setup-user as setup-venv
# Setup upstream standard venv
#
# XXX - as upstream venv gives you setuptools and friends, those should also
# be pinned in a separate requirements-setup.txt or such and installed in a
# separate bootstrapping phase to keep things actually well defined and
# stable as they get bumped forward when the upstream Python image gets
# updated - omitted for clarity here.
#
# The three lines below are probably what you're missing from stuff
# "just working"!
RUN python -m venv .venv
ENV VIRTUAL_ENVIRONMENT=/home/python/.venv
ENV PATH=/home/python/.venv/bin:"$PATH"

FROM setup-venv as setup-uv
# XXX - This would be defined via a requirements-uv.txt or such in real use.
RUN pip install uv==0.3.3

FROM setup-uv as use-uv
# XXX - This would be defined via a requirements.txt or such in real use.
RUN uv pip install pyfiglet==1.0.2

FROM use-uv as everything-works
# Separating ENTRYPOINT and CMD is in general a nice idea
ENTRYPOINT ["pyfiglet"]
CMD ["uv rocks!"]
```

Then on the downstream consumer side in your org the consumer can simply `pip install` or `uv pip install` as they please and everything will happen via that predefined venv as intended.

IMO no changes on the side of uv needed. Feature parity with all sins of the past is not the way forward.

---

_Comment by @matthew-brett on 2024-08-26 15:45_

Just to clarify - you can read the details further up, but the use-case here is the absolute beginner, running the equivalent of `uv install pygame`, in their default Python environment, and without having to explain virtualenvs.

---

_Comment by @zanieb on 2024-08-26 18:10_

> Hi - sorry - I scanned the preview labels - but couldn't see the proposed solution for having a default virtualenv - did I miss it? Or is it still in progress?

We didn't introduce any preview behavior around this — we're probably not going to support a `--user` flag or a default virtual environment.

---

_Comment by @Malix-Labs on 2024-08-26 18:15_

When working with devcontainers or nix , forbidding to use an explicit `--user` flag (and thus forcing to use a venv anyway) is a net negative

---

_Comment by @matthew-brett on 2024-08-26 18:17_

Aha - ouch - well - I'm afraid that will make `uv` inaccessible to beginners - but I understand you have to optimize to some subset of users ...

---

_Comment by @matthew-brett on 2024-08-26 18:19_

I guess the plan will be to propose `uv` to more advanced users who are comfortable with virtualenvs.   But that will necessarily mean some division of the user-base - with some using `uv` and others starting, and perhaps continuing with some other tool, that does allow just-works installs without understanding virtualenvs.

---

_Comment by @zanieb on 2024-08-26 18:39_

We very much care about supporting beginners and eliminating the need to understand virtual environments — but we don't think a global mutable environment is the way to do that.

---

_Comment by @matthew-brett on 2024-08-26 18:46_

Aha - could you say more about other approaches you are considering?


---

_Comment by @edmorley on 2024-08-27 12:19_

One caveat to bear in mind when skipping using a venv and instead using `--user` with the global Python installation: If you are using a relocated Python install, some packages doesn't correctly locate the stdlib out of the box, eg:
https://github.com/unbit/uwsgi/issues/2525

Working around that requires setting `PYTHONHOME` explicitly, which then comes with its own set of issues.

In contrast, support for [PEP-405](https://peps.python.org/pep-0405/) style venvs seems much more prevalent.

It's for this reason that I'm in the process of migrating the Heroku Python [CNB](https://buildpacks.io/) to use a venv in the layer containing the app dependencies instead of a user site-packages installation.

If you are using a non-relocated Python install (such as the Python in the `python:*` images on Docker Hub), then this issue doesn't apply. But it's something to bear in mind wrt the "venvs aren't needed for containers" argument, since it's not always true.

---

_Comment by @Malix-Labs on 2024-08-27 13:36_

Do that mean that the python ecosystem has some dependency on venv usage ?

---

_Comment by @edmorley on 2024-08-27 13:44_

More that:
(a) by design venvs are in a different location from system Python so tooling can't avoid handling the `sys.prefix` vs `sys.base_prefix` distinction, which sidesteps any issues with relocated Python if the packaging/tools don't do the right thing otherwise
(b) in general venvs appear to be the more well-trodden path (particularly when comparing to `--user`, which is rarer than 100% system installs), since so many workflows/tools now use them by default (eg Poetry, Pipenv, uv) - and sticking to the well tested path tends to result in fewer surprises overall

---

_Comment by @Malix-Labs on 2024-08-27 13:47_

1. Understandable, but isn't relocating supposed to be solved by fixing the path in /usr/bin/env too ?
2. Understandable too, are there any other language than Python having venv ?
Is working in a devcontainer an unpopular workflow in python, or at least way less popular than in other languages ?

---

_Comment by @edmorley on 2024-08-27 13:55_

> Understandable, but isn't relocating supposed to be solved by fixing the path in /usr/bin/env too ?

No that doesn't help. Please see the STR in https://github.com/unbit/uwsgi/issues/2525

(we're getting a bit off-topic here, so this will be my last reply :-) )

---

_Referenced in [astral-sh/uv#5229](../../astral-sh/uv/issues/5229.md) on 2024-08-27 14:17_

---

_Referenced in [linuxserver/docker-homeassistant#109](../../linuxserver/docker-homeassistant/issues/109.md) on 2024-10-12 14:47_

---

_Referenced in [home-assistant/core#128214](../../home-assistant/core/issues/128214.md) on 2024-10-13 23:52_

---

_Comment by @matthew-brett on 2024-12-20 14:43_

> We very much care about supporting beginners and eliminating the need to understand virtual environments — but we don't think a global mutable environment is the way to do that.

I was (as you probably saw) following the discussion over at Scipy - could you say more about the current state of play for this?  Is there a recommended `uv` way of avoiding the need to understand virtual environments?   Or is there one in the pipeline?

---

_Referenced in [scipy/scipy.org#595](../../scipy/scipy.org/pulls/595.md) on 2024-12-20 14:50_

---

_Comment by @zanieb on 2024-12-20 20:37_

> ... could you say more about the current state of play for this? Is there a recommended uv way of avoiding the need to understand virtual environments? Or is there one in the pipeline?

The current state is that you need declarative metadata, either in a project (via `pyproject.toml`) or a script (via PEP 723 metadata). I want to do more for people that want to manage declarative environments _outside_ of those contexts, e.g., an environment you'd re-use for analyzing data in several projects — but we have not designed that yet.

---

_Comment by @matthew-brett on 2024-12-20 23:03_

Do you think there will be a route to using `uv` that does not require specific activation of an environment or getting the environment from the current directory or specific declaration of dependencies?   I mean, similar to the workflow with the default environment of `conda`?  Or is that unlikely in the near future?

---

_Referenced in [PrairieLearn/PrairieLearn#11039](../../PrairieLearn/PrairieLearn/pulls/11039.md) on 2025-02-11 05:00_

---

_Comment by @63427083 on 2025-02-18 13:51_

Frankly, i don't understand how this conversation went on discussing if this feature should be implemented at all when no non-convoluted alternative like [imfing's suggestion](https://github.com/astral-sh/uv/issues/2077#issuecomment-2150406001) is given.

To illustrate my point; here's my first experience playing with uv:
```
~: uv
~: uv pip install Pillow
error: No virtual environment found; run 'uv venv' to create an environment, or pass '--system' to install into a non-virtual environment
~: uv pip install --system Pillow
Using Python 3.12.8 environment at: /usr
Resolved 1 package in 103ms
error: Failed to install: pillow-11.1.0-cp312-cp312-manylinux_2_28_x86_64.whl (pillow==11.1.0)
  Caused by: failed to create directory `/usr/local/lib64/python3.12/site-packages/`: Permission denied (os error 13)
```

And i'm already looking at obscure Github discussions to make a common Python tool available outside of a development environment. 

---

_Comment by @Rotonen on 2025-02-18 14:05_

https://docs.astral.sh/uv/concepts/tools/

---

_Comment by @charliermarsh on 2025-02-18 14:18_

You might be starting from the wrong conclusion? You either want a virtual environment (as in the hint!) or to install your package via `uv tool install`, if it's a standalone command-line executable. We haven't added `--user` because we don't see it as a better solution to solving any of those problems.


---

_Comment by @matthew-brett on 2025-02-18 14:21_

> https://docs.astral.sh/uv/concepts/tools/

Were you thinking of any particular part of that page as an answer to the question?

For context - I don't think the question was - how do I install a command line tool in an isolated environment? - it was how do install a set of particular packages into any kind of default environment - such as the system environment, or a user environment.

---

_Comment by @matthew-brett on 2025-02-18 14:31_

For context - I strongly suspect that there are always going to be a significant group of people for whom "install in a virtualenv" is not going to be a good answer - and very few of those people are trying to install an isolated command line tool.   My guess is that, given the move of both `pip` and `uv` to make virtualenvs a practical requirement, `conda` will become increasingly attractive as a default install for beginners and other 'just give me an environment I don't have to think about' users.

---

_Comment by @63427083 on 2025-02-18 17:57_

[Rotonen](https://github.com/Rotonen) said:
> https://docs.astral.sh/uv/concepts/tools/

[charliermarsh](https://github.com/charliermarsh) said:
>You might be starting from the wrong conclusion? You either want a virtual environment (as in the hint!) or to install your package via uv tool install, if it's a standalone command-line executable. We haven't added --user because we don't see it as a better solution to solving any of those problems.

First off, thanks for pointing me in the right direction. After all "tools" worked exactly as I needed, which is great, but I’m still a bit confused about a couple of things:
- Why isn’t "tools" seen as the obvious alternative to `pip install --user`? Like, what’s missing that makes it not the go-to solution? This was the first GitHub issue that popped up when searching for uv pip --user related stuff, so others might be wondering the same thing.
- Why not add a tooltip or some kind of hint when running `uv pip install --user`, or at least complement the somewhat unhelpful message `error: pip's '--user' is unsupported (use a virtual environment instead)`? I feel that could help avoid confusion for some more people like me coming from regular pip, expecting similar or even same behaviour.

---

_Comment by @potiuk on 2025-02-18 18:20_

@63427083 -> I heartily recommend you listen to the podcast of @charliermarsh https://realpython.com/podcasts/rpp/238/ -> he eplains there the reasoning and the whole philosophy behind uv.

Uv is **not** what `pip` installer is. Uv is mostly to support two approaches:

* tools -> this allows you to install CLI tools in their own venv (without you even knowing they have venv)
* the rest (project based env management) -> this allows you to maintain venv for the project you are developing (and if you use sync you won't even know you use venv).

I don't think the goal of uv is to allow you to install packages in a standalone, separate .venv installed in a "default" python. This is what used to be the "old" way how you used to work with Python, one that has been very problematic and everyone (including pip 24+) will not let you follow that old pattern without specifying some specific flags. Basically all the tools out there introduce "friction" so that people re-learn how to deal with python installation differently. Even if it means that those people will be unhappy and have to change their habits.

This is  - as I understand it - very deliberate decision, that follows (or even sometimes leads) the way Python Packaging is evolving, and to be frank, you will be better to change your habits to follow those patterns.

And yes, I understand you might be unhappy, but I strongly recommend you to listen to the podcast, it provides much more context and explanation and after listening to it, you might be more willing to adapt.


---

_Comment by @potiuk on 2025-02-18 18:23_

Speaking of which @charliermarsh -> there is something to the messaging question that @63427083 had -> maybe it's worth to provide more guidance to users who also have expectations about "working the same way as old ways".

---

_Comment by @matthew-brett on 2025-02-18 18:56_

> This is - as I understand it - very deliberate decision, that follows (or even sometimes leads) the way Python Packaging is evolving, and to be frank, you will be better to change your habits to follow those patterns.
> 
> And yes, I understand you might be unhappy, but I strongly recommend you to listen to the podcast, it provides much more context and explanation and after listening to it, you might be more willing to adapt.

Yes, no question - that is the move - and the move is, I believe, based on two assumptions:

* It just isn't very hard to force beginners to come to terms with virtualenvs from the get-go and
* Beginners will have good and obvious benefits from using virtualenvs from the get-go.

I can see that these assumptions may be tempting to an experienced developer, but I can tell you, having taught many many beginners, that neither of these assumptions is correct.   And so, for beginners, we'll need something else.  `--user` was that thing, but that's fine, we'll have to find some other method - such as `conda` or using a Python where the `pip` can install into the system without complaint.

I'm happy to explain why the assumptions are not correct for beginners, but I don't want to extend this thread for those who aren't working with beginners.

---

_Comment by @merwok on 2025-02-18 20:14_

A note: it’s not just a flag from pip, it’s the concept of user packages specified in PEP 370 and compatible with multiple tools. One example: on debian and derivatives, bash includes ~/.local/bin in PATH by default if it exists. So pip install --user sometool is a valid and easy way to be able to run sometool without more docs or setup.

---

_Comment by @potiuk on 2025-02-18 22:12_

> I'm happy to explain why the assumptions are not correct for beginners, but I don't want to extend this thread for those who aren't working with beginners.

But the thing is that `uv tool` does exactly that service to beginners. It installs the tool you want in it's own virtualenv but it does not even tell the user where it is or that it does it. It can't be any easier than that.

Also if you want to develop something with python you do:

* `uv init`
* `uv add dependency` 
* `uv run whatever` 

As a beginner, you don't even have to have python installed, `uv` will download and install binary version of python from `python-standalone` project that is installed in less than few seconds. All the "pesky details" of setting up python and venv are completely hidden from you, and you don't even have to be aware it's there.

Again - it can't get any simpler than that.

So maybe your comments apply to the ways how beginners **used** to have problems with starting with Python before `uv`. Simply `uv` proposes different - way simpler - workflows for beginners, that utilise `venv` under the hood. I'd argue that even telling the beginner they should use `--user` is way more complex than the worfklows that `uv` proposes.

> A note: it’s not just a flag from pip, it’s the concept of user packages specified in PEP 370 and compatible with multiple tools. One example: on debian and derivatives, bash includes ~/.local/bin in PATH by default if it exists. So pip install --user sometool is a valid and easy way to be able to run sometool without more docs or setup.

And yes, I am (painfully) aware of PEP 370 and I am aware how universally problematic it turned out to be for `packaging` maintainers. And I eventually - after initial concerns, and having experienced a lot of problems with `--user` in the past, think that the idea of proposing a different, way more maintainable and less problematic solution (and not supporting --user) is a very sound one.

---

_Comment by @potiuk on 2025-02-18 22:24_

And again (@matthew-brett ) - go and listen to the podcast. Charlie explains exactly why `uv` is created for scale but designed for beginners (or a verly similar quote - can't remember). If - after listening the podcast - you still will think that people here do not think about beginners, then I am not sure how else I can convince you :D

---

_Comment by @matthew-brett on 2025-02-18 22:44_

> > I'm happy to explain why the assumptions are not correct for beginners, but I don't want to extend this thread for those who aren't working with beginners.
> 
> But the thing is that `uv tool` does exactly that service to beginners. It installs the tool you want in it's own virtualenv but it does not even tell the user where it is or that it does it. It can't be any easier than that.

By "beginner" I mean someone who is expecting to start an interactive Python console of some sort and do something like `import pygame` or `import pandas`.   Running some Python command line tool is not a very common beginner task.

> Also if you want to develop something with python you do:
> 
>     * `uv init`
> 
>     * `uv add dependency`
> 
>     * `uv run whatever`
> 
> 
> As a beginner, you don't even have to have python installed, `uv` will download and install binary version of python from `python-standalone` project that is installed in less than few seconds. All the "pesky details" of setting up python and venv are completely hidden from you, and you don't even have to be aware it's there.
> 
> Again - it can't get any simpler than that.

Well - yes it can.   Because a) that won't match nearly all tutorials out there, and b) it depends on the user keeping in mind that they need to stay in the same directory (or subdirectory thereof) to get the relevant virtualenv.    So if they, for example, do `cd ..` then they've lost their packages, for reasons that won't be obvious to them.

Just for reference, in a course we taught in Berkeley, to stats students, we had to include a class on the filesystem in the terminal, because many inexperienced computer users have a hazy idea of how files are arranged on the computer, beyond the idea of a "Folder".   I find that many developers haven't come across these kind of problems, and indeed they are easy to miss if you don't have quite close contact with early learners.

---

_Referenced in [astral-sh/uv#11609](../../astral-sh/uv/issues/11609.md) on 2025-02-18 23:20_

---

_Comment by @zanieb on 2025-02-18 23:26_

I think this exchange is sufficient — you've communicated your opinions on this topic and I'll ask that we don't debate these specific points more here.

We care a lot of the beginner user experience and we have several team members with scientific backgrounds where "beginner" level Python experience is quite common. However, we're adverse to repeating the mistakes of the past and are interested in pursuing new norms to make workflows easier. I'll admit that it's not perfect today, there's more to do still.

I've created two issues to track actionable improvements to @63427083's comment:

- https://github.com/astral-sh/uv/issues/11609
- https://github.com/astral-sh/uv/issues/11599

---

_Comment by @potiuk on 2025-02-18 23:41_

I very much concur wiht @zanieb 's comment and the follow-up. And seeing those, as the author of the issue I consider it "closed", so I am going to do exactly that.

---

_Closed by @potiuk on 2025-02-18 23:41_

---

_Closed by @zanieb on 2025-02-18 23:48_

---

_Referenced in [astral-sh/uv#12291](../../astral-sh/uv/issues/12291.md) on 2025-03-18 17:53_

---

_Referenced in [astral-sh/uv#15923](../../astral-sh/uv/issues/15923.md) on 2025-09-18 01:21_

---
