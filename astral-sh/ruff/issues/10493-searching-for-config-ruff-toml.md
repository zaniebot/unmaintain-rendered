```yaml
number: 10493
title: Searching for .config/ruff.toml
type: issue
state: closed
author: bersace
labels:
  - configuration
assignees: []
created_at: 2024-03-20T17:15:52Z
updated_at: 2024-04-02T09:10:52Z
url: https://github.com/astral-sh/ruff/issues/10493
synced_at: 2026-01-12T15:54:50Z
```

# Searching for .config/ruff.toml

---

_@bersace_

Hi,

Ruff reads its configuration from `ruff.toml` at root directory of a project. To unclutter root directory, ruff reads its configuration from pyproject.toml. Some projects has several pyproject.toml and having a single ruff toml file is great.

JS world use `.config`: https://github.com/pi0/config-dir. Ruff project has a `.config` directory for nextest. 

I suggest that ruff also search for `.config/ruff.toml`. What do you think of this ?

Regareds,
Étienne

---

_Label `configuration` added by @MichaReiser on 2024-03-21 07:47_

---

_Comment by @MichaReiser on 2024-03-21 07:55_

Hy @bersace 

I understand that it's undesired to clutter your project root folder with configuration files. Have you considered using `ruff --config .config/ruff.toml` to tell ruff where to search for the configuration? It's not as luxurious as when Ruff automatically discovers the configuration for you but we try to keep the number of places where Ruff searches for configurations minimal because

* Ruff needs to check for the existence of a configuration file for every directory. The more configuration files it supports (or locations), the more IO operations are necessary and IO is slow. Ruff has to check this for every directory, even if it doesn't contain configuration files. 
* Supporting more locations isn't just overhead for Ruff but also for users because you now need to search for the configuration in multiple places rather than in one standardized path. 
* It's less obvious what Ruff should do if a project has both a `ruff.toml` and a `./config/ruff.toml`. Which configuration takes precedence? Should the configurations be merged and if so, in which order? 



---

_Comment by @bersace on 2024-03-21 07:59_

since `ruff.toml` is more evident than `.config/ruff.toml`, that's fair for ruff to prefer the first.

---

_Comment by @bersace on 2024-03-21 16:20_

I don't know how to configure each IDE to use the proper `--config`. 

---

_Comment by @MichaReiser on 2024-03-22 16:39_

> I don't know how to configure each IDE to use the proper `--config`.

What IDEs are you using?

---

_Comment by @bersace on 2024-03-27 19:28_

> > I don't know how to configure each IDE to use the proper `--config`.
> 
> What IDEs are you using?

I mean, configure IDE of all collaborators and contributors.

---

_Comment by @MichaReiser on 2024-03-28 10:15_

You could use [Workspace settings](https://code.visualstudio.com/docs/getstarted/settings#_workspace-settings) in VS code but I don't think you'll like it because they're saved into a `.vscode` directory. So you exchange a top-level `.ruff.toml` with a `.vscode` folder :( 



---

_Comment by @bersace on 2024-03-28 10:26_

I'm using Doom Emacs. Other use neovim, VSCode, and you-name-it. Trying to setup every IDE in every project is nogo.

About performances, I understand the point.

It looks like searching **inside** pyproject.toml is the heaviest, isn't it ? Would it be possible to limit search of `.config`  directory ? e.g. only .config sibling .git ?

---

_Comment by @MichaReiser on 2024-03-28 12:04_

The cost is more about testing if the configuration files exists because IO is slow in comparison to other computations. 

> It looks like searching inside pyproject.toml is the heaviest, isn't it ? Would it be possible to limit search of .config directory ? e.g. only .config sibling .git ?

I don't think that simplifies the problem. It would still require an additional test (it even makes it worse) to see if the `.git` directory exists. 

There's also the concern that I raised above, where it's unclear what the expected behavior is when you have the following project structure:

```
.config/
  - ruff.toml
- pyproject.toml
- ruff.toml
```

I agree that this is an edge case, but one that Ruff must handle correctly. 



---

_Comment by @bersace on 2024-03-28 13:08_


> > It looks like searching inside pyproject.toml is the heaviest, isn't it ? Would it be possible to limit search of .config directory ? e.g. only .config sibling .git ?
> 
> I don't think that simplifies the problem. It would still require an additional test (it even makes it worse) to see if the `.git` directory exists.

ruff already reads `.git/info/exclude`, isn't it ?

---

_Comment by @MichaReiser on 2024-03-28 13:29_

> ruff already reads .git/info/exclude, isn't it ?

It does, by using the `ignore` crate but I don't think we get that kind of information in the configuration discovery, so we would need to duplicate the logic or perform the check twice.

Anyway. I don't think we'll add support for this until there's wider community support for a centralized configuration folder. We don't want to support 10 or so different locations because supporting multiple locations has a performance and complexity (not just code, also for users) cost. I want to avoid a situation similar to prettier or eslint where Ruff has to test 10or so different locations (which not only hurts the performance of projects using `.config` but every project, see [this blog post](https://prettier.io/blog/2023/11/30/cli-deep-dive.html#resolving-configurations-fast))

I understand that this must be frustrating for you because having a `.config` folder only pays off once every tool you use supports it. 

How do you handle the `pyproject.toml` today or the configurations of other python tooling? Are they stored top-level or in the `.config` directory?

---

_Comment by @MichaReiser on 2024-04-02 08:40_

I'm going to close this for now because I don't see a way to implement it that is net positive for all users. I know that this must be unsatisfying for you because there's no good workaround to have all configurations in a centralized place, and am sorry for that

There have been similar requests in the past, e.g. adding https://github.com/astral-sh/ruff/issues/4970 that we've closed for similar reasons. We'll reconsider this when there's more alignment on a standardized configuration location.

---

_Closed by @MichaReiser on 2024-04-02 08:40_

---

_Comment by @bersace on 2024-04-02 09:10_

thanks for your attention on this issue. At least, the subject is discussed.

---
