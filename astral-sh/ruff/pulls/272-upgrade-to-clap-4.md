```yaml
number: 272
title: Upgrade to clap 4
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clap-4
created_at: 2022-09-28T20:49:22Z
updated_at: 2022-09-29T02:18:12Z
url: https://github.com/astral-sh/ruff/pull/272
synced_at: 2026-01-12T15:55:04Z
```

# Upgrade to clap 4

---

_@andersk_

https://epage.github.io/blog/2022/09/clap4/

---

_Comment by @charliermarsh on 2022-09-28 21:11_

This rules! Thank you.

---

_Merged by @charliermarsh on 2022-09-28 21:11_

---

_Closed by @charliermarsh on 2022-09-28 21:11_

---

_Branch deleted on 2022-09-28 21:12_

---

_Review comment by @epage on `src/main.rs`:35 on 2022-09-29 00:48_

If you remove this line, clap will automatically fill in `name = CARGO_PKG_NAME` for you

---

_Review comment by @epage on `src/main.rs`:39 on 2022-09-29 00:48_

irc that value hint is now the default for `PathBuf` automatically

---

_Review comment by @epage on `src/main.rs`:61 on 2022-09-29 00:50_

How come you accept both `--select code --select code` and `--select code code`?

Normally appls just accept `--select code,code --select code` instead.

---

_Review comment by @epage on `src/main.rs`:90 on 2022-09-29 00:50_

btw `action` is now enabled automatically for you, so you don't need the bald `action` attribute anymore.  Run `cargo check -F clap/deprecated` and it *should* tell you that.  Won't be a problem until clap v5 though

---

_@epage reviewed on 2022-09-29 00:51_

---

_@charliermarsh reviewed on 2022-09-29 00:59_

---

_Review comment by @charliermarsh on `src/main.rs`:61 on 2022-09-29 00:59_

I'd like to only accept `--select code+`, is that not what's happening here?

---

_@charliermarsh reviewed on 2022-09-29 01:01_

---

_Review comment by @charliermarsh on `src/main.rs`:61 on 2022-09-29 01:01_

(As in: either `--select F401` or `--select F401 E501` should be valid .)

---

_@epage reviewed on 2022-09-29 01:02_

---

_Review comment by @epage on `src/main.rs`:61 on 2022-09-29 01:02_

Could you write that out further to make sure I understand?

---

_@charliermarsh reviewed on 2022-09-29 01:04_

---

_Review comment by @charliermarsh on `src/main.rs`:61 on 2022-09-29 01:04_

Yes, sorry. The intent is that either `--select F401` or `--select F401 E501` should be accepted. `--select F401 --select E501` shouldn't be though (and neither should `--select F401,E501`).

Though, regardless of whether that aligns with the _actual_ behavior here, perhaps that intent is misguided?


---

_@andersk reviewed on 2022-09-29 01:16_

---

_Review comment by @andersk on `src/main.rs`:61 on 2022-09-29 01:16_

I note that `flake8` accepts `--select F401,E501` and not `--select F401 E501` or `--select F401 --select E501`.

The `--select F401 E501` syntax confused me at first because it prevents you from writing `ruff --select F401 file.py`. (You can write `ruff --select F401 -- file.py`, but nobody expects to need that.)

Generally UNIX options only ever consume a known number of arguments, and this is why.

---

_@epage reviewed on 2022-09-29 02:01_

---

_Review comment by @epage on `src/main.rs`:61 on 2022-09-29 02:01_

`--select F401 E501` without multiple occurrences would requiring overriding the default `ArgAction` for `Vec` (`Append`) with `action = ArgAction::Set`.

And yes, this general leads to ambiguities in parsing which is why we [place warnings around it](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.num_args).

I would recommend instead `#[arg(long, value_delimiter = ",")]` or `#[arg(long, value_delimiter = ",", action = ArgAction::Set)]`, depending on if your want to allow multiple occurrences.

---

_@charliermarsh reviewed on 2022-09-29 02:18_

---

_Review comment by @charliermarsh on `src/main.rs`:61 on 2022-09-29 02:18_

Thank you both, much appreciated + I'm convinced. Changed in #278.

---
