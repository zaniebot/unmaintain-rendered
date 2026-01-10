```yaml
number: 6314
title: "Support including dependency executables (e.g., `--include-deps`) in `uv tool install`"
type: issue
state: closed
author: hynek
labels:
  - enhancement
  - good first issue
  - uv tool
assignees: []
created_at: 2024-08-21T09:59:44Z
updated_at: 2025-09-11T19:15:38Z
url: https://github.com/astral-sh/uv/issues/6314
synced_at: 2026-01-10T03:23:52Z
```

# Support including dependency executables (e.g., `--include-deps`) in `uv tool install`

---

_Issue opened by @hynek on 2024-08-21 09:59_

hi, congrats on 0.3.0!

I've noticed one very useful pipx option that `uv tool` is currently missing: `--install-deps`. It means that it also adds the CLIs of packages it depends on. This is crucial for something like Ansible where the essential commands are hidden in sub-packages.

---

_Renamed from "uv tool --install-deps" to "uv tool install --install-deps" by @hynek on 2024-08-21 10:01_

---

_Comment by @ZingyAwesome on 2024-08-21 12:08_

It would be great if this could allow multiple dependencies to be installed in the same venv and for all of their executables to be exposed. More info in this [rye feature request](https://github.com/astral-sh/rye/issues/1061). This would allow me to install ansible,ansible-core and ansible-lint in the same venv, saving disk space and negating version mismatch issues.

---

_Comment by @zanieb on 2024-08-21 12:17_

I've been hesitant to provide a flag to install all executables from all dependencies — what about `--executable <name>` to include executables from dependencies?

---

_Label `enhancement` added by @zanieb on 2024-08-21 12:18_

---

_Label `uv tool` added by @zanieb on 2024-08-21 12:18_

---

_Comment by @mitsuhiko on 2024-08-21 12:19_

The way it works in `rye` is that `--include-dep` names the particular dependency and installs then the tools that that particular dependency provides so it's not quite as crazy as installing all.

---

_Comment by @ZingyAwesome on 2024-08-21 12:21_

Yeah that was what I meant, as long as you can specify multiple dependencies that way. My main issue with rye was that it allowed you to only specify 1 dependency to include executables from (or at least I couldn't get it to work with more than 1).

---

_Comment by @mitsuhiko on 2024-08-21 12:23_

I thought `--include-dep` can be provided more than once, but maybe there is a bug.

---

_Comment by @hynek on 2024-08-21 12:48_

I could perfectly live with `uv tool install ansible --include-dep=ansible-playbook --include-dep=ansible --include-dep=ansible-inventory`

Or however you call it; there's already the `--with` prefix used, so that might make sense too.

---

_Label `good first issue` added by @zanieb on 2024-08-29 23:10_

---

_Renamed from "uv tool install --install-deps" to "Support including dependency executables (e.g., `--include-deps`) in `uv tool install`" by @zanieb on 2024-09-08 13:36_

---

_Comment by @jamesharris-garmin on 2024-09-09 14:37_

This for ansible's install workflow is the last piece of me replacing pipx with uv tool so supporting this would be excellent.

---

_Comment by @lucab on 2024-09-20 16:07_

I had a look at all the linked issues and also asked some feedback out of band; I sketched some code to implement this, and I want to recap the proposed behavior. I'm not interested in debating the specific flag name right now, so let me call it ` --i-want-ponies` for the moment.

Using ansible as an example, I'll highlight these three packages:
 * `ansible`: top-level package tool, provides the `ansible-community` executable and depends on `ansible-core`.
 * `ansible-core`: mainly exists as a dependency of the above, provides 11 executables (including for example `ansible-playbook` and `ansible-inventory`).
 * `ansible-lint`: sits on its own and provides the `ansible-lint` executable.

`uv` can offer the following:
 * `uv tool install ansible`: installs `ansible-community`.
 * `uv tool install --i-want-ponies ansible-core ansible`: like the previous, plus it installs all the 11 executables from its `-core` dependency (including for example `ansible-playbook` and `ansible-inventory`).
 * `uv tool install --i-want-ponies ansible-core --i-want-ponies ansible-lint ansible`: like the previous, plus it brings in an extra package `ansible-lint` and installs its executable. It doesn't need an explicit additional `--with` if the package is not already a dependency of the top tool.

As a side note, at some point this may end up bringing in too many executables (e.g. from fat packages like `ansible-core`), so possibly there will be a need to grow an additional flag to specify an allow-list of globs to filter executables by name. I'll leave that for a separate ticket.

---

_Comment by @underyx on 2024-09-21 18:27_

For the record as a workaround you can do this today

```term
$ uv tool install ansible-core --with ansible
```

because ansible-core exposes the binaries, and the ansible package will provide all the plugins.

---

_Comment by @jamesharris-garmin on 2024-09-23 19:40_

This is a nice fix for the simple ansible case, but if my goal is to use ansible-lint I think I would have to install the tools as follows:

```bash
$ uv tool install ansible-lint --with ansible
```
In one invocation, then in a separate tool install:

```bash
$ uv tool install ansible-core --with ansible
```
which would mean that ansible-lint and the rest of ansible could get out of sync, which seems like it would be a nightmare if the two tool environments ever fell out of sync.

---

_Comment by @meoyawn on 2024-09-27 18:53_

same, plus I use `community.aws.s3_cors` module and currently forced to have 2 separate envs:

```sh
uv tool install ansible-core --with ansible --with boto3
uv tool install ansible-lint --with ansible --with boto3
```

in any case thank you so much for `uv` it's the GOAT

---

_Comment by @jamesharris-garmin on 2024-10-02 15:41_

@lucab This formulation sounds reasonable I would leave it up to developers of their tools to provide additional instructions. on how to install properly so that the user experience is correct.  I could really use this approach to eliminate sync issues.

---

_Comment by @KalleDK on 2024-10-14 08:32_

Came here for the same problem with ansible-lint, that now has it's own environment. I also have some smaller projects with the same problem.

---

_Comment by @gaby on 2024-10-17 12:54_

While the workarounds work. It's very painful to have to go through every package we are installing to figure out which deps it has to then add a massive list of flags for each one.

This should be a simple flag "--include-deps" like pipx does.

I don't to install lets say "app A" , and then spend 1hr debugging why apps are missing cause I didnt include a bunch of extra "--include-deps" for each one.

---

_Comment by @reinout on 2024-10-17 13:14_

@gaby: out of curiosity, how many packages do you have with that problem? Of the 15 I'm installing, I *only* have the missing-executable-problem with ansible, to my surprise. What I'm installing is mostly individual tools like "ruff", "docutils", which apparently don't have dependencies with executables.

If you have a "tool-collection-of-my-company" package which groups useful tools as dependencies: yeah, that'll quickly become a pain to install currently.

(I'm all in favour of `--include-deps`, btw.)



---

_Comment by @zanieb on 2024-10-17 13:26_

> I don't to install lets say "app A" , and then spend 1hr debugging why apps are missing cause I didnt include a bunch of extra "--include-deps" for each one.

It seems way to easy to pull in a bunch of random executables this way? It sounds like what you need is some sort of `uv tool show <name> --include-deps` so you can just see the executables up front.

---

_Comment by @gaby on 2024-10-17 13:33_

@reinout Yes, besides ansible, pylint, ruff, uv, jc, gitlab (which has sdk/cli), the main use case is internal enterprise tools. Most of them are `Typer` or `Textual` apps that are bundled/distributed together in one or many wheels. 

@zanieb You mean like before installing? Or if a tool is installed already to be able to see which apps it provides?

Right now we install things using pipx:

`pipx install --global MyEnterpriseApp --include-deps`. This install the app in a Global VENV (/opt/pipx/venvs/myenterpriseapp) and also adds each app cli to `/usr/local/bin/`.

These are multi-user systems were most users dont have sudo, they just use the provided tools by the system (python typer apps).

---

_Comment by @flying-sheep on 2024-10-24 09:39_

maybe instead of (or as an alternative to) `--include-deps`, there should be
1. an easy way to see all executables provided by the deps
2. a CLI option to include executables from specific deps

E.g. I’d like to install `jupyter`, `jupyter-lab`, `jupyter-execute` and so on, all into one environment.

---

_Comment by @zanieb on 2024-10-24 12:42_

We can also do... like `uv tool install <package> --executable <name> --executable <name>` and we can tell you what packages provide them or just allow installation from dependencies automatically at that point.

---

_Comment by @flying-sheep on 2024-10-24 16:34_

That would be more guessable, and would allow for advanced things like like `--executable 'jupyter-*'`

I’d still also like (a flag for) `uv tool list` to show all the binaries that _aren’t_ currently active.

---

_Comment by @stuartmaxwell on 2024-11-27 19:42_

> It seems way to easy to pull in a bunch of random executables this way? It sounds like what you need is some sort of `uv tool show <name> --include-deps` so you can just see the executables up front.

Could you explain the risk you are concerned about? With `pipx` if you use the `--include-deps` argument, you are specifically asking to install the executables from dependencies, which I wouldn't describe as installing "random executables".

Perhaps including a `--dry-run` option would help to ease concerns? e.g. `uv tool install --include-deps --dry-run ansible`. You already have the dry-run feature available in: `uv pip install --dry-run`. This could be extended to show the executables installed by each package when called with `uv tool install`.

---

_Comment by @zanieb on 2024-11-27 20:07_

> Could you explain the risk you are concerned about? With pipx if you use the --include-deps argument, you are specifically asking to install the executables from dependencies, which I wouldn't describe as installing "random executables".

Perhaps random was not a great word choice. There can be arbitrary transitive dependencies. It worry about it (1) being surprising which executables are installed and (2) that these executables could conflict with other tools.

A `--dry-run` option is a solution. I prefer a more explicit interface over `--include-deps` though.

---

_Comment by @stuartmaxwell on 2024-11-27 21:17_

> Perhaps random was not a great word choice. There can be arbitrary transitive dependencies. It worry about it (1) being surprising which executables are installed and (2) that these executables could conflict with other tools.

I think I still disagree with the term "arbitrary". If I use the `--include-deps` argument, then I'm specifically making an informed decision that I want all of the dependent executables installed too, there's nothing arbitrary about it. Sure, I might be surprised that Ansible includes the "ansible-doc" executable, which I've never used or needed, but I'm fine it being there in case I ever want to use it.

With regards to conflicting executables - isn't this only an issue when the executables are linked from the bin directory? And uv already handles the case where an executable already exists in the bin directory.


---

_Comment by @mgedmin on 2024-11-30 17:41_

Let's talk specifics.  With `pipx` I like to install ansible and various ansible-related tools (ansible-lint, molecule) in the same virtualenv, which I do with

    pipx install --incude-deps ansible
    pipx inject ansible dnspython  # some of ansible plugins I use depends on dnspython
    pipx inject --include-apps ansible ansible-lint
    pipx inject --include-apps ansible molecule

(incidentally, I'd love it if `uv tool` supported `inject`.  pipx remembers all the packages I've injected in a little .json file somewhere in the virtualenv and can reinstall them all at a later time with `pipx reinstall-all`, which is very helpful when I upgrade my system python from, say 3.12 to 3.13).

The thing here is that pipx inject has three modes:

- `pipx inject` with no flags installs the extra packages but doesn't symlink any binaries into ~/.local/bin
- `pipx inject --include-apps` installs the packages and symlinks their scripts, but doesn't symlink any scripts from transitive dependencies
- `pipx inject --include-deps` installs packages and symlinks everything

If I'd used `pipx inject --include-deps ansible-lint`, then I'd get an extra, unwanted ~/.local/bin/black, since black is one of the dependencies for ansible-lint.  I don't want my generic Python code formatter to be managed as part of an ansible-related tool virtualenv; I want to install and upgrade it separately and explicitly.

I guess this is the concern that @zanieb had: installing scripts from all transitive dependencies is sometimes undesireable.  I found that pipx's solution (a separate pipx inject command) allows me to control this in a fine-grained way, and I'd be happy if uv adopted the same solution (`uv tool inject [--include-apps|--include-deps] ...`).

---

_Comment by @SVHawk13 on 2024-12-01 22:42_

`uv tool install ansible` installs `ansible-lint`, `ansible-playbook`, etc. but only symlinks `ansible-community`. The below one-liner creates these symlinks: 

```bash
[ -d "$(uv tool dir)/ansible/bin/" ] && find "$(uv tool dir)/ansible/bin/" -mindepth 1 -maxdepth 1 -type f -executable -regextype posix-extended -regex '^((.+/)?)[^.]+' -print0 | xargs -0 ln -s -t "${HOME}/.local/bin/"
```

---

_Comment by @AndydeCleyre on 2024-12-01 23:03_

For whatever it's worth as another point of reference, my pipx clone in Zsh which wraps uv optionally takes a `--cmd` argument whose value is a comma separated list of commands to install. If that's not provided, and there's not an obvious single command from the whole installation (including dependencies), an interactive fuzzy finder in muli-select mode is invoked.

---

_Comment by @justin-yan on 2024-12-29 04:35_

I wanted to expand on the "enterprise" use case that gaby and reinout mention above (which I also have), since this thread has a lot of ansible examples, but I think this has at least one or two more use cases beyond that.

In my org, we ship an internal CLI application, that declares an optional extra `bins`, which is a collection of other CLI tools that are frequently used at the company.  It's nice to be able to `pipx install internal-cli[bins] --include-deps`, and get not only our org-internal CLI, but these other useful tools available on the $PATH in one shot.

One particularly nice thing about this approach is that as new versions of the internal CLI are published with updates to the `bins` optional extras dependency list, people only need to `pipx upgrade internal-cli` to get these new tools which can easily be aliased into a `self update` command in our org CLI.  self-update would be harder to implement if we needed to use a separate `pipx install` for every extra bin tool or something like `--executable` to explicitly list everything, though it's certainly not impossible!

Anyway, I hope that motivates a use case beyond "making ansible + uvx cleaner", but totally understand if you don't want to support this :-)

---

_Comment by @devmcp on 2025-01-10 15:27_

My use case is that I want to install jupyterlab as a tool but have access to the `jupyter` executable, which I used to get with `pipx install jupyterlab --include-deps`.

It seems that the suggested `uv tool install jupyterlab --executable jupyter` would be a good solution to this. Is there a workaround to this for now?

---

_Comment by @lgxz on 2025-01-27 14:01_

I have another idea: a new "uv tool link" command to link other files (like ansible-playbook).

```uv tool link ansible-playbook``` just do  "ln -s .local/share/uv/tools/ansible/bin/ansible-playbook .local/bin/ansible-playbook"


---

_Comment by @cu on 2025-01-28 01:45_

> I have another idea: a new "uv tool link" command to link other files (like ansible-playbook).

It's not a bad idea per se, but it means the user would have to know beforehand the names of all the executables. And some packages (e.g. Ansible) have many!

---

_Comment by @MurtadhaInit on 2025-05-03 12:20_

Any updates on this? I think the solution to this issue would greatly increase the value of using uv for me because it means I can safely get rid of pipx which I only use for that purpose (and `ansible` specifically).

---

_Comment by @zanieb on 2025-05-03 14:24_

It's just waiting for someone to finish the pull request, we're a small team with lots of priorities. Contributions are welcome, but it's nontrivial to add. 

---

_Comment by @MurtadhaInit on 2025-05-03 14:48_

> It's just waiting for someone to finish the pull request, we're a small team with lots of priorities. Contributions are welcome, but it's nontrivial to add. 

I appreciate all the effort. And thank you for this overall great tool.

---

_Comment by @aaron-ang on 2025-06-27 21:15_

hi @zanieb, I've added some comments to #14014 that addresses my new changes based on your feedback. I understand this feature is a significant change. I hope we can finalize this soon :)

---

_Closed by @zanieb on 2025-07-30 19:50_

---

_Comment by @bhuynhdev on 2025-09-11 17:32_

With the PR merged, May I ask what is the way to do `--install-deps` now, specifically in case of ansible? I did `uv tool install ansible`, but only `ansible-community` was added to my `bin` directory. `uvx ansible` works though

---

_Comment by @k-lovtsov on 2025-09-11 17:38_

> With the PR merged, May I ask what is the way to do `--install-deps` now, specifically in case of ansible? I did `uv tool install ansible`, but only `ansible-community` was added to my `bin` directory. `uvx ansible` works though

Now i install ansible with following command:

```shell
uv tool install ansible-core --with-executables-from ansible --with-executables-from yamllint --with-executables-from ansible-lint --with-executables-from molecule
```


---

_Comment by @janorga on 2025-09-11 19:07_

> With the PR merged, May I ask what is the way to do `--install-deps` now, specifically in case of ansible? I did `uv tool install ansible`, but only `ansible-community` was added to my `bin` directory. `uvx ansible` works though

There is an example specifically for Ansible also in `uv` web documentation at https://docs.astral.sh/uv/concepts/tools/#installing-executables-from-additional-packages
 
`uv tool install --with-executables-from ansible-core,ansible-lint ansible`

In my case works well, like the traditional `uvx` does.


---

_Comment by @gaby on 2025-09-11 19:15_

Using `--with-executables-from` is such a bad UX. Now I have to keep track of every dependency with executables. 

---
