---
number: 753
title: "required_unless() doesn't seem to work"
type: issue
state: closed
author: bestouff
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-11-18T15:26:49Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/753
synced_at: 2026-01-07T13:12:19-06:00
---

# required_unless() doesn't seem to work

---

_Issue opened by @bestouff on 2016-11-18 15:26_

### Rust Version
1.13.0
### Affected Version of clap
v2.18.0
### Expected Behavior Summary 
accept `--list` as an only arg
### Actual Behavior Summary
```
error: The following required arguments were not provided:
    --iface <INTERFACE>
    --file <TESTFILE>
    --server <SERVER_IP>
```
### Steps to Reproduce the issue
call with `--list`
### Sample Code or Link to Sample Code
```
        .arg(Arg::from_usage("-l, --list 'List available interfaces (and stop there)'"))
        .arg(Arg::from_usage("-i, --iface=[INTERFACE] 'Ethernet interface for fetching NTP packets'")
            .required_unless("list"))
        .arg(Arg::from_usage("-f, --file=[TESTFILE] 'Fetch NTP packets from pcap file'")
            .conflicts_with("iface")
            .required_unless("list"))
        .arg(Arg::from_usage("-s, --server=[SERVER_IP] 'NTP server IP address'")
            .required_unless("list"))
        .arg(Arg::from_usage("-p, --port=[SERVER_PORT] 'NTP server port'")
            .default_value("123"))
        .get_matches();
```
### Debug output
```
*DEBUG:clap: fn=from_usage;
*DEBUG:clap: fn=new; usage="-l, --list \'List available interfaces (and stop there)\'"
*DEBUG:clap: fn=parse;
*DEBUG:clap: iter; pos=0;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=short;
*DEBUG:clap: setting short: l
*DEBUG:clap: setting name: l
*DEBUG:clap: iter; pos=1;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=long;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting name: list
*DEBUG:clap: setting long: list
*DEBUG:clap: iter; pos=10;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=help;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting help: List available interfaces (and stop there)
*DEBUG:clap: iter; pos=55;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: vals: None
*DEBUG:clap: fn=from_usage;
*DEBUG:clap: fn=new; usage="-i, --iface=[INTERFACE] \'Ethernet interface for fetching NTP packets\'"
*DEBUG:clap: fn=parse;
*DEBUG:clap: iter; pos=0;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=short;
*DEBUG:clap: setting short: i
*DEBUG:clap: setting name: i
*DEBUG:clap: iter; pos=1;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=long;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting name: iface
*DEBUG:clap: setting long: iface
*DEBUG:clap: iter; pos=11;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=name;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting val name: INTERFACE
*DEBUG:clap: iter; pos=22;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=help;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting help: Ethernet interface for fetching NTP packets
*DEBUG:clap: iter; pos=69;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: vals: Some({0: "INTERFACE"})
*DEBUG:clap: fn=from_usage;
*DEBUG:clap: fn=new; usage="-f, --file=[TESTFILE] \'Fetch NTP packets from pcap file\'"
*DEBUG:clap: fn=parse;
*DEBUG:clap: iter; pos=0;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=short;
*DEBUG:clap: setting short: f
*DEBUG:clap: setting name: f
*DEBUG:clap: iter; pos=1;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=long;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting name: file
*DEBUG:clap: setting long: file
*DEBUG:clap: iter; pos=10;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=name;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting val name: TESTFILE
*DEBUG:clap: iter; pos=20;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=help;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting help: Fetch NTP packets from pcap file
*DEBUG:clap: iter; pos=56;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: vals: Some({0: "TESTFILE"})
*DEBUG:clap: fn=from_usage;
*DEBUG:clap: fn=new; usage="-s, --server=[SERVER_IP] \'NTP server IP address\'"
*DEBUG:clap: fn=parse;
*DEBUG:clap: iter; pos=0;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=short;
*DEBUG:clap: setting short: s
*DEBUG:clap: setting name: s
*DEBUG:clap: iter; pos=1;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=long;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting name: server
*DEBUG:clap: setting long: server
*DEBUG:clap: iter; pos=12;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=name;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting val name: SERVER_IP
*DEBUG:clap: iter; pos=23;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=help;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting help: NTP server IP address
*DEBUG:clap: iter; pos=48;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: vals: Some({0: "SERVER_IP"})
*DEBUG:clap: fn=from_usage;
*DEBUG:clap: fn=new; usage="-p, --port=[SERVER_PORT] \'NTP server port\'"
*DEBUG:clap: fn=parse;
*DEBUG:clap: iter; pos=0;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=short;
*DEBUG:clap: setting short: p
*DEBUG:clap: setting name: p
*DEBUG:clap: iter; pos=1;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=short_or_long;
*DEBUG:clap: fn=long;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting name: port
*DEBUG:clap: setting long: port
*DEBUG:clap: iter; pos=10;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=name;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting val name: SERVER_PORT
*DEBUG:clap: iter; pos=23;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: fn=help;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: setting help: NTP server port
*DEBUG:clap: iter; pos=42;
*DEBUG:clap: fn=stop_at;
*DEBUG:clap: vals: Some({0: "SERVER_PORT"})
*DEBUG:clap: fn=get_matches_with;
*DEBUG:clap: fn=create_help_and_version;
*DEBUG:clap: Building --help
*DEBUG:clap: Building --version
*DEBUG:clap: Begin parsing '"--list"' ([45, 45, 108, 105, 115, 116])
*DEBUG:clap: Starts new arg...Maybe
*DEBUG:clap: fn=possible_subcommand
*DEBUG:clap: fn=parse_long_arg;
*DEBUG:clap: Does it contain '='...No
*DEBUG:clap: Found valid flag '--list'
*DEBUG:clap: Checking if --list is help or version...Neither
*DEBUG:clap: fn=parse_flag;
*DEBUG:clap: macro=validate_multiples!;
*DEBUG:clap: fn=groups_for_arg; name=list
*DEBUG:clap: No groups defined
*DEBUG:clap: macro=arg_post_processing!;
*DEBUG:clap: Is '--list' in overrides...No
*DEBUG:clap: Does '--list' have overrides...No
*DEBUG:clap: Does '--list' have conflicts...No
*DEBUG:clap: Does '--list' have requirements...No
*DEBUG:clap: macro=_handle_group_reqs!;
*DEBUG:clap: fn=add_val_to_arg;
*DEBUG:clap: adding val: "123"
*DEBUG:clap: fn=groups_for_arg; name=port
*DEBUG:clap: No groups defined
*DEBUG:clap: fn=validate_value; val="123"
*DEBUG:clap: macro=arg_post_processing!;
*DEBUG:clap: fn=fmt
*DEBUG:clap: Is '--port <SERVER_PORT>' in overrides...No
*DEBUG:clap: fn=fmt
*DEBUG:clap: Does '--port <SERVER_PORT>' have overrides...No
*DEBUG:clap: fn=fmt
*DEBUG:clap: Does '--port <SERVER_PORT>' have conflicts...No
*DEBUG:clap: fn=fmt
*DEBUG:clap: Does '--port <SERVER_PORT>' have requirements...No
*DEBUG:clap: macro=_handle_group_reqs!;
*DEBUG:clap: fn=validate_blacklist;blacklist=[]
*DEBUG:clap: fn=validate_num_args;
*DEBUG:clap: fn=_validate_num_vals;
*DEBUG:clap: fn=create_usage;
*DEBUG:clap: fn=create_usage_no_title;
*DEBUG:clap: args_in_groups=[]
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=needs_flags_tag;
*DEBUG:clap: iter;f=list;
*DEBUG:clap: fn=groups_for_arg; name=list
*DEBUG:clap: No groups defined
*DEBUG:clap: Flag not required...(returning true)
*DEBUG:clap: args_in_groups=[]
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=create_usage;
*DEBUG:clap: fn=create_usage_no_title;
*DEBUG:clap: fn=smart_usage;
*DEBUG:clap: args_in_groups=[]
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=fmt
*DEBUG:clap: fn=color;
*DEBUG:clap: Color setting...Auto
*DEBUG:clap: fn=error;
*DEBUG:clap: fn=is_a_tty;
*DEBUG:clap: Use stderr...true
*DEBUG:clap: fn=good;
*DEBUG:clap: fn=is_a_tty;
*DEBUG:clap: Use stderr...true
```

---

_Comment by @kbknapp on 2016-11-20 19:52_

Thanks for posting this! I should have fixed quickly :wink:


---

_Label `C: parsing` added by @kbknapp on 2016-11-20 19:52_

---

_Label `D: easy` added by @kbknapp on 2016-11-20 19:52_

---

_Label `P2: need to have` added by @kbknapp on 2016-11-20 19:52_

---

_Label `T: bug` added by @kbknapp on 2016-11-20 19:52_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-20 19:52_

---

_Comment by @kbknapp on 2016-11-21 00:11_

I've got this fixed in a local branch, should have it uploaded and the PR through shortly. Then I'll upload the new version to crates.io and post back here.

---

_Closed by @homu on 2016-11-21 04:52_

---

_Comment by @bestouff on 2016-11-21 08:00_

Great thanks !

---

_Comment by @kbknapp on 2016-11-21 14:17_

v2.19.0 is out on crates.io

---
