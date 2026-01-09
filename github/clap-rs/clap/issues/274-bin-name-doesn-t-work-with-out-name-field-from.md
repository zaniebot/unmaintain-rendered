---
number: 274
title: "bin_name doesn't work with out name field. From YAML"
type: issue
state: closed
author: XAMPPRocky
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2015-09-23T10:42:33Z
updated_at: 2018-08-02T03:29:45Z
url: https://github.com/clap-rs/clap/issues/274
synced_at: 2026-01-07T13:12:19-06:00
---

# bin_name doesn't work with out name field. From YAML

---

_Issue opened by @XAMPPRocky on 2015-09-23 10:42_

When I changed the cli.yml file from `name` to `bin_name` the everything outputed to the `help` was changed except the name.
## CLI.yml

```
bin_name: Tokei
version: 1.1.1
author: Aaron P. <theaaronepower@gmail.com>
about: A quick CLOC (Count Lines Of Code) tool
args:
  - exclude:
      short: e
      long: exclude
      help: Will ignore all files and directories containing the word ie --exclude node_modules
      takes_value: true
  - sort:
      short: s
      long: sort
      takes_value: true
      possible_values: [files, total, blanks, code, commments]
      help: Will sort based on a certain column ie --sort=files will sort by file count.
  - input:
      index: 1
      multiple: true
      required: true
      help: The input file(s)/directory(ies)
  - languages:
      short: l
      long: languages
      conflicts_with:
          - input
      help: prints out supported languages and their extensions
```
## Expected Output

```
Tokei 1.1.1
Aaron P. <theaaronepower@gmail.com>
A quick CLOC (Count Lines Of Code) tool

USAGE:
    tokei [FLAGS] [OPTIONS] <input>...

FLAGS:
    -h, --help         Prints help information
    -l, --languages    prints out supported languages and their extensions
    -V, --version      Prints version information

OPTIONS:
    -e, --exclude <exclude>    Will ignore all files and directories containing the word ie --exclude node_modules
    -s, --sort <sort>          Will sort based on a certain column ie --sort=files will sort by file count. [values: files total blanks code commments]

ARGS:
    input...    The input file(s)/directory(ies)
```
## Actual Output

```
tokei 

USAGE:
    tokei [FLAGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information
```


---

_Comment by @WildCryptoFox on 2015-09-23 11:12_

@Aaronepower Other than testing, do you have any usage requirement for the YAML parser? 

I have no feedback for this exact feature as I've never used it and it'll need discussion for if we'll keep the feature in 2.0. @kbknapp Thoughts? I'd personally opt against the extra parser, but it shouldn't be too difficult to port over if you'd like to keep it. **Edit**: Please respond in the 2.0 issue.


---

_Comment by @XAMPPRocky on 2015-09-23 11:14_

What do you mean usage requirement? The YAML parser already is an extra feature.

On Wed, Sep 23, 2015 at 12:12 PM, James McGlashan
notifications@github.com wrote:

> @Aaronepower Other than testing, do you have any usage requirement for the YAML parser? 
> 
> ## I have no feedback for this exact feature as I've never used it and it'll need discussion for if we'll keep the feature in 2.0. @kbknapp Thoughts? I'd personally opt against the extra parser, but it shouldn't be too difficult to port over if you'd like to keep it.
> 
> Reply to this email directly or view it on GitHub:
> https://github.com/kbknapp/clap-rs/issues/274#issuecomment-142570816


---

_Comment by @WildCryptoFox on 2015-09-23 11:15_

@Aaronepower I meant for your use-case. Is there any reason that you are specifically using the YAML parser?


---

_Comment by @XAMPPRocky on 2015-09-23 11:16_

It keeps my main file clean?

On Wed, Sep 23, 2015 at 12:15 PM, James McGlashan
notifications@github.com wrote:

> ## @Aaronepower I meant for your use-case. Is there any reason that you are specifically using the YAML parser?
> 
> Reply to this email directly or view it on GitHub:
> https://github.com/kbknapp/clap-rs/issues/274#issuecomment-142571177


---

_Comment by @WildCryptoFox on 2015-09-23 11:28_

@Aaronepower Okay. Although not yet ready. Your current parser would be something along the lines of the following with my 2.0 macro. I might be able to backport this to `clap-1.4.2`. Note as the builder method is used internally, this is the **fastest** way to get a clap builder.

**Note**: This macro although valid with my current code, is not finalized and may change.

``` rust
clap_app!{ Tokei;
  (meta => //version: "1.0.0" // I don't yet have version ;-)
           author: "Aaron P. <theaaronepower@gmail.com>"
           about:  "A quick CLOC (Count Lines Of Code) tool")
  (args =>
    help:      [-h --help]             "Prints help information"
    languages: [-l --languages]        "Prints out supported languages and their extensions"
    version:   [-V --version]          "Prints version information"
    exclude:   [-e --exclude exclude]  "Will ignore all files and directories containing \
                                        the word"
    sort:      [-s --sort sort]        "Will sort based on a certain column I.e. \
                                        --sort=files will sort by file count."
    input:     (input ...)             "The input file(s)/directory(ies)"
  )
}
```

As for the issue you've reported, one of the others will likely have a fix soon.


---

_Comment by @kbknapp on 2015-09-23 14:36_

@Aaronepower in order to use `bin_name` you also have to have a `name` field. Sounds redundant seeing this issue I need to document it better. Reasoning being `name` is required no matter what, and `bin_name` just overrides the system determined binary name which is used in help messages. 

I just tested on 1.4.2 and everything appears to be working.

``` yaml
name: Tokei
bin_name: Tokei
version: 1.1.1
author: Aaron P. <theaaronepower@gmail.com>
about: A quick CLOC (Count Lines Of Code) tool
args:
  - exclude:
      short: e
      long: exclude
      help: Will ignore all files and directories containing the word ie --exclude node_modules
      takes_value: true
  - sort:
      short: s
      long: sort
      takes_value: true
      possible_values: [files, total, blanks, code, commments]
      help: Will sort based on a certain column ie --sort=files will sort by file count.
  - input:
      index: 1
      multiple: true
      required: true
      help: The input file(s)/directory(ies)
  - languages:
      short: l
      long: languages
      conflicts_with:
          - input
      help: prints out supported languages and their extensions
```

Output

```
Tokei 1.1.1
Aaron P. <theaaronepower@gmail.com>
A quick CLOC (Count Lines Of Code) tool

USAGE:
    Tokei [FLAGS] [OPTIONS] <input>...

FLAGS:
    -h, --help         Prints help information
    -l, --languages    prints out supported languages and their extensions
    -V, --version      Prints version information

OPTIONS:
    -e, --exclude <exclude>    Will ignore all files and directories containing the word ie --exclude node_modules
    -s, --sort <sort>          Will sort based on a certain column ie --sort=files will sort by file count. [values: files total blanks code commments]

ARGS:
    input...    The input file(s)/directory(ies)
```


---

_Comment by @sru on 2015-09-23 14:40_

@Aaronepower @kbknapp can confirm. Not sure why though.


---

_Label `T: bug` added by @sru on 2015-09-23 14:41_

---

_Label `P4: nice to have` added by @sru on 2015-09-23 14:41_

---

_Label `C: parsing` added by @sru on 2015-09-23 14:41_

---

_Label `E: tedious` added by @sru on 2015-09-23 14:41_

---

_Referenced in [clap-rs/clap#259](../../clap-rs/clap/issues/259.md) on 2015-09-23 14:41_

---

_Comment by @kbknapp on 2015-09-23 14:50_

@sru Even after/if @Aaronepower confirms that everything is working as expected by adding `name` **and** `bin_name` on 1.4.2, let's leave this issue open until the bug of having only a `bin_name` causing strange help messages is fixed. Ideally, if only a `bin_name` was included, it would either panic with a good error message, or use the `bin_name` for both values (which I personally think is a bit to subtle and misleading).


---

_Comment by @WildCryptoFox on 2015-09-23 15:34_

@kbknapp This is one of the reasons I don't like runtime parsers :wink: 


---

_Comment by @XAMPPRocky on 2015-09-26 08:21_

Yeah, does work with both.


---

_Added to milestone `1.4.3` by @kbknapp on 2015-09-28 17:00_

---

_Renamed from "bin_name doesn't work, and prints weird. From YAML" to "bin_name doesn't work wit out na me field. From YAML" by @kbknapp on 2015-09-30 04:37_

---

_Renamed from "bin_name doesn't work wit out na me field. From YAML" to "bin_name doesn't work with out na me field. From YAML" by @kbknapp on 2015-09-30 04:37_

---

_Renamed from "bin_name doesn't work with out na me field. From YAML" to "bin_name doesn't work with out name field. From YAML" by @kbknapp on 2015-09-30 04:37_

---

_Comment by @kbknapp on 2015-09-30 14:30_

I had forgot requiring `name:` isn't possible due to how subcommands are parsed....so this will just have to be documented better.

Closed for now.


---

_Closed by @kbknapp on 2015-09-30 14:30_

---
