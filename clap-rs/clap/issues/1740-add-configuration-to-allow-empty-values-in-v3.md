```yaml
number: 1740
title: Add configuration to allow empty values in v3
type: issue
state: closed
author: matchai
labels: []
assignees: []
created_at: 2020-03-11T17:58:55Z
updated_at: 2021-03-11T09:14:41Z
url: https://github.com/clap-rs/clap/issues/1740
synced_at: 2026-01-12T16:14:11Z
```

# Add configuration to allow empty values in v3

---

_@matchai_

### Describe your use case

https://github.com/clap-rs/clap/blob/master/v3_changes.md has a point about:
> - Allow empty values no longer default

For [starship/starship](https://github.com/starship/starship), the prompt setup script may occasionally provide empty values to flags. On `3.0.0-alpha.1`, it seems that empty values surface an error to the user.
It would be nice to have a configuration option to optionally accept empty values.

### Describe the solution you'd like

A new clap attribute that would allow empty values to be passed in without surfacing an error.
Perhaps `allow_empty`, or something along those lines.

```rust
#[derive(Clap, Debug)]
enum Opts {
    Prompt {
        #[clap(short, long, allow_empty)] // <-- allow_empty would allow empty strings
        status: Option<String>, // Would be `Some("")`
    },
}
```


---

_Label `T: new feature` added by @matchai on 2020-03-11 17:58_

---

_Comment by @CreepySkeleton on 2020-03-11 18:46_

It looks like [`empty_values`](https://docs.rs/clap/2.33.0/clap/struct.Arg.html#method.empty_values) is gone in 3.x, thus empty values are simply impossible in 3.0.

I would actually expect empty values to be preserved; I don't understand why would we need to strip them at all. They should be allowed by default - and processed correctly, as any other value - and maybe an optional switch to remove them, akin `empty_values` in 2.33. 

@pksunkara @TeXitoi @Dylan-DPC what do you think?

---

_Label `P2: need to have` added by @CreepySkeleton on 2020-03-11 18:47_

---

_Label `T: regression` added by @CreepySkeleton on 2020-03-11 18:47_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-03-11 18:47_

---

_Comment by @pksunkara on 2020-03-11 18:47_

I think this can be done by changing `Option<String>` to `String` and providing a default of empty value.

---

_Added to milestone `3.0` by @CreepySkeleton on 2020-03-11 18:47_

---

_Comment by @CreepySkeleton on 2020-03-11 18:49_

@pksunkara I strongly disagree here, there is a real difference between "default value is an empty one" and "no default values but the user can pass an ampty val".

---

_Comment by @CreepySkeleton on 2020-03-11 18:49_

This is also not about just derive, it is also about raw clap.

---

_Comment by @matchai on 2020-03-11 19:22_

> I think this can be done by changing `Option<String>` to `String` and providing a default of empty value.

Unfortunately, it seems like even defaulting to an empty value surfaces the same error.

![image](https://user-images.githubusercontent.com/4658208/76455426-1a994b00-63ac-11ea-9ccc-fa6cd89070b4.png)

Code:
```rust
use clap::Clap;

#[derive(Clap, Debug)]
#[clap(author, version)]
enum Opts {
    /// Prints the full starship prompt
    Prompt {
        /// The status code of the last executed command
        #[clap(short, long, default_value = "")]
        status: String
    },
}

fn main() {
    match Opts::parse() {
        Opts::Prompt { status } => {
            println!("{:?}", status);
        }
    }
}
```

---

_Comment by @CreepySkeleton on 2020-03-11 19:25_

@matchai I recommend you to stick with structopt + clap 2.33 for the time being (empty vals are allowed by default there):
```rust
#[derive(StructOpt, Debug)]
enum Opts {
    Prompt {
        #[structopt(short, long)] 
        status: Option<String>, // Would be `Some("")`
    },
}
```

---

_Comment by @TeXitoi on 2020-03-12 08:19_

 I think that empty value should even be allowed by default. Forbidding empty value can be done without any more feature, just a custom checker. I don't have any veto against having an `forbid_empty` flag, but I don't think it's really needed.

---

_Comment by @TeXitoi on 2020-03-12 08:20_

Is there any reference of why empty values are forbidden by default in v3?

---

_Comment by @CreepySkeleton on 2020-03-12 14:11_

@kbknapp why did you removed empty values' handling in 3.0?

---

_Comment by @CreepySkeleton on 2020-04-20 08:51_

Here's the situation:
* Empty values are disallowed by default
* User can allow them on per-arg basis via `ArgSetting::AllowEmptyValues`: https://github.com/clap-rs/clap/blob/aae96236b27d43ede24bd7e58668786cd1073c21/src/build/arg/settings.rs#L40
* There's no way to allow empty values globally, for all the app. 

I think we should allow empty values by default (as it used to be in clap 2) and add `ForbidEmptyValues` setting, both app-level and arg-level.

---

_Comment by @pksunkara on 2020-04-20 08:52_

I don't think we should allow them by default because it would conflict with default values.

---

_Comment by @CreepySkeleton on 2020-04-20 08:54_

No, it wouldn't.

```
app --foo '' < EXPLICIT EMPTY VALUE
app --foo=   < IMPLICIT EMPTY VALUE
app --foo    < NO VALUE - DEFAULT
app          < NO OPTION NO VALUE - DEFAULT

---

_Comment by @CreepySkeleton on 2020-04-20 08:57_

See also https://github.com/clap-rs/clap/pull/1587#issuecomment-568829343

---

_Comment by @CreepySkeleton on 2020-04-28 18:46_

@kbknapp While you're still here, I'd love to hear your feedback on this one. Why did you decide to disallow empty values by default in 3.0? For instance, [argparse allows them](https://stackoverflow.com/questions/13558964/passing-empty-string-to-argparse). What do you think about my comment above?

---

_Comment by @kbknapp on 2020-04-28 20:08_

I'll preface this with I don't have strong feelings either way on the matter. 

The main reason the were removed was that in 2.x they were a source of confusion and complicated the parsing rules solely because of the way clap was implemented. If clap had parsed values slightly differently it may have been less an issue.

The problem was only with implicit empty values (`--foo=` and `--foo` [with no `min_values(0)`]) and came from clap not really having much look-ahead/look-behind ability while in the "value parsing" state. So it was hard to distinguish between an error (missing value), or the user just not wanting a value without it being an explicit empty value `""`.

One thing I wanted to prevent (which could be argued was too strict or over-reaching) was to prevent typos where instead of `--foo=bar` one accidentally types `--foo= bar`. If one truly wanted an empty value they could do `--foo=""`.

This was somewhat compounded when parsing short arguments that coalesce (`-f` accepts a value):

```
-abcf bar --other
-abcf= bar --other

vs

-abcf"" bar --other
-abcf="" bar --ohter
```

In the first case, is `bar` a positional value, or the value for `-f`?

So that's a long answer for, "It stemmed only from the way in which clap was implemented, and made the parsing rules more complicated if you need be able to handle when the consumer hasn't told you if they want to accept implicit empty values. It's difficult to know if the value you're parsing is really a value, or a positional argument, etc." 

---

_Comment by @CreepySkeleton on 2020-04-29 12:01_

Regarding to typoproof  behavior - sorry Kevin, but it never worked anyway :)

As @rivy pointed in [that thread](https://github.com/clap-rs/clap/pull/1587#issuecomment-568829343):

> most shells parse and return --foo= as the resultant argument for all of --foo=, --foo='', and --foo="". These are supplied, and there is usually no reasonable way to differentiate at the level of the executable. So, generally, --foo= is / has to be interpreted as foo with an empty string argument.
>
> Notably, windows CMD is, as usual, an exception, but most option libraries for windows emulate the *nix standard.
 
So basically, there's no way to differentiate between `--foo=""` and `--foo=` on most systems. I even took some time to tests it :)

```rust
fn main() {
    for a in std::env::args().skip(1) {
        println!("{:?}", a);
    }
}
```

Windows:
```
D:\workspace\probe>cargo run -- --foo=
    Finished dev [unoptimized + debuginfo] target(s) in 0.18s
     Running `target\debug\probe.exe --foo=`
"--foo="

D:\workspace\probe>cargo run -- --foo=""
    Finished dev [unoptimized + debuginfo] target(s) in 0.20s
     Running `target\debug\probe.exe --foo=`
"--foo="

D:\workspace\probe>cargo run -- --foo=''
    Finished dev [unoptimized + debuginfo] target(s) in 0.17s
     Running `target\debug\probe.exe --foo=''`
"--foo=\'\'"
```

Ubuntu:
```
user@user-PC:/mnt/d/workspace/probe$ cargo run -- --foo=
    Finished dev [unoptimized + debuginfo] target(s) in 0.19s
     Running `target/debug/probe --foo=`
"--foo="

user@user-PC:/mnt/d/workspace/probe$ cargo run -- --foo=""
    Finished dev [unoptimized + debuginfo] target(s) in 0.25s
     Running `target/debug/probe --foo=`
"--foo="

user@user-PC:/mnt/d/workspace/probe$ cargo run -- --foo=''
    Finished dev [unoptimized + debuginfo] target(s) in 0.21s
     Running `target/debug/probe --foo=`
"--foo="
```

As you see, they are the same from clap's standpoint. There's no `-abcf bar --other vs -abcf"" bar --other` because they are identical.

Well, windows doesn't special case `''`, but this is because strings on Win CMD are always written as `"..."`, it doesn't understand `'...'` strings (Powershell works as bash in this regard). Nobody will try it on Win because this is not how strings work on Win.

It's all or nothing: empty values are either allowed (and I really think we should be) and work as I described above or empty values are disallowed and neither `--foo=` nor `--foo=""` nor `--foo ""` work.

---

_Comment by @kbknapp on 2020-04-29 14:05_

I agree empty values should be allowed.

Thanks for taking the time to test/report all that!

---

_Comment by @pksunkara on 2020-06-06 11:56_

Found the commit which removed this. https://github.com/clap-rs/clap/commit/cacc23473c64ec8a57471285b3978face05b4d86

---

_Comment by @pksunkara on 2020-06-06 13:24_

Just found that `AllowEmptyValues` flag is still available in settings [here](https://github.com/clap-rs/clap/blob/v3.0.0-beta.1/src/build/arg/settings.rs#L83)

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-07-08 14:18_

---

_Comment by @wchargin on 2021-02-01 06:36_

+1; removing values doesn’t make any sense to me. A simple example: a
command-line tool that takes `--prefix` (for filtering), which should
default to an empty string (to match all items).

A few well-known programs that have useful empty-string options:

  - `git show --format=`: Show patch only, without message. Useful
    enough that I have an alias for it.
  - `awk -F ''`: Use the empty regexp as the field separator (i.e.,
    split on every character).
  - `printf '%s\n' a '' b`: Print `a\n\nb\n`, with an empty line in the
    middle.
  - (macOS) `sed -i ''`: Overwrite input in place (i.e., no backup
    extension).

The empty string is a string, just as zero is a number and the empty
vector is a vector. In the vast majority of cases, when programs add
special cases where there is a logically “continuous” behavior for zero
values, they introduce confusion and prevent legitimate use cases.
This case is no exception. Please consider permitting empty values
again, preferably by default.


---

_Referenced in [tensorflow/tensorboard#4689](../../tensorflow/tensorboard/pulls/4689.md) on 2021-02-18 17:37_

---

_Referenced in [clap-rs/clap#2360](../../clap-rs/clap/pulls/2360.md) on 2021-02-22 19:35_

---

_Closed by @pksunkara on 2021-03-11 09:14_

---

_Referenced in [starship/starship#3280](../../starship/starship/issues/3280.md) on 2021-11-29 10:05_

---

_Referenced in [trunk-io/analytics-cli#21](../../trunk-io/analytics-cli/pulls/21.md) on 2024-02-15 02:52_

---

_Referenced in [WGUNDERWOOD/tex-fmt#41](../../WGUNDERWOOD/tex-fmt/pulls/41.md) on 2024-10-10 17:17_

---
