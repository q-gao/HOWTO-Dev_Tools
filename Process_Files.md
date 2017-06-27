# Process Files



`-n 1` option to xargs https://unix.stackexchange.com/questions/304692/piping-list-of-files-and-directories-to-du-only-shows-sizes-of-directories

## `find`
Get the total size of a list of files: 'c' passed to du = accumulation?
what `+` used for?
```sh
find ./photos/john_doe -type f -name '*.jpg' -exec du -ch {} + | grep total$
```

## Get File Info
Get the sizes of a list of files:
```sh
# -c : print the total size (cumulative)
du -ch *.jpg
```
