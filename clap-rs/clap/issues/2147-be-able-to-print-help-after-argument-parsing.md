```yaml
number: 2147
title: Be able to print help after argument parsing
type: issue
state: closed
author: piegamesde
labels: []
assignees: []
created_at: 2020-10-01T17:38:42Z
updated_at: 2020-10-01T17:46:19Z
url: https://github.com/clap-rs/clap/issues/2147
synced_at: 2026-01-12T16:14:12Z
```

# Be able to print help after argument parsing

---

_@piegamesde_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Describe your use case

I have an app with a few "public" subcommands, but I also set `.setting(AppSettings::AllowExternalSubcommands)` to allow more free style input. After parsing the arguments, I first check against all the registered subcommands. If none of them matches, I take the custom command using `matches.subcommand_name();`. I do a little parsing and go on with the application if it works. But if the external subcommand is not recognized, I'd like to print the usual help message. Sadly, `app.print_long_help();` does not work since `app` got moved out while parsing.

### Describe the solution you'd like

Maybe the easiest thing would be to add `get_matches_from_borrow` or something that does not consume my `App` struct. Alternatively, add help printing functionality to `ArgMatches` maybe?

### Workarounds

- Use `get_matches_from_safe_borrow` and do a lot of additional work
- Clone the `App` before parsing (or generate it twice)
- Print the help message to a buffer before parsing just in case

### Additional context

In theory, I could accept any external command value. But I prefer to restrict the values to those matching a special regex. This way, I can point the user to the "proper" usage and the app won't behave weirdly on some faulty input.


---

_Label `T: new feature` added by @piegamesde on 2020-10-01 17:38_

---

_Comment by @pksunkara on 2020-10-01 17:42_

https://docs.rs/clap/3.0.0-beta.2/clap/struct.App.html#method.get_matches_mut

---

_Closed by @pksunkara on 2020-10-01 17:42_

---

_Comment by @piegamesde on 2020-10-01 17:46_

Neat, thanks

---
