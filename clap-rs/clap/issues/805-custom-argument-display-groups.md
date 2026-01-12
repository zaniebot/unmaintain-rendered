```yaml
number: 805
title: Custom argument display groups
type: issue
state: closed
author: RazrFalcon
labels:
  - A-help
  - E-hard
assignees: []
created_at: 2017-01-03T20:34:38Z
updated_at: 2018-08-02T03:29:59Z
url: https://github.com/clap-rs/clap/issues/805
synced_at: 2026-01-12T16:14:09Z
```

# Custom argument display groups

---

_@RazrFalcon_

Hello. I want to group options into categories, like [this](https://github.com/RazrFalcon/svgcleaner/blob/master/data/help.txt). See: Elements, Attributes, Paths, etc.

It there a way to do this in the current `clap` version or is it will be implemented?

---

_Comment by @kbknapp on 2017-01-03 21:22_

Unfortunately it's not supported at this time unless you simply write the help message yourself (copy/paste your example into `App::help`). That's not a *horrible* answer, as you still get all the benefits of `clap` like context sensitive usage statements/errors, validations, aliases, relationships, etc. But you do miss out on having the help generated/updated for you. If that doesn't sound like something you'd want to do, there's also [docopt](https://github.com/docopt/docopt.rs) which is another argv parser that basically takes your help message as you have written and uses that to generate the valid arguments.

For completeness sake, you *could* force it to happen in clap, but it'd be very hacky even though functional. If you want to see, I can give you a demo :wink:

----

Having said all this, it'd be possible for me to add this is feature, and I'd be curious to know if others want this same thing. I personally think it'd be somewhat of a pain to enter, `arg.help_group("foo")` for every arg unless you're using a macro (which many actually do). Of course, I could also add an `App::help_group("group", &["arg1", "arg2", "arg3"])` but you'd still risk not keeping that list up to date as your list of args changes and evolves.

I'd like to hear some thoughts on the matter :)



---

_Label `T: RFC / question` added by @kbknapp on 2017-01-03 21:23_

---

_Referenced in [clap-rs/clap#686](../../clap-rs/clap/issues/686.md) on 2017-01-03 21:25_

---

_Comment by @kbknapp on 2017-01-03 21:27_

Possibly relates to #686 

---

_Comment by @RazrFalcon on 2017-01-03 21:48_

>That's not a horrible answer

Well... it's not, since it what I'm [doing right now](https://github.com/RazrFalcon/svgcleaner/blob/master/src/cli.rs#L165).

I don't really know what kind of API I want, but since I prefer that options should be in a specific order (I forgot to mention this in OP. In example, there are not only groups that matter, but also an order of options inside. Yes, it's visual only and do not change parsing in any way.), so I guess something like:
`Arg::group_to("group_name").group_pos(1)`
but it's looks to specific for my case and I don't know it anyone will want to use it.

I'll think about how it can be implemented, API-wise. Sounds like keeping an own help file is the easiest way...

---

_Comment by @kbknapp on 2017-01-03 23:38_

The order of arguments can already be specified manually (using [`Arg::display_order`](https://docs.rs/clap/2.19.3/clap/struct.Arg.html#method.display_order)), or placed in the order that you define them (using [`AppSettings::DeriveDisplayOrder`](https://docs.rs/clap/2.19.3/clap/enum.AppSettings.html#variant.DeriveDisplayOrder))

In fact, the hack I hinted at uses those options, along with afew others to trick the generated help message.

---

_Comment by @kbknapp on 2017-01-04 01:20_

[Here's what I meant by the hack to make it work](https://gist.github.com/kbknapp/e4b1c4671848c7c1b019e39021e28ae4)

Is this any better than copy/pasting the help? I know it's looks uglier, but it has the benefit of never falling out of date. Granted, there are drawbacks such as the "can't have a default here" portions. Although there are ways around that; such as unless you have a really good reason not to (like porting a well known CLI), it's considered better UX to use flags instead of options for the `true/false` values. This would also remove the need for defaults. If you *do* need to use an option for `true/false` you could just use `value_of("option").unwrap_or(default)` for those 4 or so args which isn't too big a deal.

---

_Comment by @RazrFalcon on 2017-01-04 06:33_

`DeriveDisplayOrder` is a good stuff, I didn't notice it.

Adding newlines to `help()` is an interesting idea, sadly I need default values. Maybe there is a way to insert dummy/custom text between args?

About `true/false`: yes, I don't like it either, but most of options should be enabled by default and I don't know is it possible for flags. Only way I see is too name default options as `keep-*`(or similar) and disabled as `remove-*`(or similar). So flag will able to disable enabled options and enable disabled. But then to change the default value I have to rename it...

---

_Comment by @kbknapp on 2017-01-04 14:37_

> About `true/false`

I think convention is to use `--no-*` for something that defaults to true and `--*` for something that defaults to false.

Such as `--remove-comments <FLAG> [defualt: true]` -> `--comments`, or `--remove-gradient-attributes <FLAG> [defualt: false]` -> `--no-gradient-attributes`

Another option is to group like functionality into a single option. For example, all the "removal" flags become:

```
--remove-element <ELEMENT>...    Removes a particular element [values: comments, declarations,
                                 nonsvg-elements, unused-defs, title, desc, metadata, 
                                 dupl-lineargradient, dupl-radialgradient, dupl-fegaussianblur, 
                                 invalid-stops, invisible-elements]
``` 

Then you could set this up to allow `--remove-element <element1> <element2>` or `--remove-element <element1> --remove-element <element2>` depending on whichever you prefer.

We're digressing from the original topic, but that's fine because I enjoy CLI UX :smile: 

----

I'm going to play with adding, "display groups" and see how it feels. I'll post back here with an example once I have it working.

---

_Comment by @RazrFalcon on 2017-01-04 16:51_

`--no-*` is a great idea, it's like in GCC, but is such behavior handled by clap? Or is there should be two flags for each option?

I thought about groping, but it's harder to generate, from GUI for example, and also it's harder to write, because it will be one, very long line. And I'm already have more than 50 options. In current version I can separate each option with the `\` (for new lines). Which is useful.

Actual problem with flags, is that most of them are enabled by default. So it will be just a bunch of keys which disables staff, like: `--keep-*`, `--skip-*`, etc. Which is counter-intuitive.

PS: I don't mind discussing CLI UX, since there are not mach articles about it. And `docopt` doc, which you mention, is one of them. Also there are not much applications with a lot of CLI arguments. I look up for: gcc, clang, mpv. And they all simply shows alphabetically sorted list of options. Some exceptions are `qmake`, which uses grouping just like me. But the way they do this [is not really interesting](https://github.com/qt/qtbase/blob/dev/qmake/option.cpp#L125). And `eix`, but they [do the same](https://github.com/vaeth/eix/blob/master/src/eix.cc#L89).

---

_Comment by @kbknapp on 2017-01-04 19:21_

>  is such behavior handled by clap? Or is there should be two flags for each option?

No, if an argument defaults to true, that means the functionality is "on" by default. So the user not passing the flag is consent for that action to take place (default true), but then you have a `--no-*` to change the default functionality, or turn off that functionality. So you only need one flag per item. 

Here's the comparison for items defaulting to true: 

New | Previous
------|---------- 
 **nothing** |  `--option true`
`--no-option` | `--option false`

The inverse also applies. Something that defaults to false, *doesn't* do some particular action. Then supplying `--action` turns that feature back on.

New | Previous
------|---------- 
 `--option` |  `--option true`
**nothing** | `--option false`

It makes your CLI more concise, and help messages are easier to read as well.

> grouping [..] it's harder to write, because it will be one, very long line. 

`clap` handles this part of your help message for you (Look up `Arg::possible_values`, work proxy is preventing me from linking to it). So no writing required (other than specifying the options that are valid). You can also prevent them from being printed in the help message if you think it's too long via `AppSettings::HidePossibleValues`

> Actual problem with flags, is that most of them are enabled by default. So it will be just a bunch of keys which disables staff, like: --keep-*, --skip-*, etc. Which is counter-intuitive.

I think it's actually the opposite. And your help message for that flag can say, "By default svgcleaner removes X, this flag disables that removal" Because the other way around, what you'll end up seeing is people running `svgcleaner --option true` even though it's the default, they'll think you *have* to specify something, "Or else it wouldn't be an option."

There's other oddities as well, such as users forgetting whether it's `true/false`, `yes/no`, `on/off`, `y/n` or any other variant. So you may end creating parser to allow any of the above, or translating a `true/false` instead of simply checking if `--no-*` was used or not. Done.

I'm not saying it's *wrong*, as it can totally be done with `true/false` it's just slightly harder :)

> I don't mind discussing CLI UX, since there are not mach articles about it. 

I'm actually working on releasing an mdBook (same as Rust book) about CLI UX in general, and using Rust, since it's something I really enjoy working on and thinking about. It'll be a while before it's ready though.

---

_Comment by @RazrFalcon on 2017-01-05 17:16_

Thanks. I'll keep thinking about it.

Waiting for a book.

---

_Renamed from "Custom options grouping categories " to "Custom argument display groups" by @kbknapp on 2017-01-30 05:12_

---

_Comment by @kbknapp on 2017-04-05 18:43_

I did some playing with this and ergonomics aren't great. It's also a huge rework of how clap writes a help message and thus I'm gonna have to close this for now as it's not feasible to include in the current version. Perhaps 3.x will make this more doable, but it's still unlikely.

I am, however, still open to suggestions on how this could work.

---

_Closed by @kbknapp on 2017-04-05 18:43_

---

_Comment by @RazrFalcon on 2017-04-05 18:52_

Ok. I've ended up thinking that a custom help is the best solution.

---

_Comment by @kbknapp on 2017-04-05 19:01_

Bah, I'm gonna keep this open. It *is* something I'd like to have. It probably won't happen soon, but I do want this style of feature.

---

_Reopened by @kbknapp on 2017-04-05 19:01_

---

_Label `C: args` added by @kbknapp on 2017-04-05 19:02_

---

_Label `C: help message` added by @kbknapp on 2017-04-05 19:02_

---

_Label `D: hard` added by @kbknapp on 2017-04-05 19:02_

---

_Label `P3: want to have` added by @kbknapp on 2017-04-05 19:02_

---

_Label `T: new feature` added by @kbknapp on 2017-04-05 19:02_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-04-05 19:02_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:57_

---

_Comment by @rtsuk on 2017-08-08 16:04_

Adding my vote for this feature. My Fuchsia-specific cargo wrapper [Fargo](https://fuchsia.googlesource.com/fargo/) could really benefit from it.

---

_Comment by @kbknapp on 2017-08-09 02:31_

I'm trying to get 3.x out the door in the coming weeks so I can start knocking out all the current feature/bug reports. This particular feature is high on my list though :wink:

---

_Label `W: 3.x` added by @kbknapp on 2018-02-05 15:28_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-02-05 15:28_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-15 14:51_

---

_Referenced in [clap-rs/clap#1211](../../clap-rs/clap/pulls/1211.md) on 2018-03-18 12:25_

---

_Comment by @kbknapp on 2018-04-04 03:38_

Closed with #1211 (will be released in v3)

---

_Closed by @kbknapp on 2018-04-04 03:38_

---

_Referenced in [clap-rs/clap#1250](../../clap-rs/clap/issues/1250.md) on 2018-04-18 09:18_

---

_Referenced in [BurntSushi/ripgrep#1814](../../BurntSushi/ripgrep/issues/1814.md) on 2021-07-28 13:37_

---
