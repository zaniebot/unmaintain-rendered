```yaml
number: 4874
title: "\"Cookbook\" entry for \"just give me back the Clap 3.x colors\""
type: issue
state: closed
author: ssokolow
labels:
  - C-enhancement
  - A-help
  - S-wont-fix
assignees: []
created_at: 2023-05-02T08:43:13Z
updated_at: 2023-10-28T13:46:41Z
url: https://github.com/clap-rs/clap/issues/4874
synced_at: 2026-01-12T16:14:16Z
```

# "Cookbook" entry for "just give me back the Clap 3.x colors"

---

_@ssokolow_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.5

### Describe your use case

I finally got around to looking into upgrading off Clap 3.x now that it's possible to have colorized `--help` output again and realized "Waitaminute... why should I have to go digging through the docs and spend time becoming proficient in writing new themes when there's only *one* theme I want... the one that I currently have by staying on Clap 3.x?"

### Describe the solution you'd like

Can we please get a [cookbook](https://docs.rs/clap/3.2.22/clap/_derive/_cookbook/index.html) entry with some code I can just copy-paste to preserve that old behaviour, similar to how, when Twitter Bootstrap switched to flat design, they offered a supplementary CSS file that would re-enable the old look and feel if added?

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @ssokolow on 2023-05-02 08:43_

---

_Label `A-help` added by @epage on 2023-05-02 14:06_

---

_Label `S-wont-fix` added by @epage on 2023-05-02 14:06_

---

_Comment by @epage on 2023-05-02 14:07_

Another suggestion that came up in #3186 and reddit is that we provides presets for the old colors.

However, I feel that the old colors are a dead end (particularly yellow for help headings) and I'm wanting to encourage people to think about what colors they use, rather than blindly copy or opt-in to the old ones, so I'm included to reject this.

---

_Closed by @epage on 2023-05-02 14:07_

---

_Comment by @ssokolow on 2023-05-02 14:24_

You can't force people to think... force is almost guaranteed to just breed resentment (if they're aware it's an attempt to force them) and manual copying of the old style.

If you want to kill off the old color scheme, then appeal to laziness by providing a new color preset that people find acceptable.

(Given that I'm lazy and busy, I'll just stay on 3.x longer because I can't currently justify the time to familiarize myself with the API so I can manually replicate the output of an old build's `--help`.)

---

_Comment by @epage on 2023-05-02 14:29_

Keep in mind, the new feature is unstable and so its not ideal for someone who isn't wanting to be dealing with some tweaking at this time

---

_Comment by @ssokolow on 2023-05-02 14:31_

Ahh, in that case, I assume the most correct thing is to just fire off another slew of `@dependabot ignore this minor version`s to my projects?

After all, I *did* port them to Rust specifically to minimize the ongoing maintenance burden.

---

_Comment by @epage on 2023-05-02 14:40_

If you don't enable `unstable-styles`, you are unaffected.

If you do enable `unstable-styles`, then yes, you either want to use a `=` version requirement be very conscious of upgrades being performed.

---

_Comment by @epage on 2023-05-02 14:43_

Also, forgot to respond to these

> You can't force people to think... force is almost guaranteed to just breed resentment (if they're aware it's an attempt to force them) and manual copying of the old style.

I'm not forcing; I'm just not helping.  This is a common practice in API design to adjust where burdens are felt to encourage specific behavior.

> If you want to kill off the old color scheme, then appeal to laziness by providing a new color preset that people find acceptable.

Except when I tried to engage with people to find an acceptable combination, no one participated.  So I'm shifting that burden to the community.  We'll see what patterns develop and what of them are acceptable for pulling into clap.

---

_Comment by @ssokolow on 2023-05-02 15:02_

> If you don't enable `unstable-styles`, you are unaffected.
>
> If you do enable `unstable-styles`, then yes, you either want to use a = version requirement be very conscious of upgrades being performed.

In clap's case, my main concern is the amount of work to catch up to an API change. (i.e. How much API churn is likely.)

I suppose I should just keep waiting for now. No need to get adventurous.

Also, I just noticed that you appear to have a typo in the [_features](https://docs.rs/clap/latest/clap/_features/index.html) page.

> **unstable-unstable-styles:** Custom theming support for clap

---

> Except when I tried to engage with people to find an acceptable combination, no one participated. So I'm shifting that burden to the community. We'll see what patterns develop and what of them are acceptable for pulling into clap.

What if transcribing the old color scheme into the new API emerges as the dominant pattern?

Visual design *is* a skill unto itself and, if people don't have that and feel that the old one was "good enough", they might just transcribe it to the new API and move on.

---

_Comment by @praveenperera on 2023-05-23 16:16_

I still have no idea how to enable some colors on the help text  

---

_Comment by @donovanglover on 2023-10-28 13:34_

If anyone else needs help adding color to their program, [this is how I did it](https://github.com/donovanglover/hyprdim/commit/c9be9b037616c5b929d177c8c5dfb82f34242d8d).

---

_Comment by @ssokolow on 2023-10-28 13:46_

> If anyone else needs help adding color to their program, [this is how I did it](https://github.com/donovanglover/hyprdim/commit/c9be9b037616c5b929d177c8c5dfb82f34242d8d).

Thanks. I've had about half a dozen GitHub Dependabot PRs sitting untouched for a couple of months now because I just haven't had time to familiarize myself with the details of updating my Clap 3.x schemas and that'll make it easier to fit in.

---
