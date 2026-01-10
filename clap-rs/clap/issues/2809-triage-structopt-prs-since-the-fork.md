---
number: 2809
title: Triage Structopt PRs since the fork
type: issue
state: closed
author: epage
labels:
  - C-bug
  - C-enhancement
  - A-derive
assignees: []
created_at: 2021-10-04T19:17:45Z
updated_at: 2021-10-11T20:33:09Z
url: https://github.com/clap-rs/clap/issues/2809
synced_at: 2026-01-10T01:27:26Z
---

# Triage Structopt PRs since the fork

---

_Issue opened by @epage on 2021-10-04 19:17_

### Discussed in https://github.com/clap-rs/clap/discussions/2658

<div type='discussions-op-text'>

<sup>Originally posted by **epage** August  2, 2021</sup>
Dependabot notified me of a structopt update that included https://github.com/TeXitoi/structopt/pull/483.  This seems relevant for clap but made me wonder about the problem generally.

When was our last sync-up with structopt and how do we track it so things don't fall through the cracks for future changes?</div>

---

_Label `T: bug` added by @epage on 2021-10-04 19:17_

---

_Label `T: enhancement` added by @epage on 2021-10-04 19:17_

---

_Label `D: medium` added by @epage on 2021-10-04 19:17_

---

_Label `C: derive macros` added by @epage on 2021-10-04 19:17_

---

_Added to milestone `3.0` by @epage on 2021-10-04 19:17_

---

_Comment by @epage on 2021-10-04 20:56_

ea76fa1b1b273e65e3b0b1046643715b49bec51f is the last commit in  https://github.com/clap-rs/clap_derive/commits/master?after=acea444f962a0d7c8126273739d4df8bdb152624+34&branch=master from purely structopt.  It looks like some fixes went in since then but unsure which

Post fork, already ported
- [x] https://github.com/TeXitoi/structopt/pull/143
- [x] https://github.com/TeXitoi/structopt/pull/148
- [x] https://github.com/TeXitoi/structopt/pull/146
- [x] https://github.com/TeXitoi/structopt/pull/154
- [x] https://github.com/TeXitoi/structopt/pull/158
- [x] https://github.com/TeXitoi/structopt/pull/161
- [x] https://github.com/TeXitoi/structopt/commit/3d5f9bd063613dde84528624f7192c9eefb6ae21
- [x] https://github.com/TeXitoi/structopt/pull/169
- [x] https://github.com/TeXitoi/structopt/pull/186
- [x] https://github.com/TeXitoi/structopt/pull/190
- [x] https://github.com/TeXitoi/structopt/pull/191
- [x] https://github.com/TeXitoi/structopt/pull/192
- [x] https://github.com/TeXitoi/structopt/pull/193
- [x] https://github.com/TeXitoi/structopt/pull/194
- [x] https://github.com/TeXitoi/structopt/pull/195
- [x] https://github.com/TeXitoi/structopt/pull/204
- [x] https://github.com/TeXitoi/structopt/pull/198
- [x] https://github.com/TeXitoi/structopt/pull/216
- [x] https://github.com/TeXitoi/structopt/pull/213
- [x] https://github.com/TeXitoi/structopt/pull/218
- [x] https://github.com/TeXitoi/structopt/pull/221
- [x] https://github.com/TeXitoi/structopt/pull/225
- [x] https://github.com/TeXitoi/structopt/pull/231
- [x] https://github.com/TeXitoi/structopt/pull/234
- [x] https://github.com/TeXitoi/structopt/pull/236
- [x] https://github.com/TeXitoi/structopt/pull/239
- [x] https://github.com/TeXitoi/structopt/pull/244
- [x] https://github.com/TeXitoi/structopt/pull/246
- [x] https://github.com/TeXitoi/structopt/pull/248
- [x] https://github.com/TeXitoi/structopt/pull/250
- [x] https://github.com/TeXitoi/structopt/pull/252
- [x] https://github.com/TeXitoi/structopt/pull/260
- [x] https://github.com/TeXitoi/structopt/pull/271
- [x] https://github.com/TeXitoi/structopt/pull/275
- [x] https://github.com/TeXitoi/structopt/pull/279
- [x] https://github.com/TeXitoi/structopt/pull/278
- [x] https://github.com/TeXitoi/structopt/pull/282
- [x] https://github.com/TeXitoi/structopt/pull/284
- [x] https://github.com/TeXitoi/structopt/pull/281
- [x] https://github.com/TeXitoi/structopt/pull/287
- [x] https://github.com/TeXitoi/structopt/pull/290
- [x] https://github.com/TeXitoi/structopt/pull/293
- [x] https://github.com/TeXitoi/structopt/pull/302
- [x] https://github.com/TeXitoi/structopt/pull/296
- [x] https://github.com/TeXitoi/structopt/pull/313
- [x] https://github.com/TeXitoi/structopt/pull/312
- [x] https://github.com/TeXitoi/structopt/pull/325
- [x] https://github.com/TeXitoi/structopt/pull/334
- [x] https://github.com/TeXitoi/structopt/pull/314
- [x] https://github.com/TeXitoi/structopt/pull/334
- [x] https://github.com/TeXitoi/structopt/pull/346
- [x] https://github.com/TeXitoi/structopt/pull/342
- [x] https://github.com/TeXitoi/structopt/pull/381
- [x] https://github.com/TeXitoi/structopt/pull/399
- [x] https://github.com/TeXitoi/structopt/pull/400
- [x] https://github.com/TeXitoi/structopt/pull/401

Start of new port series
- [x] https://github.com/TeXitoi/structopt/pull/414, see #2821 
- [x] https://github.com/TeXitoi/structopt/pull/412, see https://github.com/clap-rs/clap/pull/2822
- [x] https://github.com/TeXitoi/structopt/pull/440, re-implemented in https://github.com/clap-rs/clap/pull/1992
- [x] https://github.com/TeXitoi/structopt/pull/448, see https://github.com/clap-rs/clap/pull/2823
- [x] https://github.com/TeXitoi/structopt/pull/450, https://github.com/clap-rs/clap/pull/2824
- [ ] https://github.com/TeXitoi/structopt/pull/483, split out into https://github.com/clap-rs/clap/issues/2769
- [x] https://github.com/TeXitoi/structopt/pull/491, see https://github.com/clap-rs/clap/pull/2825
- [x] https://github.com/TeXitoi/structopt/pull/494, see https://github.com/clap-rs/clap/pull/2826






---

_Comment by @epage on 2021-10-04 20:58_

@pksunkara and @CreepySkeleton since you two were involved a lot during the late stages of `clap_derive`, **if** its quick to do a scan to see what is already ported to `clap_derive`, that'd be appreciated.  If not, I'm fully willing to take on doing this deep dive.

---

_Comment by @epage on 2021-10-05 21:33_

Looks like I found the point of divergence.  We have everything up to 401 and need 414 and after

---

_Referenced in [clap-rs/clap#2821](../../clap-rs/clap/pulls/2821.md) on 2021-10-06 17:28_

---

_Referenced in [clap-rs/clap#2822](../../clap-rs/clap/pulls/2822.md) on 2021-10-06 17:34_

---

_Referenced in [clap-rs/clap#2823](../../clap-rs/clap/pulls/2823.md) on 2021-10-06 18:12_

---

_Referenced in [clap-rs/clap#2824](../../clap-rs/clap/pulls/2824.md) on 2021-10-06 18:15_

---

_Referenced in [clap-rs/clap#2825](../../clap-rs/clap/pulls/2825.md) on 2021-10-06 18:24_

---

_Referenced in [clap-rs/clap#2826](../../clap-rs/clap/pulls/2826.md) on 2021-10-06 18:52_

---

_Comment by @pksunkara on 2021-10-09 02:24_

- [ ] No coverage for [test](https://github.com/TeXitoi/structopt/pull/401/files#diff-57e8e1f836204e3d43fdd26c8c05d8aa25edc6db8c921f99f5d482c3f56e05a9R134)

---

_Referenced in [clap-rs/clap#2769](../../clap-rs/clap/issues/2769.md) on 2021-10-11 19:16_

---

_Comment by @epage on 2021-10-11 19:28_

>     * [ ]  No coverage for [test](https://github.com/TeXitoi/structopt/pull/401/files#diff-57e8e1f836204e3d43fdd26c8c05d8aa25edc6db8c921f99f5d482c3f56e05a9R134)

I'm not even sure what that is trying to test; I forgot what structopt does when `flatten`ing in that context.

As for `clap_derive`, we don't even allow an `enum` to `flatten` a `struct`, its a compiler error due to mismatched types.  Of the various combinations of bad `flatten` and `subcommand`, it looks like the only one we cover is
```rust
#[derive(clap::Parser)]
struct Opt {
    #[clap(flatten)]
    sub: SubCmd,
}

#[derive(clap::Parser)]
enum SubCmd {}

fn main() {}
```

I'm mixed on the value of expanding out the rest of the combinations but either way, don't think it would block 3.0.

---

_Comment by @epage on 2021-10-11 19:29_

I'm delegating the generics work to https://github.com/clap-rs/clap/issues/2769 which means we are done or practically done with this.  Rather than this lingering on for each new issue, I'm going to go ahead and close it.  We can create new issues for each additional structopt porting effort and prioritize accordingly.

---

_Closed by @epage on 2021-10-11 19:29_

---

_Comment by @pksunkara on 2021-10-11 20:33_

> As for clap_derive, we don't even allow an enum to flatten a struct, its a compiler error due to mismatched types.

Yeah, we need an UI test for that.

---

_Referenced in [clap-rs/clap#2850](../../clap-rs/clap/pulls/2850.md) on 2021-10-11 20:55_

---
