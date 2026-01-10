---
number: 1597
title: Allow changing SUBCOMMAND placeholder in usage
type: issue
state: closed
author: clarfonthey
labels:
  - S-waiting-on-decision
assignees: []
created_at: 2019-11-10T02:19:56Z
updated_at: 2020-08-11T11:39:31Z
url: https://github.com/clap-rs/clap/issues/1597
synced_at: 2026-01-10T01:26:59Z
---

# Allow changing SUBCOMMAND placeholder in usage

---

_Issue opened by @clarfonthey on 2019-11-10 02:19_

<!-- Issuehunt Badges -->
[<img alt="Issuehunt badges" src="https://img.shields.io/badge/IssueHunt-%240%20Funded-%2300A156.svg" />](https://issuehunt.io/r/clap-rs/clap/issues/1597)
<!-- /Issuehunt Badges -->


(Note: I'm using v2 right now, but I believe that v3 doesn't change this.)

Basically, the usage will use the placeholder `<SUBCOMMAND>` or `[SUBCOMMAND]` depending on whether the subcommand is used, and it won't let you change this. Sometimes, it may actually be more beneficial to change this, e.g.:

```
USAGE:
    util list <THING>
```

as opposed to:

```
USAGE:
    util list <SUBCOMMAND>
```


<!-- Issuehunt content -->

---

<details>
<summary>
<b>IssueHunt Summary</b>
</summary>


### Backers (Total: $0.00)



#### [Become a backer now!](https://issuehunt.io/r/clap-rs/clap/issues/1597)
#### [Or submit a pull request to get the deposits!](https://issuehunt.io/r/clap-rs/clap/issues/1597)
### Tips

- Checkout the [Issuehunt explorer](https://issuehunt.io/r/clap-rs/clap/) to discover more funded issues.
- Need some help from other developers? [Add your repositories](https://issuehunt.io/r/new) on IssueHunt to raise funds.
</details>
<!-- /Issuehunt content-->

---

_Comment by @CreepySkeleton on 2020-02-01 11:20_

@clarfon Do you have some concrete design in mind?

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-01 11:20_

---

_Label `W: maybe` added by @CreepySkeleton on 2020-02-01 11:20_

---

_Referenced in [clap-rs/clap#1459](../../clap-rs/clap/issues/1459.md) on 2020-02-01 14:21_

---

_Label `C: usage strings` added by @pksunkara on 2020-02-01 19:39_

---

_Comment by @clarfonthey on 2020-02-02 00:25_

I would, but I haven't taken a proper look at the changes for 3.0 and don't know where it would properly integrate with that. I was under the assumption it'd just be a property on the thing that accepts subcommands.

---

_Comment by @pksunkara on 2020-02-02 05:49_

That sounds correct.

---

_Comment by @jsdw on 2020-02-10 07:37_

I ran into a desire for this as well using `structopt`. My use case is that I want to require a certain value from a limited set of options (eg "foo", "bar", "lark") and in the case that one of those values is provided ("foo", for instance) I need some other config as well.

A more concrete example using `structopt` (though any such changes would need to happen here first I'd assume):

```
#[derive(Debug, Clone, StructOpt)]
struct Opts {
    /// General arg 1
    #[structopt(long)]
    apple: u16,

    /// General arg 2
    #[structopt(long)]
    banana: u16,

    /// Required; may lead to other args needed
    #[structopt(subcommand)]
    thing: Thing
}

#[derive(Debug, Clone, StructOpt)]
enum Thing {
    Foo,
    Bar,
    Lark {
        #[structopt(long)]
        other_opt: String
    }
}
```

Here, I want "thing" to be as close to a normal positional arg as possible, but to require other opts if one of them is selected. I'd therefore like to be able to rename SUBCOMMAND to something like THING so that it's clearer what is being selected.

If there is a better way to achieve this though, I'm all ears :)

---

_Added to milestone `3.1` by @pksunkara on 2020-04-09 08:00_

---

_Comment by @issuehunt-oss[bot] on 2020-05-27 14:31_

[@pksunkara](https://issuehunt.io/u/pksunkara) has funded $5.00 to this issue.

---
- Submit pull request via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1597) to receive this reward.
- Want to contribute? Chip in to this issue via [IssueHunt](https://issuehunt.io/repos/31315121/issues/1597).
- Checkout the [IssueHunt Issue Explorer](https://issuehunt.io/issues) to see more funded issues.
- Need help from developers? [Add your repository](https://issuehunt.io/r/new) on IssueHunt to raise funds.

---

_Label `:dollar: Funded on Issuehunt` added by @issuehunt-oss[bot] on 2020-05-27 14:31_

---

_Referenced in [clap-rs/clap#1986](../../clap-rs/clap/pulls/1986.md) on 2020-06-26 02:35_

---

_Closed by @bors[bot] on 2020-07-10 13:12_

---

_Comment by @issuehunt-oss[bot] on 2020-08-11 11:39_

[@pksunkara](https://issuehunt.io/u/pksunkara) has cancelled funding for this issue.(Cancelled amount: $5.00) [See it on IssueHunt](https://issuehunt.io/repos/31315121/issues/1597)

---

_Label `:dollar: Funded on Issuehunt` removed by @issuehunt-oss[bot] on 2020-08-11 11:39_

---
