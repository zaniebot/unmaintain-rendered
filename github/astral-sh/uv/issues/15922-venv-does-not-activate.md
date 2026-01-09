---
number: 15922
title: venv does not activate
type: issue
state: closed
author: ajaxdude
labels:
  - question
assignees: []
created_at: 2025-09-17T23:40:28Z
updated_at: 2025-09-18T15:47:10Z
url: https://github.com/astral-sh/uv/issues/15922
synced_at: 2026-01-07T13:12:19-06:00
---

# venv does not activate

---

_Issue opened by @ajaxdude on 2025-09-17 23:40_

### Question

new to UV. When I "source .venv/bin/activate" I get a prompt but the python version does NOT activate and is still my base (3.13) when I type Python --version. But if I type uv run python --version then it says 3.12 which is the venv I want. I don't get it. I check the .venv and it seems fine. Any ideas?

### Platform

fedora 42

### Version

0.7.12

---

_Label `question` added by @ajaxdude on 2025-09-17 23:40_

---

_Comment by @zanieb on 2025-09-18 00:27_

After `source .venv/bin/activate` what's `uv python find -v`, `echo $VIRTUAL_ENV` and `whence -a python`?

---

_Comment by @zanieb on 2025-09-18 00:31_

What shell are you using?

---

_Comment by @ajaxdude on 2025-09-18 06:35_

I figured it out. Using zsh. In my toml file the order of the blocks matter. Once I put the block with the {{if .venv}} before the block with type=text it worked.

---

_Comment by @ajaxdude on 2025-09-18 15:45_

here is what fixed it for me:

#:schema https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/schema.json

version = 3
final_space = true
console_title_template = '{{ .Shell }} in {{ .Folder }}'

terminal_background = "p:t-background"

[palette]
main-bg = "#24283b"
terminal-red = "#f7768e"
pistachio-green = "#9ece6a"
terminal-green = "#73daca"
terminal-yellow = "#e0af68"
terminal-blue = "#7aa2f7"
celeste-blue = "#b4f9f8"
light-sky-blue = "#7dcfff"
terminal-white = "#c0caf5"
white-blue = "#a9b1d6"
blue-bell = "#9aa5ce"
pastal-grey = "#cfc9c2"
terminal-magenta = "#24BCA8"
blue-black = "#565f89"
terminal-black = "#414868"
t-background = "p:main-bg"

[[blocks]]
  type = 'prompt'
  alignment = 'left'
  newline = true

  [[blocks.segments]]
    type = "text"
    style = "plain"
    background = "transparent"
    foreground = "p:terminal-blue"
    template = "➜ "

  [[blocks.segments]]
    type = 'path'
    style = 'plain'
    background = 'transparent'
    foreground = 'p:terminal-blue'
    template = '{{ .Path }}'

    [blocks.segments.properties]
      style = 'full'

  [[blocks.segments]]
    type = 'git'
    style = 'plain'
    foreground = 'p:light-sky-blue'
    background = 'transparent'
    template = ' {{ .HEAD }}{{ if or (.Working.Changed) (.Staging.Changed) }}*{{ end }} <p:white-blue>{{ if gt .Behind 0 }}⇣{{ end }}{{ if gt .Ahead 0 }}⇡{{ end }}</>'

    [blocks.segments.properties]
      branch_icon = ''
      commit_icon = '@'
      fetch_status = true

[[blocks]]
  type = 'rprompt'
  overflow = 'hidden'

  [[blocks.segments]]
    type = 'executiontime'
    style = 'plain'
    foreground = 'p:terminal-yellow'
    background = 'transparent'
    template = '{{ .FormattedMs }}'

    [blocks.segments.properties]
      threshold = 5000

[[blocks]]
  type = 'prompt'
  alignment = 'left'
  newline = true

  [[blocks.segments]]
    type = 'python'
    style = 'plain'
    foreground = "p:pistachio-green"
    background = 'transparent'
    template = "\ue235 {{ if .Error }}{{ .Error }}{{ else }}{{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }}{{ end }}"
    
  [[blocks.segments]]
    type = 'uv'
    style = 'plain'
    foreground = "p:pistachio-green"
    background = 'transparent'
    template = "\ue235 {{ if .Error }}{{ .Error }}{{ else }}{{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }}{{ end }}"

  [[blocks.segments]]
    type = 'text'
    style = 'plain'
    foreground_templates = [
      "{{if gt .Code 0}}p:terminal-red{{end}}",
      "{{if eq .Code 0}}p:terminal-magenta{{end}}",
    ]
    background = 'transparent'
    template = "❯"
     
    [transient_prompt]
      foreground_templates = [
        "{{if gt .Code 0}}p:terminal-red{{end}}",
        "{{if eq .Code 0}}p:terminal-magenta{{end}}",
      ]
      background = 'transparent'
      template = '❯ '

    [secondary_prompt]
      foreground = 'p:terminal-blue'
      background = 'transparent'
      template = '❯❯ '

---

_Comment by @zanieb on 2025-09-18 15:47_

Thanks!

---

_Closed by @zanieb on 2025-09-18 15:47_

---
