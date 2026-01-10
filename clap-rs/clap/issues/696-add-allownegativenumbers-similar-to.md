---
number: 696
title: Add AllowNegativeNumbers (similar to AllowLeadingHyphen, but more restricted)
type: issue
state: closed
author: laishulu
labels: []
assignees: []
created_at: 2016-10-19T17:16:25Z
updated_at: 2018-08-02T03:29:54Z
url: https://github.com/clap-rs/clap/issues/696
synced_at: 2026-01-10T01:26:34Z
---

# Add AllowNegativeNumbers (similar to AllowLeadingHyphen, but more restricted)

---

_Issue opened by @laishulu on 2016-10-19 17:16_

I have a tool which should be invoked by `mytool --foo`, but when I invoke by `mytool --foo --bar`, it will just run as normal without any warning. 

I don't know why, because I remember in previous version, I will get an error and exit, sometimes with suggestions like "Do you mean ....".

It's quite dangerous especially for optional arguments, because any typo will passed the check while the user just think he give correct arguments.


---

_Comment by @kbknapp on 2016-10-19 18:02_

Can you post the source, or a link to the source? It could be some option set that's causing it.


---

_Comment by @laishulu on 2016-10-19 18:09_

Got it. If I enable the "AllowLeadingHyphen" setting, then check/suggest/exit does not work.


---

_Comment by @kbknapp on 2016-10-19 18:11_

Ah, yeah the docs warn that, that particular setting will silence what would have otherwise been certain errors.

You can still manually validate those values using a `Arg::validator`, and produce the appropriate `clap::Error` if needed.

I'm going to close this, but let me know if you need any help with the solution! :smile: 


---

_Closed by @kbknapp on 2016-10-19 18:11_

---

_Comment by @laishulu on 2016-10-19 18:25_

1. any instruction/docs on my case?
   
   > You can still manually validate those values using a Arg::validator, and produce the appropriate clap::Error if needed.
2. my app settings is constructed by many yaml files. Is validator compatible with yml?
3. Can I disable `AllowLeadingHyphen`, and use some escape to provide negative values, this seems to be the easiest solution?
4. Is it possible to provide: word with leading hypen allowed only when its previous word is a valid argument name?


---

_Comment by @kbknapp on 2016-10-19 19:12_

@chenhouwu 

> any instruction/docs on my case?

See:
- The docs page for [`Arg::validator`](http://kbknapp.github.io/clap-rs/clap/struct.Arg.html#method.validator) (note, https://docs.rs is more up to date, but I can't link to that page due to firewall settings right now).
- The [example page for validators](https://github.com/kbknapp/clap-rs/blob/master/examples/15_custom_validator.rs)

For example, lets say you only want to support numbers, which might be negative (i.e. `-20` or `20`)

``` rust
fn is_num(s: String) -> Result<(), String> {
    if let Err(..) = s.parse::<i64>() {
        return Err(String::from("Not a valid number!"));
    }
    Ok(())
}

let m = App::new("test")
    .setting(AppSettings::AllowLeadingHyphen)
    .arg(Arg::with_name("num")
        .validator(is_num))
    .get_matches();

```

> my app settings is constructed by many yaml files. Is validator compatible with yml?

_Kind of_. You can add all args in the YAML like normal, but have to add the arg using a validator manaully. i.e.

``` rust
fn some_function(s: String) -> Result<(), String) {
    Ok(())
}

fn main() {
    let yml = load_yaml!("17_yaml.yml");
    let m = App::from_yaml(yml)
    .arg(Arg::with_name("needs_validator")
        .validator(some_function))
    .get_matches();
}
```

> Can I disable `AllowLeadingHyphen`, and use some escape to provide negative values, this seems to be the easiest solution?

Yes. But it depends on the shell being used as to which escape codes must be used.

> Is it possible to provide: word with leading hypen allowed only when its previous word is a valid argument name?

The best way to do that is with a `Arg::validator`. But that is in the same position as before :stuck_out_tongue_winking_eye: 


---

_Comment by @laishulu on 2016-10-20 00:21_

Which one do you mean:

> I can still use `AllowLeadingHyphen`, and then use the `Arg::validator` to validate the value.

Or:

> I should disable `AllowLeadingHyphen`, and then use `Arg::validator` to enable for specific argument.

But without reading the clap codes, I think the leading hyphen check is a pre-check before the validator, so there is a paradox:
1. If `AllowLeadingHyphen` together with `validator`, then the wrong arguments checking (e.g. the `--bar` argument which should be presented) is missing.
2.  If without `AllowLeadingHyphen` together with `validator`, then the negative value will be rejected even before passed to the `validator`.

Any misunderstanding?


---

_Comment by @laishulu on 2016-10-20 01:55_

> Kind of. You can add all args in the YAML like normal, but have to add the arg using a validator manaully. i.e.

```
fn some_function(s: String) -> Result<(), String) {
    Ok(())
}

fn main() {
    let yml = load_yaml!("17_yaml.yml");
    let m = App::from_yaml(yml)
    .arg(Arg::with_name("needs_validator")
        .validator(some_function))
    .get_matches();
}
```

Do you mean:

> You can add all **other** args in the YAML like normal?

If an arg is already in the YAML, then is it valid to add or update its settings from rust codes?


---

_Comment by @kbknapp on 2016-10-20 03:01_

> If an arg is already in the YAML, then is it valid to add or update its settings from rust codes?

That is correct. You can add all **other** args, besides the ones which need to be validated.

> But without reading the clap codes, I think the leading hyphen check is a pre-check before the validator, so there is a paradox

You can't disable `AllowLeadingHyphen` if you want to accept "invalid" args (i.e. things like `-20`). But you're also correct in that if you _do_ keep `AllowLeadingHyphen` you must validate (such as with a validator) **all positional** args that are part of that command (or subcommand). If the source you're working with is open, I can better answer a more specific and not abstract answer.

Sorry for any confusion :smile: 


---

_Comment by @laishulu on 2016-10-20 04:31_

I have implemented a work around just as question `#3`:

> Can I disable AllowLeadingHyphen, and use some escape to provide negative values, this seems to be the easiest solution?

I use `n0.005` in cli to represent `-0.005`.


---

_Comment by @laishulu on 2016-10-20 04:51_

But I recommend to implement `AllowNegativeNumber`, any time the parse see a negative digit number, it will regard it as a number instead of an arg.


---

_Comment by @kbknapp on 2016-10-20 14:27_

Interesting, I hadn't thought of adding `AllowNegativeNumber`. I'll play around with an idea and see if I can get a working solution. Thanks!


---

_Reopened by @kbknapp on 2016-10-20 14:27_

---

_Renamed from "Does not exit or display any error when wrong arguments are provided." to "Add AllowNegativeNumbers (similar to AllowLeadingHyphen, but more restricted)" by @kbknapp on 2016-10-20 14:28_

---

_Label `T: new feature` added by @kbknapp on 2016-10-20 16:19_

---

_Label `P3: want to have` added by @kbknapp on 2016-10-20 16:19_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-20 16:19_

---

_Label `C: settings` added by @kbknapp on 2016-10-20 16:19_

---

_Added to milestone `2.15.0` by @kbknapp on 2016-10-20 16:19_

---

_Closed by @homu on 2016-10-21 20:59_

---

_Comment by @kbknapp on 2016-10-21 21:29_

@chenhouwu try out v2.15.0 on crates.io and see if that works for your use case.


---

_Comment by @laishulu on 2016-10-22 03:10_

it works, great!


---
