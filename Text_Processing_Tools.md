# Text Processing Tools

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
