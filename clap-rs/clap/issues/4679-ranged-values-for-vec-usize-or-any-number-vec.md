```yaml
number: 4679
title: "Ranged values for Vec<usize> or any number Vec type like 1-10 or 1..10"
type: issue
state: closed
author: Atreyagaurav
labels:
  - C-enhancement
assignees: []
created_at: 2023-01-27T19:12:27Z
updated_at: 2023-02-06T15:49:46Z
url: https://github.com/clap-rs/clap/issues/4679
synced_at: 2026-01-12T16:14:16Z
```

# Ranged values for Vec<usize> or any number Vec type like 1-10 or 1..10

---

_@Atreyagaurav_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.4

### Describe your use case

I have a program that can skip certain lines based on their line numbers. Consider it similar to head command, but instead of just head 5 lines, it can also skip certain lines. I'm made it skip the line numbers provided to it.

For example if I do: `program --skip 1,3,4 file.csv` it'll skip lines 1,3 and 4, with `value_delimiter = ','` in the clap arg. Problem is if I want to skip a range support 1-5, I have to either make a different argument that takes a number and skips everything larger than or smaller than that number, or make it take a range separately and then parse it into a different variable. A way to just pass the numbers from CLI in the same argument would be perfect. 

### Describe the solution you'd like

The solution I'd like is a feauture in args like "`expand_range` that you can set true" or "`range_delimiter` you can give a string" and if it's a Vec number argument (or similar) then it'll expand the range from the input.

Just like how `value_delimiter` works. But those two should work in the same argument. For example: `1,4-10,15` should be expanded into `[1,4,5,6,7,8,9,10,15]`. If it's easy to implement only using rust syntax range `..` and `..=` works too. But it shouldn't be infinite. 

```
use clap::Parser;

#[derive(Parser)]
#[command(name = "test")]
struct Cli {
    #[arg(short, long, value_delimiter = ',', range_delimiter='-')]
    lines: Vec<usize>,
}

fn main() {
    let args: Cli = Cli::parse();
    println!("{:?}", args.lines);
}
```


### Alternatives, if applicable

Once you have the range character like `-`, you just have to split by the `range_delimiter` expand the range, and then flatten it. 

```
use clap::Parser;

#[derive(Parser)]
#[command(name = "test")]
struct Cli {
    #[arg(short, long, value_delimiter = ',')]
    lines: Vec<String>,
}

fn main() {
    let args: Cli = Cli::parse();
    let lines: Vec<usize> = args
        .lines
        .iter()
        .map(|r| {
            let mut spl = r.split('-');
            let start: usize = spl.next().unwrap().parse().unwrap();
            if let Some(end) = spl.next() {
                (start..=end.parse().unwrap()).collect()
            } else {
                vec![start]
            }
        })
        .flatten()
        .collect();
    println!("{:?}", lines);
}
```

### Additional Context

_No response_

---

_Label `C-enhancement` added by @Atreyagaurav on 2023-01-27 19:12_

---

_Comment by @epage on 2023-01-27 19:41_

What do you feel is the value of doing this natively within clap vs providing a function to `value_parser` that parses numbers and ranges?

For me, this feel specialized with a lot of potential customization when we are working to remove those in favor of general solutions to hep with #2037 and #1365

---

_Comment by @Atreyagaurav on 2023-01-31 21:59_

Providing function would work, but I considered this one of a common use case to get the vector of integers hence thought it might be better to implement from the clap side. But if there is the problem with reducing the overall size for compilation then I don't know. I'm not from CS so not that knowledgeable about that.

Maybe making these kind of functions into helper functions that you can include from the clap features might work to keep the main parser lightweight. But again those things are totally outside of my expertise. 

Main reason I made this issue is because I thought "there should be range feature, if there is delimiter for multiple values". But if you guys are working on keeping those things minimal, then feel free to ignore this.

---

_Comment by @epage on 2023-01-31 22:03_

If you feel this should be general, I'd recommend starting something like this out in a crate.  We can use that go gain interest to gauge if we should bring it into clap proper.

---

_Closed by @epage on 2023-01-31 22:03_

---

_Comment by @Atreyagaurav on 2023-02-04 04:14_

I made a package but couldn't figure out how to use it as a value parser for clap properly. I'd like it to parse the values into vector, then flatten that vector into the `arg.skip`.

My parser is here: https://github.com/Atreyagaurav/number_range

Cargo.toml
```
[dependencies]
number_range = "0.1.3"
clap = { version = "4.0", features = ["derive"] }
```

So the parser works as expected, with something like this:
```
use clap::Parser;
use number_range::NumberRangeOptions;

#[derive(Parser)]
struct Cli {
    /// List of numbers for processing
    #[arg(
        short,
        long,
        value_name = "N1,N2,…",
        // value_parser(number_range_parser)
    )]
    skip: String,
}

fn number_range_parser(num: &str) -> Result<Vec<usize>, String> {
    let mut rng_parser = NumberRangeOptions::default();
    let rng = rng_parser.with_range_sep('-').parse::<usize>(num)?;
    Ok(rng.collect())
}

fn main() {
    let cli: Cli = Cli::parse();
    println!("{:?}", number_range_parser(&cli.skip));
}
```

Compiling and running:
```
     Running `target/debug/clap-rng -s 1,3-5`
Ok([1, 3, 4, 5])
```

But trying to use it as `value_parser`:
```
use clap::Parser;
use number_range::NumberRangeOptions;

#[derive(Parser)]
struct Cli {
    /// List of numbers for processing
    #[arg(short, long, value_name = "N1,N2,…", value_parser(number_range_parser))]
    skip: Vec<usize>,
}

fn number_range_parser(num: &str) -> Result<Vec<usize>, String> {
    let mut rng_parser = NumberRangeOptions::default();
    let rng = rng_parser.with_range_sep('-').parse::<usize>(num)?;
    Ok(rng.collect())
}

fn main() {
    let cli: Cli = Cli::parse();
    println!("{:?}", cli.skip);
}
```

Gives this: 
```
     Running `target/debug/clap-rng -s 1,3-5`
thread 'main' panicked at 'Mismatch between definition and access of `skip`. Could not downcast to usize, need to downcast to alloc::vec::Vec<usize>
', src/main.rs:8:11
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

Which seems to me like clap assumes since it's `Vec<usize>` it'll have multiple occurrences of that flag and the parser is only supposed to parse one of them. Even if I do `number_of_values(1)`, `num_args(1)` and `action(ArgAction::Set)` it still gives the same error. `multiple(false)` doesn't work in the `clap="4.0"` not sure if that'd have fixed it. 

Making it `skip: Vec<Vec<usize>>`, does give me `[[1, 3, 4, 5]]`, so the parser is working. 

---

_Comment by @epage on 2023-02-06 15:49_

The problem is we need to look up the value with the right type and convert it to our type.  We have no attributes for this at the time (see #3912).  As for how to workaround it, we textually check that your type is `"Vec"`, so just doing `std::vec::Vec` will bypass it (see #4626)

---
