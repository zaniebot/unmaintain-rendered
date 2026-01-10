---
number: 5156
title: Show the type of arguments in help message?
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-help
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2023-10-02T17:47:03Z
updated_at: 2025-05-22T13:51:24Z
url: https://github.com/clap-rs/clap/issues/5156
synced_at: 2026-01-10T01:28:07Z
---

# Show the type of arguments in help message?

---

_Issue opened by @epage on 2023-10-02 17:47_

### Discussed in https://github.com/clap-rs/clap/discussions/5153

<div type='discussions-op-text'>

<sup>Originally posted by **tigerros** October  1, 2023</sup>
Let's say you have this:

```rust
#[derive(Parser, Debug)]
#[command(author, version, about, long_about = None)]
pub struct MyParser {
    #[arg(long)]
    pub foo: u32,
}
```

Running `--help` will show this message:

```
Options:
      --foo <FOO>
  -h, --help       Print help
  -V, --version    Print version
```

Is it possible to also show the type? For example:

```
--foo <FOO> (unsigned 32 bit integer)
```

`u32` would be nice, but it's not standard.</div>

---

_Label `C-enhancement` added by @epage on 2023-10-02 17:47_

---

_Label `A-help` added by @epage on 2023-10-02 17:47_

---

_Label `A-derive` added by @epage on 2023-10-02 17:47_

---

_Label `S-waiting-on-design` added by @epage on 2023-10-02 17:47_

---

_Comment by @epage on 2023-10-02 17:47_

> There is an argument parser for another language that does this; I wish I could remember which.
> 
> If we did this, it could work to fill in similar to `[possible values: foo, bar]` in style.
> 
> However, I'm concerned about
> 
>   * Clutter in the help output
> 
>   * Not being in the user's terms (while most CLI users are programmers, not all are), even if its "unsigned 32 bit integer".
> 
> 
> However, the `value_name` fills a similar role and I think it'd be a worthwhile to look into leveraging that. `long` and `id` are more akin to a field name while `value_name` is more akin to the type (in flags, positionals are weird in that they need to convey both). Framing this way, makes me feel like we could have a type associated term as the value name.
> 
> I had considered such a thing in #2683 but held off in #3732.


---

_Comment by @epage on 2023-10-02 17:56_

My thought
- Add to `TypedValueParser` a `fn value_name(&self) -> String` with an inherent impl that returns `VALUE`
  - Update various built-in value parsers to return a meaningful name
  - Add to `TypedValueParser` a `fn deferred_name(&self, name: impl Fn() -> String) -> NamedValueParser` so it can be added to closures, etc
  - Adjust things so deriving `ValueEnum` will also allow setting `deferred_name`, with the default being the type's name, turned into SCREAMING_CASE
  - Add a setting to some value parsers so users can decide between showing the values vs name
- Update `--help` to show the value parser's `value_name` for flags
  - Is there anything we can do for positionals?
  - Is the divergence between named and positional values worth it?

So this would look like
- `--option <BOOL>` (placeholder) or `--option <true|false>` (literal
- `--option <PATH>`
  - [clio](https://docs.rs/clio/latest/clio/) could then specialize this further as `--option <FILE>` and `--option <DIR>`
- `--option <NUM>` (placeholder) or `--option <10..30>` (literal)
- `--option <MY_ENUM>` (placeholder) or `--option <foo|bar>` (literal)

---

_Comment by @tigerros on 2023-10-02 18:36_

> * Clutter in the help output
> * Not being in the user's terms (while most CLI users are programmers, not all are), even if its "unsigned 32 bit integer".

By making it optional both of these issues are solved, no?

---

_Comment by @epage on 2023-10-02 20:01_

In clap, we have to balance features with compile time, binary size, and API size.   Adding the suggested solution for a niche would hurt the whole.   If you are set on it being implemented that way, you can extend your doc comments to include the type information.

---

_Comment by @epage on 2023-11-06 19:09_

We talked about this some in the WG-CLI meeting

We'd like to keep configuration down to a minimum due to compile time + binary size concerns.  This means we'd likely not want to make placerholder vs literal configurable.  In that case, we leaned towards literals (when a meaningful enough one can be made).  If we default to literals, then working around it with `value_name("10..30")` requires them to duplicate information they provide to clap.  On the other hand, if we default to literals, then the work around is to do `value_name("NUM")` which is a unique textual representation.

---

_Comment by @epage on 2023-11-06 19:24_

Current proposal

Add:
```rust
trait TypedValueParser {
    // ... existing

    /// Fallback for [`Arg::value_name`] for options
    fn get_value_name(&self) -> Option<String> {
        None
    }

    /// Fallback for [`Arg::value_name`] for options
    fn value_parser(self, name: fn() -> String) -> NamedValueParser {
        NamedValueParser { name, self }
    }
}

pub NamedValueParser {
    name: fn() -> String,
    inner: impl TypedValueParser
}

// ....

let value_names = if let Some(value_names) = arg.get_value_names() {
    value_names
} else if arg.is_positional() {
    [arg.get_id()]
} else {
    self.get_value_parser().get_value_names()
};
```

Hmm, the main problem with this is this mostly helps `clap_derive` and `clap_derive` [implicitly calls `Arg::value_name`](https://github.com/clap-rs/clap/blob/master/clap_derive/src/derives/args.rs#L252).  So we'd need to decide if we wanted to make that conditional or not.

---

_Comment by @epage on 2023-11-09 16:25_

Just realized that a way to force showing of the literal would be to respect hide_possible_values.  This would help cases like #5203.

---

_Referenced in [clap-rs/clap#5392](../../clap-rs/clap/issues/5392.md) on 2024-11-14 18:07_

---

_Comment by @epage on 2025-05-22 13:49_

In addition to help, we should ensure we cover the "missing required argument" error as mentioned in #6008.

---

_Referenced in [clap-rs/clap#6008](../../clap-rs/clap/issues/6008.md) on 2025-05-22 13:49_

---

_Referenced in [clap-rs/clap#6056](../../clap-rs/clap/issues/6056.md) on 2025-08-06 16:50_

---

_Referenced in [clap-rs/clap#6109](../../clap-rs/clap/issues/6109.md) on 2025-08-25 14:50_

---

_Referenced in [clap-rs/clap#380](../../clap-rs/clap/issues/380.md) on 2025-09-08 15:24_

---
