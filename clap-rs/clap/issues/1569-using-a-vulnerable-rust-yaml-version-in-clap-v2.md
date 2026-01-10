---
number: 1569
title: using a vulnerable rust-yaml version in clap v2
type: issue
state: closed
author: Licenser
labels: []
assignees: []
created_at: 2019-10-08T12:15:39Z
updated_at: 2021-12-09T15:56:23Z
url: https://github.com/clap-rs/clap/issues/1569
synced_at: 2026-01-10T01:26:58Z
---

# using a vulnerable rust-yaml version in clap v2

---

_Issue opened by @Licenser on 2019-10-08 12:15_

clap depends on a version of rust-yaml that has a vulnerability - it would be nice to update to a newer version: https://rustsec.org/advisories/RUSTSEC-2018-0006

### Affected Version of clap

* 2.33.0



---

_Comment by @rlabrecque on 2019-10-10 05:28_

I just started using https://github.com/RustSec/cargo-audit via https://github.com/actions-rs/audit-check and clap is the only library causing issues because of rust-yaml. Would definitely be nice to get this updated.

---

_Comment by @CreepySkeleton on 2019-10-10 05:47_

`clap 3.0` gets this fixed in #1541 , so this issue is about backporting the update to 2.34. 

Also, I don't think the uncontrolled recursion case is possible with clap, so this is entirely about appeasing the audit gods :confused: 

---

_Comment by @Licenser on 2019-10-10 09:38_

In part yes, but it also removes the burden from maintainers to figure out that it's 'only' in clap and that clap isn't affected.

As a user I'm not certain where exactly in the clap workflow yaml files are read, so it's hard to decide if it's an actual issue or a false positive. I sure could dig through the clap codebase and find out, but that's quite a lot of work.

So it's not only appeasing the audit gods but making security easier for everyone, which is a big + :D

---

_Comment by @CreepySkeleton on 2019-10-10 15:03_

> As a user I'm not certain where exactly in the clap workflow yaml files are read, so it's hard to decide if it's an actual issue or a false positive. I sure could dig through the clap codebase and find out, but that's quite a lot of work.

Yaml files are an alternative to building the `clap::App` directly in your code. A developer of a CLI does it, those files never originate from a third party source. In fact, the maximum possible depth is about 10 for "no subcommands" CLI, every **nested** subcomman could add up to 10. I'm pretty sure there are not many apps out there that use mare than 4 nested subcommands.



> As a user I'm not certain where exactly in the clap workflow yaml files are read, so it's hard to decide if it's an actual issue or a false positive. I sure could dig through the clap codebase and find out, but that's quite a lot of work.

To be clear - I'm not saying that we shouldn't do it, I'm just pointing out that there's no practical way this could go sought today :smile: 

---

_Comment by @CreepySkeleton on 2019-10-10 15:26_

@Dylan-DPC So how do we do it? I suggest to create `v2-backports` branch based on [this commit](https://github.com/clap-rs/clap/commit/784524f7eb193e35f81082cc69454c8c21b948f7) and merge it there. Once done, you can release 2.34.0

---

_Comment by @Licenser on 2019-10-10 15:51_

No hurry on it :)

---

_Referenced in [tremor-rs/tremor-runtime#5](../../tremor-rs/tremor-runtime/issues/5.md) on 2019-10-11 07:55_

---

_Comment by @CreepySkeleton on 2020-02-01 11:48_

The situation: clap v2 depends on rust-yaml v0.3 and the fix is not backported to it. Blocked on it.

---

_Label `W: blocking on external` added by @CreepySkeleton on 2020-02-01 11:49_

---

_Comment by @CreepySkeleton on 2020-02-01 11:52_

Blocked on https://github.com/chyh1990/yaml-rust/issues/126

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-01 11:53_

---

_Referenced in [clap-rs/clap#1387](../../clap-rs/clap/issues/1387.md) on 2020-02-01 15:52_

---

_Referenced in [clap-rs/clap#1541](../../clap-rs/clap/pulls/1541.md) on 2020-02-01 18:23_

---

_Renamed from "using a voulnerable rust-yaml version" to "using a vulnerable rust-yaml version" by @pksunkara on 2020-02-01 18:33_

---

_Renamed from "using a vulnerable rust-yaml version" to "using a vulnerable rust-yaml version in clap v2" by @pksunkara on 2020-02-01 18:33_

---

_Label `T: vulnerability` added by @pksunkara on 2020-02-01 18:42_

---

_Referenced in [clap-rs/clap#1854](../../clap-rs/clap/issues/1854.md) on 2020-04-23 07:51_

---

_Comment by @CreepySkeleton on 2020-04-23 08:06_

To be fair this is not a vulnerability. Vulnerability in computing is defined as (emphasis mine)

> In computer security, a vulnerability is a weakness which **can be exploited by a threat actor**, such as an attacker, to perform unauthorized actions within a computer system. To exploit a vulnerability, **an attacker must have at least one applicable tool or technique that can connect to a system weakness**.

There's no such tool for an attacker because `yml` files are included at compile time via `include_str!`; it's a trusted input. I'm totally for fixing this - as soon as it's fixed upstream, but the `T: vulnerability` label is uncalled for.

---

_Label `T: vulnerability` removed by @CreepySkeleton on 2020-04-23 08:06_

---

_Comment by @ckaran on 2020-05-05 13:11_

@CreepySkeleton you're right that this isn't a true vulnerability, but @Licenser is **definitely** correct when he said:

> In part yes, but it also removes the burden from maintainers to figure out that it's 'only' in clap and that clap isn't affected.

False positives waste time and lead to fatigue, which can lead to people finding ways of turning off the warnings, which leads to true positives getting through.  So this needs to get fixed as quickly as possible, even if it isn't a vulnerability.

---

_Comment by @CreepySkeleton on 2020-05-05 14:01_

@ckaran Well yes, except [we can't fix it unless it's backported to yaml 0.3](https://github.com/clap-rs/clap/issues/1854#issuecomment-618245009), not really, because doing so would be a breaking change and potentially break god knows how many crates out there for **no** benefit. 

We _might_ be willing to publish a breaking change to fix a _real_ vulnerability, but this one does not qualify as such.

Come to think of it, clap is not the one to blame here, it does everything right. It's cargo-audit who emits a false-positive warning and causes the fatigue. If you ask me, an audit system must have a way to mark certain software as a known false positive and suppress the warning. Is there one?

---

_Comment by @ckaran on 2020-05-05 14:40_

> If you ask me, an audit system must have a way to mark certain software as a known false positive and suppress the warning. Is there one?

I don't think so, but I've just started using it.  @tarcieri, is there a way to mark a vulnerability as a false positive?

---

_Comment by @CreepySkeleton on 2020-05-05 14:47_

Clarification:  @tarcieri, is there a way to mark a specific vulnerability in a specific version or range of versions of a specific crate that depends on a vulnerable crate but never triggers the vulnerability as a false positive?

---

_Comment by @tarcieri on 2020-05-05 14:51_

The main way to flag vulnerabilities as false positives is either via a command line `--ignore` argument to `cargo audit` or [via its configuration](https://github.com/RustSec/cargo-audit/blob/master/audit.toml.example#L6).

Regarding transitive dependencies where the vulnerable code path is never called (which appears to be what's happening here?), detecting these scenarios is a long-time requested feature. We'd like to be able to do a call graph analysis. Here's a relevant issue:

https://github.com/RustSec/cargo-audit/issues/89

---

_Comment by @CreepySkeleton on 2020-05-05 15:14_

> Regarding transitive dependencies where the vulnerable code path is never called

Not quite. The allege vulnerability in question is unrestrained recursion that leads to stack overflow, and this code is indeed called here. But the input we ever feed to the yaml parser (the one with that triggers recursion) is **trusted** (is included via `include_str!` at compile time) and the nesting level is  never big enough to trigger the overflow, not even close. So yeah, it's pretty subtle, but not a vulnerability.

> The main way to flag vulnerabilities as false positives is either via a command line --ignore argument to cargo audit or via its configuration.

The problem with this workflow is that users have to see the warning first, then discover that the report in question is a false one, then individually mark it as false positive for themselves. This is the fatigue @ckaran was talking about and it won't go away until an automatic solution (from user's perspective) is developed.

---

_Comment by @tarcieri on 2020-05-05 15:28_

I'm afraid there aren't presently any good solutions for that other than publishing a release which is able to use transitive dependency which isn't in the vulnerable range.

---

_Comment by @kbknapp on 2020-05-05 15:50_

Just throwing my thoughts in the ring as well. I 100% agree with @CreepySkeleton here, there isn't much we can do unless `yaml_rust` backports the fix into a 0.3.x branch linked above. I think the clap team has put in a lot of effort here, as a clap maintainer has backported the fix to `yaml_rust` and opened the PR. Realistically the only thing we can do next is wait for the PR to be merged, or make a breaking change in clap. If this were an active vulnerability, I would be willing to make a breaking change in clap, but this isn't. 

Having said that, I *also* agree with the sentiment that it's difficult for users to go through the headache of finding out what deps are causing which errors, and whether or not it's a real vulnerability. However, I would caveat that with that's just part of dependency management. Without better tooling, that's just the state of things.

The only other solution I can think of at the moment is if we add a blurb in our docs for the affected API that says something to the effect of, "There is a known false positive when using `cargo-audit`, link to discussion, use this `cargo-audit` config to ignore the error."

---

_Comment by @ckaran on 2020-05-05 15:53_

> The only other solution I can think of at the moment is if we add a blurb in our docs for the affected API that says something to the effect of, "There is a known false positive when using cargo-audit, link to discussion, use this cargo-audit config to ignore the error."

Assuming that the PR for the backport is accepted soon, this might be the best answer.

---

_Comment by @tarcieri on 2020-05-05 16:03_

>  "There is a known false positive when using cargo-audit, link to discussion, use this cargo-audit config to ignore the error."

I'd also be fine with adding this same text to the advisory itself.

---

_Comment by @CreepySkeleton on 2020-05-05 16:04_

> The only other solution I can think of at the moment is if we add a blurb in our docs for the affected API that says something to the effect of, "There is a known false positive when using cargo-audit, link to discussion, use this cargo-audit config to ignore the error."

This kind of solution is unlikely to change anything because users get the warning from `cargo audit`. They don't even have to know that this API - and the attached note - exists in the dirst place. t\all they see is 
```
ID:       RUSTSEC-2018-0006
Crate:    yaml-rust
Version:  0.3.5
Date:     2018-09-17
URL:      https://rustsec.org/advisories/RUSTSEC-2018-0006
Title:    Uncontrolled recursion leads to abort in deserialization
Solution:  upgrade to >= 0.4.1
Dependency tree: 
yaml-rust 0.3.5
└── clap 2.33.0
```

Which doesn't mention anything about the relevant API.

In order to be useful, this attachment must be visible. I'd suggest put it in readme and docs.rs front page, somewhere near the top, and paint it **bold**. 

---

_Comment by @CreepySkeleton on 2020-05-05 16:04_

> I'd also be fine with adding this same text to the advisory itself.

_That_ would be marvelous.

---

_Comment by @ckaran on 2020-05-05 16:09_

> I'd also be fine with adding this same text to the advisory itself.

OK, so just so I'm 100% clear, this is going to be in `cargo audit`'s database itself, correct?  How do you handle a situation where a crate is using both `clap` and `rust-yaml`?  That is, this is not a vulnerability for `clap`, but for other uses it may be, so you want to make sure that you don't accidentally turn off all auditing against `rust-yaml` just because you happen to include clap in your project.


---

_Referenced in [rustsec/advisory-db#288](../../rustsec/advisory-db/issues/288.md) on 2020-05-05 16:11_

---

_Comment by @CreepySkeleton on 2020-05-05 16:14_

> so you want to make sure that you don't accidentally turn off all auditing against rust-yaml just because you happen to include clap in your project.

Right, it must be worded in a way that explicitly conveys that `clap` is NOT vulnerable, but `rust-yaml` IS.

---

_Comment by @tarcieri on 2020-05-05 16:14_

@ckaran this would just be in the description of the advisory. `cargo audit` shows a dependency tree of how crates with advisories are used in the project, allowing the user to see whether it's just `clap` pulling in `yaml-rust` or something else.

For what it's worth, I also opened a ticket about potentially doing some sort of structured whitelisting in advisories as well:

https://github.com/RustSec/advisory-db/issues/288

In that case, `cargo audit` could check the dependency tree automatically, rather than relying on the user to do it.

---

_Comment by @ckaran on 2020-05-05 16:17_

@tarcieri that's perfect!  The description can be a stopgap, and the ticket can be a future enhancement.

---

_Label `W: 2.x` removed by @pksunkara on 2021-05-26 10:59_

---

_Referenced in [clap-rs/clap#2506](../../clap-rs/clap/issues/2506.md) on 2021-05-28 08:01_

---

_Referenced in [mozilla/application-services#4611](../../mozilla/application-services/pulls/4611.md) on 2021-10-26 20:18_

---

_Referenced in [epage/clapng#127](../../epage/clapng/issues/127.md) on 2021-12-06 18:37_

---

_Comment by @epage on 2021-12-09 15:56_

clap3 is using a newer rust-yaml and is at rc0, so I'm going ahead and closing this in favor of that release.

---

_Closed by @epage on 2021-12-09 15:56_

---
