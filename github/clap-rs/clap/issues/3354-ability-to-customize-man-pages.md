---
number: 3354
title: Ability to customize man pages
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - S-waiting-on-design
  - A-man
assignees: []
created_at: 2022-01-28T21:00:12Z
updated_at: 2024-10-07T20:58:52Z
url: https://github.com/clap-rs/clap/issues/3354
synced_at: 2026-01-07T13:12:19-06:00
---

# Ability to customize man pages

---

_Issue opened by @epage on 2022-01-28 21:00_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0

### Describe your use case

We can't extract all man page sections from an `App` and need to allow the user specifying more sections.

In addition, some sections won't be good enough

See #552 for ideas for sections

### Describe the solution you'd like

I think there are two solutions
- Allow injecting data
- Allow the user to control which sections get generated

### Alternatives, if applicable

_No response_

### Additional Context

This was deferred out of #3174 

---

_Label `C-enhancement` added by @epage on 2022-01-28 21:00_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-28 21:00_

---

_Label `A-man` added by @epage on 2022-01-28 21:00_

---

_Referenced in [clap-rs/clap#3364](../../clap-rs/clap/pulls/3364.md) on 2022-01-28 21:53_

---

_Comment by @epage on 2022-02-10 22:27_

#3364 expanded `clap_mangen::Man` from just `render`to also let you render individual sections so you can pull the clap-generated section into your own man template, of whatever form that takes.

I guess the question is if that is good enough or if we also want to allow people to inject their own data into our template.  If so, what should that look like?

An early version of `clap_mangen` had
```rust
    /// Add a custom section to the man pages.
    pub fn custom_section(
        mut self,
        title: impl Into<String>,
        body: Vec<impl Into<String>>,
    ) -> Self;
```
https://github.com/sondr3/clap/blob/9605d5c4e3148d91034446e902b31691a52eb064/clap_man/src/lib.rs#L98
- All of the custom sections had a hard coded location towards the end.  There was not a way to choose where to locate the section, to override, or merge with a built-in section
  - Env variables is an example of where merging might be interesting since we can extract what env variables clap knows about and mix that in with what the user also deals with.
- This worked with `String`.  Is that what we want or something from `roff`

---

_Comment by @taladar on 2022-02-11 06:48_

@epage asked me in the discussion I made https://github.com/clap-rs/clap/discussions/3442 to post this here too:

clap_mangen currently does not do much yet to generate man pages that can not directly be fully derived from the clap configuration.

I think there are a couple of man page sections which are fairly standardized (as in a lot of CLI program man pages have them if applicable) but they are also highly modular.

It might be useful to have some way to assemble these in clap_mangen from different sources to avoid duplicate effort on writing the same documentation over and over again.

For the pieces from which these are assembled it might make sense to stick to standard types that require no dependencies so libraries could easily include a relevant snippet or two (e.g. database library could document the format of its DATABASE_URL variable or a logging library could document RUST_LOG).

Since some libraries might want to include several related ones which should be displayed in a group in a specific order it would probably be useful to allow adding vectors of several such objects too. Potentially the type should also include section headings.

Some of the sections might want to generate some content from clap config and the rest could be specified manually.

The following sections come to mind (after using grep on my man1 folder) 

### Environment Variables

Usually these list all the environment variables (besides the standard ones like PATH or HOME) that influence the functioning of the program. A good example for a long list of variables would be the git man page.

### Files

The files section usually contains config files, some relative, some using variables like the XDG ones for their paths. Some common groups of files are those where several locations are tried in a specific order until a file is found or those where settings missing from one file are looked up in another (e.g. local, user-wide, system-wide).

Usually the locations are listed, followed by a block of explanation

### Exit Status

This section documents the various exit statuses and their meaning, each exit status is fairly independent though this is less likely than other sections to need inclusion of values provided by libraries.

It might also make sense to allow the user to include some information on when the program might be expected to panic or have other abnormal exits, either in this or in a dedicated section rendered near this one.

### See Also / Further Documentation

This section could easily include links for the main application but also for libraries used, e.g. when using sqlx for a server application one might want to link to the format for migrations and sqlx migrate run,...

This is also where other man pages should be mentioned. Perhaps clap_mangen could automatically include the ones it generates here if a crate has multiple binaries or if support for generating config file man pages is added later.

### Reporting Bugs

This usually includes information on how to report bugs, probably less modular than some of the other sections but could still benefit from modular assembly if there are libraries involved that require their bugs to be reported elsewhere.

### Copyright / Acknowledgements / License

This could benefit from modularity to allow people to include snippets about libraries they use that require them to mention their use.

### Author / Authors

This might be similar to the above but more about the authors of the current program, not sure if it would benefit from modularity but it certainly couldn't hurt to be able to add independent snippets per author.

### Bugs / Known problems / Caveats

Making this modular would allow library authors to just export a symbol that the CLI program author includes and the known bugs in the library change automatically without further work on the CLI program author's part.

### Examples

This is probably less about libraries but it could still be beneficial to assemble examples from different parts of the CLI program's codebase.

### History

This section often seems to include less of a changelog and more bits and pieces about the historical context of a program, e.g. in which historic OS versions it first appeared (or the programs which inspired its creation) or in which standards it was influded

### Conclusion

I think this could be done in a relatively simple way for most of these sections, most likely as a type similar to

(Option<String>, Vec<(String, String)>)

where the first String would be a heading within the relevant section and the second would be a list of e.g. Environment Variable name and a string documenting it. Obviously for certain sections it would have to be modified, e.g. for the files section you might want the name to be a Vec<String> instead.

It might also be possible to use custom types from some light-weight crate most libraries interested in the feature wouldn't mind including. Ideally the format should not be too clap-specific  or man page specific though to maximize adoption.

So, after writing all of this I was wondering what others here think about the idea.

---

_Comment by @epage on 2022-02-11 13:19_

@taladar could you elaborate more on your proposed solution, including how we should integrate that into our current processing?

Example questions for us to be considering
- How does the user control ensure their custom sections are placed in the right location?
  - Depending on the solution, this might break down into two parts, commonly accepted static section names (e.g. Environment Variables) and sections we don't know of
- How can the user control ordering?
  - This deals with ensuring a commonly accepted order, dealing with sections we don't know their commonly accepted order for, and custom ordering
- How can the user replace an automatically generated section?
- How can the user disable an automatically generated section?

Note: a valid answer can be "we won't support that" (e.g. custom ordering of commonly accepted sections).  If so, be sure to give your reasoning.

---

_Comment by @str4d on 2024-01-09 06:38_

`man man-pages` gives the following order for these conventional or suggested sections:
- `NAME`
- `SYNOPSIS`
- `CONFIGURATION` (Normally only in Section 4)
- `DESCRIPTION`
- `OPTIONS` (Normally only in Sections 1, 8)
- `EXIT STATUS` (Normally only in Sections 1, 8)
- `RETURN VALUE` (Normally only in Sections 2, 3)
- `ERRORS` (Typically only in Sections 2, 3)
- `ENVIRONMENT`
- `FILES`
- `VERSIONS` (Normally only in Sections 2, 3)
- `CONFORMING TO`
- `NOTES`
- `BUGS`
- `EXAMPLES`
- `AUTHORS` (Discouraged)
- `REPORTING BUGS` (Not used in man-pages)
- `COPYRIGHT` (Not used in man-pages)
- `SEE ALSO`

As another data point, I currently use the `man` crate for generating manpages. It uses a builder pattern to generate the following pattern of sections (most of which are optional) in this order:
- `NAME`
- `SYNOPSIS`
- `DESCRIPTION`
- `FLAGS`
- `OPTIONS`
- `ENVIRONMENT`
- Arbitrary sequence of custom sections.
- `EXIT STATUS`
- `EXAMPLES`
- `AUTHORS` / `AUTHORS`

I don't have a personal need to reorder these sections; I just want to generate manpage layouts that are familar to their consumers. The only one I specifically want to use (on top of what `clap_mangen` currently supports) is `EXAMPLES`.

I'd also like to be able to have more control over the `EXTRA` section:
- I currently put a few simple examples into `Command::after_help`, but I'd want to replace those with an `EXAMPLES` section.
- My `Command::after_help` text includes a hyphenated bullet-point list, the newlines for which get collapsed.

I could potentially achieve these by replacing the `after_help` with different text, but IDK how to get lists to render correctly via that (and I see that `roff 0.2` provides no support for this, so it would be up to `clap_mangen`).

---

_Referenced in [clap-rs/clap#5768](../../clap-rs/clap/pulls/5768.md) on 2024-10-07 09:02_

---

_Comment by @aparcar on 2024-10-07 15:10_

Hi, based on the feedback of #5768 I'd like to join this conversation.

> All of the custom sections had a hard coded location towards the end. There was not a way to choose where to locate the section, to override, or merge with a built-in section

From my understanding it's possible to call the order of sections individually instead of using the `render` function, this looks like the answer to most questions raised in this thread. An example below:

```rust
        let man = clap_mangen::Man::new(cmd);
        let _ = man.render_title(&mut std::io::stdout());
        let _ = man.render_name_section(&mut std::io::stdout());
        let _ = man.render_synopsis_section(&mut std::io::stdout());
        let _ = man.render_subcommands_section(&mut std::io::stdout());
        let _ = man.render_options_section(&mut std::io::stdout());
        let _ = man.render_custom_section(&mut std::io::stdout(), "SEE ALSO", SEE_ALSO_MAN);
```

The approach below allows to change the desired order (subcommands before options) and also add a custom section wherever desired, in this case (boringly) at the end.

> Env variables is an example of where merging might be interesting since we can extract what env variables clap knows about and mix that in with what the user also deals with.

For this special case one could pass environment variables plus description as a list of dicts, let `clap` merge and sort it with the defined env vars and print it.

> This worked with String. Is that what we want or something from roff

For readability I'd accept strings as input which may contain Markdown, limited to *roman*, *italic* and *bold*, following the current capabilities of [Roff](https://docs.rs/roff/latest/roff/#functions). 
 
> * How does the user control ensure their custom sections are placed in the right location?

See example above

>   * Depending on the solution, this might break down into two parts, commonly accepted static section names (e.g. Environment Variables) and sections we don't know of

One could define an enum for the allowed sections but then again, what's the advantage of that compared to allowing it freely? 

> * How can the user control ordering?

See example above

>   * This deals with ensuring a commonly accepted order, dealing with sections we don't know their commonly accepted order for, and custom ordering

See example above

> * How can the user replace an automatically generated section?

A user could just call something like `man.render_custom_section([...], "SYNOPSIS", CUSTOM_SYNOPSIS)` and inject it's own text into something that is normally offered via `man.render_synopsis_section`

> * How can the user disable an automatically generated section?

Just don't call that specific section.

> Note: a valid answer can be "we won't support that" (e.g. custom ordering of commonly accepted sections). If so, be sure to give your reasoning.

Since this discussion is open for more than two years, I'd find it helpful to add the low level function mentioned in https://github.com/clap-rs/clap/issues/3354#issuecomment-1035595159 and then continue the work for higher level "convenience" function, like the semi-automatic `ENVIRONMENT` section.

---

_Comment by @epage on 2024-10-07 17:22_

@aparcar I think something is missing about your post.  It sounds like you are saying that calling individual sections on `Man` is good enough for these use cases but then bring up adding custom sections. I'm not seeing justification for why you think custom sections are still needed despite people being able to render sections as needed, including injecting their own content between them.

---

_Comment by @aparcar on 2024-10-07 19:43_

@epage Customizing man pages means in my understanding two things:
* Change order or sections or disable them
* Add custom content

The former is possible via this commit https://github.com/clap-rs/clap/commit/5d0ef1f420917fc246d972ba0955a48a88c8c44b (thanks), the latter would be possible with a "low level" API call like suggested in #5768 

I implemented the above mentioned example again without the `render_custom_section()` from #5768 and it works fine, too, just looks strange:

```rust
        let mut roff = Roff::default();
        let man = clap_mangen::Man::new(cmd);
        let _ = man.render_title(&mut std::io::stdout());
        let _ = man.render_name_section(&mut std::io::stdout());
        let _ = man.render_synopsis_section(&mut std::io::stdout());
        let _ = man.render_subcommands_section(&mut std::io::stdout());
        let _ = man.render_options_section(&mut std::io::stdout());
        roff.control("SH", ["EXIT STATUS"]);
        roff.text([roman(EXIT_STATUS_MAN)]);
        roff.control("SH", ["SEE ALSO"]);
        roff.text([roman(SEE_ALSO_MAN)]);
        roff.control("SH", ["STANDARDS"]);
        roff.text([roman(STANDARDS_MAN)]);
        roff.control("SH", ["AUTHORS"]);
        roff.text([roman(AUTHORS_MAN)]);
        roff.control("SH", ["BUGS"]);
        roff.text([roman(BUGS_MAN)]);
        let _ = roff.to_writer(&mut std::io::stdout());
```

I'm fine sticking to this approach, just thought the custom call would cover 99% of user cases.


---

_Comment by @epage on 2024-10-07 19:54_

> I implemented the above mentioned example again without the render_custom_section() from https://github.com/clap-rs/clap/pull/5768 and it works fine, too, just looks strange:

So the problem is not the ability to add custom content but the ergonomics (all of this assuming explicitly specifying all sections is accepted).

I doubt we'd have a `custom_section` or `render_custom_section` take a `&str` or `String` but would instead take ROFF.  We wouldn't want to limit the formatting of such a custom section.

At that point, there won't be much of a difference between what you wrote and would would be done with a `render_custom_section`.

---

_Comment by @aparcar on 2024-10-07 20:13_

Regarding the ergonomics, would it make sense to allow basic markdown to allow some styling? The example code of `rustup.rs` includes lengthy strings, wouldn't it be handy to have Markdown support there which is rendered equally in `--help` and manages? This would render touching any ROFF stuff obsolete.

---

_Comment by @epage on 2024-10-07 20:58_

A markdown -> roff converter could be made that people could use.  If thats all thats done, then I don't think its worth building in.  This becomes tricky if people want `long_about` and other long text to be  interpeted as markdown.  Work along that lines should be discussed in its own issue.

---
