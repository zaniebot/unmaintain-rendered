---
number: 8004
title: "Add an argument which makes `uv pip compile` consider installed packages"
type: issue
state: open
author: antony-frolov
labels: []
assignees: []
created_at: 2024-10-08T14:13:23Z
updated_at: 2024-10-08T20:56:00Z
url: https://github.com/astral-sh/uv/issues/8004
synced_at: 2026-01-07T13:12:17-06:00
---

# Add an argument which makes `uv pip compile` consider installed packages

---

_Issue opened by @antony-frolov on 2024-10-08 14:13_

Hi! I'm trying to use `uv pip compile` to lock my dependencies for a docker image build. I've got a package already installed in my base image which is not present in any package index, so I need the dependency resolver to consider installed packages.

An option which makes `compile` consider installed packages when resolving dependencies would solve that problem.

I've already tried to hack `compile.rs` to consider installed packages [here](https://github.com/astral-sh/uv/compare/main...antony-frolov:uv:main) and my lock worked just fine!

I've created a similar [issue](https://github.com/jazzband/pip-tools/issues/2123) in `jazzband/pip-tools` a while ago but is seems like it's development is stale, so I'm happy to use `uv` instead.

P.S.: I'm very new to Rust so I'm afraid I won't be able to submit a ready to ship PR myself

---

_Comment by @notatallshaw-gts on 2024-10-08 14:58_

What would be the difference between this and `uv pip install --dry-run`?

Which considers what is installed and gives you the output of what changed.

---

_Comment by @antony-frolov on 2024-10-08 15:38_

> What would be the difference between this and `uv pip install --dry-run`?

@notatallshaw-gts, it seems like indeed `uv pip install --dry-run` does about the thing I need. Though the output format of `uv pip compile` seem much more friendly.

Had to something like this to get a working `requirements.txt`:
```bash
uv pip install -c constraints.txt -r requirements.in --dry-run --system 2>&1 \
        | grep "+ " | sed "s/+ //g" > requirements.txt
```

Also no comments from `uv pip install --dry-run` about dependency resolution and no option to emit index-urls (which would be convenient).

I do think though that my case is not uncommon and `uv pip compile` is more convenient for such dependency locking use cases.



---

_Comment by @notatallshaw-gts on 2024-10-08 15:45_

Yeah, pip has a `--report` option to make the output of `--dry-run` more parsable. I guess uv assumes you will use the "pip compile" option.

The only thing about a flag for "pip compile" that includes the current environment is it would, presumably, be incompatible with options that let you resolve things that aren't your environment, e.g. different Python version, different platform, universal resolution etc.

So to me it would make more sense to have `--report` options for install with `--dry-run`. But maybe uv team feel differently.

---

_Comment by @antony-frolov on 2024-10-08 20:49_

Actually, it feels like with `pip install` I can't as well do a single package upgrade for a locked environment (which I can do with `pip compile` using `--upgrade-package` with an output-file already present). If there is a way that I'm missing please share!

---

_Comment by @notatallshaw-gts on 2024-10-08 20:55_

It's a little involved, but you could use constraints:

```
uv pip freeze --exclude-editable | grep -v '{package_to_upgrade}=='  > locked_versions.txt
uv pip install --dry-run {package_to_upgrade} --upgrade -c locked_versions.txt
```

---
