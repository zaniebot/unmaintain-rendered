---
number: 174
title: Consider auto-excluding files matched by gitignore files
type: issue
state: closed
author: Jackenmen
labels: []
assignees: []
created_at: 2022-09-13T00:59:27Z
updated_at: 2022-12-14T21:56:26Z
url: https://github.com/astral-sh/ruff/issues/174
synced_at: 2026-01-07T13:12:14-06:00
---

# Consider auto-excluding files matched by gitignore files

---

_Issue opened by @Jackenmen on 2022-09-13 00:59_

Patterns would need to be read from:
- `.gitignore` file in the same directory as the path, or in any parent directory (the latter allows tools to put a `.gitignore` file with `*` in the directory they create to auto-exclude themselves)
- patterns read from `$GIT_DIR/info/exclude` (allows user-specific patterns for a specific repository)
- patterns read from the file specified by Git's configuration variable `core.excludesFile` (allows global user-specific patterns, I personally use this one)

Reference: https://git-scm.com/docs/gitignore

This might come at a performance cost but it definitely would be a useful feature.

---

_Label `enhancement` added by @charliermarsh on 2022-09-13 01:20_

---

_Referenced in [astral-sh/ruff#176](../../astral-sh/ruff/issues/176.md) on 2022-09-13 02:03_

---

_Comment by @charliermarsh on 2022-09-15 14:24_

(Working on this.)

---

_Referenced in [astral-sh/ruff#202](../../astral-sh/ruff/pulls/202.md) on 2022-09-15 15:02_

---

_Comment by @sgryjp on 2022-09-17 23:29_

Hi! I thought using [`ignore`](https://docs.rs/ignore/latest/ignore/) crate instead of `walkdir`+`gitignore` can be an option. The crate is used inside very popular and performant tools such as [ripgrep](https://github.com/BurntSushi/ripgrep/tree/13.0.0/crates/ignore) and [fd](https://github.com/sharkdp/fd/blob/v8.4.0/Cargo.toml#L41) so we can expect reliability and performance at the same time. It also allow us to choose UNLICENSE so you will have no license conflicts. Could you consider using it?

---

_Referenced in [astral-sh/ruff#232](../../astral-sh/ruff/issues/232.md) on 2022-09-20 14:32_

---

_Comment by @messense on 2022-11-26 03:44_

+1 for this.

As for using the `ignore` crate, IMO it should work generally, but be aware that it's not 100% compatible with git ignore, see https://github.com/PyO3/maturin/issues/885#issuecomment-1112108447

---

_Comment by @Jackenmen on 2022-12-04 13:11_

I like that the `ignore` crate has all of the functionality I described in the issue description built-in - it supports `.gitignore` files at all directory levels of the repository, supports `.git/info/exclude`, *and* supports global gitignore!
> There are many rules that influence whether a particular file or directory is skipped by this iterator. Those rules are documented here. Note that the rules assume a default configuration.
> [...]
> - Second, ignore files are checked. Ignore files currently only come from git ignore files (`.gitignore`, `.git/info/exclude` and the configured global gitignore file), plain `.ignore` files, which have the same format as gitignore files, or explicitly added ignore files. The precedence order is: `.ignore`, `.gitignore`, `.git/info/exclude`, global gitignore and finally explicitly added ignore files. Note that precedence between different types of ignore files is not impacted by the directory hierarchy; any .ignore file overrides all .gitignore files. Within each precedence level, more nested ignore files have a higher precedence than less nested ignore files.
> [...]

https://docs.rs/ignore/latest/ignore/struct.WalkBuilder.html#ignore-rules

(reading of `.ignore` files can be disabled separately from the other functionality in case it's something you don't want)

---

_Referenced in [astral-sh/ruff#777](../../astral-sh/ruff/issues/777.md) on 2022-12-04 13:30_

---

_Comment by @charliermarsh on 2022-12-04 14:50_

Definitely happy to support this! I just haven't personally prioritized the work since _most_ exclusions are doable with the current configuration settings, even if less convenient. But we should do it.

---

_Comment by @Jackenmen on 2022-12-04 14:59_

Personally I'm mostly missing a way to ignore files only on my system, in projects where I don't have significant enough stake to change the exclusions they've set up. At best I could maybe make alias `ruff` to `ruff --extend-exclude files,folders` or something like that but that probably overrides the configuration from the pyproject.toml so it wouldn't really work universally across all projects. Meanwhile with ignore files I can either put a file/directory in global gitignore, make sure that there's `.gitignore` with `*` in a directory I want to ignore, or I can put a file/directory in `.git/info/exclude`.

---

_Comment by @jhallard on 2022-12-13 22:28_

Just a slight bump on this, we've had some issues sneak in with the pyproject.toml and .gitignore lists getting out of sync which caused us to accidentally lint a GB or so of third party code, would be convenient if those automatically sync'd. 

---

_Comment by @charliermarsh on 2022-12-13 23:20_

(I still want to explore this, and do so via the ignore crate. Just need to find time.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-13 23:51_

---

_Comment by @charliermarsh on 2022-12-13 23:51_

I'll give this a try (but my newborn just came home today so bear with me :))

---

_Comment by @jhallard on 2022-12-13 23:52_

@charliermarsh 1. Congratulations!! 🎉 2. you're a beast. We're not blocked on this by any means so take care of yourself! 

---

_Comment by @charliermarsh on 2022-12-14 00:09_

@jhallard - Thank you so much! I just couldn't help but share :)

---

_Referenced in [astral-sh/ruff#1234](../../astral-sh/ruff/pulls/1234.md) on 2022-12-14 04:26_

---

_Closed by @charliermarsh on 2022-12-14 20:58_

---

_Comment by @charliermarsh on 2022-12-14 21:56_

This is going out in the next release: https://github.com/charliermarsh/ruff/pull/1234.

The behavior is documented in the README (and in `BREAKING_CHANGES.md`), and there's an opt-out setting (`respect-gitignore = false`).


---
