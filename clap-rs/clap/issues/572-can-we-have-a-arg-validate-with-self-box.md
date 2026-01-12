```yaml
number: 572
title: "Can we have a `Arg::validate_with(self, Box<Validator>) -> Self` function?"
type: issue
state: closed
author: matthiasbeyer
labels:
  - C-enhancement
  - A-validators
assignees: []
created_at: 2016-07-06T13:13:39Z
updated_at: 2020-09-15T09:26:43Z
url: https://github.com/clap-rs/clap/issues/572
synced_at: 2026-01-12T16:14:09Z
```

# Can we have a `Arg::validate_with(self, Box<Validator>) -> Self` function?

---

_@matthiasbeyer_

I have a use-case where it would be nice to be able to pass custom data to the validator (mainly for passing the error message).

My use-case is: I want to write a validator which checks an argument for whether it is a path and whether this path exists. If the path does not exist, is not readable ... etc, I want to return an appropriate error message.
I want to implement this functionality in a sub-crate of my project and I want to do it generically, so one has only to construct an object and tell it "if the validation fails, use this error message: 'someerrorhappened'". I could then use this type to tell clap "Hey, use this as validator". I would have the benefit that I would write the validation function once, but still have a bit of dynamic in creating my error messages.

---

I propose a trait

``` rust
pub trait Validator {
    fn (&self, String) -> Result<(), String>;
}
```

and a function on `Arg`:

``` rust
pub fn validate_with(self, Box<Validator>) -> Self;
```


---

_Referenced in [clap-rs/clap#573](../../clap-rs/clap/pulls/573.md) on 2016-07-06 13:13_

---

_Comment by @kbknapp on 2016-07-23 18:42_

Apologies for how long it's taken me to get to this, I've been travelling.

I might be misunderstanding what you're trying to do, but the current implementation should allow for that already unless you're talking about not knowing the function beforehand. i.e. the difference in what you're proposing, is using dynamic dispatch of a trait object instead of statically dispatching to a particular function. If it's something other than this, please elaborate.

If using dynamic dispatch is truly what you're wanting to do, I'm not opposed to adding that, but I'd want to name it something less...overlapping? Or we'd have to really document well what the difference is and when to use each.


---

_Comment by @matthiasbeyer on 2016-07-24 11:26_

The point is simply to be able to pass context to the validator function.

For example, if I have an argument which gets a path, I might want to check whether this path exists. But I also might want to check whether this is a file, is readable, is not longer than 100kloc, etc. This could all be done by one validator function, yes. But if my App runs in different states, I might not mind if the file is above 100kloc or even readable (for example if I have a `--dry-run` where I do not touch external files at all).

One way how to solve the problem from above is using several validator functions, each function for one of the requirements. Another one would be to be able to pass state to the validator function.

I thought the latter would be a bit nicer, so I started to implement it in #573. I hope I did not overlook an already existing functionality with this! :smile: 


---

_Comment by @ssokolow on 2017-02-12 01:07_

I was actually about to open an issue with a similar goal.

In my case, I have some validators which could be generic (things like "path is readable") but they're not because I need my error messages to indicate which argument is failing validation. (And I don't care how it's achieved, as long as the result is acceptably easy for users to understand.)

---

_Label `T: enhancement` added by @kbknapp on 2018-07-22 00:52_

---

_Label `P4: nice to have` added by @kbknapp on 2018-07-22 00:52_

---

_Label `D: intermediate` added by @kbknapp on 2018-07-22 00:52_

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 00:52_

---

_Label `C: validators` added by @kbknapp on 2018-07-22 00:52_

---

_Added to milestone `3.1` by @pksunkara on 2020-02-05 08:36_

---

_Comment by @pksunkara on 2020-04-09 06:46_

Related to #1471 

---

_Comment by @bradwood on 2020-06-25 21:10_

FWIW, I'm quite keen on this feature as well. Not so much dynamic dispatch per se, but more simply the ability to be able to pass in a parameter of my own choosing to the validator _in addition_ to the what clap itself passes.

---

_Comment by @CreepySkeleton on 2020-06-25 22:12_

I see that we've got *two* different questions on the table here:

1. Using trait objects vs using multiple functions
    ```rust
    // -------------
    // Multiple fns
    
    fn path_exists(s: &str);
    
    fn do_nothing(s: &str);
    
    if dry_run {
        app.validator(do_nothing)
    } else {
        app.validator(path_exists)
    }
    
    // --------------
    // Trait objects
    
    struct PathExists;
    struct DoNothing;
    
    impl clap::Validator for PathExists {}
    impl clap::Validator for DoNothing {}
    
    if dry_run {
        app.validator(Box::new(DoNothing))
    } else {
        app.validator(Box::new(PathExists))
    }
    ```

    I don't see _any_ difference. Arguably, multiple fns are better.

2. Ability to pass some user-defined context to the validator. Guys, maybe I'm missing something and is about to say something stupid again, but why can't you just... *capture the context*???
    ```rust
    let context = Something;

    app.validator(move |arg| {
        // context has been moved here 
        println!("{}", context);
    })
    ```

Maybe we could add `validator_mut(FnMut() -> Result)` if somebody out there needs to *mutate* the context data, but otherwise I see this as a completely made up problem. 

@ssokolow @bradwood @matthiasbeyer To avoid haunting ghosts, can somebody supply a code snippet that is of an issue? Something that you'd like to do, but it's either currently impossible or possible but too cumbersome? The snippet should also reveal how trait objects could have solved the issue. 

---

_Comment by @ssokolow on 2020-06-25 22:28_

It's been over three years and I can't remember what situation I specifically needed this for, given that the simple case is adequately satisfied by how Clap prefixes the validator-returned message with `Invalid value for '<placeholder name>':`

Heck, given that I've moved to StructOpt (and am about to test out Clap 3.x's derive API), it'd probably be a good idea to think up a more declarative-API-friendly solution to custom messages for generic validators anyway if such a feature comes to pass.

---

_Comment by @CreepySkeleton on 2020-06-25 22:52_

> generic validators 

I suppose your understanding of what "generic validators" being is different from mine. Could you elaborate? Preferably with (pseudo) code examples.

Why and in what do you see validators being not flexible or friendly enough for the derive?

---

_Comment by @ssokolow on 2020-06-26 00:01_

By "generic validators", I mean things like `path_readable_file` in the [`validators.rs`](https://github.com/ssokolow/rust-cli-boilerplate/blob/master/template/src/validators.rs) from my rust-cli-boilerplate. Reusable validators designed for recurring roles.

(Once I've got things cleaned up, I want to flesh that out into a collection of them and split it out into a crate for others to use independent of my project boilerplate.)

For example, I might have a script which takes two or three positional arguments and want more than just the `<metavar>` in the clap-provided error prefix to clarify what the value should be.

In that context, I envision defining a wrapper for each validator assignment, just to alter the error message, will result in both a cluttered `.rs` file and cluttered `cargo doc` output.

(Also, please excuse the mess in `validators.rs`. I let rust-cli-boilerplate languish for a while and I've only started to clean it up now.)

---

_Comment by @CreepySkeleton on 2020-06-30 09:27_

> For example, I might have a script which takes two or three positional arguments and want more than just the <metavar> in the clap-provided error prefix to clarify what the value should be.

And that's a _third_ question on the table, and I think it's quite irrelevant to trait objects - clap would use it's own prefix indefinitely. I suggest you to open a separate issue for that.

From what I've picked from `validators.rs`, I can see you use the "multiple fns" approach, but I really fail to see how trait objects could help you there. (Frankly, I don't really get what you need help with...)

OK, let's wait a bit for the other participants supply their input. Otherwise I'll proceed to closing this issue as "maintainers don't get what the request is".

---

_Comment by @ssokolow on 2020-07-01 21:18_

I wasn't specifically talking about trait objects. I was explaining a use case which, ideally, shouldn't require that a second feature be implemented, given the overlap. (Both involve the desire to specify a custom error message for a specific use of a general-purpose validator.)

If anything, it was an argument to not settle on the idea of using trait objects too quickly.

---

_Comment by @CreepySkeleton on 2020-07-01 22:23_

@ssokolow Here's your problem as I perceive it:

Clap validators don't have the full control over the error message displayed. The message consists of two parts: the `Invalid value for <arg_name>: ` prefix, and the string returned from the validator as `Err`. Validators have full control over the latter - they produce it, but they have no control over the prefix which is displayed no matter what.

Your desire is to modify (or just omit) the prefix. Is my understanding correct? If so, this is unrelated to the issue (which is totally about trait objects that can't possibly help you with it), and I urge you to open a separate issue for that if you want it to be addressed.

---

_Comment by @ssokolow on 2020-07-02 03:27_

> Your desire is to modify (or just omit) the prefix. Is my understanding correct?

No, my desire is to minimize the boilerplate involved in per-field customization of failure messages on fields that share the same stock validator.

I mentioned it here because the initial poster's rationale is:

> I have a use-case where it would be nice to be able to pass custom data to the validator (mainly for passing the error message).

...and that's how I'd have accomplished it in Python.

---

_Referenced in [clap-rs/clap#1998](../../clap-rs/clap/pulls/1998.md) on 2020-07-02 03:35_

---

_Comment by @CreepySkeleton on 2020-07-02 03:40_

> I have a use-case where it would be nice to be able to pass custom data to the validator (mainly for passing the error message).

And then we're back to the question of why you can't just capture the "custom data" in the closure. I took a closer look and found that the validator fns have `'static` restraint, thus making captures virtually useless. In the linked PR above, I tried to address the issue, and from now on you should be able to just use closures. I'll add an example shortly.

---

_Comment by @CreepySkeleton on 2020-07-02 03:50_

@ssokolow Do you need the ability to mutate the context? Is `impl Fn` enough or you need `FnMut`? I'm asking because mutable would conflict with `Send + Sync`.

---

_Comment by @CreepySkeleton on 2020-07-02 03:57_

Ugh, I'm struggling to come up with a meaningful example. @ssokolow Could use your help here.

---

_Comment by @ssokolow on 2020-07-02 06:25_

Assuming you want something minimal enough that a working example could be presented in compact form, you could do a generic "positive, non-zero integer" validator where the failure message is overridden to something specific like "Number of archive volumes must be a positive integer".

It's most useful when the placeholder from `--help` that clap automatically adds to the message prefix is required to be somewhat opaque or vague in the name of conciseness.

---

_Comment by @CreepySkeleton on 2020-07-02 08:34_

I mean something that would demonstrate usefulness of the context, I can't come up with an example that doesn't look made up. Since you were asking for the context passing ability, I assume you have a good idea of what the example might be. It doesn't have to be too concise, 50 (or maybe even 100) LOC will do.

And what about the mutability problem? Right now, you can only have an immutable (shared) reference to the captured context, and it doesn't allow you to mutate anything. Interior mutability won't help you too, because the validator has to be `Send + Sync`. I'm actively working on it ATM and I think this can be solved with a mutex, but I'd rather like to avoid over-engineering if it isn't really needed.

---

_Comment by @ssokolow on 2020-07-04 11:05_

I'll see what I can do, but life has been busy the last few days and I haven't even been able to get to things I specifically chose to take on in as timely a fashion as I expected.

---

_Comment by @ssokolow on 2020-07-17 09:23_

Sorry for going silent. One of my hard drives started to give signs that it may fail soon, and I'm due for a bunch of hardware replacement anyway (eg. can you believe I still don't have an SSD?), so, I've been busy double-checking my backups and preparing for an all-at-once large-scale system upgrade when the new hardware arrives.

I'll let you know once I'm ready to give you something.

---

_Comment by @CreepySkeleton on 2020-07-17 16:48_

> can you believe I still don't have an SSD

Mate, I found you. We should unite and start a new brand movement "weirdos who don't upgrade hardware while it works fine". Affiliate it with bitcoin and bigdata and the world will succumb! 

---

_Comment by @ssokolow on 2020-07-17 19:21_

\*chuckle* I wouldn't say "works fine". It's quite annoying when my nightly backups push everything out of the disk cache and the system then chugs as I have to deal with the consequences of having so many of my applications depending on a forest of small files that need to be seek-time'd back into the cache from all around the platter.

That, and I have another drive from the same production run in one of my machines that started to get bad sectors, so no need to play chicken with it and risk losing data added since the last nightly incremental. (They've both been running more or less 24/7 since 2007.)

---

_Comment by @CreepySkeleton on 2020-09-15 09:26_

Well, I think the " be able to pass context to the validator function." is solved now.

---

_Closed by @CreepySkeleton on 2020-09-15 09:26_

---
