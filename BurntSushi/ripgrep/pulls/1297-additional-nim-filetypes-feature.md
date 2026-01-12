```yaml
number: 1297
title: "Additional nim filetypes [feature]"
type: pull_request
state: merged
author: hiteshjasani
labels: []
assignees: []
merged: true
base: master
head: nim
created_at: 2019-06-11T02:18:01Z
updated_at: 2019-06-12T20:31:38Z
url: https://github.com/BurntSushi/ripgrep/pull/1297
synced_at: 2026-01-12T18:23:13Z
```

# Additional nim filetypes [feature]

---

_@hiteshjasani_

Add nim build configuration (.nimble) and code filtering (.nimf) file types.


---

_Comment by @BurntSushi on 2019-06-11 11:43_

I don't use nim, but just to double check, are the build configuration and code filtering files actual Nim source code? For example, the `rust` file type does not search `Cargo.toml`.

---

_Comment by @hiteshjasani on 2019-06-11 12:25_

That's a very good question.  Let me review how other filetypes are handled and I'll do another commit with changes.  At first blush, it may make sense to break out these files from the main nim source.


---

_Comment by @hiteshjasani on 2019-06-11 12:53_

Added two new categories:

* `nimf` - contains nim templating
* `nimscript` - contains nimscript files and nimble files (which are nimscript files)

Here's the thinking behind the new categorization.

* [Nimscript](https://nim-lang.org/docs/nims.html) files are a subset of nim language.  `.nimble` files are similar to `Rakefile` and `Makefile` in that they contain code as well as configuration.  They aren't part of the main codebase so it does make sense to break them out separately.
* Templates in nim are called [Source Code Filters](https://nim-lang.org/docs/filters.html) and use the `.nimf` extension.  They would be similar to `.jsp`, jinja templates or `.lhs`.  While they are most likely part of the main codebase, they also have extra cruft in them.  You want to be able to search them, but maybe only in certain cases.
* There's a little inconsistency with how templating is handled in ripgrep.  For some languages such as Java, PHP or Haskell, template files (.jsp, .php, .lhs) are considered part of the source code files.  For Python, it's broken out into separate Jinja templates.  Personally I think breaking it out, when possible, is the better solution.


---

_Comment by @BurntSushi on 2019-06-11 13:08_

Thanks! Would you perhaps mind getting another Nim programmer or two to chime in and sign off on this? Once a new file type is created, I generally don't want to remove it.

---

_Comment by @BurntSushi on 2019-06-11 13:11_

> They aren't part of the main codebase so it does make sense to break them out separately.

This isn't quite the right calculus I think. We can't really use reasons like "part of the main codebase" because ripgrep doesn't have the context to make those decisions. For example, in the Rust world, there is a `build.rs` file which is normal Rust code, but is only used for build configuration and is arguably also not part of the main codebase. Nevertheless, using `-trust` will search that file.

Consistency isn't really the main goal here. I guess it's really about user expectations. If you specify `-tnim`, does the user expect Nimble files to be searched, since they are Nim source code? The same thing applies to templates. This is where I'd like to hear opinions from other Nim programmers.

(I'm happy to go along with whatever y'all choose makes sense for your ecosystem.)

---

_Comment by @hiteshjasani on 2019-06-11 16:04_

Nim is a relatively young language -- it's quickly approaching version 1.0 -- and I haven't seen (from my humble perspective) any large codebases using it -- other than the language codebase itself.  My perspective is colored by working with different languages and large codebases over the years.  When I've searched in them, I've needed to do it in priority order to find the specific needles in the haystacks.    So my comments are more about what I've experienced in general rather than with nim specifically right now.

I've generally prioritized my search as follows:

* primary - main source code files (client and server)
  * Refining search within the main codebase is generally easy with good directory structures
* secondary - ancillary files that contain some business logic or configuration (template html files, template sql files, runtime configuration)
* tertiary - build scripts, build configuration, one off db changes, db migrations

So my proposal is the following file_types:

```shell
# search all *.nim files
rg -tnim foo

# search all *.nimf source code filter (template) files
rg -tnimf foo

# search all *.nims and *.nimble files
rg -tnimscript foo
```

You know I just realized the filename globbing parameter to ripgrep.  So without any changes to ripgrep, the following works and makes me happy.

```shell
# search all *.nim files
rg -g*.nim foo

# search all *.nimf source code filter (template) files
rg -g*.nimf foo

# search all *.nims and *.nimble files
rg -g*.nims -g*.nimble foo

# search all nim related files
rg -g*.nim* foo
```

@araq, @dom96 do you have any preferences on nim filetypes in ripgrep?  I'll try to summarize below.

A quick summary of the options:

1.  Do nothing - ripgrep supports filename globbing so you can choose the specific file extensions to search.  Can also choose to just do nothing for now and see how things play out in the long run.
2. Add `nimf` and `nimscript` file_types to ripgrep - my current commit proposal.
3. Add `*.nims`, `*.nimble` and `*.nimf` to the `nim` file_type - puts all file extensions with some amount of nim source code in them under a single file type.

Since learning about the filename globbing, I'm happy with any of the options including doing nothing for now and just taking a wait and see approach.


---

_Comment by @dom96 on 2019-06-11 20:49_

> Add *.nims, *.nimble and *.nimf to the nim file_type - puts all file extensions with some amount of nim source code in them under a single file type.

I would say this option makes the most sense, for more granularity you can always use the ``-g`` flag. As far as expectations are concerned, I would expect the "nim" type to search through all Nim-related source code files which includes .nim, .nimf, .nimble and .nims. There is also ``.nim.cfg`` which is an INI-like format that doesn't contain any Nim source code so we should exclude it I think.

----

@BurntSushi By the way, would you accept a PR to add support for style insensitive search to ripgrep? That would allow finding ``foo_bar`` and ``fooBar`` using just the search term ``fooBar``.

---

_@BurntSushi approved on 2019-06-12 17:04_

LGTM! Thanks for chiming in @dom96!

---

_Comment by @BurntSushi on 2019-06-12 17:05_

@dom96 

> By the way, would you accept a PR to add support for style insensitive search to ripgrep? That would allow finding foo_bar and fooBar using just the search term fooBar.

Most likely, yes, as long as the implementation is simple. Are you envisioning a transformation on a literal? If so, I could see that working. A transformation on an arbitrary regex might be a bit trickier.

---

_Merged by @BurntSushi on 2019-06-12 18:02_

---

_Closed by @BurntSushi on 2019-06-12 18:02_

---

_Branch deleted on 2019-06-12 18:32_

---

_Comment by @dom96 on 2019-06-12 20:31_

> Are you envisioning a transformation on a literal? If so, I could see that working.

Yeah, I think that would be good enough and should be fairly trivial to implement. @hiteshjasani interested? :)

---
