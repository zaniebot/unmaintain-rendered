```yaml
number: 4346
title: "[needs repro] potentially misleading ordering of usage in help output"
type: issue
state: closed
author: EverlastingBugstopper
labels:
  - A-help
  - S-wont-fix
assignees: []
created_at: 2022-10-04T14:49:24Z
updated_at: 2022-11-08T16:13:17Z
url: https://github.com/clap-rs/clap/issues/4346
synced_at: 2026-01-12T16:14:15Z
```

# [needs repro] potentially misleading ordering of usage in help output

---

_@EverlastingBugstopper_

for a subcommand with some options and one positional required argument, the help output looks something like `my-cli my-subcommand [FLAGS] [OPTIONS] <my-positional>`. however, if you try to put flags before the positional, (`my-cli my-subcommand --my-arg my-arg-val my-positional-val`), you get a clap error that looks like this: 

```
error: The following required arguments were not provided:
    <my-positional>

USAGE:
    my-cli my-subcommand <my-positional> --my-arg <my-arg-value>
```

i'm fine with that usage error, and we have docs that indicate putting the positional before the args, but it's confusing that the help message usage doesn't match the usage in the help output.

---

_Referenced in [apollographql/rover#1365](../../apollographql/rover/issues/1365.md) on 2022-10-04 14:50_

---

_Comment by @epage on 2022-10-04 15:15_

Generally, positionals can go before or after flags.  The ordering of the usage is dependent on several factors, but is fine as long as its legal.

And without a minimal reproduction case, there isn't much we can act on from this issue.  Please update the issue following ou help template (e.g. [this issue](https://github.com/clap-rs/clap/issues/4339)) so we have enough details to act appropriately to the issue.

---

_Label `S-triage` added by @epage on 2022-10-04 15:15_

---

_Renamed from "incorrect ordering of help output" to "[needs repro] potentially misleading ordering of usage in help output" by @EverlastingBugstopper on 2022-10-04 16:03_

---

_Comment by @EverlastingBugstopper on 2022-10-04 16:05_

> Generally, positionals can go before or after flags. The ordering of the usage is dependent on several factors, but is fine as long as its legal.

That was my understanding as well. The issue referencing this was opened here: https://github.com/apollographql/rover/issues/1365.

I'm working on a minimal reproduction, thanks for the kind nudge in that direction, I'll post back soon.

---

_Comment by @epage on 2022-10-04 16:16_

Looking at the introspect command
```rust
#[derive(Debug, Serialize, Deserialize, Parser)]
pub struct IntrospectOpts {
    /// The endpoint of the subgraph to introspect
    #[serde(skip_serializing)]
    pub endpoint: Url,

    /// headers to pass to the endpoint. Values must be key:value pairs.
    /// If a value has a space in it, use quotes around the pair,
    /// ex. -H "Auth:some key"

    // The `name` here is for the help text and error messages, to print like
    // --header <key:value> rather than the plural field name --header <headers>
    #[clap(name="key:value", multiple=true, long="header", short='H', parse(try_from_str = parse_header))]
    #[serde(skip_serializing)]
    pub headers: Option<Vec<(String, String)>>,

    /// poll the endpoint, printing the introspection result if/when its contents change
    #[clap(long)]
    pub watch: bool,
}
```

`--headers` is defined as `multiple`.  That is an alias for `multiple_occurrences` (already on via the derive) and `multiple_values`

From the [multiple_values docs](https://docs.rs/clap/3.2.22/clap/builder/struct.Arg.html#method.multiple_values)

> # WARNING:
>
> Setting multiple_values for an argument that takes a value, but with no other details can be dangerous in some circumstances. Because multiple values are allowed, --option val1 val2 val3 is perfectly valid. Be careful when designing a CLI where positional arguments are also expected as clap will continue parsing values until one of the following happens:

In general, we recommend using multiple occurrences and/or value delimiters.  Multiple values only work in CLIs under very specific circumstances.

If you want to keep using multiple values and want the usage to match your usage case, you can customize the usage with`#[clap(override_usage = "")]`.  We are unlikely to modify the usage to auto-detect and handle this case as we want to make the recommend cases easy and the discouraged cases hard (but still possible).

---

_Comment by @epage on 2022-10-04 16:18_

Another workaround in the future is we are considering allowing developers to hint to clap which values are valid and which aren't to know when to stop parsing.  See https://github.com/clap-rs/clap/issues/4283

---

_Comment by @EverlastingBugstopper on 2022-10-06 18:16_

> In general, we recommend using multiple occurrences and/or value delimiters. Multiple values only work in CLIs under very specific circumstances.

What do you mean by multiple occurrences? I think I understand value delimiters (comma-delimited would look like `--headers "x-header-one: one-value, x-header-two: two-value"` etc.) but not sure what you mean by multiple occurrences. 

---

_Comment by @EverlastingBugstopper on 2022-10-06 18:17_

I'm also not sure what you mean by `#[clap(override_usage = "")]` - what would I be overriding in this case?

---

_Comment by @epage on 2022-10-06 18:47_

> What do you mean by multiple occurrences? I think I understand value delimiters (comma-delimited would look like --headers "x-header-one: one-value, x-header-two: two-value" etc.) but not sure what you mean by multiple occurrences.

`--headers "x-header-one: one-value, x-header-two: two-value"` is one occurrence of the option.  `--headers "x-header-one: one-value" --headers "x-header-two: two-value"` is multiple occurrences of `headers`

> I'm also not sure what you mean by #[clap(override_usage = "")] - what would I be overriding in this case?

The "USAGE" in help / error.  For example, `#[clap(override_usage = "my-cli my-subcommand <my-positional> [FLAGS] [OPTIONS]")]`



---

_Comment by @EverlastingBugstopper on 2022-10-06 19:44_

Oh I see. So I think that I only want multiple occurrences enabled. Right now I've set "multiple" on that field, which enables BOTH multiple occurrences AND multiple values. If I remove the multiple=true, and just leave it as an `Option<Vec<(String, String)>>` it should automatically derive multiple occurrences, which is what I want here. 

---

_Referenced in [apollographql/rover#1369](../../apollographql/rover/pulls/1369.md) on 2022-10-06 21:19_

---

_Label `A-help` added by @epage on 2022-11-08 04:49_

---

_Label `S-triage` removed by @epage on 2022-11-08 04:49_

---

_Label `S-wont-fix` added by @epage on 2022-11-08 04:49_

---

_Comment by @epage on 2022-11-08 04:51_

As this is a corner case that we generally discourage and developers can provide their own usage, I lean towards not acting on this.

If people want to make the case for why this should be re-opened, they are welcome to.

---

_Closed by @epage on 2022-11-08 04:51_

---

_Comment by @EverlastingBugstopper on 2022-11-08 16:13_

Yup! Totally agree - thanks for the help on this 😄 

---
