---
number: 897
title: Custom Help Formatting Papercuts
type: issue
state: closed
author: mitsuhiko
labels: []
assignees: []
created_at: 2017-03-12T14:15:04Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/897
synced_at: 2026-01-07T13:12:19-06:00
---

# Custom Help Formatting Papercuts

---

_Issue opened by @mitsuhiko on 2017-03-12 14:15_

This is some ideas that would make the usability of custom help pages significantly better. Currently the problems mostly involve subcommands.

## Subcommand Name in Help Page

Currently if you have a binary like `foo-bar` and a subcommd named `blub-blah` the help page always formats the first line as `foo-bar-blub-blah`. There is no option to make it show up as `foo-bar blub-blah` which makes a lot more sense for many applications.

## Longer version of `about()`

Currently for the main command one can use `about()` to inject longer text (somewhat, rewrapping does come up as an issue) however `about()` is used for subcommands in the listing so that cannot get too large.

Would it be possible to introduce a new `explanation()` command that accepts a list of "paragraphs" that are rendered after `USAGE` but before `OPTIONS`? Eg:

```rust
App::new("my-subcommand")
    .about("Does something")
    .explanation(&["paragraph 1 with some text", "paragraph 2 with some text"])
```

Each paragraph itself would be rewrapped and a double newline inserted between paragraphs. Embedded newlines within a paragraph would most likely have to be preserved. Not sure if there is already support for this sort of stuff.

---

_Comment by @mitsuhiko on 2017-03-12 14:16_

For context: `sentry-cli` (https://github.com/getsentry/sentry-cli) currently has really bad help pages because it's a massive effort to customize them for each subcommand without replacing the entirety of the help system.

---

_Comment by @kbknapp on 2017-03-12 17:02_

I've been contemplating adding something similar to this for args to allow a short version of the help and a longer one. This would probably fit nicely with what you're requesting.

One question though, why a list of paras instead of a single blob of text which could include its own paras?

Aside: you could use [`App::template`](https://docs.rs/clap/2.21.0/clap/struct.App.html#method.template) in the mean time to accomplish this until I get this exact request implemented.

---

_Label `C: help pages gen` added by @kbknapp on 2017-03-12 17:02_

---

_Label `C: subcommands` added by @kbknapp on 2017-03-12 17:02_

---

_Label `D: intermediate` added by @kbknapp on 2017-03-12 17:02_

---

_Label `P4: nice to have` added by @kbknapp on 2017-03-12 17:02_

---

_Label `T: new feature` added by @kbknapp on 2017-03-12 17:02_

---

_Label `W: 2.x` added by @kbknapp on 2017-03-12 17:02_

---

_Comment by @mitsuhiko on 2017-03-12 17:17_

The paragraphs might be unnecessary if the text wrapper is paragraph aware. 

---

_Comment by @kbknapp on 2017-04-05 19:55_

This should be closed with v2.23 that was just released

---

_Closed by @kbknapp on 2017-04-05 19:55_

---

_Comment by @mitsuhiko on 2017-04-09 16:39_

I just saw that you added `long_about`. Sadly it works nothing like what this ticket suggests. Here is how it works currently:

If you pass `--help` the subcommand list shows the long about, if you pass `-h` it shows the short about. On the subcommand's help page only the short about is shown at all times.

Instead what I am looking for is that a) `-h` either does not exist or does the same thing as `--help` and that the long help is only used on the subcommand's own help page.

---

_Comment by @kbknapp on 2017-04-09 18:23_

@mitsuhiko in that case, if you don't use `about` and *only* use `long_about`, it should do what you're looking for.

---

_Comment by @mitsuhiko on 2017-04-09 18:25_

Bit then it shows thevlong help texts in the subcommand list which is what
I do not want.

Regards,
Armin

On Sun, Apr 9, 2017 at 11:23 Kevin K. <notifications@github.com> wrote:

> @mitsuhiko <https://github.com/mitsuhiko> in that case, if you don't use
> about and *only* use long_about, it should do what you'r elooking for.
>
> —
> You are receiving this because you were mentioned.
>
>
> Reply to this email directly, view it on GitHub
> <https://github.com/kbknapp/clap-rs/issues/897#issuecomment-292802866>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAAc5ImhtACbTL2WJ8jZvc7kQ41TxK_bks5ruSIVgaJpZM4MajRk>
> .
>


---

_Comment by @kbknapp on 2017-04-09 18:34_

Ah ok, I misunderstood. Then using templates is probably the best way.

```rust
let TEMPLATE = "\
\
{bin} {version}
{author}
{about}

USAGE:{usage}

EXPLAINATIONS:
{after-help}

FLAGS:
{flags}

OPTIONS:
{options}

ARGS:
{positionals}";

App::new("my-subcommand")
    .about("Does something")
    .template(TEMPLATE)
    .after_help("paragraph 1 with some text \n\nparagraph 2 with some text");

---

_Reopened by @kbknapp on 2017-04-09 18:34_

---

_Comment by @mitsuhiko on 2017-04-09 18:36_

That's a massive amount of work though and scales badly with large number
of subcommands. Also requires to customize the template manually depending
on the configuration of the command.

I am aware of that option and I decided against it which is why I opened
this ticket.

Regards,
Armin
On Sun, Apr 9, 2017 at 11:34 Kevin K. <notifications@github.com> wrote:

> Ah ok, I misunderstood. Then using templates is probably the best way.
>
> let TEMPLATE = "\\{bin} {version}{author}{about}USAGE:{usage}EXPLAINATIONS:{after-help}FLAGS:{flags}"
>
> OPTIONS:
> {options}
>
> ARGS:
> {positionals}";App::new("my-subcommand")    .about("Does something")    .template(TEMPLATE)    .after_help("paragraph 1 with some text \n\nparagraph 2 with some text");
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/kbknapp/clap-rs/issues/897#issuecomment-292803511>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAAc5HIaY2ohRepC_4H9xCImeaosdfuKks5ruSShgaJpZM4MajRk>
> .
>


---

_Comment by @kbknapp on 2017-04-09 18:46_

Understood. I've re-opened this issue in order to find a better solution. Thanks for the clear up!

---

_Comment by @netvl on 2017-05-03 19:47_

I also encountered this problem. I expected that `about` and `long_about` behave like this:
* if one of `about` or `long_about` is specified, then it is used for the help message after version/author and for the subcommand description in the parent command;
* if both of them are present, then `about` is used for the subcommand description and `long_about` is used for the help message after version/author.

For example, suppose we have a command `program` and a subcommand `do-something`. Then if `do-something` has both `about` and `long_about` configured, then `program --help` displays the following:
```
program 0.1.0
Program help text here.

USAGE:
    ....

....

SUBCOMMANDS:
    do-something    Text of the short about here
```
and `program do-something --help` displays the following:
```
program-do-something
Text of the long about here.

Maybe containing multiple paragraphs.

USAGE:
    ...
```

Also, if the `program` itself has both long and short abouts, the long about should be used for the main description text ("Program help text here." above). I'm not sure whether the short about would be needed for programs then, but if I recall correctly it has also something to do with the completion generation, so it probably remains useful.

In my opinion, such behavior aligns with the intuition why short and longs about texts are really necessary and how they work, and it also allows one to follow the style of full stops usage, when in short abouts there is no ending dot, while keeping the "narrative" format of the long abouts with proper punctuation.

Currently, if long about is configured together with the short about, it is also used for the subcommands descriptions, and not used for the main description text, which seems quite counterintuitive to me.

---

_Comment by @kbknapp on 2017-05-06 22:45_

I think the argument can be made that `about` should *always* be used for the subcommand list. If one wanted more details you can view that subcommand's help message (which isn't possible in args, hence the `long_help`).

---

_Added to milestone `2.25.0` by @kbknapp on 2017-05-06 22:46_

---

_Removed from milestone `2.25.0` by @kbknapp on 2017-10-12 21:35_

---

_Closed by @kbknapp on 2018-06-10 18:27_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-06-12 14:00_

---
