```yaml
number: 3266
title: "rg kicks out odd output from tmux-*.log files"
type: issue
state: open
author: AronGahagan
labels: []
assignees: []
created_at: 2026-01-22T03:59:34Z
updated_at: 2026-01-22T03:59:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3266
synced_at: 2026-01-22T04:09:07Z
```

# rg kicks out odd output from tmux-*.log files

---

_@AronGahagan_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.1.0

### How did you install ripgrep?

NixOS package

### What operating system are you using ripgrep on?

NixOS 25.11

### Describe your bug.

start tmux -vv - it creates logs
run rg which finds results in those logs
output garbles the screen, but prompt is still accessible

### What are the steps to reproduce the behavior?

start tmux -vv - it creates logs (e.g., tmux-client-143942.log, tmux-out-143944.log, tmux-server-143944.log)
run rg which finds results in those logs
output garbles the screen, but prompt is still accessible

### What is the actual behavior?

```
$ tmux -vv
# screen changes
$ echo this is a test
this is a test
$ rg test
dotfiles/tmux.confent-attached 'run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"'
143:18:set-hook -g client-attached 'run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"'
144:19:set-hook -g session-created 'run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"'
145:20:set-hook -g pane-focus-in 'run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"'
dotfiles/bashrc
dotfiles/bashrct-agent updatestartuptty /bye >/dev/null 2>&1
146:97:  gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
tmux-out-143944.log
tmux-server-143944.log
147:17661:1769053765.388591 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
148:17669:1769053765.388606 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
149:17677:1769053765.388621 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
150:17927:1769053765.389049 cmd_parse_build_commands 15:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
151:17931:1769053765.389056 cmd_parse_build_commands 16:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
152:17935:1769053765.389064 cmd_parse_build_commands 17:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
153:18125:1769053765.389425 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)
154:18131:1769053765.389436 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)
155:18137:1769053765.389448 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)
156:18265:1769053765.389786 cmd_parse_build_commands: set-option -g status-justify left ;; set-option -g status-left-length 200 ;; set-option -g status-right-length 200 ;; set-option -g status-position top ;; set-option -g status-style "bg=#1e1e2e" ;; set-option -g status-right "#(whoami)@#H %Y-%m-%d %H:%M " ;; set-option -g display-time 4000 ;; set-option -gs escape-time 10 ;; set-option -g focus-events on ;; set-option -a terminal-features XXX:RGB ;; set-option -g update-environment "DISPLAY SSH_ASKPASS SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHORITY WAYLAND_DISPLAY" ;; set-option -g mouse on ;; set-option -g renumber-windows on ;; set-option -g wrap-search on ;; set-environment -g SSH_AUTH_SOCK /run/user/1000/gnupg/S.gpg-agent.ssh ;; set-hook -g client-attached "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; set-hook -g session-created "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; set-hook -g pane-focus-in "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; bind-key -n M-| split-window -h ;; bind-key -n M-_ split-window -v ;; unbind-key \\" ;; unbind-key \\% ;; bind-key r source-file /home/contrafiat/.tmux.conf ;; bind-key c new-window -c "#{pane_current_path}" ;; bind-key j choose-window "join-pane -h -s \\"%%\\"" ;; bind-key J choose-window "join-pane -s \\"%%\\"" ;; set-option -g window-status-style "fg=#9399b2,bg=#1e1e2e" ;; set-option -g window-status-current-style "fg=#fab387,bg=#313244,bold" ;; set-option -g window-status-format " #I:#W " ;; set-option -g window-status-current-format " #I:#W " ;; bind-key -n C-PgUp previous-window ;; bind-key -n C-PgDn next-window ;; bind-key -n M-Left select-pane -L ;; bind-key -n M-Right select-pane -R ;;
                                                         bind-key -n M-Up select-pane -U ;; bind-key -n M-Down select-pane -D ;; bind-key -n M-S-Left swap-window -t "-1;" select-window -t -1 ;; bind-key -n M-S-Right swap-window -t "+1;" select-window -t +1
157:18611:1769053765.390531 cmdq_fire_command <global>: (2814) set-hook -g client-attached "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\""
158:18629:1769053765.390571 yylex_token: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
159:18631:1769053765.390576 cmd_parse_build_commands 0:1: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
160:18632:1769053765.390580 args_parse_flags: next gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
161:18634:1769053765.390584 args_parse: 1 = gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 (type STRING)
162:18635:1769053765.390589 cmd_parse_build_commands: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
163:18639:1769053765.390601 cmdq_fire_command <global>: (2817) set-hook -g session-created "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\""
164:18657:1769053765.390638 yylex_token: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
165:18659:1769053765.390643 cmd_parse_build_commands 0:1: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
166:18660:1769053765.390647 args_parse_flags: next gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
167:18662:1769053765.390651 args_parse: 1 = gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 (type STRING)
[0]  0:[tmux]                                                                                                                                                                   contrafiat@thinkpad 2026-01-21 22:50
29729:1769053765.425087 free job 0x5587410e7c00: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
283:36361:1769053770.684242 input_parse_buffer: %0 ground, 25 bytes: \033[?2004l\rthis is a test\r\n
284:36366:1769053770.684255 screen_write_collect_end: 14 this is a test (at 0,3)
285:36370:1769053770.684265 /dev/pts/0: this is a test
286:37614:1769053777.000353 input_parse_buffer: %0 ground, 1949 bytes: \033[0m\033[35mtmux-out-143944.log\033[0m\r\n\033[0m\033[32m111\033[0m:\033[39m$ \033[?2026h\033[?25l\033[?12l\033[?25h\033[?2026l\033[?2026h\033[?25l\033[48;2;30;30;46m\033[H[0] \033[38;2;250;179;135m\033[48;2;49;50;68m\033[1m 0:bash \033(B\033[m\033[48;2;30;30;46m                                                                                                                                                                    contrafiat@thinkpad 2026-01-21 22:49 \033(B\033[m\033[?12l\033[?25h\033[4;3H\033[?2026lecho "this is a \033[0m\033[1m\033[31mtest\033[0m"\r\r\n\033[0m\033[32m112\033[0m:this is a \033[0m\033[1m\033[31mtest\033[0m\r\r\n\033[0m\033[32m115\033[0m:\033[39m$ \033[?2026h\033[?25l\033[?12l\033[?25h\033[?2026lrg \033[0m\033[1m\033[31mtest\033[0m\r\r\n\r\n\033[0m\033[35mhosts.nix\033[0m\r\n\033[0m\033[32m411\033[0m:127.0.0.1 be.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m673\033[0m:127.0.0.1 de.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m749\033[0m:127.0.0.1 economist\033[0m\033[1m\033[31mtest\033[0mcollect.insightfirst.com\r\n\033[0m\033[32m770\033[0m:127.0.0.1 fi.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m850\033[0m:127.0.0.1 int.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m1010\033[0m:#127.0.0.1 nl.si\033[0m\033[1m\033[31mtest\033[0mat.com\t# may interfere with duo.nl\r\n\033[0m\033[32m1172\033[0m:127.0.0.1 se.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m1178\033[0m:127.0.0.1 si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m1179\033[0m:127.0.0.1 si\033[0m\033[1m\033[31mtest\033[0mats.tiscali.co.uk\r\n\033[0m\033[32m1216\033[0m:127.0.0.1 statistics.dynamicsi\033[0m\033[1m\033[31mtest\033[0mats.com\r\n\033[0m\033[32m1428\033[0m:127.0.0.1 uk.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m1593\033[0m:127.0.0.1 www.\033[0m\033[1m\033[31mtest\033[0mracking.com\r\n\033[0m\033[32m1873\033[0m:127.0.0.1 \033[0m\033[1m\033[31mtest\033[0m.ishvara-yoga.com\r\n\033[0m\033[32m2710\033[0m:127.0.0.1 ec\033[0m\033[1m\033[31mtest\033[0mlampsplus1.112.2o7.net\r\n\033[0m\033[32m4355\033[0m:127.0.0.1 adserver.ra\033[0m\033[1m\033[31mtest\033[0mar.net\r\n\033[0m\033[32m4489\033[0m:127.0.0.1 ads.gigaom.com.php5-12.website\033[0m\033[1m\033[31mtest\033[0mlink.com\r\n\033[0m\033[32m4927\033[0m:127.0.0.1 ads\033[0m\033[1m\033[31mtest\033[0m.reklamstore.com\r\n\033[0m\033[32m7328\033[0m:127.0.0.1 oixcrv-ruby\033[0m\033[1m\033[31mtest\033[0m.net\r\n\033[0m\033[32m7334\033[0m:127.0.0.1 oix-ruby\033[0m\033[1m\033[31mtest\033[0m.net\r\n
282:$
tmux-server-144244.log11379 job write 0x5587410e0630: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1, pid 143947, output left 0
17661:1769053920.600763 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" 2>&1, pid 143947
17669:1769053920.600778 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
17677:1769053920.600794 yylex_token: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"2>&1, pid 143947
17927:1769053920.601219 cmd_parse_build_commands 15:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
17931:1769053920.601226 cmd_parse_build_commands 16:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"bye >/dev/null 2>&1"
17935:1769053920.601233 cmd_parse_build_commands 17:3: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
18125:1769053920.601594 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)ull 2>&1"
18131:1769053920.601606 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)
18137:1769053920.601618 args_parse: 3 = run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1" (type STRING)
18265:1769053920.601955 cmd_parse_build_commands: set-option -g status-justify left ;; set-option -g status-left-length 200 ;; set-option -g status-right-length 200 ;; set-option -g status-position top ;; set-option -g status-style "bg=#1e1e2e" ;; set-option -g status-right "#(whoami)@#H %Y-%m-%d %H:%M " ;; set-option -g display-time 4000 ;; set-option -gs escape-time 10 ;; set-option -g focus-events on ;; set-option -a terminal-features XXX:RGB ;; set-option -g update-environment "DISPLAY SSH_ASKPASS SSH_AGENT_PID SSH_CONNECTION WINDOWID XAUTHORITY WAYLAND_DISPLAY" ;; set-option -g mouse on ;; set-option -g renumber-windows on ;; set-option -g wrap-search on ;; set-environment -g SSH_AUTH_SOCK /run/user/1000/gnupg/S.gpg-agent.ssh ;; set-hook -g client-attached "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; set-hook -g session-created "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; set-hook -g pane-focus-in "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\"" ;; bind-key -n M-| split-window -h ;; bind-key -n M-_ split-window -v ;; unbind-key \\" ;; unbind-key \\% ;; bind-key r source-file /home/contrafiat/.tmux.conf ;; bind-key c new-window -c "#{pane_current_path}" ;; bind-key j choose-window "join-pane -h -s \\"%%\\"" ;; bind-key J choose-window "join-pane -s \\"%%\\"" ;; set-option -g window-status-style "fg=#9399b2,bg=#1e1e2e" ;; set-option -g window-status-current-style "fg=#fab387,bg=#313244,bold" ;; set-option -g window-status-format " #I:#W " ;; set-option -g window-status-current-format " #I:#W " ;; bind-key -n C-PgUp previous-window ;; bind-key -n C-PgDn next-window ;; bind-key -n M-Left select-pane -L ;; bind-key -n M-Right select-pane -R ;; bind-key -n M-Up select-pane -U ;; bind-key -n M-Down select-pane -D ;; bind-key -n M-S-Left swap-window -t "-1;" select-window -t -1 ;; bind-key -n M-S-Right swap-window -t "+1;" select-window -t +1s is a test
18611:1769053920.602689 cmdq_fire_command <global>: (2814) set-hook -g client-attached "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\""26h\033[?25l\033[?12l\033[?25h\033[?2026l\033[?2026h\18629:1769053920.602728 yylex_token: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 \033(B\033[m\033[48;2;30;30;46m                                                                                         18631:1769053920.602733 cmd_parse_build_commands 0:1: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&149 \033(B\033[m\033[?12l\033[?25h\033[4;3H\033[?2026lecho "this is a \033[0m\033[1m\033[31mtest\033[0m"\18632:1769053920.602737 args_parse_flags: next gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1[0m:\033[39m$ \033[?2026h\033[?25l\033[?12l\033[?25h\033[?2026lrg \033[0m\033[1m\033[31mtest\033[0m\r\r\n\r\n\018634:1769053920.602741 args_parse: 1 = gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 (type STRING).com\r\n\033[0m\033[32m673\033[0m:127.0.0.1 de.si\033[0m\033[1m\033[31mtest\033
18635:1769053920.602745 cmd_parse_build_commands: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"                                                                   [0mat.com\r\n\033[0m\033[32m718639:1769053920.602758 cmdq_fire_command <global>: (2817) set-hook -g session-created "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\""3[0mat.com\r\n\033[0m\033[32m850\033[0m:127.0.0.1 int18657:1769053920.602795 yylex_token: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&13[0m\033[1m\033[31mtest\033[0mat.com\t# may interfere with duo.nl\r\n\033[0m\033[32m1172\033[0m:127.0.0.1 se.si\033[0m\0318659:1769053920.602799 cmd_parse_build_commands 0:1: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1om\r\n\033[0m\033[32m1179\033[0m:127.0.0.1 si\033[0m\033[1m\033[31mtest\033[0mats.tiscali.co.uk\r\n\033[18660:1769053920.602803 args_parse_flags: next gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1m\033[32m1428\033[0m:127.0.0.1 uk.si\033[0m\033[1m\033[31mtest\033[0mat.com\r\n\033[0m\033[32m1593\033[0m:127.018662:1769053920.602807 args_parse: 1 = gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 (type STRING)033[31mtest\033[0m.ishvara-yoga.com\r\n\033[0m\033[32m2710\033[0m:127.0.0.1 ec\033[0m\033[1m\033[31mtest18663:1769053920.602810 cmd_parse_build_commands: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"                                                                                                18667:1769053920.602822 cmdq_fire_command <global>: (2820) set-hook -g pane-focus-in "run-shell \\"gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1\\""                                                       18685:1769053920.602861 yylex_token: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
18687:1769053920.602865 cmd_parse_build_commands 0:1: gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
18688:1769053920.602868 args_parse_flags: next gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1
18690:1769053920.602872 args_parse: 1 = gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1 (type STRING)
18691:1769053920.602876 cmd_parse_build_commands: run-shell "gpg-connect-agent updatestartuptty /bye >/dev/null 2>&1"
19852:1769053920.607017 clients_calculate_size: type is latest
20057:1769053920.609063 clients_calculate_size: type is latest
# ...concatenated for space
```

### What is the expected behavior?

should have only returned a few matches in subdirectories

note: this happens with vanilla grep too actually (?)

---
