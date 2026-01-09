---
number: 2443
title: "Should `derive` be in the default feature list?"
type: issue
state: closed
author: pksunkara
labels:
  - A-derive
assignees: []
created_at: 2021-04-17T09:15:13Z
updated_at: 2021-05-20T22:30:55Z
url: https://github.com/clap-rs/clap/issues/2443
synced_at: 2026-01-07T13:12:19-06:00
---

# Should `derive` be in the default feature list?

---

_Issue opened by @pksunkara on 2021-04-17 09:15_

It's currently in it. But I am thinking of changing it to not be in it.

@BurntSushi I would like your opinion on this. What do you think?

---

_Added to milestone `3.0` by @pksunkara on 2021-04-17 09:15_

---

_Comment by @BurntSushi on 2021-04-17 10:39_

Depending on how it impacts compile times, I would probably disable it every time. With that said, I kind of already expect to disable clap features when I use it, so I don't really see this as that big of a deal personally. The other mitigating factor is that clap tends to show up a direct dependency of a CLI tool and nowhere else, so if you disable default features, it's highly likely that they'll actually be disabled. (Unlike other dependencies that might appear in multiple places. If a feature gets enabled anywhere, it's enabled everywhere.)

---

_Comment by @pksunkara on 2021-04-17 10:47_

Yeah, I completely agree. I am just trying to decide which default is better and how many users (cli developers) want that default.

1. Keep the `derive` in default features list:

    1. Users want to use it:

       ```toml
       clap = "3.0.0"
       ```

    2. Users want to disable it:

       ```toml
       clap = { version = "3.0.0", features = ["std", "cargo"], default-features = false }
       ```

2. Don't use `derive` in default features list:

    1. Users want to use it:

       ```toml
       clap = { version = "3.0.0", features = ["derive"] }
       ```

    2. Users want to disable it:

       ```toml
       clap = "3.0.0"
       ```



---

_Comment by @BurntSushi on 2021-04-17 11:11_

My sense is that a lot of people like the derive functionality, so it might make sense to use it by default. But... If that's wrong and most folks aren't using it, then I think that's going to add a pretty big compilation hit for no reason. So I don't really know what the right answer is unfortunately.

You could follow serde's lead here, which requires opting into the derive functionality.

---

_Label `C: derive macros` added by @pksunkara on 2021-04-25 15:55_

---

_Comment by @kbknapp on 2021-04-26 23:51_

Yeah I don't have a good answer for this one either. I can see it both ways. I hear a lot of people that love the derive functionality, but I also hear a lot of people that are very sensitive to compile times and code bloat. I think the longer I use Rust I personally fall in the camp of preferring opt-in vs opt-out, however those feelings aren't super strong.

I think if we make it the default, there should be some prominent verbiage that it can be disabled for faster compile times at the expense more verbose code. Likewise, if we go the other way making it opt-in there should be some prominent verbiage showing that it can be enabled for less verbose code at the expense of compile times.

Perhaps a good case study would be to look at other prominent crates with derive functionality and see what their defaults are. I believe most of them are *off* by default (i.e. opt-in) which fits with Rust's ideals. Some that come to mind are `serde`, and `tokio` (not for `derive` specifically, but the aggressive cargo features that are all pretty much opt-in).

Another angle that might give insight is to look at the dependency or download statistics of `clap` 2.x and `structopt` (or other `derive` based arg parsers). I would not be surprised to find that those using `derive` based parsers are in the minority, but also more vocal about their support as it's a decently novel paradigm in strong and statically typed languages.

Having said all that, I think it would make most sense to me to make it opt-in for 3.x, as we gather more data and use cases/work out any issues. Then re-address should we get to a future breaking change, or obtain enough data that heavily swings it the other direction.

Just my 2 cents, I'd welcome other arguments for or against.

---

_Comment by @pksunkara on 2021-05-20 22:30_

Thanks to both of you for your opinions. I am more of a purist which is why I was leaning towards making `derive` opt-in.

But one thing I observed is that an example program is easier to understand with `derive` compared to the traditional API based example. Therefore, I have decided to leave the `derive` in default features list.

---

_Closed by @pksunkara on 2021-05-20 22:30_

---
