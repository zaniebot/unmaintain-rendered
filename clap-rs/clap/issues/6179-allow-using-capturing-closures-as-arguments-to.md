---
number: 6179
title: "Allow using capturing closures as arguments to the `Command::defer` function"
type: issue
state: open
author: calebkiage
labels:
  - C-enhancement
  - S-waiting-on-decision
assignees: []
created_at: 2025-11-08T19:09:43Z
updated_at: 2025-11-16T15:59:35Z
url: https://github.com/clap-rs/clap/issues/6179
synced_at: 2026-01-10T01:28:23Z
---

# Allow using capturing closures as arguments to the `Command::defer` function

---

_Issue opened by @calebkiage on 2025-11-08 19:09_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.17

### Describe your use case

I have an application that builds a CLI and uses defer for some sub-commands that are expensive to construct. The first phase of the application loads a data structure with shared configs and computations that will be used to construct the sub-commands. Currently, it's not possible to use defer with the shared state since defer needs a function pointer. Loading the shared state can be expensive and I would like to do it once.

### Describe the solution you'd like

Allowing code like the following will be great since it allows having the option to share preloaded data in the defer call.

```rust
let shared_state = ['a', 'b', 'c']; // Can be expensive e.g. loaded from network. might also be used in the command handling section

let mut root = Command::new("test").defer(move |cmd| {
    for c in shared_state {
        cmd.subcommand(Command::new(c));
    }
});
```

### Alternatives, if applicable

We could also load all the data in the closure to avoid captures and also load the same data again elsewhere to use it, but it would be inefficient.

### Additional Context

_No response_

---

_Label `C-enhancement` added by @calebkiage on 2025-11-08 19:09_

---

_Label `S-triage` added by @calebkiage on 2025-11-08 19:09_

---

_Referenced in [clap-rs/clap#6180](../../clap-rs/clap/pulls/6180.md) on 2025-11-08 19:11_

---

_Comment by @epage on 2025-11-10 14:58_

`Command::defer` was primarily created for derives though most work to use it is deferred to clap v5.

The proposed solution here (and written out in #6180) doesn't seem like it would help accomplish the specified goal of 

> allows having the option to share preloaded data

`Command` has no lifetimes and by extension  the closure cannot have any lifetimes so #6180 requires everything to be moved into the closure, removing the ability to share state.

It would also be helpful to get into the use case more.  Why are you using `Command::defer` (as in what performance impact did you observe), what are the requirements behind this shared state (e.g. could a `OnceLock` be used?), etc.

Note that #6180 doesn't just allow creation of stateful deferreds but also changes behavior for `Command::defer` to make multiple calls additive which is not discussed here and is a breaking change.

---

_Comment by @calebkiage on 2025-11-10 17:43_

> Command has no lifetimes and by extension the closure cannot have any lifetimes so https://github.com/clap-rs/clap/pull/6180 requires everything to be moved into the closure, removing the ability to share state.

The solution allows sharing state if you use an Arc and clone it. I didn't want to add lifetimes to the Command object since that would be a big change and unlikely to be merged.

>  Why are you using Command::defer (as in what performance impact did you observe)

I'm creating a CLI tool that takes an OpenAPI document, indexes the paths in a database and generates a CLI over the endpoints. Think of the Github CLI, but can work with any indexed API from the same tool. I'm using the [Microsoft Graph API](https://github.com/microsoftgraph/msgraph-metadata/blob/04e061aae911c49242ecba96a5368bf35763b321/openapi/v1.0/openapi.yaml) for performance testing since it has over 10k endpoints.

I've had multiple iterations of the tool, because the naive implementation where I built up the full command tree and made a Command object was noticeably slow. It would load data that was never used when running the CLI. The process of building up the data structure needed to create the Command objects is also slow since it involves a mix of loading endpoint segment associations from the DB and deserializing endpoint details from the OpenAPI document (for help and parameter information). I haven't explored using a OnceLock. I'll look into it.

What the current iteration does is build a different version of the CLI based on the passed in arguments. i.e. say I have an endpoint like `GET /users/{id}/documents/{id}/edits` and the user passed in the `-h` arg. I'll only create a Command with a `users` root command and `documents` subcommand. The CLI won't have the `edits` subcommand since it's not needed. This is fast but means I have to duplicate some of the logic clap is meant to handle with defer. I would have loved to use defer for all the commands to only load commands when needed. However, it being a function pointer means I can't share the db object or the configuration object and want to avoid creating it every time or deserializing the OpenAPI document for each call to defer.

This solution gives me what I need to get started on that.

I can remove the chaining. I just thought that it's more intuitive that replacing the existing function silently.

---

_Label `S-triage` removed by @epage on 2025-11-10 21:09_

---

_Label `S-waiting-on-decision` added by @epage on 2025-11-10 21:09_

---

_Comment by @epage on 2025-11-10 21:09_

I'll have to think on this more.

> I can remove the chaining. I just thought that it's more intuitive that replacing the existing function silently.

Clap has a mixture of replacing and appending arguments and it is a big of a mess.  I'm tempted to make it consistent to only have replace.

---

_Comment by @calebkiage on 2025-11-16 15:59_

Ok, I'll close the PR for now to reduce the number of open PRs.

---
