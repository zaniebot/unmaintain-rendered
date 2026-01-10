---
number: 6140
title: Custom sorting of arguments in help message
type: issue
state: open
author: thomas-zahner
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2025-09-25T13:04:59Z
updated_at: 2025-09-26T07:49:33Z
url: https://github.com/clap-rs/clap/issues/6140
synced_at: 2026-01-10T01:28:22Z
---

# Custom sorting of arguments in help message

---

_Issue opened by @thomas-zahner on 2025-09-25 13:04_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

master

### Describe your use case

By using [`next_display_order`](https://docs.rs/clap/latest/clap/struct.Command.html#method.next_display_order) I've managed to get clap sort arguments alphabetically in the `--help` message. However, I don't like the sorting behaviour of clap. I would like to sort my arguments by the long argument names first.

Currently my help message looks like:

```
  -V, --version
          Print version

  -X, --method <METHOD>
          Request method

          [default: get]
```

But I want `--method` to be before `--version`. This aligns with the other programs such as cURL. (see `curl --help` or `man curl`)

Sorting is done by the following code in help_template.rs:

```rust
fn option_sort_key(arg: &Arg) -> (usize, String) {
    // Formatting key like this to ensure that:
    // 1. Argument has long flags are printed just after short flags.
    // 2. For two args both have short flags like `-c` and `-C`, the
    //    `-C` arg is printed just after the `-c` arg
    // 3. For args without short or long flag, print them at last(sorted
    //    by arg name).
    // Example order: -a, -b, -B, -s, --select-file, --select-folder, -x

    let key = if let Some(x) = arg.get_short() {
        let mut s = x.to_ascii_lowercase().to_string();
        s.push(if x.is_ascii_lowercase() { '0' } else { '1' });
        s
    } else if let Some(x) = arg.get_long() {
        x.to_string()
    } else {
        let mut s = '{'.to_string();
        s.push_str(arg.get_id().as_str());
        s
    };
    (arg.get_display_order(), key)
}
```

I would like to swap the first `if let` with the second one for my use case.

### Describe the solution you'd like

If possible I would like to simply swap the two `if let` statements as mentioned above. Would you accept this change? Or do you think this is an unnecessary opinionated breaking change?

### Alternatives, if applicable

If you think this would be too opinionated and break the behaviour of too many users, can we make the sorting behaviour configurable, by somehow providing a sorting function to clap? This way `option_sort_key` would only be used if the user provided none.

### Additional Context

This if for [lychee-bin](https://github.com/lycheeverse/lychee/) where I want to sort the help message for better usability and align the output more closely to cURL's help message. See: https://github.com/lycheeverse/lychee/pull/1858

---

_Label `C-enhancement` added by @thomas-zahner on 2025-09-25 13:05_

---

_Label `S-triage` added by @thomas-zahner on 2025-09-25 13:05_

---

_Comment by @epage on 2025-09-25 16:15_

The paths forward for this are either
- Change our sort order
- Workaround it with `display_order`
- Wait for help generation to be broken out to allow easier customization without costs (#2913, #2914, see also #3476)

With clap, we try to balance
- Binary size
- Build times
- Usability
- Flexibility

The more customization we add, the larger our binary size and build times.  A single function / bool won't make a difference but its the general approach we take to these questions that can mean whether we add 1 or 30.

For usability, a problem we've found is there are diminishing returns for more customization.  The larger our API, the harder it is for people to find what they are looking for, the less of clap they use.  So adding a piece of help customization to the main API negatively affects all users.

As we provide a way to customize this, that means this is a lower priority.

As for what the default sort order should be, It's hard to say.  It would be good for someone to dig into the history of our current sort order to identify what went into the decisions that were made.  It would also be good to do a survey of various tools to see if there is a pattern to them to see what we can learn.

---

_Label `S-waiting-on-design` added by @epage on 2025-09-25 16:15_

---

_Label `S-triage` removed by @epage on 2025-09-25 16:15_

---

_Label `A-help` added by @epage on 2025-09-25 16:15_

---

_Comment by @thomas-zahner on 2025-09-26 07:49_

Thanks for the swift reply. I definitely understand your concerns and I agree that additional customization affects users negatively. I dug through the man pages (the help messages might differ for some programs, but that's probably rare) of various well known commands and came to the conclusion that the current behaviour by clap seems to be more common.

## Programs which order by short and long flags (as clap currently does)

```
cat
ls
mv
touch
ln
top
grep
df
du
nano
```

## ordered by long flags (the behaviour I personally prefer)

```
curl
rg
tail
```

## unordered (manual ordering)

This is not directly relevant to this issue, but still interesting to see that this is so common.

```
man 
fd
echo
wget
tree
cargo
rustc
chmod
chown
cp
rm
kill
uname
free
lscpu
ip
ss
hexdump
sed
gawk
diff
```

---
