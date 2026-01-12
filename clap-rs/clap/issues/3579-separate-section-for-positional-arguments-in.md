```yaml
number: 3579
title: Separate section for Positional Arguments in generated manpages
type: issue
state: open
author: ducaale
labels:
  - C-enhancement
  - S-waiting-on-design
  - A-man
assignees: []
created_at: 2022-03-26T16:52:58Z
updated_at: 2022-03-28T20:20:36Z
url: https://github.com/clap-rs/clap/issues/3579
synced_at: 2026-01-12T16:14:15Z
```

# Separate section for Positional Arguments in generated manpages

---

_@ducaale_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.0

### Describe your use case

N/A

### Describe the solution you'd like

I would like `clap_mangen` to group positional arguments into their own section.
```
POSITIONAL ARGUMENTS
       PATTERN
           A regular expression used for searching. To match a pattern beginning with a dash, use the -e/--regexp
           option.

       PATH
           A file or directory to search. Directories are searched recursively. Paths specified explicitly on the
           command line override glob and ignore rules.

OPTIONS
       -A, --after-context NUM
           Show NUM lines after each match.

           This overrides the --context flag.

       --auto-hybrid-regex
           When this flag is used, ripgrep will dynamically choose between supported regex engines depending on the
           features used in a pattern. When ripgrep chooses a regex engine, it applies that choice for every regex
           provided to ripgrep (e.g., via multiple -e/--regexp or -f/--file flags).
```

### Alternatives, if applicable

Because `clap_mangen` allows rendering sections as needed, I can copy code from `clap_mangen` and customise it as needed

```rs
let items: Vec<_> = app
    .get_arguments()
    .filter(|i| !i.is_hide_set())
    .map(|i| i.clone())
    .collect();

let man = clap_mangen::Man::new(app);
let mut buf = Vec::new();

man.render_title(&mut buf).unwrap();
man.render_name_section(&mut buf).unwrap();
man.render_synopsis_section(&mut buf).unwrap();
man.render_description_section(&mut buf).unwrap();

let mut roff = roff::Roff::new();
roff.control("SH", ["POSITIONAL ARGUMENTS"]);
for pos in items.iter().filter(|a| a.is_positional()) {
    let mut name = pos.get_value_names().unwrap()[0];
    let header = vec![roff::bold(&name)];
    let mut body = vec![];
    if let Some(help) = pos.get_long_help().or_else(|| pos.get_help()) {
        body.push(roff::roman(&help.to_string()));
    }
    roff.control("TP", []).text(header).text(body);
}
roff.to_writer(&mut buf).unwrap();
```


### Additional Context

N/A

---

_Label `C-enhancement` added by @ducaale on 2022-03-26 16:52_

---

_Label `S-waiting-on-design` added by @epage on 2022-03-26 17:10_

---

_Label `A-man` added by @epage on 2022-03-26 17:10_

---

_Comment by @epage on 2022-03-26 17:12_

What happens if put them in a "POSITION ARGUMENTS" `help_heading`, is the result good enough for your purposes?

Any thoughts @larswirzenius or @sondr3 about what we should be doing generally?  Putting them in a separate section would match with `--help`.

---

_Comment by @ducaale on 2022-03-26 17:18_

>What happens if put them in a "POSITION ARGUMENTS" help_heading, is the result good enough for your purposes?

Unfortunately, I don't think that works at the moment.

---

_Referenced in [clap-rs/clap#3580](../../clap-rs/clap/pulls/3580.md) on 2022-03-26 17:19_

---

_Comment by @epage on 2022-03-26 18:03_

> > What happens if put them in a "POSITION ARGUMENTS" help_heading, is the result good enough for your purposes?
> 
> Unfortunately, I don't think that works at the moment.

Right, looks like we don't support help headings, thats discussed in #3363 

---

_Comment by @epage on 2022-03-26 18:04_

Found where this was discussed in the https://github.com/clap-rs/clap/pull/3174#discussion_r768905319

epage:
> So we are putting named arguments before positionals. I see that `git` does this.
> 
> Whether this or many other formatting details, how standardized are these practices? What kinds of things are people likely to disagree with and want different from what is provided?

@sondr3 
> I honestly have no idea, I though about maybe sorting them alphabetically but from the manpages I looked at there seemed to be no good rhyme or reason to the order for them, so I just used an arbitrary one.



---

_Comment by @ducaale on 2022-03-26 18:34_

Thanks for linking previous discussions about this. I have been using curl and ripgrep as a reference but didn't look into other programs that have positional args.

* ripgrep: positional arguments are grouped into their own section before the options section
* curl: each positional argument has its own section and these come before the options section

By the way, isn't git's positional args the same as clap's subcommands? If that is the case, then `clap_mangen` already renders subcommands into its own section after the options section.




---

_Comment by @larswirzenius on 2022-03-27 06:59_

From a long-time man page mass consumer point of view, putting positional arguments into their own section would be unusual, and as such would make the man page different from other man pages. It might be better, of course, but for someone who uses man pages as a quick reference, any unusual structure makes it harder to find what I want quickly.

A subsection would make more sense.

However, as a personal opinion, I would structure the command line argument structure so that there are very few different kinds of positional arguments. For grep, a pattern and filenames are needed, but I don't think that warrants its own section, or even subsection.

---

_Comment by @epage on 2022-03-28 18:11_

> By the way, isn't git's positional args the same as clap's subcommands? If that is the case, then clap_mangen already renders subcommands into its own section after the options section.

If you run `man git-diff`, you'll see `<path>...` at the very bottom of `OPTIONS`.

---

_Comment by @ducaale on 2022-03-28 19:55_

Oh, I see.

Now that I know `clap_mangen` closely follows how git's man page is structured, I am changing my opinion and wouldn't mind if this issue and PR #3579 get closed.

---

_Comment by @epage on 2022-03-28 20:20_

I wouldn't say its a requirement to follow git but that we are looking to prior art to help guide what the man generation should look like and git is a good example because it both gets a lot of attention and is complex enough to have examples of anything we come across. 

---

_Referenced in [chmln/sd#231](../../chmln/sd/pulls/231.md) on 2023-10-02 01:18_

---
