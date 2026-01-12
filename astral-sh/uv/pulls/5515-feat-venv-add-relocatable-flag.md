```yaml
number: 5515
title: "feat(venv): add relocatable flag"
type: pull_request
state: merged
author: paveldikov
labels:
  - enhancement
assignees: []
merged: true
base: main
head: relocatable
created_at: 2024-07-28T14:19:01Z
updated_at: 2024-07-30T20:59:40Z
url: https://github.com/astral-sh/uv/pull/5515
synced_at: 2026-01-12T16:06:51Z
```

# feat(venv): add relocatable flag

---

_@paveldikov_

## Summary

Adds a `--relocatable` CLI arg to `uv venv`. This flag does two things:

* ensures that the associated activation scripts do not rely on a hardcoded
  absolute path to the virtual environment (to the extent possible; `.csh` and
  `.nu` left as-is)
* persists a `relocatable` flag in `pyvenv.cfg`.

The flag in `pyvenv.cfg` in turn instructs the wheel `Installer` to create script
entrypoints in a relocatable way (use `exec` trick + `dirname $0` on POSIX;
use relative path to `python[w].exe` on Windows).

Fixes: #3863

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

* Relocatable console scripts covered as additional scenarios in existing test cases.
* Integration testing of boilerplate generation in `venv`.
* Manual testing of `uv venv` with and without `--relocatable`

<!-- How was it tested? -->


---

_@samypr100 reviewed on 2024-07-28 16:53_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1760 on 2024-07-28 16:53_

Likely worth marking this as a preview/experimental flag

---

_@paveldikov reviewed on 2024-07-28 17:00_

---

_Review comment by @paveldikov on `crates/uv-cli/src/lib.rs`:1760 on 2024-07-28 17:00_

would that be a simple `hide = true`, or is there a special annotation for that? (couldn't find one)

---

_Comment by @charliermarsh on 2024-07-28 17:08_

This has a little bit of overlap with #5509 which just does the installer piece and isn't exposed to users.

---

_Comment by @charliermarsh on 2024-07-28 17:13_

One potential issue here is that users can't use `relocatable` with `--target` or `--prefix`, since those commands don't operate within a virtualenv 🤔 

---

_Comment by @paveldikov on 2024-07-28 17:17_

> This has a little bit of overlap with #5509 which just does the installer piece and isn't exposed to users.

Yes, I see that! Feel free to cherry-pick pieces out of this as you see fit (and vice versa -- let me know if I am straying too far away from the direction you had in mind)

---

_Comment by @paveldikov on 2024-07-28 17:24_

> One potential issue here is that users can't use `relocatable` with `--target` or `--prefix`, since those commands don't operate within a virtualenv 🤔

I thought `--python` was vastly more recommended for this very reason?

---

_Marked ready for review by @paveldikov on 2024-07-28 17:27_

---

_@charliermarsh reviewed on 2024-07-28 17:28_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:134 on 2024-07-28 17:28_

Does this assume that the relative path doesn't contain multiple segments? (That's true for virtual environments, but could be false in general, right?)

---

_Comment by @charliermarsh on 2024-07-28 17:29_

> Yes, I see that! Feel free to cherry-pick pieces out of this as you see fit (and vice versa -- let me know if I am straying too far away from the direction you had in mind)

Sounds good! Ultimately pretty similar, I'll likely merge yours first.

---

_Review comment by @paveldikov on `crates/install-wheel-rs/src/wheel.rs`:134 on 2024-07-28 17:30_

On second thought, this check is probably unnecessary and I might just pass the already-known `is_relocatable` as an argument, rather than trying to reverse-engineer it.

---

_@paveldikov reviewed on 2024-07-28 17:30_

---

_Comment by @charliermarsh on 2024-07-28 17:32_

We might want to add some tests by wiring this up through `TestContext` (perhaps with a new constructor to make it easiest).

---

_Comment by @paveldikov on 2024-07-28 17:35_

> We might want to add some tests by wiring this up through `TestContext` (perhaps with a new constructor to make it easiest).

I'll try to dig a little deeper tonight, but if you have examples of similar tests handy, that would be grand!

---

_Comment by @charliermarsh on 2024-07-28 17:37_

@paveldikov -- A basic test would be like `install_many` in `pip_sync.rs`. I think we just want light coverage for, like: installing and importing a package; installing and running a script; moving the virtualenv (even just renaming) and doing the same.

---

_@paveldikov reviewed on 2024-07-28 17:37_

---

_Review comment by @paveldikov on `crates/install-wheel-rs/src/wheel.rs`:134 on 2024-07-28 17:37_

Refactored.

---

_Comment by @charliermarsh on 2024-07-28 17:37_

You can run `cargo test -p uv --test pip_sync install_many` to run an individual test.

---

_@samypr100 reviewed on 2024-07-28 17:55_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1760 on 2024-07-28 17:55_

I was thinking using `PreviewMode` for this in `venv_impl`

---

_@paveldikov reviewed on 2024-07-28 19:01_

---

_Review comment by @paveldikov on `crates/uv-cli/src/lib.rs`:1760 on 2024-07-28 19:01_

Added warning (assuming that's what you meant?)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-28 19:04_

---

_Review comment by @samypr100 on `crates/uv-cli/src/lib.rs`:1760 on 2024-07-28 19:04_

Yes, thanks!

---

_@samypr100 reviewed on 2024-07-28 19:04_

---

_Comment by @paveldikov on 2024-07-28 19:06_

Added a test case for the boilerplate on the `venv` side. Let me know if there are other areas in the critical path that you'd like to see covered -- I've mostly focussed on the two extremities (`venv` and `installer`) since this is where most of the action is happening.

edit: woops, saw that you'd asked for
* covering package install scenario
* check if renaming venv base dir makes entrypoint still work

Going to add those in. Does any of the already-existing stub packages in `scripts/` contain meaningful console_scripts?

---

_Comment by @paveldikov on 2024-07-28 20:36_

Added coverage for `pip_install`.

>installing and importing a package; installing and running a script; moving the virtualenv (even just renaming) and doing the same.

Unless I'm reading this wrong, running a console script also implies importing the package, so I didn't handle that separately.

---

_Review requested from @charliermarsh by @paveldikov on 2024-07-28 20:39_

---

_Comment by @charliermarsh on 2024-07-28 20:46_

Thank you! Haven't reviewed yet but per your last question, I believe `black_editable` has an entrypoint at least.

---

_Comment by @paveldikov on 2024-07-28 20:47_

> I believe black_editable has an entrypoint at least.

Yep, I did find it and used it specifically for its Hello World :-P

---

_Comment by @charliermarsh on 2024-07-28 23:24_

Cool, this generally looks good to me! I'm going to make a few small tweaks to make it a bit easier to rebase #5509, but not anticipating any major changes. Thank you for the tests too.

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:146 on 2024-07-28 23:27_

What's the benefit of this over just `dirname $0` alone as in https://github.com/astral-sh/uv/pull/5509/files#diff-c1686969b46b2c133e184a8c25069ada51363d91354e35fac208bb67239fcf2cR183? (I have no idea!)

---

_@charliermarsh reviewed on 2024-07-28 23:27_

---

_@charliermarsh approved on 2024-07-28 23:50_

---

_Label `enhancement` added by @charliermarsh on 2024-07-29 00:01_

---

_@charliermarsh reviewed on 2024-07-29 00:04_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:40 on 2024-07-29 00:04_

I moved these next to each other because part of me wishes that it were on `Layout` (though I don't think creating `layout` should require querying the filesystem, so leaving them separate).

---

_Merged by @charliermarsh on 2024-07-29 00:10_

---

_Closed by @charliermarsh on 2024-07-29 00:10_

---

_Comment by @charliermarsh on 2024-07-29 00:11_

Thanks!

---

_@samypr100 reviewed on 2024-07-29 03:07_

---

_Review comment by @samypr100 on `crates/install-wheel-rs/src/wheel.rs`:146 on 2024-07-29 03:07_

Seems like there's a check if the directory exists (avoiding readlink/realpath friends) before substituting it with PWD which makes this a bit more cross-posix-platform and handle a couple error scenarios. Interestingly enough, I'd expect the `/` to be inside the echo though if this was the intention? Unless it's expected if cd fails that the output is `"/"` instead of `""`. Thoughts?

---

_@samypr100 reviewed on 2024-07-29 03:08_

---

_Review comment by @samypr100 on `crates/uv-virtualenv/src/activator/activate.bat`:22 on 2024-07-29 03:08_

This seems to be for expanding a relative path in command prompt since the previous command didn't, awesome!

---

_@paveldikov reviewed on 2024-07-29 07:40_

---

_Review comment by @paveldikov on `crates/install-wheel-rs/src/wheel.rs`:146 on 2024-07-29 07:40_

> Seems like there's a check if the directory exists (avoiding readlink/realpath friends) before substituting it with PWD which makes this a bit more cross-posix-platform and handle a couple error scenarios.

Yes, that's exactly it, `readlink -f` and `realpath` are a real POSIX-feature-availability-matrix nightmare, so that's what the `CDPATH= cd -- SOME_PATH && echo "$PWD"` part is -- a cross-platform way of doing `realpath SOME_PATH`.

The `--` bits in the middle are there to ensure that `$0` is parsed as a _positional argument_ (and never as an _option_), just in case decided to call their console script `-myscript` ;^)

> Interestingly enough, I'd expect the / to be inside the echo though if this was the intention? Unless it's expected if cd fails that the output is "/" instead of "". Thoughts?

There was no intention behind the slash's positioning, to be honest -- if the `echo` fails we are in undefined territory -- I think I really just placed it according to my aesthetic whims! Perhaps a `set -eo pipefail` could further harden this boilerplate to ensure that it fails fast? (need to check if this is portable, though)

I guess one might argue that resolving the path down to absolute is not needed in the first place! I guess that would strip much of the complexity off, leaving you with `r#""$(dirname -- "$0")/""#`.

---

_@paveldikov reviewed on 2024-07-29 07:46_

---

_Review comment by @paveldikov on `crates/install-wheel-rs/src/linker.rs`:40 on 2024-07-29 07:46_

I did originally put this inside of `Layout`, but for some reason decided to back out of that.

> (though I don't think creating layout should require querying the filesystem, so leaving them separate).

Could use the `layout.as_relocatable()` pattern? (only mark it as relocatable if specifically requested by the caller)

---

_Comment by @chrisrodrigue on 2024-07-30 09:47_

Please excuse my ignorance… does this mean that portable virtual environments are possible now?

---

_Comment by @paveldikov on 2024-07-30 20:59_

> Please excuse my ignorance… does this mean that portable virtual environments are possible now?

It depends -- a lot of people, myself included, would be wary of using the word 'portable'. (it could imply factors such as host OS or system architecture, in which case: no, these environments are defintely not portable.) But on a high level, yes, _assuming_ a homogenised host environment, these virtual environments can be shipped around from one host to another with most (if not all) functionality being retained.

(also, this is a preview feature, so I imagine it is probably premature to talk of it as a done deal. Matter of fact, I just found a bug with the activate script on `ksh`, which I am now to fix!)

---
