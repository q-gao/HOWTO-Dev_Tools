<!-- TOC -->

- [Text/Data Processing](#textdata-processing)
    - [`sort`](#sort)
- [Terminal Multiplexer](#terminal-multiplexer)
    - [tmux](#tmux)
        - [Issues](#issues)
            - [Del not working](#del-not-working)
    - [screen](#screen)

<!-- /TOC -->

# Text/Data Processing

## `sort`

[sort on a column](https://stackoverflow.com/questions/17430470/sort-a-tab-delimited-file-based-on-column-sort-command-bash)
- `-k`: key
- `-V`: numerical

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