```yaml
number: 4814
title: "Add support for \"nested commands\" (as defined in the description) or \"Parsing from the string\" as opposed to `sys.env`"
type: issue
state: closed
author: Davoodeh
labels:
  - C-enhancement
assignees: []
created_at: 2023-03-31T14:17:12Z
updated_at: 2023-03-31T16:50:52Z
url: https://github.com/clap-rs/clap/issues/4814
synced_at: 2026-01-12T16:14:16Z
```

# Add support for "nested commands" (as defined in the description) or "Parsing from the string" as opposed to `sys.env`

---

_@Davoodeh_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.2.1

### Regarding Issue/Discussions search
First off, I am not sure if I actually made a good job searching in the issues. I skimmed through the results of queries for "String", "Nested", "Extend" and some other keywords in both issues and discussions (all states, closed and open). I could not find anything related. If I missed it, or if this is not in match with the scope or philosophy of the project, I sincerely apologize.

### Describe your use case

#### Use case
Imagine somebody wrote an argparser for an application crate which we want to include in our program. So the example is that we want to `use` (as in import, not as in edit) somebody else's source + clap argparser in our code. Doesn't matter why we just do.

This is VERY similar to "subcommand" option. However with a twist, it is not possible for us to edit the source of the `Args` struct they wrote. So we need to use it as as a subcommand but without doing it in a traditional clap sense. There is no way to add `Subcommand` drive to the struct.

![Example Diagram (code in <details>)](https://www.plantuml.com/plantuml/dsvg/XLBDRjH03BxFKtpQ2rBPx0DmGEq1YLue4kNUISPDHioCLsEdeKAyEsCboueWmIMAxS_VZvDRKLzrpiaDoOlWmtUbMdo25nn-5vyhZzNd0tuu0DqLTLT7SJ_TjlmmLu4jjfVTpbiRhl1_CToNywmhnrUXkjMDmo25bT2pAcVmypPpzqMKVB1ENBT7ZcL4Y9K6JQzGXaw4VPWt0jC-kiU9NjjEmvd7tVY4EmnKrSU2v-H7a_aRvGPVq1E4C-bawYb-8pnr7MsgJrYcE4nOOaeDkRcZS75tmQsYBHIujIiMha6EVvuBKN3pwU7n1XuozmFdx4lOEOEr5a9pLbWTXQY8AXWua8bctmos7XSwuIClz4BrQ8t92om7B0e7WKvULtYB9ZYpIEmJdHoLqAZ9kUshTU_DWBZsiE9g-XqENupnnQUPn8nU3_F_Yvot7ydTDi4vfry0)

<details>
<pre>
@startuml
class External as "External Program" {
   - External Args
}

class Ours as "Our Program" {
   + Our Args
   - External Args
}

class Inputs as "User Inputs" {
}

note bottom of Inputs
Instead of inputs going directly to the External Program's Args,
They go to Our Program's Args. A certain flag of Our Program's Args
can be something like `--external` which will be directed to the external Args.
In other words, External Args is *NOT* a subcommand of Our Program and we cannot
edit its code to make it one. However, it has some methods that make this possible.
endnote

Inputs -> Ours
Ours -> External : "Some values of --external will be directed to the external program"
@enduml
</pre>
</details>

The code will look somethine like this:
```rust
// in external.rs
#[derive(Parse)
pub struct Args {
   // whatever
}
```
```rust
// in main.rs
use external::Args as ExternalArgs;

#[derive(Parse)]
pub struct  Args {
  #[arg(long, external_sub_command_without_a_subcommand_derive)]
  external: ExternalArgs,
  // whatever
}
```

#### After that (future)
Maybe after this we can flatten it as well! Meaning there is no need to write `--external` to pass everything to the external parser and it will just work if the fields do not intersect.

#### Why?
The actual feature does not matter! Trying to do this I noticed there are two more important underlying problems with Clap which lead to the user not being able to this (assuming there are no ways and it's not just me not knowing how to do these):
1. There is no way to call `Args::parse()` (in drive mode or its equivalent in functional mode) on a string. This method only takes the inputs from `env`.
2. The nesting subcommands is not intuitive and it is not a general solution. As demonstrated, the current implementation does not resolve such niche cases.

Regardless, it can have benefits like code reusability. The real problem I had is that I'm providing a `commons` library crate which has a series of common utilities a series of binary microservices can benefit from. If there is a feature like this in Clap, I can also provide a bare bone common argparser (some sort of standard argument set for every microservice binary) which every implementation can expand upon if needed.

### Describe the solution you'd like

So as I explained in the previous section, there are two ways to make such a sub-command inclusion work.
First, to tamper with the current implementation of the `Subcommand`. Second, to add a `Args::parse_from_string(s: &str) -> Result<Self>` method that does not rely on `env`, also doesn't cause any side-effects (terminates the program early).

In the builder method, this would look something like `Argparser.get_matches_from_string(&str) -> Result<Whatever>`.

I think the second feature is an absolute must! Regardless of the use case I have here. If implemented, the user can do something like this to fix my usecase:

```rust
use external::Args as ExternalArgs;

#[derive(From, Into)]
enum ExternalArgsHolder {
  Struct(ExternalArgs),
  String(String),
}

#[derive(Parser)]
struct Args {
    #[long]
    external: ExternalArgsHolder,
}

impl Args {
    pub fn parse() -> Result<Self> {
        let mut candidate = <Self as Parser>::parse();
        let external = ExternalArgs::parse_from_string(
            candidate
                .external
                .try_into()
                .expect("just received string so we are sure it gets converted to &str"),
        )?;
        candidate.external = ExternalArgsHolder::Struct(external);
        Ok(candidate)
    }
}
```

P.S: Again I'm sorry if what I wrote doesn't make sense or if it's unrelated. I couldn't put it to words better than this. I'd appreciate any constructive questions that help me clarify the examples and text better.

---

_Label `C-enhancement` added by @Davoodeh on 2023-03-31 14:17_

---

_Comment by @epage on 2023-03-31 15:33_

There are several parts to this that aren't quite clear to me.  Hopefully clarifying some specific points will help us get on the same page.

First, I want to clarify something about `Parser:s::parse`.  I'd recommend checking out the [trait definition](https://docs.rs/clap/latest/clap/trait.Parser.html) as there are many variants of that, like `try_parse` (return the error), `parse_from` (parses an in-memory value, rather than `std::env::args_os`), and `try_parse_from`.  The reason we don't support parsing a `&str` is because that has a lot of shell-specific logic in it (quoting, globbing, etc).  With that said, the [repl cookbook example](https://docs.rs/clap/latest/clap/_derive/_cookbook/repl/index.html) does show one way you can do that.

Second, so long as the foreign type implements the clap types, nothing extra is needed.  You can directly `flatten` the type into your struct.  You can also define your own subcommands and flatten into that.  Say you don't even have access to a type that is compatible with the derive but you have an opaque argument parser.  In that case, you can capture the argments with `trailing_var_arg` + `allow_hyphen_values` or external subcommands and then take the collected values and pass them along to the external parser.  Think `cargo run --manifest-path Cargo.toml -- --help`, cargo is using clap to capture `--help` and pass it along to your binary.  The `--` is needed to disambiguate whether `--help` refers to `cargo` or your program.  You don't need `--` for positionals (think `cargo test filter`, `filter` gets forwarded to your test binary).  If you don't want to require `--` then that is where external subcommands come in handy which cargo uses today for `cargo` plugins like `cargo release --help`.


---

_Comment by @Davoodeh on 2023-03-31 16:50_

@epage Thank you for your fast and detailed answer

There is A LOT that I don't know about Clap. I hoped that my initial issue reflect that.

I appreciate your useful links. I close this issue for now, till I check out the links and I will compose a new answer and reopen the issue (or simply post what I learned here for a future reference and keep it closed) after I got a conclusion. I'll be back either with more questions or more answers/examples.

---

_Closed by @Davoodeh on 2023-03-31 16:50_

---
