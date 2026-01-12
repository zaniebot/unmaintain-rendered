```yaml
number: 1820
title: Allow accepting an argument by position and by flag
type: issue
state: closed
author: casey
labels:
  - C-enhancement
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2020-04-13T23:41:32Z
updated_at: 2022-01-11T18:41:25Z
url: https://github.com/clap-rs/clap/issues/1820
synced_at: 2026-01-12T16:14:11Z
```

# Allow accepting an argument by position and by flag

---

_@casey_

It would be nice to allow CLIs to a single argument both by position, and by flag.

A possible API for this would be to add `Arg::positional(bool)`, that when passed `true` makes an arg be accepted by position, even if it already has a long or short flag.

The reason this came up is that wrote a CLI with Clap that takes all of it arguments by flag. After releasing the binary, it's clear that one of the flags is always going to be present, so I'd like to "promote" it to a positional argument, so the flag isn't required. However, I'd like to avoid breaking existing usage, so it would be ideal to make it both a flag and a positional argument.

Allowing seamless promotion from flag to flag + positional would have the nice bonus of encouraging conservative design of CLIs.

Positional arguments are the most scarce, since using more than two or three becomes unwieldy. Short flags are the next most scarce, since there are < 100 possible short flags. Long flags are essentially unlimited. So it would nice to enable the pattern of giving everything long flags initially, and then "upgradiing" commonly used arguments to short flags and positional over time.

---

_Label `T: new feature` added by @casey on 2020-04-13 23:41_

---

_Referenced in [casey/intermodal#375](../../casey/intermodal/issues/375.md) on 2020-04-13 23:54_

---

_Comment by @pksunkara on 2020-04-13 23:58_

I am really hesitant about making this part of clap itself. Clap gives users the tools to do this themselves and I think that is enough for the scope of Clap.

Relevant discussion in #1818 

---

_Comment by @CreepySkeleton on 2020-04-14 00:11_

I'll go ahead an close this because the described use case is already possible - and not too complicated - with two args and `required_unless`.

---

_Closed by @CreepySkeleton on 2020-04-14 00:11_

---

_Comment by @Dylan-DPC-zz on 2020-04-14 00:15_

I don't see why this can't be a part of clap. Keeping it open for future exploration 

---

_Reopened by @Dylan-DPC-zz on 2020-04-14 00:15_

---

_Comment by @CreepySkeleton on 2020-04-14 00:27_

If I get it right, this snipped is exactly what @casey wants:
```rust
App::new("app")
    .arg(
        // deprecated optional option
        Arg::with_name("input_opt")
            .hidden(true)
            .long("--input")
            .takes_value(true)
    )
    .arg(
        // new positional arg
        Arg::with_name("input_pos")
            .takes_value(true)
            .required_unless("input_pos")
    )
```

This is done already, what's left to explore?

---

_Comment by @casey on 2020-04-14 05:23_

Thanks for the workaround, I'll do that for now. I'm not so much thinking of it as deprecating the option for the positional, since options can be more readable, for example in shell scripts.

There are some negatives to using two args. One is that because I'm calling a number of additional methods, those need to be duplicated. Using structopt, but would likely be similar with clap-derive:

```
  #[structopt(
    long = "input",
    short = "i",
    value_name = "PATH",
    empty_values = false,
    parse(try_from_os_str = InputTarget::try_from_os_str),
    help = INPUT_HELP,
  )]
  input_option: Option<InputTarget>,
  #[structopt(
    value_name = "PATH",
    empty_values = false,
    required_unless = "input-option",
    conflicts_with = "input-option",
    parse(try_from_os_str = InputTarget::try_from_os_str),
    help = INPUT_HELP,
  )]
  input_arg: Option<InputTarget>,
```

I had to factor out the help message into a constant, `INPUT_HELP`. I decided to not use `hidden`, since I think it's slightly more understandable if the argument shows in the help message in the options section, and in the args section at the bottom.

Also, I have `required_if` constraints on two other arguments on the "input" argument, so I had to modify them to be:

```
    required_if("input-arg", "-"),
    required_if("input-option", "-"),
```

And then to actually access the value, since it's now an `Option`, I'm doing:

```
    self
      .input_arg
      .as_ref()
      .xor(self.input_option.as_ref())
      .ok_or_else(|| Error::internal("Expected `input_arg` or `input_flag` to be set"))
```

All in all this isn't too bad, since I'm able to get the CLI that I wanted, but it could be made a little less verbose if it was supported directly.

Edit: Actually, using `.xor` is more correct, which I missed the first time around.

---

_Comment by @pksunkara on 2020-04-14 08:30_

> Also, I have required_if constraints on two other arguments on the "input" argument, so I had to modify them to be:

You can use an arg group for that.

> One is that because I'm calling a number of additional methods, those need to be duplicated.

You would be adding just one more condition to check if the argument is present or not. You wouldn't be needing anymore duplication. Please give some example if I am wrong.

I am still not convinced that this is needed to be handled by clap. Let's leave this open and see if more people ask for this.

---

_Label `C: args` added by @pksunkara on 2020-04-14 08:30_

---

_Label `C: parsing` added by @pksunkara on 2020-04-14 08:30_

---

_Comment by @casey on 2020-04-14 09:10_

> You would be adding just one more condition to check if the argument is present or not. You wouldn't be needing anymore duplication. Please give some example if I am wrong.

I should have been more clear, what I was referring to was the additional configuration of each arg that needs to be duplicated and now kept in sync, in this case `empty_values = false` and `parse(try_from_os_str = InputTarget::try_from_os_str)`.

---

_Comment by @CreepySkeleton on 2020-04-14 20:48_

> I had to factor out the help message into a constant, INPUT_HELP.

This is totally expected when you want to use the same message for different args. For example, consider this `clap`-unrelated code:
```rust
// we want to pass the very same value to multiple fns

// Option 1 - duplicate
func1(&get_val());
func2(&get_val());

// Option 2 - factor out
let val = get_val;
func1(&val);
func2(&val);
```

`clap` is no different here.

> And then to actually access the value, since it's now an Option, I'm doing:

Yeah, `structopt` isn't that handy when it comes to `requires_if*`; this is a [known problem](https://github.com/TeXitoi/structopt/issues/104). So far, nobody has been able to propose a design that unambiguous _and_ easy to use _and_ considerably easy to implement, but this is a problem with derive, not with clap itself. Your suggestions/help are welcome there!

>  I was referring to was the additional configuration of each arg that needs to be duplicated and now kept in sync [...]

I believe this is intentional design of `clap`/`structopt` (I may be wrong here). Every arg requires you to configure it explicitly so anyone could tell it's properties just from the look of it, no need to look somewhere else. Code is much more read than written after all.

Keeping args in sync might be a problem indeed, but you current proposal is pretty much irrelevant here, no offense.

___

I'd like to get back to your proposal - `positional(bool)`. In clap, an argument is either positional or not (has long/short options) _by definition_, they can't be both at the same time. If you want to see it ever implemented, we will need a detailed explanation of the semantic properties backed with unambiguous examples. Thanks for your understanding.

Personally I think that the only real problem here is keeping args in sync, but I also think that `requires_if(name)` hints developer about dependencies and connections rather well, the state of affairs is "good enough". Everything else here is just a bikeshedding if you ask me.

---

_Comment by @pksunkara on 2020-04-14 21:43_

Can he do `flatten` to reuse the defined arg?

---

_Comment by @CreepySkeleton on 2020-04-14 21:45_

He can't since the flags are not *the same*. And even if he could, I hardly see a point.

---

_Comment by @casey on 2020-04-15 00:32_

> > I had to factor out the help message into a constant, INPUT_HELP.
> 
> This is totally expected when you want to use the same message for different args. For example, consider this `clap`-unrelated code:

Right, but with access to `positional(true)`, they wouldn't need to be two different args, they could be the same arg, and be configured in a single place.

> > I was referring to was the additional configuration of each arg that needs to be duplicated and now kept in sync [...]
> 
> I believe this is intentional design of `clap`/`structopt` (I may be wrong here). Every arg requires you to configure it explicitly so anyone could tell it's properties just from the look of it, no need to look somewhere else. Code is much more read than written after all.
> 
> Keeping args in sync might be a problem indeed, but you current proposal is pretty much irrelevant here, no offense.

None taken! I think the proposal is relevant, because if you could do `positional(true)` the somewhat error prone duplication could be avoided, and done in a single place.

> I'd like to get back to your proposal - `positional(bool)`. In clap, an argument is either positional or not (has long/short options) _by definition_, they can't be both at the same time. If you want to see it ever implemented, we will need a detailed explanation of the semantic properties backed with unambiguous examples. Thanks for your understanding.

Here's a concrete example:

```
let arg = Arg::with_name("input")
  .long("input")
  .positional(true)
  .takes_value(true);

let app = App::new("app")
.arg(arg);
```

Then, the following would both be the same, giving the arg `input` the value `foo`:

```
$ app --input foo
$ app foo
```

---

_Comment by @CreepySkeleton on 2020-04-17 01:20_

@casey Sorry for the delay.

The whole point of "different types - different arg" is that we want to keep it *simple*. In clap, an arg defines one entity, whether it's an option or a positional arg. Your proposal introduces a magical switch that goes against this unspoken policy (I repeat, I might have got it wrong), and what's more, a switch for a thing that does something that is already doable with a combination of existing switches.

I see you called `required_unless` a workaround, but it's pretty much the intended way to do it. It is intentional - I believe - that when you need two separate entities, you create two separate args and specify their mutual relations via `requires*`, `conflicts*`, `ArgGroup`, whatever. This is a difference between "a god option that does multiple (quite unrelated) things" and "a few smaller options that relate to each other somehow". 

My opinion is that your CLI is suffering from "multiple ways to do the same thing" syndrome. You probably don't need it, and if you believe you do, be ready to embrace the complexity it brings in.

---

_Comment by @casey on 2020-04-18 00:41_

> @casey Sorry for the delay.

No worries!

> The whole point of "different types - different arg" is that we want to keep it _simple_. In clap, an arg defines one entity, whether it's an option or a positional arg. Your proposal introduces a magical switch that goes against this unspoken policy

I think that part of the issue is that that's not how my mental model of clap works. I think of args as being the same kind of thing, whether by position, by long flag, or by short flag, and that in the same way that the same arg can have both a long flag and a short flag, they should also be able to be positional as well.

---

_Comment by @CreepySkeleton on 2020-04-18 03:25_

Our mental models of clap - and perhaps mindsets - are different, indeed. Let me try to explain it to you (and let's hope you're into math 🙏 ).

A developer comes up with a set of command line options (`X`) at his fancy. `clap` needs to *map* these options to a set of `Arg`s (`Y`). What does this *mapping* look like?

Your mental model is *surjective mapping*:

![image](https://user-images.githubusercontent.com/50968528/79626400-99666e00-8138-11ea-8ebb-dd467289a287.png)

My mental model is *bijective mapping*:

![image](https://user-images.githubusercontent.com/50968528/79626425-cca8fd00-8138-11ea-807e-641238e4d9b0.png)

The reasons I prefer bijection over everything else:

* It's easy to implement. 
* It's easy to understand. Also, most people comprehend it the same way, which is a priceless property.
* It's easy to teach. Currently, all I need to explain the difference between "positional arg" and "option" to a newcomer is: "It's positional unless you call `Arg::short/long`". Dead easy.

  With your proposal in mind, I would have to add "... but if you call `Arg::positional`, your arg may become both positional and not positional. To understand what this means exactly, please go read that part of documentation".

Anyway, I really don't think this option has any value. The thing you want to do is doable with current `clap`, the problems with keeping multiple options in sync come from redundancy of your CLI - in my opinion, this is a very edge case that is not worthy of it's own dedicated method. Please don't take the later statement as offense or something, I'm just noticing that the very fact that you are the only one who ever came here with such a request so far (IIRC) it very telling.

If many people come here with similar requests I might become open to it, but until then, I'm totally against the inconsistency the proposed option drags along.

---

_Comment by @casey on 2020-04-20 03:22_

I think it's mostly a matter of perspective: Clap already supports mapping short flags, long flags, and aliases, which could be be considered to be a surjective mapping.

---

_Comment by @kbknapp on 2020-04-29 19:22_

@casey I'm sympathetic to your situation, but I'm afraid it will create some additional confusion and complicate the parsing rules for what subjectively is a niche problem set.

I can see where you come from about all arguments being simply "arguments" and the way they're accessed (via short, long, or position) is just an implementation detail. I'd submit that both you and @CreepySkeleton are correct in your assumptions. All arguments are just arguments, and can pretty easily float between the ways they're accessed (short, long, position, etc.) depending on configuration, however @CreepySkeleton is correct in that internally clap handles these as very distinct types, which allows certain optimizations and rule simplification when parsing. The three "types" for clap are:

* positional (no short or long, essentially just a value)
* flag (a short or long, but no value)
* option (a short or long and associated value)

Clap makes the determination as to which type an argument is early on based on the supplied configuration. Having one argument be multiple types could be rather large internal change.

Also, aliases are not related to this discussion as they're just additional shorts/longs and don't change the base type of the argument. I.e. they're not additional arguments from claps point of view (even if it appears so from the users point of view).

---

To solve your case, I would suggest two things and I know they're not exactly what you're looking for but in the long run I think its a better solution:

* Set your old option to hidden (i.e. don't show it in the help message)
* Use required_unless for the new positional argument

Reason being, you are essentially phasing out the old argument. Then on next major version release, you can remove the old option. If you want to go the extra mile, you could first set the option's help message to a deprecation warning, and hide later, or even print a deprecation warning manually if they use the old option.

Essentially you're trying to move your CLI to something more consistent in the future instead of fixing the problem for now but leaving it complicated indefinitely.

---

_Comment by @casey on 2020-04-30 00:25_

That all makes sense, although even considering the maintenance burden of keeping two arguments in sync, I think I probably won't deprecate the option:

- If all arguments can be passed by option, then the user doesn't need to remember which arguments are passed by position.

- I personally sometimes find it hard to remember what positional arguments do, and find options easier to remember.

- For scripts, using options can serve as a form of documentation.

I'm not sure that deprecating the option would benefit the end user.

---

_Comment by @kbknapp on 2020-04-30 01:22_

> I personally sometimes find it hard to remember what positional arguments do, and find options easier to remember.

100% agree. A good general rule of thumb is to have no more than two positional arguments per command or subcommand for exactly that reason.

---

_Referenced in [clap-rs/clap#2940](../../clap-rs/clap/issues/2940.md) on 2021-10-25 11:28_

---

_Comment by @ctrlcctrlv on 2021-10-25 14:16_

Here's a somewhat minimal (was too lazy to remove some glif2svg options lol) example of how you can hack this together with clap 2.3x despite developers not wanting to add it: https://gist.github.com/ctrlcctrlv/bd0ba2d2a9eb638eec5b5f9dd0ec671e

---

_Comment by @ctrlcctrlv on 2021-10-25 14:25_

> Currently, all I need to explain the difference between "positional arg" and "option" to a newcomer is: "It's positional unless you call `Arg::short/long`". Dead easy.

So don't implement it that way. Implement it as `also_long(&str)` and `also_short(&str)`. If you call either on a positional argument, or both, then it becomes also optional.

---

_Referenced in [epage/clapng#154](../../epage/clapng/issues/154.md) on 2021-12-06 20:14_

---

_Label `C: args` removed by @epage on 2021-12-08 20:27_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:13_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:13_

---

_Referenced in [clap-rs/clap#3146](../../clap-rs/clap/issues/3146.md) on 2021-12-10 21:34_

---

_Comment by @epage on 2021-12-10 21:36_

I think at this point, this is too specialized to have native support in clap but instead we offer the tools to be able to do it.

I have created #3146  to track experiments with multiple arguments mapping to one `MatchedArg`.  Likely it won't work out but I do want to see what it can offer.

I'm going to go ahead and close this.  If there are any concerns that are remaining, feel free to speak up!

---

_Closed by @epage on 2021-12-10 21:36_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:41_

---

_Referenced in [wezterm/wezterm#4523](../../wezterm/wezterm/issues/4523.md) on 2023-11-05 12:28_

---

_Referenced in [containers/podlet#89](../../containers/podlet/pulls/89.md) on 2024-07-08 17:50_

---

_Referenced in [r0gue-io/pop-cli#361](../../r0gue-io/pop-cli/pulls/361.md) on 2024-12-06 09:09_

---
