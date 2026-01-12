```yaml
number: 2373
title: "Add `required_if_present*` akin to `required_unless_present*`"
type: issue
state: closed
author: nixpulvis
labels: []
assignees: []
created_at: 2021-03-01T15:22:56Z
updated_at: 2021-03-01T18:55:11Z
url: https://github.com/clap-rs/clap/issues/2373
synced_at: 2026-01-12T16:14:13Z
```

# Add `required_if_present*` akin to `required_unless_present*`

---

_@nixpulvis_

**Please complete the following tasks**

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

**Describe your use case**

The natural inverse of `required_unless_present`, which allows checking if a flag is set without comparing it's value.

**Describe the solution you'd like**

Implementations for:

- `clap::Arg::required_if_present`
- `clap::Arg::required_if_present_all`
- `clap::Arg::required_if_present_any`


---

_Label `T: new feature` added by @nixpulvis on 2021-03-01 15:22_

---

_Comment by @pksunkara on 2021-03-01 18:29_

There is `requires` for it. (https://docs.rs/clap/3.0.0-beta.2/clap/struct.Arg.html#method.requires)

---

_Closed by @pksunkara on 2021-03-01 18:29_

---

_Comment by @pksunkara on 2021-03-01 18:30_

The only function that can't probably be done is `required_if_all`, but let's create a feature request when someone actually needs it.

---

_Comment by @nixpulvis on 2021-03-01 18:33_

Why not just make the interfaces uniform so people don't bump into this like I have.

---

_Comment by @nixpulvis on 2021-03-01 18:35_

> The only function that can't probably be done is `required_if_all`, but let's create a feature request when someone actually needs it.

Following the current naming, shouldn't this be `requires_all`?

---

_Comment by @nixpulvis on 2021-03-01 18:37_

And shouldn't `requires_if` really be `requires_if_eq`?

I'm trying to get a sense of consistency from the 3.0 interface and I'm struggling.

---

_Comment by @pksunkara on 2021-03-01 18:53_

Because it's about how they get used. If you have required_if_present, you
have to write it for every arg. But if you have requires, you have to write
it for just one arg. In fact, this is how a lot of people are using it.

On Mon, Mar 1, 2021, 18:37 Nathan Lilienthal <notifications@github.com>
wrote:

> And shouldn't requires_if really be requires_if_eq?
>
> I'm trying to get a sense of consistency from the 3.0 interface and I'm
> struggling.
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/2373#issuecomment-788176014>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU35AYZO5RJUBGK55ZRLTBPNHRANCNFSM4YMXPSUQ>
> .
>


---

_Comment by @pksunkara on 2021-03-01 18:55_

And yeah some of names might need a little bit of tweaks. Do propose some
renames for 3.0. there should be an issue.

On Mon, Mar 1, 2021, 18:53 Pavan Kumar Sunkara <pavan.sss1991@gmail.com>
wrote:

> Because it's about how they get used. If you have required_if_present, you
> have to write it for every arg. But if you have requires, you have to write
> it for just one arg. In fact, this is how a lot of people are using it.
>
> On Mon, Mar 1, 2021, 18:37 Nathan Lilienthal <notifications@github.com>
> wrote:
>
>> And shouldn't requires_if really be requires_if_eq?
>>
>> I'm trying to get a sense of consistency from the 3.0 interface and I'm
>> struggling.
>>
>> —
>> You are receiving this because you modified the open/close state.
>> Reply to this email directly, view it on GitHub
>> <https://github.com/clap-rs/clap/issues/2373#issuecomment-788176014>, or
>> unsubscribe
>> <https://github.com/notifications/unsubscribe-auth/AABKU35AYZO5RJUBGK55ZRLTBPNHRANCNFSM4YMXPSUQ>
>> .
>>
>


---
