# Text Processing Tools

## grep

grep search PATTERN in to-be-searched FILES and OUTPUT
`grep --help` is fairly easy to read.

### PATTERN: Regular expression

By default, grep uses Basic Regular expression(BRE). Use `-E` to use Extended RE(ERE)


```sh
# ^: begining of line
grep -A1 "^>" t.ecsim
```

### Specify to-be-searched file

```sh
# grep "*.m" files (-r recursive. It is == "-d recurse");
grep -r --include="*.m" "Touch Velocity" *
```

### OUTPUT

#### Context Control
Output context = leading and trailing context

```
Context control:
  -B, --before-context=NUM  print NUM lines of leading context
  -A, --after-context=NUM   print NUM lines of trailing context
  -C, --context=NUM         print NUM lines of output context
  -NUM                      same as --context=NUM
```
`-A1` or `-A 1` is the same as `--after-context=1`

#### Process Matched files
`-ls` list matched files

##### `-exec`

```sh
#find will execute grep and will substitute {} with the filename(s) found.
#';': a single grep command for each file is executed
find . -exec grep chrome {} \;
#'+':  as many files as possible are given as parameters to grep at once.
find . -exec grep chrome {} +
```

## sed

![sed workflow](https://www.tutorialspoint.com/sed/images/sed_workflow.jpg)
sed has two buffers
- **pattern space**: store the current text line and sed commands edit it
- **hold space**: used to hold text for subsequent cycle processing

## My Frequently Used Scripts
`sed -f SelIndent.sed <text_file>` will indent lines without leading `'>'` where `SelIndent.sed` is
```sh
# sed auto prints "pattern space" if it is explicitly printed by 'p'.
# use -n option to suppress auto printing of pattern space
#----------------------------------------------------------
# find lines with leading '<'. '!' means not match
# 'b' means branch/jump
/^</!b AddLeadSpace
#b without label jumps to the end
b
:AddLeadSpace
# insert spaces at the begining
s/^/     /
```
Or `sed '/^</! b AddLeadSpace; b; :AddLeadSpace s/^/   /;' <text_file>`
