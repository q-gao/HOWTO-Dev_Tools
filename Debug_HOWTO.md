Debug

# core dump

* **Core size** limit
  * Default value is specified in `/etc/security/limits.conf` (all resource limits). It is zero if not specified.
  * `ulimit` is a **shell bult-in** (only in **bash**; tcsh not support it) and can only change limits for this shell and the processes started from this shell.

* Core dump handler is specified in `/proc/sys/kernel/core_pattern`. e.g.,

```sh
# pipe core dump to apport that writes it to 
#/var/crash/_path_to_program.userid.crash, 
#BUT it will only do so for applications installed from the main ubuntu
# apt repositories. Dump for others is in the current directory.
|/usr/share/apport/apport %p %s %c %P
```

```sh
# dump to current directory
echo "core.%e.%p" > /proc/sys/kernel/core_pattern 
```

* Disable/enable apport (starting at boot) in `/etc/default/apport`

## core dump gdb analysis

`gdb <executable> <core-file>` or gdb `<executable> -c <core-file>`

```sh
# show the call stack at crash
(gdb) where

# same as above, but it lists more detailed stack info
(gdb) bt full
```

[Detailed guide](http://www.unknownroad.com/rtfm/gdbtut/gdbadvanced.html#CORE): part of RMS's gdb debug tutorial.




