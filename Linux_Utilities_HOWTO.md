 
 # Terminal Multiplexer

 ## tmux

`.tmux.conf`:
```sh
# Use Ctrl-a as prefix (like in screen)
unbind C-b
set -g prefix C-a
bind C-a send-prefix
```

[tmux cheat sheet](https://gist.github.com/MohamedAlaa/2961058#file-tmux-cheatsheet-markdown)

[tmux crash course](https://robots.thoughtbot.com/a-tmux-crash-course)

### Issues

#### Del not working
tmux always sends ^? for backspace. It works out what key to expect from termios. Make sure stty verase matches what your terminal is configured to send for backspace.
https://github.com/tmux/tmux/issues/335

## screen
 
 # Misc
 ## Send email in Shell
 ```shell
 echo "hello world" | mail -s "a subject" someone@somewhere.com
 cat /path/to/file | mail -s "your subject" your@email.com
 ```