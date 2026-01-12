```yaml
number: 5995
title: Help messages reflect conflicting options
type: issue
state: open
author: XingHuang35
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2025-05-09T10:40:07Z
updated_at: 2025-05-23T14:46:24Z
url: https://github.com/clap-rs/clap/issues/5995
synced_at: 2026-01-12T16:14:17Z
```

# Help messages reflect conflicting options

---

_@XingHuang35_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4

### Describe your use case

in the struct definition, we have this:
```
/// Optional host IPs
#[clap(long, conflicts_with_all = &["config_file", "scan", "skip_host_discovery"])]
    ips: Vec<String>,

///Path to configuration file.
#[clap(short, long, global = true, visible_alias = "config", env = "CONFIG_FILE_PATH", default_value = "/etc/config/configfile.toml")]
    pub config_file: Option<String>,
```

We have the following help message, 

```
--ips
     Optional host IPs

--config-file <CONFIG_FILE>
     Path to configuration file
      
     [env: CONFIG_FILE_PATH=]
     [default: /etc/config/configfile.toml]
     [aliases: config]
```

### Describe the solution you'd like

We'd like have help messages like this:
```
--ips
     Optional host IPs
     [conflict: "config_file", "scan", "skip_host_discovery"]

--config-file <CONFIG_FILE>
     Path to configuration file
      
     [env: CONFIG_FILE_PATH=]
     [default: /etc/config/configfile.toml]
     [aliases: config]
     [conflict: "ips"]
```
Then the user would konw that "--ips" conflicts with "--config-file" when the user checks the help message, otherwise the user would encounter "--ips" conflicts with "--config-file" when executing the command.

I will soon create a PR for this.

### Alternatives, if applicable

N.A

### Additional Context

N.A

---

_Label `C-enhancement` added by @XingHuang35 on 2025-05-09 10:40_

---

_Renamed from "Help messages reflect conflicting options" to "Help messages reflect conflicting options(to continue)" by @XingHuang35 on 2025-05-09 10:40_

---

_Renamed from "Help messages reflect conflicting options(to continue)" to "Help messages reflect conflicting options" by @XingHuang35 on 2025-05-09 14:17_

---

_Comment by @XingHuang35 on 2025-05-09 14:18_

I will soon create a PR for this.

---

_Comment by @epage on 2025-05-09 14:25_

Note that our [contributing docs](https://github.com/clap-rs/clap/blob/master/CONTRIBUTING.md#where-to-start) encourage waiting on a PR until a design is resolved to keep the problem/solution discussion focused on issues which are less ephemeral and avoid wasting time on an implementation on a solution that may not be selected.

---

_Label `S-waiting-on-decision` added by @epage on 2025-05-09 14:25_

---

_Label `A-help` added by @epage on 2025-05-09 14:25_

---

_Comment by @epage on 2025-05-09 14:30_

We show conflicts in usage as them being `|`ed but that will only show up for
- Positionals
- Required arguments
- Error messages when the user has specified one of them

Overall, I have mixed feelings about our auto-generated `[]` sections and I am hesitant to add more.

#5459 asks for aliases to be rendered in a different wa.

#5392 asks to add another `[]` which is related to #4812 which asks for something in the example argument.

While this doesn't work everywhere, the solution I tend to take in my applications is to represent this in a `help_heading`.  Resolving #4589 would likely improve that.

---

_Label `S-waiting-on-decision` removed by @epage on 2025-05-09 14:30_

---

_Label `S-waiting-on-design` added by @epage on 2025-05-09 14:30_

---

_Comment by @XingHuang35 on 2025-05-12 02:39_


> While this doesn't work everywhere, the solution I tend to take in my applications is to represent this in a `help_heading`. Resolving [#4589](https://github.com/clap-rs/clap/issues/4589) would likely improve that.

Thanks Ed very much, if the `help_heading` of "Options" can express some options are conflicting with each other, I think it would be OK, like:

```
Options:
   "--ips" conflicts with "--config_file", "--scan", "--skip_host_discovery"

    --ips
         Optional host IPs
    
    --config-file <CONFIG_FILE>
         Path to configuration file
          
         [env: CONFIG_FILE_PATH=]
         [default: /etc/config/configfile.toml]
         [aliases: config]
```

But when there are several options conflicting, the help_heading could be a little long, such as
```
"--ips" conflicts with "config_file", "scan", "skip_host_discovery"
"--config_file" conflicts with "--json"
```

---

_Comment by @epage on 2025-05-23 14:46_

I was more so thinking that the help heading description could say that the arguments under this heading are all mutually exclusive, rather than anything auto-generated.  Granted, that means you are tied to your help headings and your conflicts which might not always be ideal.

---
