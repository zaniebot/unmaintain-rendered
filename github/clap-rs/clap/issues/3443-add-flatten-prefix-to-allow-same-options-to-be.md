---
number: 3443
title: add flatten prefix to allow same options to be reused.
type: issue
state: closed
author: dan-da
labels:
  - C-enhancement
  - A-derive
  - S-duplicate
assignees: []
created_at: 2022-02-11T01:03:54Z
updated_at: 2022-02-11T23:08:53Z
url: https://github.com/clap-rs/clap/issues/3443
synced_at: 2026-01-07T13:12:19-06:00
---

# add flatten prefix to allow same options to be reused.

---

_Issue opened by @dan-da on 2022-02-11 01:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

#3117 and #2293 ask for a flatten prefix in order to group related options together.

I have a related but different use-case for the same prefix feature, which I do not see an easy workaround for.

In short, I want to be able to use the same set of options multiple times, with different prefixes.   This is needed for specifying different types of connection endpoints.

### Describe the solution you'd like

So I am using a third party crate which defines a struct Config and derives StructOpt.  For simplicity, we will say it is just Config {host, port}, eg:

```
#[derive(StructOpt)]
struct Config {
  #[structopt(short = "p", long = "port")]
  port: u16,
  #[structopt(short = "a", long = "address")]
  addr: IpAddr
}
```

I need to fill this Config multiple times. eg, I want to have a server-config and a p2p-config.  So I want to do something like:

```
#[derive(StructOpt)]
struct Opts {
  #[structopt(flatten, prefix = "p2p-")]
  p2p_config: Config,
  #[structopt(flatten, prefix = "server-")]
  server_config: Config,
}
```

So the behavior i need is to:
1. prefix the long opts of Config with the value of prefix so they are grouped and do not conflict.
2. disable the short opts of Config.

If there is some existing workaround or way to do this, keeping in mind that Config is defined in a third party crate, please let me know.

I see that both the above-referenced issues were closed without fixing.  I hope that by describing this additional use-case and adding my voice to the choir, that decision might be re-visited.



### Alternatives, if applicable

_No response_

### Additional Context

The desired code will actually build today (minus prefix attribute), eg:

```
#[derive(StructOpt)]
struct Opts {
  #[structopt(flatten)]
  p2p_config: Config,
  #[structopt(flatten)]
  server_config: Config,
}
```
but it panics at startup with:
```
Non-unique argument name: port is already in use
```
This is expected because Config is used twice and flatten prefix is not supported.

---

_Label `C-enhancement` added by @dan-da on 2022-02-11 01:03_

---

_Comment by @epage on 2022-02-11 01:19_

Unfortunately, #3117 wasn't rejected because of the use case but because of the feasibility of the implementation

---

_Closed by @epage on 2022-02-11 01:19_

---

_Label `A-derive` added by @epage on 2022-02-11 01:19_

---

_Label `S-duplicate` added by @epage on 2022-02-11 01:19_

---

_Comment by @dan-da on 2022-02-11 03:14_

I understand if that is the case, but what about this [comment you made](https://github.com/clap-rs/clap/issues/3117#issuecomment-989997423)?

> The real blocker here (apart from the lack of inter-expansion communication in proc-macros as mentioned above) is clap's API. To implement this we would need Arg::get_long(), Arg::get_short(), and Arg::get/set_name(&str) methods.
> ...
> But there is still a blink of light in the darkness: structopt is being imported directly into clap and, since it will be technically one repo, **we could implement it with no problem**.

---

_Comment by @epage on 2022-02-11 03:19_

I did not make that comment.  I used my account for importing comments from the structopt repo.  In the header of that comment, it says who wrote it.

---

_Comment by @dan-da on 2022-02-11 13:28_

sorry, my mistake.  SInce this is still closed, I guess you disagree with the commenter about being able to impl it "with no problem" once integrated into clap...?

---

_Comment by @epage on 2022-02-11 13:33_

Yes.  That commenter only addressed half the problem and didn't get into dealing with `FromArgMatches` as I talk about in my [comment](https://github.com/clap-rs/clap/issues/3117#issuecomment-991257659).  That or they take the approach "its software, you can do anything".  It is possible for us to implement this but we'd then be burdened with large implementation complexity, the door open for  cross-derive communication which will require a breaking change for each new feature along this line, and a nearly impossible-to-implement trait design (which is one of our goals with the current trait design).

---

_Comment by @dan-da on 2022-02-11 23:08_

ok, thanks for sharing these insights.  I will take another approach.

---
