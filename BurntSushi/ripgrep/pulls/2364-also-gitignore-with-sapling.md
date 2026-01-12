```yaml
number: 2364
title: Also gitignore with Sapling
type: pull_request
state: closed
author: lf-
labels: []
assignees: []
base: master
head: pr2362
created_at: 2022-11-29T00:27:51Z
updated_at: 2022-11-29T17:49:44Z
url: https://github.com/BurntSushi/ripgrep/pull/2364
synced_at: 2026-01-12T18:23:14Z
```

# Also gitignore with Sapling

---

_@lf-_

Stack created with [Sapling](https://sapling-scm.com). Best reviewed with [ReviewStack](https://reviewstack.dev/BurntSushi/ripgrep/pull/2364).
* __->__ #2364

Also gitignore with Sapling

Sapling is a [new SCM system](https://sapling-scm.com), which has some
interesting properties, including Git compatibility. However, it uses a
different directory for repo state, `.sl`. Thus, ripgrep doesn't
gitignore by default in Sapling repos, even though Sapling does.



---

_Comment by @BurntSushi on 2022-11-29 12:36_

> including Git compatibility

I can't find [any docs](https://sapling-scm.com/docs/introduction/) to support this. It looks like Sapling is its own thing with [many crucial differences from `git`](https://sapling-scm.com/docs/introduction/differences-git). For ripgrep's case, I need to _at least_ be able to see some documentation on how its ignore rules work. But I can't find any. That to me, all by itself, says that it is premature to add Sapling support to ripgrep.

Even aside from that, this is the first I'm hearing about Sapling. I'm not opposed in principle to adding Sapling support to ripgrep, but the cost to adding it is not free. Aside from additional maintenance in a quite thorny area of the code that lacks SCM abstractions, there are also actual runtime costs too. Adding support for Sapling means checking for a `.sl` entry under every directory, and those calls add up. More to the point, this PR does not add `.sl` checks everywhere there are `.git` checks.

> Thus, ripgrep doesn't gitignore by default in Sapling repos, even though Sapling does.

If Sapling's ignore files are indeed fully equivalent to gitignore files, then you can likely get some mileage out of `rg --no-require-git`. That flag should cause ripgrep to respect `.gitignore` files even if ripgrep can't find a `.git` entry.

I'm going to close this PR for now because I don't see any path to merging this or something similar in the immediate future. If you'd like to continue discussion, I'd recommend opening an issue.

---

_Closed by @BurntSushi on 2022-11-29 12:36_

---

_Comment by @lf- on 2022-11-29 17:00_

> I can't find [any docs](https://sapling-scm.com/docs/introduction/) to support this. It looks like Sapling is its own thing with [many crucial differences from git](https://sapling-scm.com/docs/introduction/differences-git).

This is correct: the git compatibility is at the remote level, not at the on-disk level (see link below; it's elaborated a little in a section on there). I should have clarified. 

One of the changes they made is *removing* hgignore support and only supporting gitignore. I expect that any gitignore incompatibility is considered a bug accordingly. https://sapling-scm.com/docs/internals/internal-difference-hg#ignore-files

I made this PR under the false assumption (for some reason) that the config file for ripgrep doesn't work for my use cases, because I *saw* the --no-require-git option but somehow thought that applying it everywhere with the config file wasn't really possible. But I agree it's reasonable for now to just require anyone using Sapling to set that in the config file. 

I ran into more tools looking for .git yesterday anyway: Nix is a bad offender, requiring an index for some kind of supported VCS to do untracked file filtering on flakes. So I wind up needing to make a fake git repo in each directory, a practice which will continue for the foreseeable future. 

---

_Comment by @BurntSushi on 2022-11-29 17:49_

> I expect that any gitignore incompatibility is considered a bug accordingly.

Aye. This is definitely a requirement before adding Sapling to ripgrep. It needs to be part of the project's documentation and presented as definable behavior. Note too that gitignore support isn't just limited to the interpretation of globs within the file, but also how multiple gitignore files are combined together. There's also things like a global gitignore file and checkout-specific gitignores in `.git/info/excludes`.

The other requirement I have here---although is perhaps a bit more fungible---is that there's some critical mass of people actually using Sapling. Otherwise, everyone has to pay for the `.sl` check. This could possibly be ameliorated by putting Sapling support behind a flag, but I am loathe to treat the addition of flags as nearly-free. There is basically an unending list of niche cases that people want flags for unfortunately.

---
