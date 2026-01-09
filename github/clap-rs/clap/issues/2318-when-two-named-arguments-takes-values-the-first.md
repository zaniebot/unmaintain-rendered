---
number: 2318
title: When two named arguments takes values, the first named argument will always take the value of the second
type: issue
state: closed
author: Yuri6037
labels:
  - C-bug
assignees: []
created_at: 2021-01-29T20:29:39Z
updated_at: 2021-01-29T21:52:40Z
url: https://github.com/clap-rs/clap/issues/2318
synced_at: 2026-01-07T13:12:19-06:00
---

# When two named arguments takes values, the first named argument will always take the value of the second

---

_Issue opened by @Yuri6037 on 2021-01-29 20:29_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closed issues

### Code

https://gitlab.com/BlockProject3D/fpkg/-/blob/master/BPXTools/src/main.rs

Line 45

### Steps to reproduce the issue

1. Use the definition as given in Line 45 of provided main.rs
2. Use the tool as `./target/debug/bpxdbg -f file.bpx info -d 0 -o test.raw`
3. Enjoy the -d argument taking the value of -o

### Version

* **Rust**: rustc 1.47.0 (18bf6b4f0 2020-10-07)
* **Clap**: "2.27.0"

### Actual Behavior Summary

The -d argument takes the value of -o which is bad.

**If a project of yours is blocked due to this bug, please, mention it explicitly.**

Currently blocking FPKG, BlockProject 3D Framework and by extension the Engine which relies on the Framework itself requiring the FPKG tool.

### Expected Behavior Summary

Calling `./target/debug/bpxdbg -f file.bpx info -d 0 -o test.raw` should result in a file named `test.raw` generated with the content of the section at index `0` in the file `file.bpx`. Currently the argument read by the program for -d is `test.raw` same for -o.

### Additional context

When using the tool as `./target/debug/bpxdbg -f file.bpx info -d 0` no bug occurs.

### Debug output

<details>
<summary> Debug Output </summary>
<pre>
<code>
DEBUG:clap:Parser::add_subcommand: term_w=None, name=info
DEBUG:clap:Parser::add_subcommand: term_w=None, name=pack
DEBUG:clap:Parser::add_subcommand: term_w=None, name=unpack
DEBUG:clap:Parser::propagate_settings: self=bpxd, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=info, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=info, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=pack, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=pack, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: sc=unpack, settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
), g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::propagate_settings: self=unpack, g_settings=AppFlags(
    (empty),
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::create_help_and_version: Building help
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-f"' ([45, 102])
DEBUG:clap:Parser::is_new_arg:"-f":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-f"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-f"
DEBUG:clap:Parser::parse_short_arg:iter:f
DEBUG:clap:Parser::parse_short_arg:iter:f: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:f: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=file, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(REQUIRED | EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=file
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Opt("file")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"file.bpx"' ([102, 105, 108, 101, 46, 98, 112, 120])
DEBUG:clap:Parser::is_new_arg:"file.bpx":Opt("file")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=file, val="file.bpx"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."file.bpx"
DEBUG:clap:Parser::groups_for_arg: name=file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=file
DEBUG:clap:Parser::get_matches_with: Begin parsing '"info"' ([105, 110, 102, 111])
DEBUG:clap:Parser::is_new_arg:"info":ValuesDone
DEBUG:clap:Parser::possible_subcommand: arg="info"
DEBUG:clap:Parser::get_matches_with: possible_sc=true, sc=Some("info")
DEBUG:clap:Parser::parse_subcommand;
DEBUG:clap:usage::get_required_usage_from: reqs=["file", "file"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["file"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:Parser::parse_subcommand: About to parse sc=info
DEBUG:clap:Parser::parse_subcommand: sc settings=AppFlags(
    NEEDS_LONG_HELP | NEEDS_LONG_VERSION | NEEDS_SC_HELP | UTF8_NONE | COLOR_AUTO,
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-d"' ([45, 100])
DEBUG:clap:Parser::is_new_arg:"-d":NotFound
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: - found
DEBUG:clap:Parser::is_new_arg: starts_new_arg=true
DEBUG:clap:Parser::possible_subcommand: arg="-d"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::parse_short_arg: full_arg="-d"
DEBUG:clap:Parser::parse_short_arg:iter:d
DEBUG:clap:Parser::parse_short_arg:iter:d: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:d: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=section_id, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=section_id
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=section_id
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Opt("section_id")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"0"' ([48])
DEBUG:clap:Parser::is_new_arg:"0":Opt("section_id")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=section_id, val="0"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."0"
DEBUG:clap:Parser::groups_for_arg: name=section_id
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=section_id
DEBUG:clap:Parser::get_matches_with: Begin parsing '"-o"' ([45, 111])
DEBUG:clap:Parser::is_new_arg:"-o":ValuesDone
DEBUG:clap:Parser::possible_subcommand: arg="-o"
DEBUG:clap:Parser::get_matches_with: possible_sc=false, sc=None
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("section_id");
DEBUG:clap:Parser::parse_short_arg: full_arg="-o"
DEBUG:clap:Parser::parse_short_arg:iter:o
DEBUG:clap:Parser::parse_short_arg:iter:o: Found valid opt
DEBUG:clap:Parser::parse_short_arg:iter:o: p[0]=[], p[1]=[]
DEBUG:clap:Parser::parse_opt; opt=out_file, val=None
DEBUG:clap:Parser::parse_opt; opt.settings=ArgFlags(EMPTY_VALS | TAKES_VAL | DELIM_NOT_SET)
DEBUG:clap:Parser::parse_opt; Checking for val...None
DEBUG:clap:ArgMatcher::inc_occurrence_of: arg=out_file
DEBUG:clap:ArgMatcher::inc_occurrence_of: first instance
DEBUG:clap:Parser::groups_for_arg: name=out_file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Parser::parse_opt: More arg vals required...
DEBUG:clap:Parser:get_matches_with: After parse_short_arg Opt("out_file")
DEBUG:clap:Parser::get_matches_with: Begin parsing '"test.raw"' ([116, 101, 115, 116, 46, 114, 97, 119])
DEBUG:clap:Parser::is_new_arg:"test.raw":Opt("out_file")
DEBUG:clap:Parser::is_new_arg: arg_allows_tac=false
DEBUG:clap:Parser::is_new_arg: probably value
DEBUG:clap:Parser::is_new_arg: starts_new_arg=false
DEBUG:clap:Parser::add_val_to_arg; arg=out_file, val="test.raw"
DEBUG:clap:Parser::add_val_to_arg; trailing_vals=false, DontDelimTrailingVals=false
DEBUG:clap:Parser::add_single_val_to_arg;
DEBUG:clap:Parser::add_single_val_to_arg: adding val..."test.raw"
DEBUG:clap:Parser::groups_for_arg: name=out_file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:ArgMatcher::needs_more_vals: o=out_file
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("out_file");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:section_id: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:section_id: doesn't have default vals
DEBUG:clap:Parser::add_defaults:iter:out_file: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:out_file: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:out_file;
DEBUG:clap:Parser::groups_for_arg: name=out_file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_blacklist:iter:section_id;
DEBUG:clap:Parser::groups_for_arg: name=section_id
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=[];
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:out_file: vals=[
    "test.raw",
]
DEBUG:clap:Validator::validate_arg_num_vals:out_file
DEBUG:clap:Validator::validate_arg_values: arg="out_file"
DEBUG:clap:Validator::validate_arg_requires:out_file;
DEBUG:clap:Validator::validate_arg_num_occurs: a=out_file;
DEBUG:clap:Validator::validate_matched_args:iter:section_id: vals=[
    "0",
]
DEBUG:clap:Validator::validate_arg_num_vals:section_id
DEBUG:clap:Validator::validate_arg_values: arg="section_id"
DEBUG:clap:Validator::validate_arg_requires:section_id;
DEBUG:clap:Validator::validate_arg_num_occurs: a=section_id;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=[], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=sht;
DEBUG:clap:Parser::groups_for_arg: name=sht
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:usage::needs_flags_tag:iter: [FLAGS] required
DEBUG:clap:usage::create_help_usage: usage=bpxdbg info [FLAGS] [OPTIONS]
DEBUG:clap:ArgMatcher::process_arg_overrides:Some("file");
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:file: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:file: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_blacklist:iter:file;
DEBUG:clap:Parser::groups_for_arg: name=file
DEBUG:clap:Parser::groups_for_arg: No groups defined
DEBUG:clap:Validator::validate_required: required=["file"];
DEBUG:clap:Validator::validate_required:iter:file:
DEBUG:clap:Validator::validate_matched_args;
DEBUG:clap:Validator::validate_matched_args:iter:file: vals=[
    "file.bpx",
]
DEBUG:clap:Validator::validate_arg_num_vals:file
DEBUG:clap:Validator::validate_arg_values: arg="file"
DEBUG:clap:Validator::validate_arg_requires:file;
DEBUG:clap:Validator::validate_arg_num_occurs: a=file;
DEBUG:clap:usage::create_usage_with_title;
DEBUG:clap:usage::create_usage_no_title;
DEBUG:clap:usage::get_required_usage_from: reqs=["file"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["file"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:file:
DEBUG:clap:OptBuilder::fmt:file
DEBUG:clap:usage::needs_flags_tag;
DEBUG:clap:usage::needs_flags_tag:iter: f=hclap_help;
DEBUG:clap:usage::needs_flags_tag:iter: f=vclap_version;
DEBUG:clap:usage::needs_flags_tag: [FLAGS] not required
DEBUG:clap:usage::create_help_usage: usage=bpxdbg --file <file> <SUBCOMMAND>
DEBUG:clap:ArgMatcher::get_global_values: global_arg_vec=[]
====> BPX Main Header <====
Type: P
Version: 1
File size: 6220959
Number of sections: 2
====> End <====

Could not parse section index test.raw (invalid digit found in string)
</code>
</pre>
</details>


---

_Label `T: bug` added by @Yuri6037 on 2021-01-29 20:29_

---

_Comment by @pksunkara on 2021-01-29 21:12_

Needs a minimal reproducible example code.

---

_Comment by @Yuri6037 on 2021-01-29 21:38_

I won't have time to provide a minimal reproducible example code. You can still build the tool as given in the link. It builds fine.

Example use:
- `./target/debug/bpxdbg -f file.bpx pack src`
- `./target/debug/bpxdbg -f file.bpx info -d 0 -o test.raw`

---

_Comment by @pksunkara on 2021-01-29 21:45_

I don't want to go spelunking into code I am extremely unfamiliar with to find this bug when I am not even sure if it's already been fixed on master or not.

Closing the issue for now. To reopen, please provide a minimal reproducible code after checking with master.

---

_Closed by @pksunkara on 2021-01-29 21:45_

---

_Comment by @Yuri6037 on 2021-01-29 21:49_

`checking with master` I can check with master. I won't have time for a reproducible example but I can check if it is fixed on master.

EDIT: I just tried on master and I have the exact same issue.

---
