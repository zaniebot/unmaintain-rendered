---
number: 2476
title: Allow adding usage examples to help text
type: issue
state: closed
author: dbrgn
labels: []
assignees: []
created_at: 2021-05-09T18:30:03Z
updated_at: 2021-05-09T19:08:27Z
url: https://github.com/clap-rs/clap/issues/2476
synced_at: 2026-01-10T01:27:18Z
---

# Allow adding usage examples to help text

---

_Issue opened by @dbrgn on 2021-05-09 18:30_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

The [tealdeer](https://github.com/dbrgn/tealdeer/) help text currently includes some invocation examples after the regular help text (using docopt):

```
Usage:

    tldr [options] <command>...
    tldr [options]

Options:

    -h --help             Show this screen
    -v --version          Show version information
    -l --list             List all commands in the cache
    -f --render <file>    Render a specific markdown file
    -o --os <type>        Override the operating system [linux, osx, sunos, windows]
    -L --language <lang>  Override the language settings
    -u --update           Update the local cache
    -c --clear-cache      Clear the local cache
    -p --pager            Use a pager to page output
    -m --markdown         Display the raw markdown instead of rendering it
    -q --quiet            Suppress informational messages
    --show-paths          Show file and directory paths used by tealdeer
    --config-path         Show config file path (deprecated)
    --seed-config         Create a basic config
    --color <when>        Control when to use color [always, auto, never] [default: auto]

Examples:

    $ tldr tar
    $ tldr --list

To control the cache:

    $ tldr --update
    $ tldr --clear-cache

To render a local file (for testing):

    $ tldr --render /path/to/file.md
```

I'm currently migrating the codebase to Clap v3. Is there a way to add these "usage examples" to the help text?

### Describe the solution you'd like

It might be useful if usage examples could be attached to a CLI, which automatically show up in the help text.

This could be generalized to not just examples, but any additional help text. (Or is this already possible? I searched issues / discussion but could not find anything.)

Note that we'd probably need at least _some_ kind of semantics attached to this "additional help text" data, otherwise help coloring would probably break.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @dbrgn on 2021-05-09 18:30_

---

_Closed by @pksunkara on 2021-05-09 19:08_

---

_Locked by @clap-rs on 2021-05-09 19:08_

---
