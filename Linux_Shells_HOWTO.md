# Linux Shells

csh's syntax resembles C language.

<table>
<tr>
<td></td><td><b>tcsh/csh</b></td>
<td><b>bash</b></td>
</tr>

<tr>
<td>Show dir in prompt</td>
<td>
<pre lang="shell">
# %~ the current directory, using ~ for $HOME
# %/ the full pathname of the current directory
# %c or %. trailing component of the current dir
set prompt='%~ '
</pre>
</td>
<td><span style="font-weight:bold">bash</span></td>
</tr>

<tr>
<td>redirect string to stdin</td>
<td>
<b>Can not be done!!??</b>

<< string : Take stdin until the next occurrence of 'string' at the beginning
of a line (also called a here document)
</td>
<td>
<code>sed 's/:/ /' <<< $LD_LIBRARY_PATH</code>
</td>
</tr>

<tr>
<td>redirect both stdin & stderr to file</td>
<td> <code>ls >& file</code> </td>
<td>
<code>cmd >>file.txt 2>&1 # stderr 2> to the file std is using</code>
</td>
</tr>

</table>

## csh/tcsh

```sh
# re-direct stderr to stdout
 your_program >& /dev/stdout

# combine stdout and stderr in pipeline's stdin
 your_program |& tee build.log

 ```

`set` vs. `setenv`
* `set` is used for current shell. Use it in your shell script only.
* `setenv` for current and any subshells (i.e. it will automatically export variables to subshell)

```sh
set var=val
setenv var value

```

### Shell Scripting

<table>
  <tr>
    <td></td><td><b>csh/tcsh</b></td><td><b>Bash</b></td>
  </tr>
  <tr>
    <td><b>Arguments</b></td>
    <td>
    <pre lang="sh">
$0 # program name
$1 # 1st argument
$2 # 2nd argument
$3 # if it doesn't exist, no error. Just get empty

$#argv     # index of last argument
$argv[2-]  # 2nd and on argument
    </pre>
    </td>
    <td>
    <pre lang="perl">
    </pre>
    </td>        
  </tr>
</table>
