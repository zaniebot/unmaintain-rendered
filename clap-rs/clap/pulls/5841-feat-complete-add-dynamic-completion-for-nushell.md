```yaml
number: 5841
title: "feat(complete): Add dynamic completion for nushell"
type: pull_request
state: open
author: chklauser
labels: []
assignees: []
draft: true
base: master
head: nushell-dynamic-completion
created_at: 2024-12-11T23:31:58Z
updated_at: 2025-10-28T16:03:06Z
url: https://github.com/clap-rs/clap/pull/5841
synced_at: 2026-01-12T16:14:17Z
```

# feat(complete): Add dynamic completion for nushell

---

_@chklauser_

Adds an implementation of dynamic completion to `clap_complete_nushell` under the `unstable-dynamic` feature flag. Corresponding issue #5840. The issue description has more context and a more complete solution sketch. This PR doesn't implement the full solution sketched there. Ideally, we'd first agree that that solution is the way to go.

With this PR (and the `unstable-dynamic` feature flag), clap tools can generate a nushell module that offers three commands:
 - `complete`, which performs the actual completion
 - `handles`, which asks the module whether it is the correct  completer for a particular command line
 - `install`, which globally registers a completer that falls back to whatever completer was previously installed if `handles` rejects completing a command line.

The idea is that user's who already have a custom external completer can integrate the generated module's `handles` and `complete` commands into their completer.

Everyone else just puts
```nushell
use my-app-completer.nu
my-app-completer install
```
into their nushell config.

To test it, I have integrated it into a local build of the relatively complex clap-based tool, [JJ (Jujutsu)](https://github.com/martinvonz/jj), and it worked very well so far.

# TODO

- [ ] Consensus for solution
- [ ] Automated tests

---

_@epage reviewed on 2025-10-24 00:41_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:20 on 2025-10-24 00:41_

nit: prefer grouping by std, external, internal

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:82 on 2025-10-24 00:41_

nit: prefer newline between items

---

_@epage reviewed on 2025-10-24 00:41_

---

_@epage reviewed on 2025-10-24 00:42_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:81 on 2025-10-24 00:42_

Please put the top-level item at the top of the file

---

_@epage reviewed on 2025-10-24 00:43_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:81 on 2025-10-24 00:43_

For `clap_complete`, we defined separate types.  Is there a reason you haven't done that here?

---

_@epage reviewed on 2025-10-24 00:43_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:81 on 2025-10-24 00:43_

Also, not a fan of trait impls being separated from the type they are a part of

---

_@epage reviewed on 2025-10-24 00:44_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-24 00:44_

How come this is case ignoring and accepting both names?

---

_@epage reviewed on 2025-10-24 00:46_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:115 on 2025-10-24 00:46_

`args.len()` is a `usize`, so doing a `(usize - 1).max(0)`  isn't too meaningful.  You likely need `args.len().saturating_sub(1)`

---

_@epage reviewed on 2025-10-24 00:47_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:128 on 2025-10-24 00:47_

Why do we buffer before writing?

---

_@epage reviewed on 2025-10-24 00:48_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:98 on 2025-10-24 00:48_

Isn't that a `ModeVar(var).to_string()`? 

---

_@epage reviewed on 2025-10-24 00:49_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:99 on 2025-10-24 00:49_

Creating an internal environment variable in what is needing to be a public interface is something we'll need to consider more thoroughly

---

_@chklauser reviewed on 2025-10-27 20:38_

---

_Review comment by @chklauser on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-27 20:38_

The primary motivation was the [robustness principle](https://en.wikipedia.org/wiki/Robustness_principle) (and I'm well aware of the downsides/criticism of the principle). As a user of nu/nushell, I get the impression that they cannot make up their mind as to what the official name for their shell is. The binary and the repository are called `nu`, the website, social media accounts, etc. are all called `nushell`

So the question is: what do we want people to type `COMPLETE=nu their-clap-binary` (to match the name of the binary/command used to launch nushell) or do we want people to type `COMPLETE=nushell their-clap-binary` (to match the marketing name for nushell)? 

I personally tend towards `nushell`, but I don't have a very strong opinion.

---

_@epage reviewed on 2025-10-27 20:50_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-27 20:50_

> The primary motivation was the [robustness principle](https://en.wikipedia.org/wiki/Robustness_principle) (and I'm well aware of the downsides/criticism of the principle).

In terms of case insensitivity, that is best left to a wider conversation on how we should handle this for all shells while this PR conforms to our existing practices.

As for names, so far we only accept the file stem stored in `SHELL`

---

_@chklauser reviewed on 2025-10-27 20:55_

---

_Review comment by @chklauser on `clap_complete_nushell/src/dynamic.rs`:81 on 2025-10-27 20:55_

No, I simply didn't realize the types for dynamic completion were distinct from the aot ones. I'll create a separate type then.

---

_@chklauser reviewed on 2025-10-27 20:56_

---

_Review comment by @chklauser on `clap_complete_nushell/src/dynamic.rs`:128 on 2025-10-27 20:56_

The crate I used to serialize JSON [(write_json) only offers an API that takes a String](https://docs.rs/write-json/latest/write_json/fn.array.html). If you'd rather avoid this buffering, we can go look for a an alternative way to serialize JSON (without going full serde)

---

_@chklauser reviewed on 2025-10-27 21:03_

---

_Review comment by @chklauser on `clap_complete_nushell/src/dynamic.rs`:99 on 2025-10-27 21:03_

One alternative would be to re-use the `COMPLETE` variable. We could register two "shells" (or maybe dispatch based on the name used?)

`COMPLETE=nushell your-clap-tool` generates the auto-update code; `COMPLETE=nushell-registration your-clap-tool` generates the actual integration code. 

Seems a bit more of a hack on the one hand, but it would be safer in the sense that the two are mutually exclusive and in that we don't use some other, undocumented variable.

---

_@epage reviewed on 2025-10-27 21:04_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:128 on 2025-10-27 21:04_

Ugh.  We can probably pass for now.  Maybe we can eventually switch it to [json_write](https://docs.rs/json-write/latest/json_write/trait.JsonWrite.html) (which I maintain) as it matures.

---

_@epage reviewed on 2025-10-27 21:08_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:99 on 2025-10-27 21:08_

Yeah, I want to switch all completions to a two-step process for #5668 and have been wondering about doing something like that.  This is all unstable so we can change it over time so we don't need to block on it but we will eventually need to figure it out.

---

_@chklauser reviewed on 2025-10-27 21:16_

---

_Review comment by @chklauser on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-27 21:16_

I saw that the powershell integration also uses `powershell` (instead of the binary name `pwsh`) => I'll change it to just match `nushell` (case sensitive)

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-27 21:22_

That is again related to the `SHELL` lookup (#4447).  If that is incorrect, then that is a bug.

---

_@epage reviewed on 2025-10-27 21:22_

---

_@epage reviewed on 2025-10-28 15:58_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:87 on 2025-10-28 15:58_

To re-iterate differently, since `SHELL` would end up pointing to `nu`, that is the name we should use

---

_@epage reviewed on 2025-10-28 15:59_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:22 on 2025-10-28 15:59_

Please don't comment the groupings

---

_Review comment by @epage on `clap_complete_nushell/src/lib.rs`:31 on 2025-10-28 16:02_

Is this still needed?

---

_@epage reviewed on 2025-10-28 16:02_

---

_@epage reviewed on 2025-10-28 16:03_

---

_Review comment by @epage on `clap_complete_nushell/src/dynamic.rs`:26 on 2025-10-28 16:03_

Is there a reason you have all of these traits implemented?

---
