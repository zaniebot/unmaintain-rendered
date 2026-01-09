---
number: 5447
title: Environment variable name aliases
type: issue
state: open
author: Aeron
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-parsing
assignees: []
created_at: 2024-04-08T09:11:18Z
updated_at: 2025-10-16T21:13:55Z
url: https://github.com/clap-rs/clap/issues/5447
synced_at: 2026-01-07T13:12:20-06:00
---

# Environment variable name aliases

---

_Issue opened by @Aeron on 2024-04-08 09:11_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.8

### Describe your use case

Clap already supports aliases for short and long argument names, which is superb, especially for keeping backward compatibility. Yet environment variables lack this feature that can be useful in all the same cases.

For example, if my CLI changes one of its arguments’ name, I can keep compatibility with `short_alias` and `alias` functions and derive parameters. But if such an argument has an environment variable assigned, I either need to keep the name that became inconsistent with the argument name or introduce a breaking CLI change.

For another case, the same will be true if I no longer need separate arguments for something that now can be treated as one value, so I need to merge two argument names. The `short_aliases` and `aliases` will cover it great. But environment variables will hold me back from doing so.

### Describe the solution you'd like

It’d be nice to have the same alias options for environment variables, like `env_aliases` and `env_alias`—a super-similar implementation to what argument names have.

### Alternatives, if applicable

_No response_

### Additional Context

I can prepare a PR if there are no objections, considerations, or pitfalls I’m missing. Maybe I overlooked something, or there are some limitations.

---

_Label `C-enhancement` added by @Aeron on 2024-04-08 09:11_

---

_Label `S-waiting-on-decision` added by @epage on 2024-04-09 15:53_

---

_Label `A-parsing` added by @epage on 2024-04-09 15:53_

---

_Comment by @epage on 2024-04-09 15:59_

> I can prepare a PR if there are no objections, considerations, or pitfalls I’m missing. Maybe I overlooked something, or there are some limitations.

I appreciate the willingness.  My main hesitation is just around figuring out where the line is for what feature we provide.  Sometimes providing features that overlap just enough with out use case lead people wanting a lot more like #2763.  Each feature also has an impact, on discoverability, on binary size (#2038), and and build-times (#1365).  This is behind an `env` feature which helps but even that has its downsides.

So I think I want to sit on this a bit, let my thoughts settle on this a bit and to see what other input comes along.

---

_Comment by @survived on 2024-07-05 11:31_

I was surprised to find out that env aliases are not supported, which was unexpected because there are aliases for short/long arg names. I think env aliases do improve usability and user experience.

---

_Comment by @dinhani on 2024-07-31 08:52_

I also want this feature.

For our deployed services, environment variables are the main way to configure applications because they are integrated with configurations/secrets from cloud providers.

Aliases would help preventing breaking changes.

Today we have to do something like this before parsing configurations:

```rust
if let Ok(value) = env::var("SOME_ALIAS_NAME") {
    env::set_var("CANONICAL_NAME", value);
}
```

---

_Referenced in [risinglightdb/sqllogictest-rs#257](../../risinglightdb/sqllogictest-rs/pulls/257.md) on 2025-03-07 06:26_

---

_Comment by @Will-Low on 2025-07-14 15:28_

I have a use-case where I need to deprecate an environment variable and replace it with a new name, while maintaining backwards compatibility. I've explored the below workarounds, but each is hacky:

1. Manipulate the args before parsing to include a field that is always true. This allows us to use something like:
   ```rust
    #[arg(
        env = NEW_ENV_VAR,
        default_value_if(
            "my_always_true_value",
            ArgPredicate::IsPresent,
            dynamically_source_from_deprecated_env_var_or_defaullt()
        ),
        // This is to maintain the correct default value in the help text
        default_value = DEFAULT_VALUE
    )]
   ```
2. Create an `Option<T>` arg for both the deprecated and new env vars. Write a method on the parsed arguments to determine which value to use or returns the default value. This does not allow defining a default value on the arg, so it is missing from the help text. If you print the arg values, it is also confusing, since the default value case will show that neither new nor deprecated value is set.
3. Dynamically determine the default value. However, if the environment variable is set, the help text will use the user-defined value as the listen default, which is incorrect. This requires disabling the default value from the help text, which means that, in order to match the format of the other args, the whole help text needs to be manually crafted.
    ```rust
    #[arg(
        long,
        env = NEW_ENV_VAR,
        default_value = dynamically_source_from_deprecated_env_var_or_default(),
        help = format!("help text... [default: {DEFAULT_VAL}]"),
        hide_default_value = true
    )]
    ```

I really think we should consider a way of bettering supporting this use-case. Given the alias pattern used elsewhere, I strongly believe that this would be the most ergonomic approach.

---

_Comment by @kpcyrd on 2025-09-19 09:01_

I'm also interested in this.

I know very little about those kind of macros, but would it be possible to support "env is either a string or a list of strings"? That would allow:

```rust
#[arg(long, env = ["NEW_ENV_VAR", "DEPRECATED_ENV_VAR"])]
```
and then I could phase out the deprecated env vars over time. When both env vars exist, the first match in the list would win.

As I understand it this wouldn't have impact on the binary size unless actively used, and the compile time overhead would be gated behind the `env` feature at least.

---

_Comment by @epage on 2025-09-19 16:10_

If its an array, then they would all be equal rather than one being a primary, and there being visible and hidden aliases.

---

_Referenced in [clap-rs/clap#6155](../../clap-rs/clap/issues/6155.md) on 2025-10-16 14:42_

---

_Comment by @programmerjake on 2025-10-16 21:13_

I think it would be useful to be able to have some kind of customizable priority order -- not just prioritizing the main env var over all aliases. for my use case I actually want an alias to take priority over the main env var.

---
