---
number: 4501
title: "May `Parser::update_from` not deny the presence of flags (option of type `bool`) ?"
type: issue
state: open
author: TD-Sky
labels:
  - C-bug
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-11-22T07:13:49Z
updated_at: 2023-06-19T16:40:48Z
url: https://github.com/clap-rs/clap/issues/4501
synced_at: 2026-01-10T01:27:56Z
---

# May `Parser::update_from` not deny the presence of flags (option of type `bool`) ?

---

_Issue opened by @TD-Sky on 2022-11-22 07:13_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.0.26

### Describe your use case

I'm seeking a way to merge default options coming from environment variable and runtime options.

Even though the method `Parser::update_from` cannot distinguish the difference between `None` and default value, it is the right way. I used `unwrap_or` with custom default value to bypass this feature but found it very troublesome to deal with flags.

Here is the example code:

```rust
use clap::Parser;

#[derive(Parser)]
struct Cli {
    #[arg(long)]
    boolean: bool,
}

fn main() {
    let mut cli = Cli::parse();

    println!("flag `boolean`: {}", cli.boolean);

    cli.update_from(["issue"]);    // binary name

    println!("flag `boolean`: {}", cli.boolean);
}
```

Run with flag:

```bash
$ cargo run -- --boolean
```

Output:

```
flag `boolean`: true
flag `boolean`: false
```

The present flag is denied by `update_from` without `--boolean`.

### Describe the solution you'd like

I wish `update_from` to not overwrite the presence of flags, `true` values should be kept during updating.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @TD-Sky on 2022-11-22 07:13_

---

_Label `A-derive` added by @epage on 2022-11-22 14:12_

---

_Label `C-enhancement` removed by @epage on 2022-11-22 14:12_

---

_Label `C-bug` added by @epage on 2022-11-22 14:12_

---

_Label `S-waiting-on-design` added by @epage on 2022-11-22 14:12_

---

_Comment by @epage on 2022-11-22 14:15_

I wonder how well we can detect this case as users can overwrite defaults.  We can check for various combinations of `default_value` and `default_missing_value` but those will be in textual form and we'd then need to also load up the value parser to check that.

---

_Comment by @epage on 2022-11-22 14:15_

Oh, I guess the simpler thing is for `update_from` to ignore any value whose `value_source` is `Default`.

---

_Comment by @epage on 2022-11-22 14:17_

The next question is if that would be disruptive enough for anyones workflow to be either a minor compatibility break (reserved for clap v4.X.0) or a major compatibility break (reserved for clap vX.0.0)

---

_Comment by @TD-Sky on 2022-11-22 14:32_

`Parser::update_from` may not be an often used API and many projects are still using imperative style.

---

_Comment by @TD-Sky on 2022-11-22 14:33_

So a minor compatibility break would be OK :)

---

_Comment by @epage on 2022-11-22 14:42_

Minor or major is not a matter of how many people are impacted but by a characterization of how it affects applications that might or might not be relying on the behavior.

And honestly, I keep wondering if we should just remove `update_from`.  I wish I could tell how many people were using it and how important it is to their workflow.

---

_Comment by @TD-Sky on 2022-11-22 14:51_

The semantic of `update_from` is very appropriate. I think it not worth to highlight the importance by removing this API.

---

_Referenced in [skim-rs/skim#491](../../skim-rs/skim/pulls/491.md) on 2022-12-14 13:07_

---

_Comment by @Mehrbod2002 on 2023-01-15 17:16_

@epage 
Can I work on this ?

---

_Comment by @epage on 2023-01-16 16:15_

> The semantic of `update_from` is very appropriate. I think it not worth to highlight the importance by removing this API.

I'm not sold that `update_from` has strong enough use cases to justify its complexity and high bug ratio.  There are also various semantic holes in it that I noticed when I polished it up for clap 3.0 though I do not remember what they are at this time.

> @epage Can I work on this ?

The part that was missing was someone stepping through the analysis requested earlier

> a characterization of how it affects applications that might or might not be relying on the behavior.

to determine if this is a major incompatibility, minor incompatibility, or just a bug fix.

---

_Comment by @Mehrbod2002 on 2023-01-16 18:12_

@epage 
Understood!
Thanks for details. 

---

_Comment by @TD-Sky on 2023-06-19 04:39_

> @epage Understood! Thanks for details.

Not to rush you, but I was wondering if you are still working on this?

---

_Comment by @epage on 2023-06-19 13:36_

I triaged this but am not actively working on it.

The next step is

> The part that was missing was someone stepping through the analysis requested earlier

---

_Comment by @TD-Sky on 2023-06-19 16:25_

Is it a good idea to collect users' opinion about `update_from` by questionnaires?

---

_Comment by @epage on 2023-06-19 16:29_

To be clear, the analysis I'm referring to is in this comment: https://github.com/clap-rs/clap/issues/4501#issuecomment-1323748371

Removing `update_from` is separate from this

---

_Comment by @TD-Sky on 2023-06-19 16:40_

> To be clear, the analysis I'm referring to is in this comment: [#4501 (comment)](https://github.com/clap-rs/clap/issues/4501#issuecomment-1323748371)
> 
> Removing `update_from` is separate from this

Theoretically, this is a major compatibility break, either keep with internal changes or remove it.

---

_Referenced in [clap-rs/clap#4974](../../clap-rs/clap/issues/4974.md) on 2023-06-19 17:18_

---
