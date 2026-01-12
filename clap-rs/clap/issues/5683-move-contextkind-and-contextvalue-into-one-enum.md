```yaml
number: 5683
title: "Move `ContextKind` and `ContextValue` into one enum"
type: issue
state: open
author: TheTollingBell
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-help
assignees: []
created_at: 2024-08-18T23:07:14Z
updated_at: 2024-08-28T19:18:41Z
url: https://github.com/clap-rs/clap/issues/5683
synced_at: 2026-01-12T16:14:17Z
```

# Move `ContextKind` and `ContextValue` into one enum

---

_@TheTollingBell_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

When looking at the clap error formatters there are lots of non-rusty things such as multiple `if let` and non-exhaustive enum matching when dealing with contexts:
https://github.com/clap-rs/clap/blob/d81158599f5b3a2434845a1731377eae84780b9a/clap_builder/src/error/format.rs#L105-L106

Not only is this code messy, it also makes using contexts generally annoying for an end user without digging through the code to see which contexts have been implemented and what kinds of values they can take.


### Describe the solution you'd like

This could be done better by merging `ContextKind` and `ContextValue` into one enum as well as switching the code in existing formatters to use `match` instead of `Error.get()`. (Unless there is some reason that it is being used as such.) 

Enforcing that certain `ContextKind` be restricted to only certain values would make the context API more accessible as well as allowing a singular match instead of multiple if statements in formatters. This can be done with a single `Context` enum that contains the `ContextKind` and multiple nested `<context type>ContextValue` enums.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @TheTollingBell on 2024-08-18 23:07_

---

_Label `M-breaking-change` added by @epage on 2024-08-19 16:09_

---

_Label `A-help` added by @epage on 2024-08-19 16:09_

---

_Label `S-waiting-on-decision` added by @epage on 2024-08-19 16:09_

---

_Comment by @epage on 2024-08-19 16:30_

> it also makes using contexts generally annoying for an end user without digging through the code to see which contexts have been implemented and what kinds of values they can take.

Are you referring to a person creating an error or rendering it?  Some of your comments make it sound like both perspectives and I'm wanting to better understand your use case.  For whichever side you are on, what problem are you solving by using this API?

> Enforcing that certain ContextKind be restricted to only certain values would make the context API more accessible as well as allowing a singular match instead of multiple if statements in formatters. 

Something to consider is how far do we go on being more statically typed.  The `Context` is dependent on the `ErrorKind`, so we could make an `Error` enum with variants that have variants of context which have variants of values.  That is a large explosion of types that require hand implementing every combination of variants without the ability to reuse code for the default implementation (which runs counter to our goals of reducing binary size and compile times).  Due to those goals, we also wanted to allow people to have an even cheaper implementation of error reporting that renders as key-value pairs.  Admittedly, people have likely not taken us up on that.

This API also gives us a lot more flexibility to evolve things without committing to any specific details, allowing us to gracefully degrade.

Originally, our error type was not open to extension for content or rendering.  Opening it up was to offer some extra flexibility for more advanced cases and is lower in our design priority list compared to other values.

---

_Comment by @epage on 2024-08-19 16:30_

As for some of the details...

> non-exhaustive enum matching

With your proposed API, users rendering their own errors would still need to be non-exhaustive because we'd use `#[non_exhaustive]` to allow evolving the API.

> s switching the code in existing formatters to use match instead of Error.get(). (Unless there is some reason that it is being used as such.)

`Error` is presented as a container of context, with the Kinds being unique keys.  This becomes a bit messier to enforce uniqueness with a single `Context`.



---

_Comment by @TheTollingBell on 2024-08-19 22:21_

> Are you referring to a person creating an error or rendering it?

I'm kind of referring to both sides, not only were contexts not well documented for me as an end-user (what was and wasn't supported by each formatter). It was annoying to improve it because it was hard to communicate to a potential end-user what should and shouldn't be used. If not this, then at least provide documentation over either the formatter implementations or the `ContextKind`/`ContextValue` enums so it is clear what is and isn't implemented.

> large explosion of types that require hand implementing every combination of variants without the ability to reuse code for the default implementation

While this concern is valid, as it stands the context API is relatively hard to use and unspecific. Perhaps a feature-gated `context!` macro which would allow compile time errors if an invalid combination of `ContextKind`/`ContextValue` are given?

---

_Comment by @epage on 2024-08-20 16:27_

> I'm kind of referring to both sides, not only were contexts not well documented for me as an end-user (what was and wasn't supported by each formatter).

Sounds like your primary interest is in generating errors and in support of improving that, the complexity of implementing a formatter was a concern.

For generating errors, could you expand on why you are doing so?  What problems are you solving within your application?  Understanding use cases is important for knowing what to improve, how to improve it, and how urgent improving it is.


---

_Comment by @TheTollingBell on 2024-08-20 22:12_

> For generating errors, could you expand on why you are doing so?

I was trying to write a `TypedValueParser` for a type that could be parsed directly from a string. I used `NonEmptyStringValueParser` to parse to a `String` which I then converted to the type. When I saw the context API I thought it would be useful in order to include extra data about what step of parsing went wrong but in whatever "standardized" way `clap` thought I should.

Maybe the above usage is a misuse, but it seemed simpler and more lightweight to use the auto-generated messages instead of something custom. To add to the confusion, the example given [here](https://docs.rs/clap/4.5.16/clap/builder/trait.TypedValueParser.html#example) uses `ContextKind::InvalidArg` which doesn't even show anything when using the default formatter.

---

_Comment by @epage on 2024-08-21 15:44_

> I was trying to write a TypedValueParser for a type that could be parsed directly from a string. I used NonEmptyStringValueParser to parse to a String which I then converted to the type. When I saw the context API I thought it would be useful in order to include extra data about what step of parsing went wrong but in whatever "standardized" way clap thought I should.

Thanks for providing this context!

You can do `NonEmptyStringValueParser::new().try_map(...)` to report your own error and have it wrapped in all of the clap error report trappings.  What "special" reporting were you looking at doing on top of what this provides?

---

_Comment by @TheTollingBell on 2024-08-21 17:32_

> What "special" reporting were you looking at doing on top of what this provides?

`try_map()` is great but providing context like alternative arguments as well as formatting an error in the correct style for a formatter makes the context API feel much more polished than just dumping an unformatted string into an `Err`.

---

_Comment by @epage on 2024-08-22 18:59_

Could you provide concrete examples of what you are trying to get out of context, of that special reporting?  Its helpful to understand the specific problems you are running into to make sure we are on the same page and can consider the right set of solutions, rather than solutions for a situation I guessed that isn't what you are doing.

For example, looking over `format.rs`, I don't see a reason to use it.  If I were, the things that seem the most relevant are
- `ContextKind::Suggested*`
- Showing possible values or subcommands
- Adding in the coloring
- Staying character for character consistent with clap

---

_Comment by @TheTollingBell on 2024-08-23 23:20_

```rust
impl TypedValueParser for MonitorParser {
    type Value = &'static Monitor;

    fn parse_ref(
        &self,
        cmd: &Command,
        arg: Option<&Arg>,
        value: &OsStr,
    ) -> Result<Self::Value, Error> {
        // guaranteed to be non-empty by parser
        let value = NonEmptyStringValueParser::new().parse_ref(cmd, arg, value)?;
        let monitors = get_monitors();

        match monitors {
            Ok(monitors) => monitors.iter().find(|v| v.name == value).ok_or(
                Error::new(ErrorKind::ValueValidation)
                    .with_context(ContextKind::InvalidArg, ContextValue::String(value))
                    .with_context(
                        ContextKind::ValidValue,
                        ContextValue::Strings(monitors.iter().map(|v| v.name.clone()).collect()),
                    )
                    .with_cmd(cmd),
            ),

            Err(e) => Err(Error::new(ErrorKind::ValueValidation)
                .with_context(
                    ContextKind::Custom,
                    ContextValue::String(format!("Error whilst fetching monitors: {e}")),
                )
                .with_cmd(cmd)),
        }
    }
}
```

### Expected Result:
Some kind of output saying `Value Validation failed` alongside all of the contexts given.

### Actual Result:
```
error: invalid value for one of the arguments

For more information, try '--help'.
```

---

_Comment by @epage on 2024-08-27 20:24_

As a heads up, I was primarily looking for your "Expected Result" which you left out.

I'm assuming you are looking for
```
error: invalid value 'slo' for '-O <option>'
  [possible values: slow, fast, \"ludicrous speed\"]

  tip: a similar value exists: 'slow'

For more information, try '--help'.
```
(with arguments/values actually related to your code)

I'm assuming the use of `InvalidArg` when it should be `InvalidValue` is an example of why you are looking for things to be more strictly defined.

I'm also assuming the reason the possible values aren't defined ahead of time is that its a slow, failable operation.

A workaround would be to have your `TypedValueParser` create a `PossibleValuesParser` on demand to report everything.

This type of pattern seems reasonable enough that I wonder if we should have built-in support, e.g. `PossibleValuesParser` could accept a closure of possible values.  While it doesn't solve the bigger picture, it makes reasonably expected cases much more ergonomic than a big picture solution.

---

_Comment by @TheTollingBell on 2024-08-27 22:32_

I think that this a great compromise between an in-depth system and nothing. I would be happy to write a PR adding a `LazyPossibleValuesParser` that would allow the passing of a closure that returns all possible values. The only question I have is handling `Result` types from the closure.

Most likely same technique used `TypedValueParser::try_map()`?

---

_Comment by @epage on 2024-08-28 19:18_

Yes, feel free to post a PR for `LazyPossibleValuesParser` and to handle errors like `try_map`.

I was originally hoping to have this built-in to `PossibleValuesParser` but that would require boxing the `Fn` which would require being more strict on the error type as well as other compromises.  In a future release we could merge the two but I think there would be problems with making the constructor for `LazyPossibleValuesParser` that accepts possible values.

---
